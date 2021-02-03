## 实战

### 搭建开发环境

```python
python --version
Python 3.8.1
pip install aiohttp
pip install jinja2
pip install aiomysql
```

### 编写Web App骨架

```python
import logging; logging.basicConfig(level=logging.INFO)
import asyncio
from aiohttp import web

def index(request):
    return web.Response(body=b'<h1>Awesome</h1>', content_type='text/html')

async def init(loop):
    app = web.Application()
    app.router.add_route('GET', '/', index)
    # srv = await loop.create_server(app.make_handler(), '127.0.0.1', 9000)
    # srv = await loop.create_server(app._make_handler(), '127.0.0.1', 9000)
    runner = web.AppRunner(app)
    await runner.setup()
    srv = await loop.create_server(runner.server, '127.0.0.1', 9000)
    logging.info('server started at http://127.0.0.1:9000...')
    return srv

loop = asyncio.get_event_loop()
loop.run_until_complete(init(loop))
loop.run_forever()
```

### 编写ORM

对象关系映射（Object Relational Mapping），将操作数据库函数封装起来。由于Web框架使用协程异步模型，所有用户都是一个线程服务的，不能调用同步IO操作，否则等待一个IO时无法响应其他用户。`aiomysql`为MySQL数据库提供了异步IO的驱动。

```python
import asyncio, logging, aiomysql

# 创建日志函数
def log(sql, args=()):
    logging.info('SQL: %s' % sql)

# 异步IO创建连接池函数，pool用法见下：
# https://aiomysql.readthedocs.io/en/latest/pool.html?highlight=create_pool
async def create_pool(loop, **kw):
    logging.info('create database connection pool...')
    # 声明 __pool 为全局变量
    global __pool
    # 使用这些基本参数来创建连接池
    __pool = await aiomysql.create_pool(
        host=kw.get('host', 'localhost'),
        port=kw.get('port', 3306),
        user=kw['user'],
        password=kw['password'],
        db=kw['db'],
        charset=kw.get('charset', 'utf8'),
        autocommit=kw.get('autocommit', True),
        maxsize=kw.get('maxsize', 10),
        minsize=kw.get('minsize', 1),
        loop=loop
    )

async def select(sql, args, size=None):
    log(sql, args)
    global __pool
    # with-as: 方便执行清理工作，如 close 和 exit：
    # https://www.jianshu.com/p/c00df845323c
    # await 可以让它后面执行的语句等一会，防止多个程序同时执行，达到异步效果
    with (await __pool) as conn:
        # cursor 游标，conn应该也是个池
        # aiomysql.DictCursor 返回字典
        cur = await conn.cursor(aiomysql.DictCursor)
        # cursor 游标实例调用 execute 来执行一条单独的 SQL 语句，参考自：
        # https://docs.python.org/zh-cn/3.8/library/sqlite3.html?highlight=execute#cursor-objects
        await cur.execute(sql.replace('?', '%s'), args or ())
        # size 为空时为 False，上面定义了初始值为 None ，具体得看传入的参数有没有定义 size
        if size:
            # fetchmany 可以获取行数为 size 的多行查询结果集，返回一个列表
            rs = await cur.fetchmany(size)
        else:
            # fetchall 可以获取一个查询结果的所有（剩余）行，返回一个列表
            rs = await cur.fetchall()
        #  close() ，立即关闭 cursor ，从这一时刻起该 cursor 将不再可用
        await cur.close()
        # 日志：提示返回了多少行
        logging.info('rows returned: %s' % len(rs))
        # 现在我们知道了，这个 select 函数给我们从 SQL 返回了一个列表
        return rs

# execute ：Insert,Update,Delete
async def execute(sql, args):
    log(sql)
    global __pool
    with (await __pool) as conn:
        try:
            cur = await conn.cursor()
            await cur.execute(sql.replace('?', '%s'), args)
            # rowcount 获取行数，应该表示的是该函数影响的行数
            affected = cur.rowcount
            await cur.close()
        except BaseException as _:
            # 源码 except BaseException as e: 反正不用这个 e ，改掉就不报错
            # 将错误抛出，BaseEXception
            raise
        # 返回行数
        return affected

# 这个函数只在下面的 Model元类中被调用， 作用好像是加数量为 num 的'?'
def create_args_string(num):
    L = []
    for n in range(num):
        L.append('?')
    return ', '.join(L)


# Model 只是一个基类，所以先定义 ModelMetaclass ，再在定义 Model 时使用 metaclass 参数
# 参考教程： https://www.liaoxuefeng.com/wiki/1016959663602400/1017592449371072
class ModelMetaclass(type):
    # __new__()方法接收到的参数依次是：
    # cls：当前准备创建的类的对象 class
    # name：类的名字 str
    # bases：类继承的父类集合 Tuple
    # attrs：类的方法集合
    def __new__(cls, name, bases, attrs):
        # 排除 Model 类本身，返回它自己
        if name == 'Model':
            return type.__new__(cls, name, bases, attrs)
        # 获取 table 名称
        tableName = attrs.get('__table__', None) or name
        # 日志：找到名为 name 的 model
        logging.info('found model: %s (table: %s)' % (name, tableName))
        # 获取 所有的 Field 和主键名
        mappings = dict()
        fields = []
        primaryKey = None
        # attrs.items 取决于 __new__ 传入的 attrs 参数
        for k, v in attrs.items():
            # isinstance 函数：如果 v 和 Field 类型相同则返回 True ，不相同则 False
            if isinstance(v, Field):
                logging.info(' found mapping: %s ==> %s' % (k, v))
                mappings[k] = v
                # 这里的 v.primary_key 我理解为 ：只要 primary_key 为 True 则这个 field 为主键
                if v.primary_key:
                    # 找到主键，如果主键 primaryKey 有值时，返回一个错误
                    if primaryKey:
                        raise RuntimeError('Duplicate primary key for field: %s' % k)
                    # 然后直接给主键赋值
                    primaryKey = k
                else:
                    # 没找到主键的话，直接在 fields 里加上 k
                    fields.append(k)
        if not primaryKey:
            # 如果主键为 None 就报错
            raise RuntimeError('Primary key not found.')
        for k in mappings.keys():
            # pop ：如果 key 存在于字典中则将其移除并返回其值，否则返回 default
            attrs.pop(k)
        escaped_fields = list(map(lambda f: '`%s`' % f, fields))
        attrs['__mappings__'] = mappings  # 保存属性和列的映射关系
        attrs['__table__'] = tableName  # table 名
        attrs['__primary_key__'] = primaryKey  # 主键属性名
        attrs['__fields__'] = fields  # 除主键外的属性名
        # 构造默认的 SELECT, INSERT, UPDAT E和 DELETE 语句
        attrs['__select__'] = 'select `%s`, %s from `%s`' % (primaryKey, ', '.join(escaped_fields), tableName)
        attrs['__insert__'] = 'insert into `%s` (%s, `%s`) values (%s)' % (
        tableName, ', '.join(escaped_fields), primaryKey, create_args_string(len(escaped_fields) + 1))
        attrs['__update__'] = 'update `%s` set %s where `%s`=?' % (
        tableName, ', '.join(map(lambda f: '`%s`=?' % (mappings.get(f).name or f), fields)), primaryKey)
        attrs['__delete__'] = 'delete from `%s` where `%s`=?' % (tableName, primaryKey)
        return type.__new__(cls, name, bases, attrs)


# metaclass 参数提示 Model 要通过上面的 __new__ 来创建
class Model(dict, metaclass=ModelMetaclass):
    def __init__(self, **kw):
        # super 用来引用父类 ModelMetaclass ？ super 文档：
        # https://docs.python.org/zh-cn/3.8/library/functions.html?highlight=super#super
        super(Model, self).__init__(**kw)

    # 返回参数为 key 的自身属性， 如果出错则报具体错误
    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)

    # 设置自身属性
    def __setattr__(self, key, value):
        self[key] = value

    # 通过属性返回想要的值
    def getValue(self, key):
        return getattr(self, key, None)

    #
    def getValueOrDefault(self, key):
        value = getattr(self, key, None)
        if value is None:
            # 如果 value 为 None，定位某个键； value 不为 None 就直接返回
            field = self.__mappings__[key]
            if field.default is not None:
                # 如果 field.default 不是 None ： 就把它赋值给 value
                value = field.default() if callable(field.default) else field.default
                logging.debug('using default value for %s: %s' % (key, str(value)))
                setattr(self, key, value)
        return value

    # *** 往 Model 类添加 class 方法，就可以让所有子类调用 class 方法
    @classmethod
    async def findAll(cls, where=None, args=None, **kw):
        ## find objects by where clause
        sql = [cls.__select__]
        # where 默认值为 None
        # 如果 where 有值就在 sql 加上字符串 'where' 和 变量 where
        if where:
            sql.append('where')
            sql.append(where)
        if args is None:
            # args 默认值为 None
            # 如果 findAll 函数未传入有效的 where 参数，则将 '[]' 传入 args
            args = []

        orderBy = kw.get('orderBy', None)
        if orderBy:
            # get 可以返回 orderBy 的值，如果失败就返回 None ，这样失败也不会出错
            # oederBy 有值时给 sql 加上它，为空值时什么也不干
            sql.append('order by')
            sql.append(orderBy)
        # 开头和上面 orderBy 类似
        limit = kw.get('limit', None)
        if limit is not None:
            sql.append('limit')
            if isinstance(limit, int):
                # 如果 limit 为整数
                sql.append('?')
                args.append(limit)
            elif isinstance(limit, tuple) and len(limit) == 2:
                # 如果 limit 是元组且里面只有两个元素
                sql.append('?, ?')
                # extend 把 limit 加到末尾
                args.extend(limit)
            else:
                # 不行就报错
                raise ValueError('Invalid limit value: %s' % str(limit))
        rs = await select(' '.join(sql), args)
        # 返回选择的列表里的所有值 ，完成 findAll 函数
        return [cls(**r) for r in rs]

    @classmethod
    async def findNumber(cls, selectField, where=None, args=None):
        ## find number by select and where
        # 找到选中的数及其位置
        sql = ['select %s _num_ from `%s`' % (selectField, cls.__table__)]
        if where:
            sql.append('where')
            sql.append(where)
        rs = await select(' '.join(sql), args, 1)
        if len(rs) == 0:
            # 如果 rs 内无元素，返回 None ；有元素就返回某个数
            return None
        return rs[0]['_num_']

    @classmethod
    async def find(cls, pk):
        ## find object by primary key
        # 通过主键找对象
        rs = await select('%s where `%s`=?' % (cls.__select__, cls.__primary_key__), [pk], 1)
        if len(rs) == 0:
            return None
        return cls(**rs[0])

    # *** 往 Model 类添加实例方法，就可以让所有子类调用实例方法
    async def save(self):
        args = list(map(self.getValueOrDefault, self.__fields__))
        args.append(self.getValueOrDefault(self.__primary_key__))
        rows = await execute(self.__insert__, args)
        if rows != 1:
            logging.warning('failed to insert record: affected rows: %s' % rows)

    async def update(self):
        args = list(map(self.getValue, self.__fields__))
        args.append(self.getValue(self.__primary_key__))
        rows = await execute(self.__update__, args)
        if rows != 1:
            logging.warning('failed to update by primary key: affected rows: %s' % rows)

    async def remove(self):
        args = [self.getValue(self.__primary_key__)]
        rows = await execute(self.__delete__, args)
        if rows != 1:
            logging.warning('failed to remove by primary key: affected rows: %s' % rows)
    # save , update , remove 这三个可以对比着来看

# 定义 Field
class Field(object):
    def __init__(self, name, column_type, primary_key, default):
        self.name = name
        self.column_type = column_type
        self.primary_key = primary_key
        self.default = default

    def __str__(self):
        return '<%s, %s:%s>' % (self.__class__.__name__, self.column_type, self.name)

# 定义 Field 子类及其子类的默认值
class StringField(Field):
    def __init__(self, name=None, primary_key=False, default=None, ddl='varchar(100)'):
        super().__init__(name, ddl, primary_key, default)

class BooleanField(Field):
    def __init__(self, name=None, default=False):
        super().__init__(name, 'boolean', False, default)

class IntegerField(Field):
    def __init__(self, name=None, primary_key=False, default=0):
        super().__init__(name, 'bigint', primary_key, default)

class FloatField(Field):
    def __init__(self, name=None, primary_key=False, default=0):
        super().__init__(name, 'real', primary_key, default)

class TextField(Field):
    def __init__(self, name=None, default=None):
        super().__init__(name, 'text', False, default)
```

### 编写Model

有了ORM就可以把Web App需要的3个表用`Model`表示出来：

```python
import time, uuid

from www.orm import Model, StringField, BooleanField, FloatField, TextField

def next_id():
    # uuid.uuid4() 可以生成一个随机的 UUID ，目的是区别不同事务（大概）
    # hex 可以把自身返回为一个16进制整数，所以这个函数就是生成各种 id ，里面还包含时间
    return '%015d%s000' % (int(time.time() * 1000), uuid.uuid4().hex)

class User(Model):
    __table__ = 'users'
    # varchar 为 MySQL 里的数据类型
    id = StringField(primary_key=True, default=next_id, ddl='varchar(50)')
    email = StringField(ddl='varchar(50)')
    passwd = StringField(ddl='varchar(50)')
    admin = BooleanField()
    name = StringField(ddl='varchar(50)')
    image = StringField(ddl='varchar(500)')
    created_at = FloatField(default=time.time)
    # time.time 可以设置当前日期和时间， 把日期和时间储存为 float 类型 ， 记录到 create_at 里

class Blog(Model):
    __table__ = 'blogs'

    id = StringField(primary_key=True, default=next_id, ddl='varchar(50)')
    user_id = StringField(ddl='varchar(50)')
    user_name = StringField(ddl='varchar(50)')
    user_image = StringField(ddl='varchar(500)')
    name = StringField(ddl='varchar(50)')
    summary = StringField(ddl='varchar(200)')
    content = TextField()
    created_at = FloatField(default=time.time)

class Comment(Model):
    __table__ = 'comments'

    id = StringField(primary_key=True, default=next_id, ddl='varchar(50)')
    blog_id = StringField(ddl='varchar(50)')
    user_id = StringField(ddl='varchar(50)')
    user_name = StringField(ddl='varchar(50)')
    user_image = StringField(ddl='varchar(500)')
    content = TextField()
    created_at = FloatField(default=time.time)
```

数据库的测试：

```python
# https://www.liaoxuefeng.com/wiki/1177760294764384
# 这里对我们前面写的进行测试， 看看能不能工作
# 安装好 MySQL 后， 第一步：从开始菜单打开 'MySQL Notifier 1.1.8' ，并在任务栏中开启它
# 第二步：开始菜单打开 'MySQL 5.6 Command Line Client' ，并输入安装时你设置的 root 密码
# 输密码的时候不会显示密码，你只管输，然后回车
# 第三步：把下面注释的代码复制到 SQL 命令行里，然后回车
# 这里初始化了一个名为 moe 的数据库表
'''
-- schema.sql
drop database if exists moe;
drop user if exists 'www-data'@'localhost';
create database moe;
use moe;
create user 'www-data'@'localhost' identified by 'www-data';
alter user 'www-data'@'localhost' identified with mysql_native_password by 'www-data';
grant select, insert, update, delete on moe.* to 'www-data'@'localhost';
create table users (
    `id` varchar(50) not null,
    `email` varchar(50) not null,
    `passwd` varchar(50) not null,
    `admin` bool not null,
    `name` varchar(50) not null,
    `image` varchar(500) not null,
    `created_at` real not null,
    unique key `idx_email` (`email`),
    key `idx_created_at` (`created_at`),
    primary key (`id`)
) engine=innodb default charset=utf8;
create table blogs (
    `id` varchar(50) not null,
    `user_id` varchar(50) not null,
    `user_name` varchar(50) not null,
    `user_image` varchar(500) not null,
    `name` varchar(50) not null,
    `summary` varchar(200) not null,
    `content` mediumtext not null,
    `created_at` real not null,
    key `idx_created_at` (`created_at`),
    primary key (`id`)
) engine=innodb default charset=utf8;
create table comments (
    `id` varchar(50) not null,
    `blog_id` varchar(50) not null,
    `user_id` varchar(50) not null,
    `user_name` varchar(50) not null,
    `user_image` varchar(500) not null,
    `content` mediumtext not null,
    `created_at` real not null,
    key `idx_created_at` (`created_at`),
    primary key (`id`)
) engine=innodb default charset=utf8;
'''

# 第四步：把下面这部分代码放在 python 里跑 （直接跑我这个 test_sql.py 也行）
import www.orm
import asyncio
from www.models import User, Blog, Comment
# 这部分源码来自 https://aodabo.tech/blog/001546713871394a2814d2c180b4e6f8d30c62a3eaf218a000
async def test(loop):                      # *** 注意此处的密码填自己设的密码 ***
    await www.orm.create_pool(loop=loop, user='root', password='369874125', db='moe')
                                           # *** 注意此处的密码填自己设的密码 ***
    u = User(name='Test', email='test@qq.com', passwd='1234567890', image='about:blank')
    await u.save()
    ## 网友指出添加到数据库后需要关闭连接池，否则会报错 RuntimeError: Event loop is closed
    www.orm.__pool.close()
    await www.orm.__pool.wait_closed()
if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    loop.run_until_complete(test(loop))
    loop.close()

# 第五步：在 sql 命令行里输入 'SELECT * FROM users;'  然后回车（别漏了分号）
# 显示 test@qq.com 代表测试成功了
```



### 编写Web框架

### 编写MVC

### 构建前端

### 编写API

### 用户注册和登录

### 编写日志创建页

### 编写日志列表页

### 提升开发效率

### 完成Web App

### 部署Web App

### 编写移动App