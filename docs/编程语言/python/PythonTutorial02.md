## 常用内建模块

### datetime

datetime是Python处理日期和时间的标准库。

**获取当前日期和时间**

```python
>>> from datetime import datetime
>>> now = datetime.now() # 获取当前datetime
>>> print(now)
2021-01-21 14:59:11.992729
>>> print(type(now))
<class 'datetime.datetime'>
```

注意`datetime`模块还包含一个`datetime`类，通过`from datetime import datetime`导入。如果仅导入`import datetime`，则必须引用全名`datetime.datetime`。

**获取指定日期和时间**

```python
>>> from datetime import datetime
>>> dt = datetime(2021, 1, 21, 15, 00) # 用指定日期时间创建datetime
>>> print(dt)
2021-01-21 15:00:00
```

**datetime转换为timestamp**

1970年1月1日 `00:00:00 UTC+00:00`时区的时刻称为epoch time，记为`0`（1970年以前的时间timestamp为负数），当前时间就是相对于epoch time的秒数，称为timestamp。

```python
timestamp = 0 = 1970-1-1 00:00:00 UTC+0:00
```

对应北京时间：

```
timestamp = 0 = 1970-1-1 08:00:00 UTC+8:00
```

把一个`datetime`类型转换为timestamp只需要简单调用`timestamp()`方法：

```python
>>> from datetime import datetime
>>> dt = datetime(2021, 1, 21, 12, 00) # 用指定日期时间创建datetime
>>> dt.timestamp() # 把datetime转换为timestamp
1611201600.0
```

注意Python的timestamp是一个浮点数，整数表示秒。在其它编程语言（如Java和JavaScript）中整数表示毫秒，只需要把timestamp除以1000就得到Python的浮点表示方法。

**timestamp转换为datetime**

使用`datetime`提供的`fromtimestamp()`方法：

```python
>>> from datetime import datetime
>>> t = 1611201600.0
>>> print(datetime.fromtimestamp(t))
2021-01-21 12:00:00
```

转换是在timestamp和本地时间做转换：

```python
# 本地时间是指当前操作系统设定的时区时间，如北京时区是东8区本地时间：
2021-01-21 12:00:00
# 实际就是UTC+8:00时区的时间
2021-01-21 12:00:00
# 而英国伦敦标准时间与北京时间差了8小时，也就是UTC+0:00时区时间：
2021-01-21 04:00:00 UTC+0:00
```

timestamp也可以直接转换到UTC标准时区时间：

```python
>>> from datetime import datetime
>>> t = 1611201600.0
# 本地时间
>>> print(datetime.fromtimestamp(t))
2021-01-21 12:00:00
# UTC时间
>>> print(datetime.utcfromtimestamp(t))
2021-01-21 04:00:00
```

**str转换为datetime**

通过`datetime.strptime()`实现，需要一个日期和时间的格式化字符串：

```python
>>> from datetime import datetime
>>> cday = datetime.strptime('2021-1-21 12:00:00', '%Y-%m-%d %H:%M:%S')
>>> print(cday)
2021-01-21 12:00:00
```

字符串`'%Y-%m-%d %H:%M:%S'`规定了日期和时间部分的格式。[详细说明参考文档](https://docs.python.org/zh-cn/3/library/datetime.html#strftime-strptime-behavior)

**datetime转换为str**

通过`strftime()`实现，需要一个日期和时间的格式化字符串：

```python
>>> from datetime import datetime
>>> now = datetime.now()
>>> print(now.strftime('%a, %b %d %H:%M'))
Thu, Jan 21 16:22
```

**datetime加减**

```python
>>> from datetime import datetime, timedelta
>>> now = datetime.now()
>>> now
datetime.datetime(2021, 1, 21, 16, 22, 58, 332219)
>>> now + timedelta(hours=10)
datetime.datetime(2021, 1, 22, 2, 22, 58, 332219)
>>> now - timedelta(days=1)
datetime.datetime(2021, 1, 20, 16, 22, 58, 332219)
>>> now + timedelta(days=2, hours=12)
datetime.datetime(2021, 1, 24, 4, 22, 58, 332219)
```

**本地时间转换为UTC时间**

例如北京时间是UTC+8:00时区的时间，而UTC时间指UTC+0:00时区的时间。

一个`datetime`类型有一个时区属性`tzinfo`默认为`None`，所以无法区分这个`datetime`是哪个时区，除非给`datetime`设置一个时区：

```python
>>> from datetime import datetime, timedelta, time
# 创建时区UTC+8:00
>>> tz_utc_8 = timezone(timedelta(hours=8))
>>> now = datetime.now()
>>> now
datetime.datetime(2021, 1, 21, 16, 30, 47, 985082)
# 强制设置为UTC+8:00, 恰好系统时区是UTC+8:00
>>> dt = now.replace(tzinfo=tz_utc_8)
>>> dt
datetime.datetime(2021, 1, 21, 16, 30, 47, 985082, tzinfo=datetime.timezone(date
time.timedelta(seconds=28800)))
```

**时区转换**

可以通过`utcnow()`拿到当前的UTC时间，再转换为任意时区的时间：

```python
# 拿到UTC时间，并强制设置时区为UTC+0:00:
>>> utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)
>>> print(utc_dt)
2021-01-21 08:35:45.838118+00:00
# astimezone()将转换时区为北京时间:
>>> bj_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))
>>> print(bj_dt)
2021-01-21 16:35:45.838118+08:00
# astimezone()将转换时区为东京时间:
>>> tokyo_dt = utc_dt.astimezone(timezone(timedelta(hours=9)))
>>> print(tokyo_dt)
2021-01-21 17:35:45.838118+09:00
# astimezone()将bj_dt转换时区为东京时间:
>>> tokyo_dt2 = bj_dt.astimezone(timezone(timedelta(hours=9)))
>>> print(tokyo_dt2)
2021-01-21 17:35:45.838118+09:00
```

**练习**

假设你获取了用户输入的日期和时间如`2015-1-21 9:01:30`，以及一个时区信息如`UTC+5:00`，均是`str`，请编写一个函数将其转换为timestamp：

```python
# -*- coding:utf-8 -*-
import re
from datetime import datetime, timezone, timedelta
def to_timestamp(dt_str, tz_str):
# 格式化时间str转datetime
    dt = datetime.strptime(dt_str, '%Y-%m-%d %H:%M:%S')
# 获取时区
    reg_tz = re.compile(r'\w+([+|-])(\d+)\:(\d+)')
    sig, h, m = reg_tz.match(tz_str).groups()
# 重置时区
    dt = dt.replace(tzinfo=timezone(timedelta(hours=int(sig+h), minutes=int(sig+m))))
    return dt.timestamp()
# 测试:
t1 = to_timestamp('2015-6-1 08:10:30', 'UTC+7:00')
assert t1 == 1433121030.0, t1
t2 = to_timestamp('2015-5-31 16:10:30', 'UTC-09:00')
assert t2 == 1433121030.0, t2
print('ok')
```

### collections

**namedtuple**

`tuple`可以表示不变集合，例如，一个点的二维坐标就可以表示成：

```python
>>> p = (1, 2)
```

`namedtuple`是一个函数，它用来创建一个自定义的`tuple`对象，并且规定了`tuple`元素的个数，并可以用属性而不是索引来引用`tuple`的某个元素。具备tuple的不变性，又可以根据属性来引用：

```python
>>> from collections import namedtuple
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(1, 2)
>>> p.x
1
>>> p.y
2
# 类似用坐标和半径表示一个圆
# namedtuple('名称', [属性list]):
Circle = namedtuple('Circle', ['x', 'y', 'r'])
```

创建的`Point`对象时`tuple`的一种子类：

```python
>>> isinstance(p, Point)
True
>>> isinstance(p, tuple)
True
```

**deque**

使用`list`存储数据时按索引访问元素很快，但是插入和删除元素很慢，因为`list`是线性存储，数据量大的时候插入和删除效率很低。

`deque`实现高效插入和删除操作的双向列表，适合用于队列和栈：

```python
>>> from collections import deque
>>> q = deque(['a', 'b', 'c'])
>>> q.append('x')
>>> q.appendleft('y')
>>> q
deque(['y', 'a', 'b', 'c', 'x'])
>>> q.pop() # 删末尾
>>> q.popleft() # 删左开头
```

**defaultdict**

使用`dict`时，如果引用的Key不存在就会抛出`KeyError`。如果希望key不存在时返回一个默认值可以用`defaultdict`，返回函数在创建对象时传入：

```python
>>> from collections import defaultdict
>>> dd = defaultdict(lambda: 'N/A')
>>> dd['key1'] = 'abc'
>>> dd['key1'] # key1存在
'abc'
>>> dd['key2'] # key2不存在，返回默认值
'N/A'
```

**OrderedDict**

使用`dict`时Key是无序的。对`dict`做迭代时无法确定Key的顺序。

要保持Key的顺序，可以用`OrderedDict`：

```python
>>> from collections import OrderedDict
>>> d = dict([('a', 1), ('b', 2), ('c', 3)])
>>> d # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
>>> od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
>>> od # OrderedDict的Key是有序的
OrderedDict([('a', 1), ('b', 2), ('c', 3)])
# OrderedDict的key按照插入顺序排列而不是key本身
>>> od = OrderedDict()
>>> od['z'] = 1
>>> od['y'] = 2
>>> od['x'] = 3
>>> list(od.keys())
['z', 'y', 'x']
```

`OrderedDict`可以实现一个FIFO（先进先出）的dict，当容量超出限制时，先删除最早添加的Key：

```python
from collections import OrderedDict

class LastUpdatedOrderedDict(OrderedDict):

    def __init__(self, capacity):
        super(LastUpdatedOrderedDict, self).__init__()
        self._capacity = capacity

    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:
            last = self.popitem(last=False)
            print('remove:', last)
        if containsKey:
            del self[key]
            print('set:', (key, value))
        else:
            print('add:', (key, value))
        OrderedDict.__setitem__(self, key, value)
```

**ChainMap**

`ChainMap`可以把一组`dict`串起来并组成一个逻辑上的`dict`。`ChainMap`本身也是一个dict，但是查找的时候会按照顺序在内部的dict依次查找。

举个例子：应用程序往往都需要传入参数，参数可以通过命令行传入，可以通过环境变量传入，还可以有默认参数。用`ChainMap`实现参数的优先级查找，即先查命令行参数，如果没有传入，再查环境变量，如果没有就使用默认参数。

查找`user`和`color`这两个参数：

```python
from collections import ChainMap
import os, argparse

# 构造缺省参数:
defaults = {
    'color': 'red',
    'user': 'guest'
}

# 构造命令行参数:
parser = argparse.ArgumentParser()
parser.add_argument('-u', '--user')
parser.add_argument('-c', '--color')
namespace = parser.parse_args()
command_line_args = { k: v for k, v in vars(namespace).items() if v }

# 组合成ChainMap:
combined = ChainMap(command_line_args, os.environ, defaults)

# 打印参数:
print('color=%s' % combined['color'])
print('user=%s' % combined['user'])
```

```python
# 没有参数时打印默认参数
$ python3 use_chainmap.py 
color=red
user=guest
# 传入命令行参数时，优先使用命令行参数
$ python3 use_chainmap.py -u bob
color=red
user=bob
# 同时传入命令行参数和环境变量，命令行参数优先级较高
$ user=admin color=green python3 use_chainmap.py -u bob
color=green
user=bob
```

**Counter**

`Counter`是一个简单的计数器，实际上也是`dict`的一个子类，例如统计字符出现的个数（次数）：

```python
>>> from collections import Counter
>>> c = Counter()
>>> for ch in 'programming':
...     c[ch] = c[ch] + 1
...
>>> c
Counter({'g': 2, 'm': 2, 'r': 2, 'a': 1, 'i': 1, 'o': 1, 'n': 1, 'p': 1})
>>> c.update('hello') # 也可以一次性update
>>> c
Counter({'r': 2, 'o': 2, 'g': 2, 'm': 2, 'l': 2, 'p': 1, 'a': 1, 'i': 1, 'n': 1, 'h': 1, 'e': 1})
```

### base64

Base64是一种用64个字符来表示任意二进制数据的方法。原理是先准备一个包含64个字符的数组，对二进制数据进行处理，每3个字节一组，一共是3×8=24 bit，划为4组就是64bit。

![](https://qwq.lsaiah.cn/picgo/949444125467040.png)

得到4个数字作为索引，查表获得相应4个字符就是编码后的字符串。所以Base64编码会把3字节的二进制数据编码为4字节的文本数据，长度增加33%。如果二进制数据不是3的倍数最后剩下1或2个字节，用`\x00`字节在末尾补足后在编码末尾加上1个或2个`=`号，表示补了多少字节，解码的时候会自动去掉。

Python内置的`base64`可以直接进行base64的编解码：

```python
>>> import base64
>>> base64.b64encode(b'binary\x00string')
b'YmluYXJ5AHN0cmluZw=='
>>> base64.b64decode(b'YmluYXJ5AHN0cmluZw==')
b'binary\x00string'
```

由于Base64编码后可能出现字符`+`和`/`，在URL中不能直接作为参数，所以还有一种url safe的Base64编码，把`+`和`/`分别变成`-`和`_`：

```python
>>> base64.b64encode(b'i\xb7\x1d\xfb\xef\xff')
b'abcd++//'
>>> base64.urlsafe_b64encode(b'i\xb7\x1d\xfb\xef\xff')
b'abcd--__'
>>> base64.urlsafe_b64decode('abcd--__')
b'i\xb7\x1d\xfb\xef\xff'
```

Base64是一种通过查表的编码方法，不能用于加密。适用于小段内容的编码，比如数字证书签名、Cookie的内容等。由于`=`字符也可能出现在Base64编码中，在URL、Cookie里面会造成歧义，所以很多Base64编码后会把`=`去掉：

```python
# 标准Base64:
'abcd' -> 'YWJjZA=='
# 自动去掉=:
'abcd' -> 'YWJjZA'
```

Base64是把3个字节变为4个字节，所以Base64编码的长度永远是4的倍数，因此需要加上`=`把Base64字符串的长度变为4的倍数，就可以正常解码了。

**练习**

写一个能处理去掉`=`的base64解码函数：

```python
# -*- coding: utf-8 -*-
import base64
def safe_base64_decode(s):
    # 判断是否是4的整数，不够的在末尾加等号
    if len(s) % 4 != 0:
        s = s + bytes('=', encoding='utf-8') * (4 - len(s) % 4)
    # 解决字符串和bytes类型
    if not isinstance(s, bytes):
        s = bytes(s, encoding='utf-8')
    # 解码 
    base64_str = base64.urlsafe_b64decode(s)
    return base64_str
# 测试:
assert b'abcd' == safe_base64_decode(b'YWJjZA=='), safe_base64_decode('YWJjZA==')
assert b'abcd' == safe_base64_decode(b'YWJjZA'), safe_base64_decode('YWJjZA')
print('ok')
```

### struct

在C语言中可以用struct、union来处理字节，以及字节和int，float的转换。在Python中由于`b'str'`可以表示字节，所以字节数组＝二进制str。

要把一个32位无符号整数变成字节（4个长度的`bytes`），配合位运算符这样写：

```python
>>> n = 10240099
>>> b1 = (n & 0xff000000) >> 24
>>> b2 = (n & 0xff0000) >> 16
>>> b3 = (n & 0xff00) >> 8
>>> b4 = n & 0xff
>>> bs = bytes([b1, b2, b3, b4])
>>> bs
b'\x00\x9c@c'
```

非常麻烦，而且不能是浮点数。Python提供了一个`struct`模块来解决`bytes`和其他二进制数据类型的转换。`struct`的`pack`函数把任意数据类型变成`bytes`：

```python
>>> import struct
>>> struct.pack('>I', 10240099)
b'\x00\x9c@c'
```

第一个参数`'>I'`是处理指令，`>`表示字节顺序是big-endian，也就是网络序，`I`表示4字节无符号整数。后面的参数个数要和处理指令一致。

`unpack`把`bytes`变成相应的数据类型：

```python
>>> struct.unpack('>IH', b'\xf0\xf0\xf0\xf0\x80\x80')
(4042322160, 32896)
```

参数`>IH`表示后面的`bytes`依次变为`I`：4字节无符号整数和`H`：2字节无符号整数。性能要求不高的地方利用`struct`更方便，[数据类型参考文档](https://docs.python.org/zh-cn/3/library/struct.html#format-characters)。

Windows的位图文件（.bmp），用`struct`分析下，读取前30个字节来分析：

```python
f = open("123.bmp", 'rb')
s = f.read(0x1e)
>>> s = b'\x42\x4d\x38\x8c\x0a\x00\x00\x00\x00\x00\x36\x00\x00\x00\x28\x00\x00\x00\x80\x02\x00\x00\x68\x01\x00\x00\x01\x00\x18\x00'
```

文件头的结构顺序：两个字节：`'BM'`表示Windows位图，`'BA'`表示OS/2位图； 一个4字节整数：表示位图大小； 一个4字节整数：保留位，始终为0； 一个4字节整数：实际图像的偏移量； 一个4字节整数：Header的字节数； 一个4字节整数：图像宽度； 一个4字节整数：图像高度； 一个2字节整数：始终为1； 一个2字节整数：颜色数。

组合起来用`unpack`读取显示`b'B'`、`b'M'`说明是Windows位图，位图大小为640x360，颜色数为24：

```python
>>> struct.unpack('<ccIIIIIIHH', s)
(b'B', b'M', 691256, 0, 54, 40, 640, 360, 1, 24)
```

**练习**

编写一个`bmpinfo.py`，可以检查任意文件是否是位图文件，如果是，打印出图片大小和颜色数。

```python
# -*- coding: utf-8 -*-
import base64, struct
bmp_data = base64.b64decode('Qk1oAgAAAAAAADYAAAAoAAAAHAAAAAoAAAABABAAAAAAADICAAASCwAAEgsAA' +
'AAAAAAAAAAA/3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3/' +
'/f/9//3//f/9//3//f/9/AHwAfAB8AHwAfAB8AHwAfP9//3//fwB8AHwAfAB8/3//f/9/A' +
'HwAfAB8AHz/f/9//3//f/9//38AfAB8AHwAfAB8AHwAfAB8AHz/f/9//38AfAB8/3//f/9' +
'//3//fwB8AHz/f/9//3//f/9//3//f/9/AHwAfP9//3//f/9/AHwAfP9//3//fwB8AHz/f' +
'/9//3//f/9/AHwAfP9//3//f/9//3//f/9//38AfAB8AHwAfAB8AHwAfP9//3//f/9/AHw' +
'AfP9//3//f/9//38AfAB8/3//f/9//3//f/9//3//fwB8AHwAfAB8AHwAfAB8/3//f/9//' +
'38AfAB8/3//f/9//3//fwB8AHz/f/9//3//f/9//3//f/9/AHwAfP9//3//f/9/AHwAfP9' +
'//3//fwB8AHz/f/9/AHz/f/9/AHwAfP9//38AfP9//3//f/9/AHwAfAB8AHwAfAB8AHwAf' +
'AB8/3//f/9/AHwAfP9//38AfAB8AHwAfAB8AHwAfAB8/3//f/9//38AfAB8AHwAfAB8AHw' +
'AfAB8/3//f/9/AHwAfAB8AHz/fwB8AHwAfAB8AHwAfAB8AHz/f/9//3//f/9//3//f/9//' +
'3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//3//f/9//38AAA==')
def bmp_info(data):
    result = struct.unpack('<ccIIIIIIHH', bmp_data[:30])
    if result[0:2] == (b'B', b'M'):
        return {'width': result[6], 'height': result[7], 'color': result[9]}
    print('This is not a bmp file!')
# 测试
bi = bmp_info(bmp_data)
assert bi['width'] == 28
assert bi['height'] == 10
assert bi['color'] == 16
print('ok')
```

### hashlib

Python的hashlib提供了常见的摘要算法（又称哈希算法，散列算法。通过一个函数把任意长度的数据转换为一个长度固定的数据串，通常用16进制的字符串表示）如MD5，SHA1等。

通过函数`f()`对任意长度的数据`data`计算出固定长度的摘要`digest`，通过`digest`反推`data`非常困难，用于检查原始数据是否被人篡改过。

计算出一个字符串的MD5值：

```python
import hashlib
md5 = hashlib.md5()
md5.update('how to use md5 in python hashlib?'.encode('utf-8'))
print(md5.hexdigest())
d26a53750bc40b38b65a520292f69306
```

如果数据量很大，可以分块多次调用`update()`，计算的结果是一样的：

```python
import hashlib
md5 = hashlib.md5()
md5.update('how to use md5 in '.encode('utf-8'))
md5.update('python hashlib?'.encode('utf-8'))
print(md5.hexdigest())
d26a53750bc40b38b65a520292f69306
```

MD5是最常见的摘要算法，速度很快，生成128bit字节，通常用32位的16进制字符串表示。另一种SHA1摘要算法：

```python
import hashlib
sha1 = hashlib.sha1()
sha1.update('how to use sha1 in '.encode('utf-8'))
sha1.update('python hashlib?'.encode('utf-8'))
print(sha1.hexdigest())
2c76b57293ce30acef38d98f6046927161b46a44
```

SHA1的结果是160 bit字节，通常用一个40位的16进制字符串表示。比SHA1更安全的算法是SHA256和SHA512，越安全越慢长度越长。

**摘要算法应用**

用户登录密码在数据库中是以MD5摘要存储的，用户登录时先计算用户输入的明文密码MD5然后和数据库MD5密码对比，如果一致说明密码正确。

**练习**

根据用户输入的口令，计算出存储在数据库中的MD5口令：

```python
import hashlib
def calc_md5(password):
    md5 = hashlib.md5()
    md5.update(password.encode('utf-8'))
    return md5.hexdigest()
```

设计一个验证用户登录的函数，根据用户输入的口令是否正确，返回True或False：

```python
db = {
    'lsaiah': 'e10adc3949ba59abbe56e057f20f883e',
    'bob': '878ef96e86145580c38c87f0410ad153',
    'alice': '99b1c2188db85afee403b1536010c2c9'
}
def login(user, password):
    md5_password = calc_md5(password)
    if db[user] == md5_password:
        return True
    else:
        return False
    
if __name__ == '__main__':
    print('主程序开始运行')
    username = input('请输入您的用户名：')
    password = input('请输入您的密码')
    print('您的账号为：'+username)
    print('您的密码口令为：'+calc_md5(password))

# 测试:
assert login('lsaiah', '123456')
assert login('bob', 'abc999')
assert login('alice', 'alice2008')
assert not login('lsaiah', '1234567')
assert not login('bob', '123456')
assert not login('alice', 'Alice2008')
print('ok')
```

对于简单口令可以得到反推表：

```python
'e10adc3949ba59abbe56e057f20f883e': '123456'
'21218cca77804d2ba1922c33e0151105': '888888'
'5f4dcc3b5aa765d61d8327deb882cf99': 'password'
```

为了加强保护确保存储口令不在彩虹表，通过对原始口令加盐实现：

```python
def calc_md5(password):
    return get_md5(password + 'the-Salt')
```

但是如果用户使用了相同的口令，通过把登录名作为Salt的一部分来计算MD5实现不同用户相同口令得到不同MD5。

**练习**

根据用户输入的登录名和口令模拟用户注册，计算更安全的MD5：

```python
db = {}

def register(username, password):
    db[username] = get_md5(password + username + 'the-Salt')
```

然后根据修改后的MD5算法实现用户登录的验证：

```python
import hashlib, random

def get_md5(s):
    return hashlib.md5(s.encode('utf-8')).hexdigest()

class User(object):
    def __init__(self, username, password):
        self.username = username
        self.salt = ''.join([chr(random.randint(48, 122)) for i in range(20)])
        self.password = get_md5(password + self.salt)
        
db = {
    'lsaiah': User('lsaiah', '123456'),
    'bob': User('bob', 'abc999'),
    'alice': User('alice', 'alice2008')
}

def login(username, password):
    user = db[username]
    if user.password == get_md5(password + user.salt)
    	return True
    else:
        return False

# 测试:
assert login('lsaiah', '123456')
assert login('bob', 'abc999')
assert login('alice', 'alice2008')
assert not login('lsaiah', '1234567')
assert not login('bob', '123456')
assert not login('alice', 'Alice2008')
print('ok')
```

### hmac

Hmac算法：Keyed-Hashing for Message Authentication通过一个标准算法，在计算哈希的过程中，把key混入计算。和自定义的加salt算法不同，Hmac算法针对所有哈希算法都通用，无论是MD5还是SHA-1。采用Hmac替代自定义的salt算法，可以使程序算法更标准化也更安全。

Python自带的hmac模块实现了标准的Hmac算法。首先需要准备待计算的原始消息message，随机key，哈希算法，采用MD5：

```python
>>> import hmac
>>> message = b'Hello, world!'
>>> key = b'secret'
>>> h = hmac.new(key, message, digestmod='MD5')
>>> # 如果消息很长，可以多次调用h.update(msg)
>>> h.hexdigest()
'fa4ee7d173f2d97ee79022d1a7355bcf'
```

传入的key和message都是`bytes`类型，`str`类型需要首先编码为`bytes`。

**练习**

将上一节的salt改为标准的hmac算法验证用户口令：

```python
import hmac, random

def hmac_md5(key, s):
    return hmac.new(key.encode('utf-8'), s.encode('utf-8'), 'MD5').hexdigest()

class User(object):
    def __init__(self, username, password):
        self.username = username
        self.key = ''.join([chr(random.randint(48, 122)) for i in range(20)])
        self.password = hmac_md5(self.key, password)

db = {
    'michael': User('michael', '123456'),
    'bob': User('bob', 'abc999'),
    'alice': User('alice', 'alice2008')
}

def login(username, password):
    user = db[username]
    if user.password == hmac_md5(user.key, password)
    	return True
    else:
        return False

# 测试:
assert login('michael', '123456')
assert login('bob', 'abc999')
assert login('alice', 'alice2008')
assert not login('michael', '1234567')
assert not login('bob', '123456')
assert not login('alice', 'Alice2008')
print('ok')
```

### itertools

Python内建模块`itertools`提供了非常有用的用于操作迭代对象的函数。`itertools`模块提供的函数返回值不是list，而是`Iterator`，只有用`for`循环迭代的时候才计算。

无限迭代器`count()`：

```python
>>> import itertools
>>> natuals = itertools.count(1)
>>> for n in natuals:
...    	print(n)
...
1
2
3
...
```

`cycle()`会把传入的一个序列无限重复：

```python
>>> import itertools
>>> cs = itertools.cycle('ABC') # 注意字符串也是序列的一种
>>> for c in cs:
...     print(c)
...
'A'
'B'
'C'
'A'
'B'
'C'
...
```

`repeat()`负责把一个元素无限重复下去，如果提供第二个参数就可以限定重复次数：

```python
>>> ns = itertools.repeat('A', 3)
>>> for n in ns:
...     print(n)
...
A
A
A
```

无限序列虽然可以无限迭代下去，但是通常通过`takewhile()`等函数根据条件判断来截取出一个有限的序列：

```python
>>> natuals = itertools.count(1)
>>> ns = itertools.takewhile(lambda x: x <= 10, natuals)
>>> list(ns)
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

`itertools`提供的几个迭代器操作函数更加有用：

**chain()**

把一组迭代对象串联起来，形成一个更大的迭代器：

```python
>>> for c in itertools.chain('ABC', 'XYZ'):
...     print(c)
# 迭代效果：'A' 'B' 'C' 'X' 'Y' 'Z'
```

**groupby()**

把迭代器中相邻的重复元素挑出来放在一起：

```python
>>> for key, group in itertools.groupby('AAABBBCCAAA'):
...     print(key, list(group))
...
A ['A', 'A', 'A']
B ['B', 'B', 'B']
C ['C', 'C']
A ['A', 'A', 'A']
```

实际上挑选规则是通过函数完成的，只要作用于函数的两个元素返回的值相等，这两个元素就被认为是在一组的，而函数返回值作为组的key。如果我们要忽略大小写分组，就可以让元素`'A'`和`'a'`都返回相同的key：

```python
>>> for key, group in itertools.groupby('AaaBBbcCAAa', lambda c: c.upper()):
...     print(key, list(group))
...
A ['A', 'a', 'a']
B ['B', 'B', 'b']
C ['c', 'C']
A ['A', 'A', 'a']
```

**练习**

利用Python提供的itertools模块，来计算圆周率这个序列的前N项和：

```python
import itertools
def pi(N):
    ' 计算pi的值 '
    # step 1: 创建一个奇数序列: 1, 3, 5, 7, 9, ...
	odd = itertools.count(start=1,step=2)
    # step 2: 取该序列的前N项: 1, 3, 5, 7, 9, ..., 2*N-1.
	odd_n = itertools.takewhile(lambda x:x<=2*N-1,odd)
    # step 3: 添加正负符号并用4除: 4/1, -4/3, 4/5, -4/7, 4/9, ...
	tmp = map(lambda x: 4 / x if x % 4 == 1 else -4 / x, odd_n)
    # step 4: 求和:
    return sum(tmp)
# 测试:
print(pi(10))
print(pi(100))
print(pi(1000))
print(pi(10000))
assert 3.04 < pi(10) < 3.05
assert 3.13 < pi(100) < 3.14
assert 3.140 < pi(1000) < 3.141
assert 3.1414 < pi(10000) < 3.1415
print('ok')
```



### contextlib

### urllib

### XML

### HTMLParser

## 常用第三方模块

## 图形界面

## 网络编程

## 电子邮件

## 访问数据库

## Web开发

## 异步IO

### 协程

线程和进程的操作是由程序触发系统接口，最后的执行者是系统，它本质上是操作系统提供的功能。而协程的操作则是程序员指定的，在python中通过yield，人为的实现并发处理。

协程存在的意义：对于多线程应用，CPU通过切片的方式来切换线程间的执行，线程切换时需要耗时。协程，则只使用一个线程，分解一个线程成为多个“微线程”，在一个线程中规定某个代码块的执行顺序。

协程的适用场景：当程序中存在大量不需要CPU的操作时（IO）。
常用第三方模块gevent和greenlet。（本质上，gevent是对greenlet的高级封装，因此一般用它就行，这是一个相当高效的模块。）

**greenlet**

greenlet就是通过switch方法在不同的任务之间进行切换：

```python
from greenlet import greenlet

def test1():
    print(12)
    gr2.switch()
    print(34)
    gr2.switch()

def test2():
    print(56)
    gr1.switch()
    print(78)

gr1 = greenlet(test1)
gr2 = greenlet(test2)
gr1.switch()
```

**gevent**

通过joinall将任务f和它的参数进行统一调度，实现单线程中的协程。代码封装层次很高：

```python
from gevent import monkey; monkey.patch_all()
import gevent
import requests

def f(url):
    print('GET: %s' % url)
    resp = requests.get(url)
    data = resp.text
    print('%d bytes received from %s.' % (len(data), url))

gevent.joinall([
        gevent.spawn(f, 'https://www.python.org/'),
        gevent.spawn(f, 'https://www.yahoo.com/'),
        gevent.spawn(f, 'https://github.com/'),
])
```

### asyncio

### async/await

### aiohttp