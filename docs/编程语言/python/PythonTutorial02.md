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

在Python中，读写文件资源要注意使用完毕后关闭它，使用`try...finally`：

```python
try:
    f = open('/path/to/file', 'r')
    f.read()
finally:
    if f:
        f.close()
```

Python的`with`语句可以在使用完毕后自动关闭资源：

```python
with open('/path/to/file', 'r') as f:
    f.read()
```

并不是只有`open()`函数返回的fp对象才能使用`with`语句，任何对象只要正确实现了上下文管理就可以使用。实现上下文管理是通过`__enter__`和`__exit__`这两个方法实现的。例如：

```python
class Query(object):
    def __init__(self, name):
        self.name = name
    def __enter__(self):
        print('Begin')
        return self    
    def __exit__(self, exc_type, exc_value, traceback):
        if exc_type:
            print('Error')
        else:
            print('End')    
    def query(self):
        print('Query info about %s...' % self.name)
```

这样就可以把自己写的资源对象用于`with`语句：

```python
with Query('Bob') as q:
    q.query()
```

**@contextmanager**

编写`__enter__`和`__exit__`仍然很繁琐，因此Python的标准库`contextlib`提供了更简单的写法，上面的代码可以改写如下：

```python
from contextlib import contextmanager

class Query(object):
    def __init__(self, name):
        self.name = name
    def query(self):
        print('Query info about %s...' % self.name)

@contextmanager
def create_query(name):
    print('Begin')
    q = Query(name)
    yield q
    print('End')
```

`@contextmanager`这个装饰器decorator接受一个生成器generator，用`yield`语句把`with ... as var`把变量输出出去，`with`语句就可以正常地工作了：

```python
with create_query('Bob') as q:
    q.query()
```

很多时候要在某段代码执行前后自动执行特定代码，也可以用`@contextmanager`实现：

```python
@contextmanager
def tag(name):
    print("<%s>" % name)
    yield
    print("</%s>" % name)

with tag("h1"):
    print("hello")
    print("world")
    
<h1>
hello
world
</h1>
```

执行顺序是：

1. `with`语句首先执行`yield`之前的语句，因此打印出`<h1>`；
2. `yield`调用会执行`with`语句内部的所有语句，因此打印出`hello`和`world`；
3. 最后执行`yield`之后的语句，打印出`</h1>`。

**@closing**

如果一个对象没有实现上下文就不能用于`with`语句，用`closing()`把该对象变为上下文对象。例如用`with`语句使用`urlopen()`：

```python
from contextlib import closing
from urllib.request import urlopen

with closing(urlopen('https://www.python.org')) as page:
    for line in page:
        print(line)
```

`closing`也是一个经过`@contextmanager`装饰的生成器generator：

```python
@contextmanager
def closing(thing):
    try:
        yield thing
    finally:
        thing.close()
```

[IBM参考文献](https://developer.ibm.com/zh/articles/os-cn-pythonwith/)

[文献2](https://www.cnblogs.com/nnnkkk/p/4309275.html)

### urllib

urllib提供了一系列用于操作url的功能。

**Get**

urllib的`request`模块可以非常方便地抓取URL内容，发送一个GET请求到指定的页面，然后返回HTTP的响应：

```python
from urllib import request

with request.urlopen('https://www.v2ex.com/api/nodes/show.json?name=python') as f:
    data = f.read()
    print('Status:', f.status, f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', data.decode('utf-8'))
```

可以看到HTTP响应头和JSON数据：

```http
Status: 200 OK
Date: Mon, 25 Jan 2021 05:14:28 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Set-Cookie: __cfduid=d65b072104474c970efb9a54ab908afc01611551668; expires=Wed, 2
4-Feb-21 05:14:28 GMT; path=/; domain=.v2ex.com; HttpOnly; SameSite=Lax; Secure
Vary: Accept-Encoding
X-Rate-Limit-Remaining: 119
Etag: W/"aacef407589604583b33be37acde2201ab191176"
X-Rate-Limit-Reset: 1611543600
Cache-Control: public, max-age=3600
X-Rate-Limit-Limit: 120
Google: XY
X-Frame-Options: sameorigin
CF-Cache-Status: HIT
Age: 2568
cf-request-id: 07d99010920000eb853e243000000001
Expect-CT: max-age=604800, report-uri="https://report-uri.cloudflare.com/cdn-cgi
/beacon/expect-ct"
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
Server: cloudflare
CF-RAY: 616f82c74c3ceb85-LAX
Data: {"avatar_large": "https://cdn.v2ex.com/navatar/8613/985e/90_large.png?m=16
10084956", "name": "python", "avatar_normal": "https://cdn.v2ex.com/navatar/8613
/985e/90_normal.png?m=1610084956", "title": "Python", "url": "https://www.v2ex.c
om/go/python", "topics": 14019, "footer": "", "header": "\u8fd9\u91cc\u8ba8\u8bb
a\u5404\u79cd Python \u8bed\u8a00\u7f16\u7a0b\u8bdd\u9898\uff0c\u4e5f\u5305\u62e
c Django\uff0cTornado \u7b49\u6846\u67b6\u7684\u8ba8\u8bba\u3002\u8fd9\u91cc\u66
2f\u4e00\u4e2a\u80fd\u591f\u5e2e\u52a9\u4f60\u89e3\u51b3\u5b9e\u9645\u95ee\u9898
\u7684\u5730\u65b9\u3002", "title_alternative": "Python", "avatar_mini": "https:
//cdn.v2ex.com/navatar/8613/985e/90_mini.png?m=1610084956", "stars": 9436, "alia
ses": [], "root": false, "id": 90, "parent_node_name": "programming"}
```

如果模拟浏览器发送GET请求，要使用`Request`对象添加HTTP头，例如模拟iphone6浏览器请求V2EX首页：

```python
from urllib import request

req = request.Request('https://www.v2ex.com/')
req.add_header('User-Agent', 'Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25')
with request.urlopen(req) as f:
    print('Status:', f.status, f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', f.read().decode('utf-8'))
```

这样会返回适合iPhone的移动版网页：

```http
Status: 200 OK
Date: Mon, 25 Jan 2021 05:37:31 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Set-Cookie: __cfduid=d1258f26a17af8c2200b24bf2e8bf20391611553051; expires=Wed, 24-Feb-21 05:37:31 GMT; path=/; domain=.v2ex.com; HttpOnly; SameSite=Lax; Secure
Vary: Accept-Encoding
Set-Cookie: PB3_SESSION="2|1:0|10:1611553051|11:PB3_SESSION|36:djJleDoyMjEuMjI0LjYyLjM0OjkzMzY3ODcw|cbff87940e8619e0a2a56a8efbfb264fde343dc66974f73a8feef8a533a4ce9c"; expires=Sat, 30 Jan 2021 05:37:31 GMT; httponly; Path=/
Set-Cookie: V2EX_LANG=zhcn; Path=/
Google: XY
X-Frame-Options: sameorigin
CF-Cache-Status: DYNAMIC
cf-request-id: 07d9a52aca0000e811c0b01000000001
Expect-CT: max-age=604800, report-uri="https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct"
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
Server: cloudflare
CF-RAY: 616fa48ad99fe811-LAX
Data: <!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta content="True" name="HandheldFriendly">
<meta name="Referrer" content="unsafe-url">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
...
<link rel="apple-touch-icon" sizes="180x180" href="/static/apple-touch-icon-180.png?v=91e795b8b5d9e2cbf2d886c3d4b7d63c">
...
</head>
<body>
...
</body>
...
</html>
```

**Post**

POST发送一个请求需要把参数`data`以bytes形式传入，模拟微博登录，先读取登录的邮箱和口令，然后按照weibo.cn的登录页的格式以`username=xxx&password=xxx`的编码传入：

```python
from urllib import request, parse

print('Login to weibo.cn...')
email = input('Email: ')
passwd = input('Password: ')
login_data = parse.urlencode([
    ('username', email),
    ('password', passwd),
    ('entry', 'mweibo'),
    ('client_id', ''),
    ('savestate', '1'),
    ('ec', ''),
    ('pagerefer', 'https://passport.weibo.cn/signin/welcome?entry=mweibo&r=http%3A%2F%2Fm.weibo.cn%2F')
])

req = request.Request('https://passport.weibo.cn/sso/login')
req.add_header('Origin', 'https://passport.weibo.cn')
req.add_header('User-Agent', 'Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25')
req.add_header('Referer', 'https://passport.weibo.cn/signin/login?entry=mweibo&res=wel&wm=3349&r=http%3A%2F%2Fm.weibo.cn%2F')

with request.urlopen(req, data=login_data.encode('utf-8')) as f:
    print('Status:', f.status, f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', f.read().decode('utf-8'))
```

登录成功响应如下：

```http
Status: 200 OK
Server: nginx/1.6.1
Date: Mon, 25 Jan 2021 06:11:52 GMT
Content-Type: text/html
Transfer-Encoding: chunked
Connection: close
Vary: Accept-Encoding
Cache-Control: no-cache, must-revalidate
Expires: Sat, 26 Jul 1997 05:00:00 GMT
Pragma: no-cache
Access-Control-Allow-Origin: https://passport.weibo.cn
Access-Control-Allow-Credentials: true
DPOOL_HEADER: dryad49
Data: Data: {"retcode":20000000,"msg":"","data":{...,"uid":"1658384301"}}
// 登录失败
Data: {"retcode":50011002,"msg":"\u7528\u6237\u540d\u6216\u5bc6\u7801\u9519\u8bef","data":{"errline":1056}}
```

**Handler**

如果还需要更复杂的控制，比如通过Proxy访问，用`ProxyHandler`来处理：

```python
proxy_handler = urllib.request.ProxyHandler({'http': 'http://www.example.com:3128/'})
proxy_auth_handler = urllib.request.ProxyBasicAuthHandler()
proxy_auth_handler.add_password('realm', 'host', 'username', 'password')
opener = urllib.request.build_opener(proxy_handler, proxy_auth_handler)
with opener.open('http://www.example.com/login.html') as f:
    pass
```

**练习**

利用urllib读取JSON，然后将JSON解析为Python对象：

```python
import json
from urllib import request
def fetch_data(url):
    req = request.Request(url)
    with request.urlopen(req) as f:
        if not f.status == 200: raise ValueError('请求失败')
        return json.loads(f.read().decode('utf-8'))
# 测试
URL = 'http://www.httpbin.org/get'
data = fetch_data(URL)
print(data)
assert data['url'] == 'http://www.httpbin.org/get'
print('ok')
```

### XML

XML虽然比JSON复杂在Web中应用也少了，但还是有很多地方用，必须了解如何操作XML。

**DOM和SAX**

DOM会把整个XML读入内存解析为树，因此占用内存大解析慢，优点是可以任意遍历树的节点。SAX是流模式边读边解析，占用内存小解析快，缺点是需要自己处理事件。

在Python中使用SAX解析XML，通常需要`start_element`，`end_element`和`char_data`这3个事件函数，当SAX解析器读取到一个节点：

```html
<a href="/">python</a>
```

1. start_element事件，在读取`<a href="/">`时；
2. char_data事件，在读取`python`时；
3. end_element事件，在读取`</a>`时。

```python
from xml.parsers.expat import ParserCreate

class DefaultSaxHandler(object):
    def start_element(self, name, attrs):
        print('sax:start_element: %s, attrs: %s' % (name, str(attrs)))

    def end_element(self, name):
        print('sax:end_element: %s' % name)

    def char_data(self, text):
        print('sax:char_data: %s' % text)

xml = r'''<?xml version="1.0"?>
<ol>
    <li><a href="/python">Python</a></li>
    <li><a href="/ruby">Ruby</a></li>
</ol>
'''

handler = DefaultSaxHandler()
parser = ParserCreate()
parser.StartElementHandler = handler.start_element
parser.EndElementHandler = handler.end_element
parser.CharacterDataHandler = handler.char_data
parser.Parse(xml)
```

读取一大段字符串时，`CharacterDataHandler`可能被多次调用，在`EndElementHandler`里面再合并。

生成XML的方法是拼接字符串：

```python
L = []
L.append(r'<?xml version="1.0"?>')
L.append(r'<root>')
L.append(encode('some & data'))
L.append(r'</root>')
return ''.join(L)
```

**练习**

请利用SAX编写程序解析XML格式的天气预报，获取苏州所有地区当天天气预报信息（地区名：｛当日天气状态：’‘，最高温度：’‘，最低温度｝）：

`http://flash.weather.com.cn/wmaps/xml/suzhou.xml`

```python
from xml.parsers.expat import ParserCreate  # 引入xml解析模块
from urllib import request  # 引入URL请求模块


class WeatherSaxHandler(object):  # 定义一个天气事件处理器
    weather = {'city': 1, 'cityname': [], 'forecast': []}  # 初始化城市city和预报信息forecast
    def start_element(self, name, attrs):  # 定义开始标签处理事件
        if name == 'suzhou':
            self.weather['city'] = '：苏州'
        if name == 'city':  # 获取location信息
            self.weather['cityname'].append(attrs['cityname'])  # 获取地区名
            # 获取forecast信息
            self.weather['forecast'].append({
                'state': attrs['stateDetailed'],
                'high': attrs['tem2'],
                'low': attrs['tem1']
            })

def parseXml(xml_str):  # 定义xml解析器
    handler = WeatherSaxHandler()
    parser = ParserCreate()
    parser.StartElementHandler = handler.start_element
    parser.Parse(xml_str)  # 解析xml文本
    print('City' + handler.weather['city'])
    for (x, y) in zip(handler.weather['cityname'], handler.weather['forecast']):  # 打印天气信息
        print('Region：' + x)
        print(y)
    return handler.weather

# 测试:
URL = 'http://flash.weather.com.cn/wmaps/xml/suzhou.xml'
with request.urlopen(URL, timeout=4) as f:
    data = f.read()
result = parseXml(data.decode('utf-8'))
```

### HTMLParser

使用`HTMLParser`解析HTML，可以把网页中的文本、图像、视频等解析出来：

```python
from html.parser import HTMLParser

class MyHTMLParser(HTMLParser):
    def handle_starttag(self, tag, attrs):
        print("Encountered a start tag:", tag)

    def handle_endtag(self, tag):
        print("Encountered an end tag :", tag)

    def handle_data(self, data):
        print("Encountered some data  :", data)

parser = MyHTMLParser()
parser.feed('<html><head><title>Test</title></head>'
            '<body><h1>Parse me!</h1></body></html>')
```

```
Encountered a start tag: html
Encountered a start tag: head
Encountered a start tag: title
Encountered some data  : Test
Encountered an end tag : title
Encountered an end tag : head
Encountered a start tag: body
Encountered a start tag: h1
Encountered some data  : Parse me!
Encountered an end tag : h1
Encountered an end tag : body
Encountered an end tag : html
```

`feed()`方法可以多次调用，特殊字符英文表示的`&nbsp;`，数字表示的`&#1234;`。

**练习**

[查看网页源码](https://www.python.org/events/python-events/)，解析HTML，输出会议时间、地点。

```python
# -*-coding:UTF-8-*-
from html.parser import HTMLParser
from urllib.request import Request,urlopen
import re

def get_data(url):
   headers = {
      'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.96 Safari/537.36'
      }
   req = Request(url, headers=headers)
   with urlopen(req, timeout=25) as f:
      data = f.read()
      print(f'Status: {f.status} {f.reason}')
      print()
   return data.decode("utf-8")

class MyHTMLParser(HTMLParser):
   def __init__(self):
      super().__init__()
      self.__parsedata='' # 设置一个空的标志位
      self.info = []

   def handle_starttag(self, tag, attrs):
      if ('class', 'event-title') in attrs:
         self.__parsedata = 'name'  # 通过属性判断如果该标签是我们要找的标签，设置标志位
      if tag == 'time':
         self.__parsedata = 'time'
      if ('class', 'say-no-more') in attrs:
         self.__parsedata = 'year'
      if ('class', 'event-location') in attrs:
         self.__parsedata = 'location'

   def handle_endtag(self, tag):
      self.__parsedata = ''# 在HTML 标签结束时，把标志位清空

   def handle_data(self, data):
      if self.__parsedata == 'name':
         # 通过标志位判断，输出打印标签内容
         self.info.append(f'会议名称:{data}')
      if self.__parsedata == 'time':
         self.info.append(f'会议时间:{data}')
      if self.__parsedata == 'year':
         if re.match(r'\s\d{4}', data): # 因为后面还有两组 say-no-more 后面的data却不是年份信息,所以用正则检测一下
            self.info.append(f'会议年份:{data.strip()}')
      if self.__parsedata == 'location':
         self.info.append(f'会议地点:{data} \n')

def main():
   parser = MyHTMLParser()
   URL = 'https://www.python.org/events/python-events/'
   data = get_data(URL)
   parser.feed(data)
   for s in parser.info:
      print(s)

if __name__ == '__main__':
   main()
```

```python
Status: 200 OK

会议名称:BelPy 2021
会议时间:30 Jan. – 31 Jan. 
会议年份:2021
会议地点:Online 

会议名称:PyCascades 2021
会议时间:19 Feb. – 21 Feb. 
会议年份:2021
会议地点:Online 

会议名称:PyCon Cameroon 2021
会议时间:18 March – 20 March 
会议年份:2021
会议地点:Douala, Cameroon 

会议名称: GeoPython 2021
会议时间:22 April – 23 April 
会议年份:2021
会议名称:PyCon US 2021
会议时间:12 May – 20 May 
会议年份:2021
会议地点:Online 

会议名称:PyCon Namibia 2021
会议时间:18 June – 19 June 
会议年份:2021
会议地点:Windhoek, Namibia 

会议名称:Python Pizza New Year's Party
会议时间:31 Dec.
会议年份:2020
会议地点:Online 

会议名称:PyCon Tanzania 2020
会议时间:14 Dec. – 15 Dec. 
会议年份:2020
会议地点:Dar es Salaam, Tanzania 
```

## 常用第三方模块

除了内置模块外还可以使用第三方模块[PyPI](https://pypi.org/)

### Pillow

图像处理标准库PIL：Python Imaging Library，仅支持Python2.7。兼容版本叫[Pillow](https://github.com/python-pillow/Pillow)支持最新Python 3.x。了解更多强大功能参考[中文文档](https://pillow-cn.readthedocs.io/zh_CN/latest/)

**安装pillow**

```python
pip install Pillow
```

**操作图像**

图像缩放：

```python
from PIL import Image
# 打开一个jpg图像文件，注意是当前路径:
im = Image.open('test.jpg')
# 获得图像尺寸:
w, h = im.size
print('Original image size: %sx%s' % (w, h))
# 缩放到50%:
im.thumbnail((w//2, h//2))
print('Resize image to: %sx%s' % (w//2, h//2))
# 把缩放后的图像用jpeg格式保存:
im.save('thumbnail.jpg', 'jpeg')
```

还可实现切片、旋转、滤镜、输出文字、调色板等功能。模糊效果也只需几行代码：

```python
from PIL import Image, ImageFilter

# 打开一个jpg图像文件，注意是当前路径:
im = Image.open('test.jpg')
# 应用模糊滤镜:
im2 = im.filter(ImageFilter.BLUR)
im2.save('blur.jpg', 'jpeg')
```

PIL的`ImageDraw`提供了一系列绘图方法可以直接绘图。比如生成字母验证码图片：

```python
from PIL import Image, ImageDraw, ImageFont, ImageFilter
import random

# 随机字母:
def rndChar():
    return chr(random.randint(65, 90))

# 随机颜色1:
def rndColor():
    return (random.randint(64, 255), random.randint(64, 255), random.randint(64, 255))

# 随机颜色2:
def rndColor2():
    return (random.randint(32, 127), random.randint(32, 127), random.randint(32, 127))

# 240 x 60:
width = 60 * 4
height = 60
image = Image.new('RGB', (width, height), (255, 255, 255))
# 创建Font对象:
font = ImageFont.truetype('Arial.ttf', 36)
# 创建Draw对象:
draw = ImageDraw.Draw(image)
# 填充每个像素:
for x in range(width):
    for y in range(height):
        draw.point((x, y), fill=rndColor())
# 输出文字:
for t in range(4):
    draw.text((60 * t + 10, 10), rndChar(), font=font, fill=rndColor2())
# 模糊:
image = image.filter(ImageFilter.BLUR)
image.save('code.jpg', 'jpeg')
```

### requests

相比urllib模块，requests模块更简单更实用。

**安装requests**

```python
pip install requests
```

**Get**

通过Get访问一个页面：

```python
>>> import requests
>>> r = requests.get('https://www.douban.com/') # 豆瓣首页
>>> r.status_code
200
>>> r.text
<!DOCTYPE html>
...
</html>
```

对于带参数的URL，传入一个dict作为`params`参数：

```python
>>> r = requests.get('https://www.douban.com/search', params={'q': 'python', 'cat': '1001'})
>>> r.url # 实际请求的URL
'https://www.douban.com/search?q=python&cat=1001'
```

requests自动检测编码，可以使用`encoding`属性查看：

```python
>>> r.encoding
UTF-8
```

无论响应是文本还是二进制内容都可以用`content`属性获得`bytes`对象：

```python
>>> r.content
b'<!DOCTYPE html>\n<html lang="zh-CN">\n...
```

对于特定类型的响应例如JSON可直接获取：

```python
>>> r = requests.get('https://www.v2ex.com/api/topics/hot.json')
>>> r.json()
[{'node': {'avatar_large': 'https://cdn.v2ex.com/', 'name': 'beijing',...}]
```

需要传入HTTP Header时，传入一个dict作为`headers`参数：

```python
>>> r = requests.get('https://www.douban.com/', headers={'User-Agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit'})
>>> r.text
'\n\n<!DOCTYPE html>\n<html itemscope itemtype="http://schema.org/WebPage" class
="ua-mobile ">\n  <head>\n      <meta charset="UTF-8">\n      <title>豆瓣(手机版
)</title>\n...
```

**Post**

传入`data`参数作为POST请求的数据：

```python
>>> r = requests.post('https://accounts.douban.com/login', data={'form_email': 'abc@example.com', 'form_password': '123456'})
```

requests默认使用`application/x-www-form-urlencoded`对POST数据编码。可直接传入JSON数据：

```python
params = {'key': 'value'}
r = requests.post(url, json=params) # 内部自动序列化为JSON
```

上传文件需要更复杂的编码格式，requests把它简化成`files`参数，使用`'rb'`即二进制模式读取，获取的`bytes`长度才是文件的长度：

```python
>>> upload_files = {'file': open('report.xls', 'rb')}
>>> r = requests.post(url, files=upload_files)
```

把`post()`方法替换为`put()`，`delete()`等，就可以以PUT或DELETE方式请求资源。

除了能轻松获取响应内容外，requests对获取HTTP响应的其他信息也非常简单。例如获取响应头：

```python
>>> r.headers
{Content-Type': 'text/html; charset=utf-8', 'Transfer-Encoding': 'chunked', 'Content-Encoding': 'gzip', ...}
>>> r.headers['Content-Type']
'text/html; charset=utf-8'
```

requests对Cookie做了特殊处理，使得我们不必解析Cookie就可以轻松获取指定的Cookie：

```python
>>> r.cookies['ts']
'example_cookie_12345'
```

要在请求中传入Cookie，只需准备一个dict传入`cookies`参数：

```python
>>> cs = {'token': '12345', 'status': 'working'}
>>> r = requests.get(url, cookies=cs)
```

要指定超时，传入以秒为单位的timeout参数：

```python
>>> r = requests.get(url, timeout=5) # 5秒后超时
```

### chardet

Python提供了Unicode表示的`str`和`bytes`两种数据类型，可以通过`encode()`和`decode()`方法转换，但是在不知道编码的情况下对`bytes`做`decode()`不好做。对于未知编码的`bytes`，要把它转换成`str`，需要先猜测编码。chardet第三方库用它来检测编码。

**安装**

```python
pip install chardet
```

**使用**

当我们拿到一个`bytes`时，就可以用chardet检测编码。检测出的编码是`ascii`，`confidence`字段表示检测的概率是1.0：

```python
>>> chardet.detect(b'Hello, world!')
{'encoding': 'ascii', 'confidence': 1.0, 'language': ''}
```

检测GBK编码的中文，检测到编码是`GB2312`（GBK是GB2312的超集，两者是同一种编码）检测正确的概率是74%，`language`字段指出的语言是`'Chinese'`：

```python
>>> data = '离离原上草，一岁一枯荣'.encode('gbk')
>>> chardet.detect(data)
{'encoding': 'GB2312', 'confidence': 0.7407407407407407, 'language': 'Chinese'}
```

检测UTF-8编码：

```python
>>> data = '离离原上草，一岁一枯荣'.encode('utf-8')
>>> chardet.detect(data)
{'encoding': 'utf-8', 'confidence': 0.99, 'language': ''}
```

日文编码检测：

```python
>>> data = '最新の主要ニュース'.encode('euc-jp')
>>> chardet.detect(data)
{'encoding': 'EUC-JP', 'confidence': 0.99, 'language': 'Japanese'}
```

用chardet获取到编码后[支持编码列表](https://chardet.readthedocs.io/en/latest/supported-encodings.html)，再转换为`str`，就可以方便后续处理。

### psutil

用Python来编写脚本简化日常的运维工作是Python的一个重要用途。在Linux下有许多系统命令可以让我们时刻监控系统运行的状态，如`ps`，`top`，`free`等等。要获取这些系统信息，Python可以通过`subprocess`模块调用并获取结果。但这样做很麻烦，尤其是要写很多解析代码。

在Python中获取系统信息的另一个好办法是使用`psutil`这个第三方模块。psutil = process and system utilities，不仅可以通过一两行代码实现系统监控，还可以跨平台使用。

**安装psutil**

```python
pip install psutil
```

**获取CPU信息**

```python
>>> import psutil
>>> psutil.cpu_count() # CPU逻辑数量
4
>>> psutil.cpu_count(logical=False) # CPU物理核心
2
# 2说明是双核超线程, 4则是4核非超线程
```

统计CPU的用户／系统／空闲时间：

```python
>>> psutil.cpu_times()
scputimes(user=3440.2120525, system=1163.9234609999985, idle=39970.311018299995, interrupt=264.5620959, dpc=91.9313893)
```

实现类似`top`命令的CPU使用率，每秒刷新一次累计10次：

```python
>>> for x in range(10):
...     print(psutil.cpu_percent(interval=1, percpu=True))
[4.7, 3.1, 6.3, 0.0]
[18.7, 20.0, 17.2, 0.0]
[9.4, 6.1, 6.3, 0.0]
[1.6, 9.0, 6.3, 0.0]
[16.9, 7.6, 15.4, 0.0]
```

**获取内存信息**

使用psutil获取物理内存和交换内存信息，返回字节为单位的整数：

```python
>>> psutil.virtual_memory()
svmem(total=12808732672, available=5786382336, percent=54.8, used=7022350336, free=5786382336)
>>> psutil.swap_memory()
sswap(total=25615515648, used=8623194112, free=16992321536, percent=33.7, sin=0, sout=0)
```

**获取磁盘信息**

可以通过psutil获取磁盘分区、磁盘使用率和磁盘IO信息。`opts`中包含`rw`表示可读写，`journaled`表示支持日志：

```python
>>> psutil.disk_partitions() # 磁盘分区信息
[sdiskpart(device='C:\\', mountpoint='C:\\', fstype='NTFS', opts='rw,fixed', maxfile=255, maxpath=260), sdiskpart(device='D:\\', mountpoint='D:\\', fstype='NTFS', opts='rw,fixed', maxfile=255, maxpath=260)]
>>> psutil.disk_usage('/') # 磁盘使用情况
sdiskusage(total=322122547200, used=124638212096, free=197484335104, percent=38.7)
>>> psutil.disk_io_counters() # 磁盘IO
sdiskio(read_count=124293, write_count=172095, read_bytes=4392716800, write_bytes=4837870080, read_time=3279, write_time=325)
```

**获取网络信息**

psutil可以获取网络接口和网络连接信息：

```python
>>> psutil.net_io_counters() # 获取网络读写字节／包的个数
snetio(bytes_sent=30396275, bytes_recv=667421107, packets_sent=312184, packets_recv=578178, errin=0, errout=0, dropin=0, dropout=0)
>>> psutil.net_if_addrs() # 获取网络接口信息
{
  'lo0': [snic(family=<AddressFamily.AF_INET: 2>, address='127.0.0.1', netmask='255.0.0.0'), ...],
  'en1': [snic(family=<AddressFamily.AF_INET: 2>, address='10.0.1.80', netmask='255.255.255.0'), ...],
  'en0': [...],
  'en2': [...],
  'bridge0': [...]
}
>>> psutil.net_if_stats() # 获取网络接口状态
{
  'lo0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=16384),
  'en0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1500),
  'en1': snicstats(...),
  'en2': snicstats(...),
  'bridge0': snicstats(...)
}
```

使用`net_connections()`获取当前网络连接信息：

```python
>>> psutil.net_connections()
Traceback (most recent call last):
  ...
PermissionError: [Errno 1] Operation not permitted

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  ...
psutil.AccessDenied: psutil.AccessDenied (pid=3847)
```

可能会得到一个`AccessDenied`错误，原因是psutil获取信息也是要走系统接口，而获取网络连接信息需要root权限，这种情况下可以退出Python交互环境，用`sudo`重新启动：

```python
[root@VM-0-16-centos ~]# sudo python3
Python 3.6.8 (default, Apr 16 2020, 01:36:27) 
[GCC 8.3.1 20191121 (Red Hat 8.3.1-5)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import psutil
>>> psutil.net_connections()
[
    sconn(fd=83, family=<AddressFamily.AF_INET6: 30>, type=1, laddr=addr(ip='::127.0.0.1', port=62911), raddr=addr(ip='::127.0.0.1', port=3306), status='ESTABLISHED', pid=3725),
    sconn(fd=84, family=<AddressFamily.AF_INET6: 30>, type=1, laddr=addr(ip='::127.0.0.1', port=62905), raddr=addr(ip='::127.0.0.1', port=3306), status='ESTABLISHED', pid=3725),
    sconn(fd=93, family=<AddressFamily.AF_INET6: 30>, type=1, laddr=addr(ip='::', port=8080), raddr=(), status='LISTEN', pid=3725),
    sconn(fd=103, family=<AddressFamily.AF_INET6: 30>, type=1, laddr=addr(ip='::127.0.0.1', port=62918), raddr=addr(ip='::127.0.0.1', port=3306), status='ESTABLISHED', pid=3725),
    sconn(fd=105, family=<AddressFamily.AF_INET6: 30>, type=1, ..., pid=3725),
    sconn(fd=106, family=<AddressFamily.AF_INET6: 30>, type=1, ..., pid=3725),
    sconn(fd=107, family=<AddressFamily.AF_INET6: 30>, type=1, ..., pid=3725),
    ...
    sconn(fd=27, family=<AddressFamily.AF_INET: 2>, type=2, ..., pid=1)
]
```

```python
import psutil

def show_process():
    '''列出所有当前正在运行的进程pid-name信息'''
    for proc in psutil.process_iter():
        try:
            pinfo = proc.as_dict(attrs=['pid', 'name'])
        except psutil.NoSuchProcess:
            pass
        else:
            print(pinfo)

if __name__ == '__main__':
    show_process()
```

**获取进程信息**

通过psutil可以获取到所有进程的详细信息，和获取网络信息一样需要root权限：

```python
>>> psutil.pids() # 所有进程ID
[3865, 3864, 3863, 3856, 3855, 3853, 3776, ..., 45, 44, 1, 0]
>>> p = psutil.Process(3776) # 获取指定进程ID=3776，其实就是当前Python交互环境
>>> p.name() # 进程名称
'python3.8'
>>> p.exe() # 进程exe路径
'/Users/michael/lsaiah/bin/python3.8'
>>> p.cwd() # 进程工作目录
'/Users/lsaiah'
>>> p.cmdline() # 进程启动的命令行
['python3']
>>> p.ppid() # 父进程ID
3765
>>> p.parent() # 父进程
<psutil.Process(pid=3765, name='bash') at 4503144040>
>>> p.children() # 子进程列表
[]
>>> p.status() # 进程状态
'running'
>>> p.username() # 进程用户名
'lsaiah'
>>> p.create_time() # 进程创建时间
1511052731.120333
>>> p.terminal() # 进程终端
'/dev/ttys002'
>>> p.cpu_times() # 进程使用的CPU时间
pcputimes(user=0.081150144, system=0.053269812, children_user=0.0, children_system=0.0)
>>> p.memory_info() # 进程使用的内存
pmem(rss=8310784, vms=2481725440, pfaults=3207, pageins=18)
>>> p.open_files() # 进程打开的文件
[]
>>> p.connections() # 进程相关网络连接
[]
>>> p.num_threads() # 进程的线程数量
1
>>> p.threads() # 所有线程信息
[pthread(id=1, user_time=0.090318, system_time=0.062736)]
>>> p.environ() # 进程环境变量
{'SHELL': '/bin/bash', 'PATH': '/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:...', 'PWD': '/Users/michael', 'LANG': 'zh_CN.UTF-8', ...}
>>> p.terminate() # 结束进程
Terminated: 15 <-- 自己把自己结束了
```

psutil还提供了一个`test()`函数，可以模拟出`ps`命令的效果：

```python
[root@VM-0-16-centos ~]# sudo python3
Python 3.6.8 (default, Apr 16 2020, 01:36:27) 
[GCC 8.3.1 20191121 (Red Hat 8.3.1-5)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import psutil
>>> psutil.test()
USER         PID  %MEM     VSZ     RSS  NICE STATUS  START   TIME  CMDLINE
SYSTEM         0   0.0    0.0B   24.0K        runni         26:33  System Idle P
SYSTEM         4   0.0  124.0K  368.0K    32  runni  08:39  04:53  System
SYSTEM       312   0.0  556.0K    1.3M    32  runni  08:39  00:00  \SystemRoot\S
```

psutil还可以获取用户信息、Windows服务等很多有用的系统信息。[参考psutil的官网](https://github.com/giampaolo/psutil)

## virtualenv

系统安装的Python3只有一个版本，所有第三方的包都会被`pip`安装到Python3的`site-packages`目录下。在多个应用开发中相同的包可能需要不同版本，virtualenv就可以用来为一个应用创建一套隔离的Python运行环境。

首先安装`virtualenv`：

```python
pip install virtualenv
```

然后假设要开发一个新项目，需要一套独立的python运行环境。第一步先创建目录：

```shell
mkdir myproject
cd myproject/
```

第二步创建一个独立的Python运行环境，命名为`venv`：

```python
virtualenv --no-site-packages venv
Using base prefix '/usr/local/.../Python.framework/Versions/3.4'
created virtual environment CPython3.8.1.final.0-64 in 1577ms
  creator CPython3Windows(dest=C:\Users\Administrator.PC-20190504PBOJ\PycharmProjects\myproject\venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=C:\Users\Administrator.PC-20190504PBOJ\AppData\Local\
pypa\virtualenv)
    added seed packages: pip==20.3.3, setuptools==51.3.3, wheel==0.36.2
  activators BashActivator,BatchActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator
```

版本大于20默认参数`--no-site-packages`表示新的环境不复制第三方包。新的环境在当前目录下的`venv`目录，用`source`进入该环境，windows则直接运行Script文件夹下的activate.bat：

```python
source venv/bin/activate
(venv)Mac:myproject michael$
```

下面正常安装各种第三方包，并运行`python`命令：

```python
(venv)Mac:myproject michael$ pip install jinja2
...
Successfully installed jinja2-2.7.3 markupsafe-0.23
(venv)Mac:myproject michael$ python myapp.py
...
```

在`venv`环境下，用`pip`安装的包都被安装到`venv`这个环境下，系统Python环境不受任何影响。`venv`环境是专门针对`myproject`这个应用创建的。退出当前的`venv`环境，使用`deactivate`命令，windows同理：

```
(venv)Mac:myproject michael$ deactivate 
Mac:myproject michael$ 
```

此时就回到了正常的环境，现在`pip`或`python`均是在系统Python环境下执行。

推荐使用：[pipenv](https://pypi.org/project/pipenv/)，pip + virtualenv 结合体，解决了virtualenvwrapper弊端

会自动帮你创建虚拟环境，以及安装三方库

会自动的记录你的项目依赖的所有三方库

使用 pipfile 和 pipfile.lock取代了requirements.txt

## 图形界面

Python支持多种图形界面第三方库包含：Tk、wxWidgets、Qt、GTK等等，自带的库是支持Tk的Tkinter。

**Tkinter**

编写的Python代码会调用内置的Tkinter，Tkinter封装了访问Tk的接口；

Tk是一个图形库，支持多个操作系统，使用Tcl语言开发；

Tk会调用操作系统提供的本地GUI接口，完成最终的GUI。

**GUI程序**

```python
import tkinter as tk
# 从Frame派生一个Application类，这是所有Widget的父容器:
class Application(tk.Frame):
    # Application构造函数，master为窗口的父控件
    def __init__(self, master=None):
        # 初始化Application的Frame部分
        # tk.Frame.__init__(self, master)
        super().__init__(master)
        self.master = master
        # 显示窗口，并使用pack布局
        self.pack()
        # 创建控件
        self.create_widgets()

    def create_widgets(self):
        # 创建一个按钮
        self.hi_python = tk.Button(self)
        self.hi_python["text"] = "Hello World\n(click me)"
        # 绑定按钮事件
        self.hi_python["command"] = self.say_hi
        # 按钮布局
        self.hi_python.pack(side="top")

        self.quit = tk.Button(self, text="QUIT", fg="red", command=self.master.destroy)
        self.quit.pack(side="bottom")

    def say_hi(self):
        print("hi, python!")
```

在GUI中，每个Button、Label、输入框等，都是一个Widget。Frame则是可以容纳其他Widget的Widget，所有的Widget组合起来就是一棵树。`pack()`方法把Widget加入到父容器中，并实现布局。`pack()`是最简单的布局，`grid()`可以实现更复杂的布局。

在`createWidgets()`方法中，创建一个`Label`和一个`Button`，当Button被点击时触发`self.quit()`使程序退出。

实例化`Application`，并启动消息循环：

```python
def main():
    root = tk.Tk()
    app = Application(master=root)
    app.master.title('Hello World')    # 设置窗口标题
    app.mainloop()    # 主消息循环

if __name__ == '__main__':
    main()
```

GUI程序的主线程负责监听来自操作系统的消息并依次处理每一条消息。如果消息处理非常耗时，就需要在新线程中处理。

**输入文本**

对上面的GUI程序改进下，加入一个文本框，输入文本点击按钮弹出消息对话框。当用户点击按钮时触发`hello()`，通过`self.nameInput.get()`获得用户输入的文本后，使用`tkMessageBox.showinfo()`可以弹出消息对话框。：

```python
import tkinter as tk
# 从Frame派生一个Application类，这是所有Widget的父容器:
class Application(tk.Frame):
    # Application构造函数，master为窗口的父控件
    def __init__(self, master=None):
        # 初始化Application的Frame部分
        # tk.Frame.__init__(self, master)
        super().__init__(master)
        self.master = master
        # 显示窗口，并使用pack布局
        self.pack()
        # 创建控件
        self.create_widgets()

    def create_widgets(self):
        # 创建一个按钮
        self.hi_python = tk.Button(self)
        self.hi_python["text"] = "Hello World\n(click me)"
        # 绑定按钮事件
        self.hi_python["command"] = self.say_hi
        # 按钮布局
        self.hi_python.pack(side="top")

        self.quit = tk.Button(self, text="QUIT", fg="red", command=self.master.destroy)
        self.quit.pack(side="bottom")

    def say_hi(self):
        print("hi, python!")

def main():
    root = tk.Tk()
    app = Application(master=root)
    app.master.title('Hello World')    # 设置窗口标题
    app.mainloop()    # 主消息循环

if __name__ == '__main__':
    main()
```

Python内置的Tkinter可以满足基本的GUI程序，如果是非常复杂的GUI程序建议用操作系统原生支持的语言和库来编写。

### 海龟绘图

Python内置了turtle库基本上100%复制了原始的海龟绘图（Turtle Graphics）所有功能。

指挥海龟绘制一个长方形：

```python
# 导入turtle包的所有内容:
from turtle import *

# 设置笔刷宽度:
width(4)

# 前进:
forward(200)
# 右转90度:
right(90)

# 笔刷颜色:
pencolor('red')
forward(100)
right(90)

pencolor('green')
forward(200)
right(90)

pencolor('blue')
forward(100)
right(90)

# 调用done()使得窗口等待被关闭，否则将立刻关闭窗口:
done()
```

可设置笔刷宽度，前进，转向，移动，设置轨迹颜色，绘制完成调用`done()`函数，让窗口进入消息循环等待被关闭，否则会立刻关闭。更多操作请参考[turtle库](https://docs.python.org/3.3/library/turtle.html#turtle-methods)的说明。

`turtle`包本身只是一个绘图库，但是配合Python代码就可以绘制各种复杂的图形。例如通过循环绘制5个五角星：

```python
from turtle import *

def drawStar(x, y):
    pu()
    goto(x, y)
    pd()
    # set heading: 0
    seth(0)
    for i in range(5):
        fd(40)
        rt(144)

for x in range(0, 250, 50):
    drawStar(x, 0)

done()
```

使用递归可以绘制出非常复杂的图形。例如绘制一棵分型树：

```python
from turtle import *

# 设置色彩模式是RGB:
colormode(255)

lt(90)

lv = 14
l = 120
s = 45

width(lv)

# 初始化RGB颜色:
r = 0
g = 0
b = 0
pencolor(r, g, b)

penup()
bk(l)
pendown()
fd(l)

def draw_tree(l, level):
    global r, g, b
    # save the current pen width
    w = width()

    # narrow the pen width
    width(w * 3.0 / 4.0)
    # set color:
    r = r + 1
    g = g + 2
    b = b + 3
    pencolor(r % 200, g % 200, b % 200)

    l = 3.0 / 4.0 * l

    lt(s)
    fd(l)

    if level < lv:
        draw_tree(l, level + 1)
    bk(l)
    rt(2 * s)
    fd(l)

    if level < lv:
        draw_tree(l, level + 1)
    bk(l)
    lt(s)

    # restore the previous pen width
    width(w)

speed("fastest")

draw_tree(l, 4)

done()
```

## 网络编程

### TCP/IP

[参考网络协议](https://notes.lsaiah.cn/#/%E6%8E%A5%E5%8F%A3%E6%B5%8B%E8%AF%95/%E7%BD%91%E7%BB%9C%E5%8D%8F%E8%AE%AE)

**IP协议**

IP协议在第三层网络层或IP层，该层包括功能强大的 Internet 协议 (Internet Protocol, IP)、地址解析协议 (Address Resolution Protocol, ARP) 和 Internet 控制消息协议 (Internet Control Message Protocol, ICMP)。

IP 协议及其关联的路由协议可能是整个 TCP/IP 套件中最重要的部分。IP 负责以下操作：

- **IP 寻址**－IP 寻址约定是 IP 协议的一部分。[设计 IPv4 寻址方案](https://docs.oracle.com/cd/E19253-01/819-7058/ipplan-5/index.html)介绍了 IPv4 寻址，[IPv6 寻址概述](https://docs.oracle.com/cd/E19253-01/819-7058/ipv6-overview-10/index.html)介绍了 IPv6 寻址。
- **主机到主机通信**－根据接收系统的 IP 地址，IP 确定包必须采用的路径。
- **包格式设置**－IP 将包组装到称为**数据报**的单元中。[Internet 层：准备传送包的位置](https://docs.oracle.com/cd/E19253-01/819-7058/ipov-38/index.html)中全面介绍了数据报。
- **分段**－如果包太大而无法通过网络介质进行传输，则发送系统上的 IP 会将包分为较小的段。然后，接收系统上的 IP 会将这些段重构为原始包。

Oracle Solaris ： 同时支持 IPv4 和 IPv6 寻址格式，这两种格式都在本书中进行了介绍。为避免对 Internet 协议进行寻址时出现混淆，请使用以下约定之一：

- 如果说明中使用了术语 "IP"，则此说明既适用于 IPv4 又适用于 IPv6。
- 如果说明中使用了术语 "IPv4"，则此说明仅适用于 IPv4。
- 如果说明中使用了术语 "IPv6"，则此说明仅适用于 IPv6。

**TCP协议**

TCP协议在第四层传输层，通过交换数据接收的确认信息并重新传送丢失的包，可确保包按顺序到达且不会出现错误。这种通信类型称为端对端。此级别的传输层协议栈包括传输控制协议 (Transmission Control Protocol, TCP)、用户数据报协议 (User Datagram Protocol, UDP) 以及流控制传输协议 (Stream Control Transmission Protocol, SCTP)。TCP 和 SCTP 可提供可靠的端对端服务。UDP 则会提供不可靠的数据报服务。

使用 TCP，应用程序可以像通过实际线路连接一样进行相互通信。TCP 发送数据的形式类似于逐个字符进行传送，而不是以独立的包进行发送。这种传输由以下各项组成：

- 起始点，用于打开连接
- 按字节顺序进行的完整传输
- 结束点，用于关闭连接。

TCP 会向传送的数据中附加一个头。此头包含许多参数，可帮助发送系统上的进程连接到接收系统上的对等进程。

TCP 通过在发送主机和接收主机之间建立端对端连接，确认包是否已到达其目的地。因此，TCP 被视为一种“可靠的、面向连接的”协议。

### TCP

**客户端**

创建一个基于TCP连接的Socket：

```python
import socket
# import ssl
# 创建一个socket:
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# s = ssl.wrap_socket(socket.socket())
# 建立连接:
s.connect(('www.sina.com.cn', 80))
# s.connect(('www.sina.com.cn', 443))
```

`socket.AF_INET`指定使用IPv4协议，指定`AF_INET6`使用IPv6，`SOCK_STREAM`指定使用面向流的TCP协议。`connect`参数是一个`tuple`传入域名自动转换到IP地址，传入80端口表示Web服务。其它服务如SMTP服务是`25`端口，FTP服务是`21`端口，端口号小于1024的是Internet标准服务的端口，端口号大于1024的可以任意使用。建立连接后向服务器发送请求，要求返回首页内容：

```python
s.send(b'GET / HTTP/1.1\r\nHost: www.sina.com.cn\r\nConnection: close\r\n\r\n')
```

TCP连接创建的是双向通道，双方都可以同时给对方发数据。HTTP协议规定客户端必须先发送请求给服务器，服务器收到后返回数据给客户端。

```python
# 接收数据:
buffer = []
while True:
    # 每次最多接收1k字节:
    d = s.recv(1024)
    if d:
        buffer.append(d)
    else:
        break
data = b''.join(buffer)
# 关闭连接:
s.close()
```

调用`recv(max)`方法，一次最多接收指定的字节数，在一个while循环中反复接收，直到`recv()`返回空数据表示接收完毕退出循环。接收完数据后调用`close()`方法关闭Socket。

接收到的数据包括HTTP头和网页本身，把HTTP头和网页分离，打印头，网页内容保存到文件：

```python
header, html = data.split(b'\r\n\r\n', 1)
# 打印头
print(header.decode('utf-8'))
# 把接收的数据写入文件:
with open('sina.html', 'wb') as f:
    f.write(html)
```

**服务器**

服务器会打开固定端口监听，每来一个客户端连接就创建该Socket。由于服务器会有大量来自客户端的连接，通过服务器地址、服务器端口、客户端地址、客户端端口来唯一确定一个Socket区分是哪个客户端绑定的。但是服务器还需要同时响应多个客户端请求，所以每个连接都需要一个新的进程或者新的线程来处理，否则服务器就只能服务一个客户端了。

写一个简单的服务器程序，接收客户端连接，把客户端发来的字符串加上Hello返回：

```python
import socket, threading, time

# 服务器首先发一条欢迎消息，等待客户端数据，然后加上Hello再发送给客户端。如果客户端发送了exit字符串关闭连接。
def tcplink(sock, addr):
    print('Accept new connection from %s:%s...' % addr)
    sock.send(b'Welcome!')
    while True:
        data = sock.recv(1024)
        time.sleep(1)
        if not data or data.decode('utf-8') == 'exit':
            break
        sock.send(('Hello, %s!' % data.decode('utf-8')).encode('utf-8'))
    sock.close()
    print('Connection from %s:%s closed.' % addr)

# 创建一个基于IPv4和TCP协议的Socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 绑定监听的地址和端口，可以绑到某一块网卡的IP地址上，也可以用0.0.0.0绑定所有网络地址，绑定127.0.0.1表示本机地址。指定端口号小于1024需要管理员权限，这里使用9999端口
s.bind(('127.0.0.1', 9999))
# 监听端口，传入的参数指定等待连接的最大数量
s.listen(5)
print('Waiting for connection...')
# 服务器程序永久循环接受来自客户端的连接,accept()会等待并返回一个客户端的连接
while True:
    # 接受一个新连接:
    sock, addr = s.accept()
    # 创建新线程来处理TCP连接:
    t = threading.Thread(target=tcplink, args=(sock, addr))
    t.start()
# 每个连接都必须创建新线程（或进程）来处理，否则单线程在处理连接的过程中无法接受其他客户端的连接
```

客户端测试：

```python
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 建立连接:
s.connect(('127.0.0.1', 9999))
# 接收欢迎消息:
print(s.recv(1024).decode('utf-8'))
for data in [b'lily',b'tom',b'jim']:
    # 发送数据:
    s.send(data)
    print(s.recv(1024).decode('utf-8'))
s.send(b'exit')
s.close()
```

运行结果：

```python
# 服务器
Waiting for connection...
Accept new connection from 127.0.0.1:55946...
Connection from 127.0.0.1:55946 closed.
# 客户端
Welcome!
Hello, lily!
Hello, tom!
Hello, jim!
```

### UDP

UDP 可提供数据报传送服务并且不会检验接收主机和发送主机之间的连接。由于 UDP 不需要建立和验证连接，因此发送少量数据的应用程序可使用 UDP。使用UDP协议时知道对方的IP地址和端口号就可以直接发数据包，但不知道是否到达。

UDP服务器需要绑定端口，`SOCK_DGRAM`指定了这个Socket的类型是UDP：

```python
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
# 绑定端口:
s.bind(('127.0.0.1', 9999))
```

绑定端口和TCP一样但是不需要调用`listen()`方法，而是直接接收来自任何客户端的数据：

```python
print('Bind UDP on 9999...')
while True:
    # 接收数据:
    data, addr = s.recvfrom(1024)
    print('Received from %s:%s.' % addr)
    s.sendto(b'Hello, %s!' % data, addr)
```

`recvfrom()`方法返回数据和客户端的地址与端口，这样服务器收到数据后直接调用`sendto()`就可以把数据用UDP发给客户端。因为例子简单省略了多线程。

客户端使用UDP先创建基于UDP的Socket，然后不需要调用`connect()`，直接通过`sendto()`给服务器发数据：

```python
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
for data in [b'lily',b'tom',b'jim']:
    # 发送数据:
    s.sendto(data, ('127.0.0.1', 9999))
    # 接收数据:
    print(s.recv(1024).decode('utf-8'))
s.close()
```

运行结果：

```python
# 服务器
Bind UDP on 9999...
Received from 127.0.0.1:49888.
Received from 127.0.0.1:49888.
Received from 127.0.0.1:49888.
# 客户端
Hello, lily!
Hello, tom!
Hello, jim!
```

UDP不需要建立连接，绑定UDP端口和TCP端口互不冲突。

## 电子邮件

MUA：Mail User Agent邮件用户代理

MTA：Mail Transfer Agent邮件传输代理

MDA：Mail Delivery Agent邮件投递代理

1. 发邮件：MUA（SMTP） -> MTA（SMTP） -> MTA（SMTP） -> 若干个MTA（SMTP） -> MDA （POP3或IMAP4）
2. 收邮件：MDA（POP3或IMAP4） <- MUA（POP3或IMAP4）

### SMTP发送邮件

Python对SMTP支持有`smtplib`和`email`两个模块，`email`负责构造邮件，`smtplib`负责发送邮件。

构造一个纯文本邮件，`MIMEText`对象第一个参数就是邮件正文，第二个参数是MIME的subtype，传入`'plain'`表示纯文本，最终的MIME就是`'text/plain'`，最后用`utf-8`编码保证多语言兼容性：

```python
from email.mime.text import MIMEText
msg = MIMEText('hello, send by Python...', 'plain', 'utf-8')
```

通过SMTP发出去：

```python
import smtplib
# 输入Email地址和口令:
from_addr = input('From: ')
password = input('Password: ')
# 输入收件人地址:
to_addr = input('To: ')
# 输入SMTP服务器地址:
smtp_server = input('SMTP server: ')

server = smtplib.SMTP(smtp_server, 25) # SMTP协议默认端口是25
# 打印出和SMTP服务器交互的所有信息
server.set_debuglevel(1)
# 登录SMTP服务器
server.login(from_addr, password)
# 发邮件，传入一个list可发多人，msg.as_string()把MIMEText对象变成str
server.sendmail(from_addr, [to_addr], msg.as_string())
server.quit()
```

```python
# 报错退信，554 DT:SPM说明发送的邮件内容包含了未被许可的信息，或被系统识别为垃圾邮件。请检查是否有用户发送病毒或者垃圾邮件；
smtplib.SMTPDataError:
554 b'DT:SPM 126 smtp7
# 邮件没有主题，收件人的名字没有显示为友好的名字，收到了邮件却提示不在收件人中
```

因为邮件主题、如何显示发件人、收件人等信息并不是通过SMTP协议发给MTA，而是包含在发给MTA的文本中。所以把`From`、`To`和`Subject`添加到`MIMEText`中发送成功，接收成功：

```python
from email import encoders
from email.header import Header
from email.mime.text import MIMEText
from email.utils import parseaddr, formataddr

import smtplib
# 格式化邮件地址，如果包含中文通过Header编码    
def _format_addr(s):
    name, addr = parseaddr(s)
    return formataddr((Header(name, 'utf-8').encode(), addr))

from_addr = input('From: ')
password = input('Password: ')
to_addr = input('To: ')
smtp_server = input('SMTP server: ')

msg = MIMEText('hello, send by Python...', 'plain', 'utf-8')
msg['From'] = _format_addr('Python爱好者 <%s>' % from_addr)
# 接收字符串而不是list，多个地址用`,`分隔。
msg['To'] = _format_addr('管理员 <%s>' % to_addr)
msg['Subject'] = Header('来自SMTP的问候……', 'utf-8').encode()
    
server = smtplib.SMTP(smtp_server, 25)
server.set_debuglevel(1)
server.login(from_addr, password)
server.sendmail(from_addr, [to_addr], msg.as_string())
server.quit()
```

**发送HTML邮件**

构造`MIMEText`对象时，把HTML字符串传进去，再把第二个参数由`plain`变为`html`就可以了：

```python
msg = MIMEText('<html><body><h1>Hello</h1>' +
    '<p>send by <a href="http://www.python.org">Python</a>...</p>' +
    '</body></html>', 'html', 'utf-8')
```

**发送附件**

构造`MIMEMultipart`对象代表邮件本身，然后往里面加一个`MIMEText`作为邮件正文，再加上表示附件的`MIMEBase`对象即可：

```python
import smtplib
from email import encoders
from email.header import Header
from email.mime.base import MIMEBase
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.utils import parseaddr, formataddr

def _format_addr(s):
    name, addr = parseaddr(s)
    return formataddr((Header(name, 'utf-8').encode(), addr))

from_addr = input('From: ')
password = input('Password: ')
to_addr = input('To: ')
smtp_server = input('SMTP server: ')

msg = MIMEMultipart()
msg['From'] = _format_addr('Python爱好者 <%s>' % from_addr)
msg['To'] = _format_addr('管理员 <%s>' % to_addr)
msg['Subject'] = Header('来自SMTP的问候……', 'utf-8').encode()

# 邮件正文是MIMEText:
msg.attach(MIMEText('send with file...', 'plain', 'utf-8'))

# 添加附件就是加上一个MIMEBase，从本地读取一个图片:
with open('/Users/Administrator.PC-20190504PBOJ/Pictures/test.png', 'rb') as f:
    # 设置附件的MIME和文件名，这里是png类型:
    mime = MIMEBase('image', 'png', filename='test.png')
    # 加上必要的头信息:
    mime.add_header('Content-Disposition', 'attachment', filename='test.png')
    mime.add_header('Content-ID', '<0>')
    mime.add_header('X-Attachment-Id', '0')
    # 把附件的内容读进来:
    mime.set_payload(f.read())
    # 用Base64编码:
    encoders.encode_base64(mime)
    # 添加到MIMEMultipart:
    msg.attach(mime)

server = smtplib.SMTP(smtp_server, 25)
server.set_debuglevel(1)
server.login(from_addr, password)
server.sendmail(from_addr, [to_addr], msg.as_string())
server.quit()
```

**发送图片**

直接在HTML邮件中加入图片链接大多数MTA会屏蔽这些链接。需按照发送附件方式，然后在HTML中通过引用`src="cid:0"`就可以把附件作为图片嵌入了，多个图片依次编号引用不同的`cid:x`即可：

```python
import smtplib
from email import encoders
from email.header import Header
from email.mime.base import MIMEBase
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.utils import parseaddr, formataddr

def _format_addr(s):
    name, addr = parseaddr(s)
    return formataddr((Header(name, 'utf-8').encode(), addr))

from_addr = input('From: ')
password = input('Password: ')
to_addr = input('To: ')
smtp_server = input('SMTP server: ')

msg = MIMEMultipart()
msg['From'] = _format_addr('Python爱好者 <%s>' % from_addr)
msg['To'] = _format_addr('管理员 <%s>' % to_addr)
msg['Subject'] = Header('来自SMTP的问候……', 'utf-8').encode()
# 引入上传的图片
msg.attach(MIMEText('<html><body><h1>Hello</h1>' +
    '<p><img src="cid:0"></p>' +
    '</body></html>', 'html', 'utf-8'))

# 添加附件就是加上一个MIMEBase，从本地读取一个图片:
with open('/Users/Administrator.PC-20190504PBOJ/Pictures/Multiavatar-xinyu.png', 'rb') as f:
    # 设置附件的MIME和文件名，这里是png类型:
    mime = MIMEBase('image', 'png', filename='Multiavatar-xinyu.png')
    # 加上必要的头信息:
    mime.add_header('Content-Disposition', 'attachment', filename='Multiavatar-xinyu.png')
    mime.add_header('Content-ID', '<0>')
    mime.add_header('X-Attachment-Id', '0')
    # 把附件的内容读进来:
    mime.set_payload(f.read())
    # 用Base64编码:
    encoders.encode_base64(mime)
    # 添加到MIMEMultipart:
    msg.attach(mime)

server = smtplib.SMTP(smtp_server, 25)
server.set_debuglevel(1)
server.login(from_addr, password)
server.sendmail(from_addr, [to_addr], msg.as_string())
server.quit()
```

**同时支持HTML和Plain格式**

如果收件人无法查看HTML格式的邮件，就可以自动降级查看纯文本邮件。用`MIMEMultipart`就可以组合一个HTML和Plain，要注意指定subtype是`alternative`：

```python
msg = MIMEMultipart('alternative')
msg['From'] = ...
msg['To'] = ...
msg['Subject'] = ...

msg.attach(MIMEText('hello', 'plain', 'utf-8'))
msg.attach(MIMEText('<html><body><h1>Hello</h1></body></html>', 'html', 'utf-8'))
# 正常发送msg对象...
```

**加密SMTP**

明文传输可能被窃听，加密SMTP就是先创建SSL安全连接再发送，如Gmail端口587。

```python
smtp_server = 'smtp.gmail.com'
smtp_port = 587
server = smtplib.SMTP(smtp_server, smtp_port)
server.starttls()
server.set_debuglevel(1)
server.login(from_addr, password)
server.sendmail(from_addr, [to_addr], msg.as_string())
server.quit()
# 网络问题无法访问Gmail的SMTP服务器
```

email.mime[详细用法](https://docs.python.org/zh-cn/3/library/email.mime.html#module-email.mime)，继承关系如下：

```python
Message
+- MIMEBase
   +- MIMEMultipart
   +- MIMENonMultipart
      +- MIMEMessage
      +- MIMEText
      +- MIMEImage
```

### POP3收取邮件

收邮件就是编写一个MUA作为客户端从MDA获取邮件，常用协议POP3。Python内置`poplib`模块，实现了POP3协议可以直接用来收邮件。收到的邮件还需要用`email`模块提供的各种类来解析原始文本变成可阅读对象。

**通过POP3下载邮件**

用POP3获取所有邮件只需要循环使用`retr()`把每一封邮件内容拿到即可：

```python
import poplib

# 输入邮件地址, 口令和POP3服务器地址:
email = input('Email: ')
password = input('Password: ')
pop3_server = input('POP3 server: ')

# 连接到POP3服务器:
server = poplib.POP3(pop3_server)
# 可以打开或关闭调试信息:
server.set_debuglevel(1)
# 可选:打印POP3服务器的欢迎文字:
print(server.getwelcome().decode('utf-8'))

# 身份认证:
server.user(email)
server.pass_(password)

# stat()返回邮件数量和占用空间:
print('Messages: %s. Size: %s' % server.stat())
# list()返回所有邮件的编号:
resp, mails, octets = server.list()
# 可以查看返回的列表类似[b'1 82923', b'2 2184', ...]
print(mails)

# 获取最新一封邮件, 注意索引号从1开始:
index = len(mails)
resp, lines, octets = server.retr(index)

# lines存储了邮件的原始文本的每一行,
# 可以获得整个邮件的原始文本:
msg_content = b'\r\n'.join(lines).decode('utf-8')
# 稍后解析出邮件:
msg = Parser().parsestr(msg_content)

# 可以根据邮件索引号直接从服务器删除邮件:
# server.dele(index)
# 关闭连接:
server.quit()
```

**解析邮件**

导入必要的模块，把邮件内容解析为`Message`对象：

```python
from email.parser import Parser
from email.header import decode_header
from email.utils import parseaddr
import poplib
msg = Parser().parsestr(msg_content)
```

但是这个`Message`对象本身可能是一个`MIMEMultipart`对象，包含嵌套的其他`MIMEBase`对象，嵌套可能还不止一层。所以用递归打印出`Message`对象的层次结构：

```python
# indent用于缩进显示:
def print_info(msg, indent=0):
    if indent == 0:
        for header in ['From', 'To', 'Subject']:
            value = msg.get(header, '')
            if value:
                if header=='Subject':
                    value = decode_str(value)
                else:
                    hdr, addr = parseaddr(value)
                    name = decode_str(hdr)
                    value = u'%s <%s>' % (name, addr)
            print('%s%s: %s' % ('  ' * indent, header, value))
    if (msg.is_multipart()):
        parts = msg.get_payload()
        for n, part in enumerate(parts):
            print('%spart %s' % ('  ' * indent, n))
            print('%s--------------------' % ('  ' * indent))
            print_info(part, indent + 1)
    else:
        content_type = msg.get_content_type()
        if content_type=='text/plain' or content_type=='text/html':
            content = msg.get_payload(decode=True)
            charset = guess_charset(msg)
            if charset:
                content = content.decode(charset)
            print('%sText: %s' % ('  ' * indent, content + '...'))
        else:
            print('%sAttachment: %s' % ('  ' * indent, content_type))
```

邮件中的Subject或者Email中包含的名字都是经过编码后的str，要正常显示必须decode：

```python
def decode_str(s):
    value, charset = decode_header(s)[0]
    if charset:
        value = value.decode(charset)
    return value
```

`decode_header()`返回一个list，因为像`Cc`、`Bcc`这样的字段可能包含多个邮件地址，所以解析出来有多个元素。这里只取第一个元素。

文本邮件的内容也是str，需要检测编码，否则非UTF-8编码的邮件都无法正常显示：

```python
def guess_charset(msg):
    charset = msg.get_charset()
    if charset is None:
        content_type = msg.get('Content-Type', '').lower()
        pos = content_type.find('charset=')
        if pos >= 0:
            charset = content_type[pos + 8:].strip()
    return charset
```

运行程序：

```python
import poplib
from email.parser import Parser
from email.header import decode_header
from email.utils import parseaddr


# 邮件的Subject或者Email中包含的名字都是经过编码后的str,要正常显示
# 就必须decode
def decode_str(s):
    # 在不转换字符集的情况下解码消息头值,返回一个list
    value, charset = decode_header(s)[0]
    if charset:
        value = value.decode(charset)
    return value


# 文本邮件的内容也是str,还需要检测编码，
# 否则，非UTF-8编码的邮件都无法正常显示：
def guess_charset(msg):
    print('msg::%s' % msg)
    # 得到字符集
    charset = msg.get_charset()
    print('charset::%s' % charset)
    if charset is None:
        # lower:所有大写字符为小写
        content_type = msg.get('Content-Type', '').lower()
        print('content_type::%s' % content_type)
        # find:检测字符串中是否包含子字符串
        # 返回charset=头字符的位置
        pos = content_type.find('charset=')
        print('pos::', pos)
        if pos >= 0:
            # strip:移除字符串头尾指定的字符(默认为空格)
            charset = content_type[pos + 8:].strip()
    print('charset::%s' % charset)
    return charset


# indent用于缩进显示：
def print_info(msg, indent=0):
    # 初始分析
    if indent == 0:
        # 遍历获取 发件人，收件人，主题
        for header in ['From', 'To', 'Subject']:
            # 获得对应的内容
            value = msg.get(header, '')
            # 有内容
            if value:
                # 如果是主题
                if header == 'Subject':
                    # 解码主题
                    value = decode_str(value)
                else:
                    # parseaddr：解析字符串中的email地址
                    hdr, addr = parseaddr(value)
                    # 解码主题
                    name = decode_str(hdr)
                    # 合成内容
                    value = u'%s <%s>' % (name, addr)
            print('%s%s：%s' % (' ' * indent, header, value))
    # 如果消息由多个部分组成，则返回True。
    if msg.is_multipart():
        # 返回list,包含所有的子对象
        parts = msg.get_payload()
        # enumerate将其组成一个索引序列，利用它可以同时获得索引和值
        for n, part in enumerate(parts):
            # 打印消息模块编号
            print('%s part %s' % (' ' * indent, n))
            # 打印分割符号
            print('%s--------------------' % (' ' * indent))
            # 递归打印
            print_info(part, indent + 1)
    else:
        # 递归结束条件，打印最基本模块
        # 返回消息的内容类型。
        content_type = msg.get_content_type()
        # 如果是text类型或者是html类型
        if content_type == 'text/plain' or content_type == 'text/html':
            # 返回list,包含所有的子对象，开启解码
            content = msg.get_payload(decode=True)
            # 猜测字符集
            charset = guess_charset(msg)
            # 字符集不为空
            if charset:
                # 解密
                content = content.decode(charset)
            # 打印内容
            print('%s Text: %s' % (' ' * indent, content + '...'))
        else:
            print('%s Attachment: %s' % (' ' * indent, content_type))


# # 输入邮件地址，口令和POP3服务器地址
email = input('Email：')
password = input('Password：')
pop3_server = input('POP 3 server：')

# 连接到POP3服务器
server = poplib.POP3_SSL(pop3_server,port=995)
# 可以打开或者关闭调试信息：
server.set_debuglevel(1)
# 可选：打印POP3服务器的欢迎文字
print(server.getwelcome().decode('utf-8'))

# 身份认证：
server.user(email)
server.pass_(password)

# stat()返回邮件数量和占用空间：
print('Message:%s.Size:%s' % server.stat())
# list()返回所有邮件的编号：
resp, mails, octets = server.list()
# 可以查看返回的列表类似[b'1 82923', b'2 2184', ...]
print(mails)

# 获取最新一封邮件，注意索引号从1开始：
index = len(mails)
resp, lines, octets = server.retr(index)

# lines存储了邮件的原始文本的每一行，
# 可以获得整个邮件的原始文本：
msg_content = b'\r\n'.join(lines).decode('utf-8')
# 稍后解析出邮件：
msg = Parser().parsestr(msg_content)

print_info(msg)
# 可以根据邮件索引号直接从服务器删除邮件：
# server.dele(index)
# 关闭连接：
server.quit()
```

运行结果：

```python
Email：814612233@qq.com
Password：授权码
POP 3 server：pop.qq.com
+OK XMail POP3 Server v1.0 Service Ready(XMail v1.0)
*cmd* 'USER 814612233@qq.com'
*cmd* 'PASS 授权码'
*cmd* 'STAT'
*stat* [b'+OK', b'4', b'226232']
Message:4.Size:226232
*cmd* 'LIST'
[b'1 7514', b'2 2639', b'3 108007', b'4 108072']
*cmd* 'RETR 4'
From：Python爱好者 <lsaiah@126.com>
To：管理员 <814612233@qq.com>
Subject：来自SMTP的问候……
 part 0
--------------------
msg::Content-Type: text/html; charset="utf-8"
MIME-Version: 1.0
Content-Transfer-Encoding: base64

PGh0bWw+PGJvZHk+PGgxPkhlbGxvPC9oMT48cD48aW1nIHNyYz0iY2lkOjAiPjwvcD48L2JvZHk+
PC9odG1sPg==

charset::None
content_type::text/html; charset="utf-8"
pos:: 11
charset::"utf-8"
  Text: <html><body><h1>Hello</h1><p><img src="cid:0"></p></body></html>...
 part 1
--------------------
  Attachment: image/png
*cmd* 'QUIT'
```

这封邮件是一个`MIMEMultipart`，包含两部分：第一部分又是一个`MIMEMultipart`，第二部分是一个附件。而内嵌的`MIMEMultipart`是一个`alternative`类型，包含一个纯文本格式的`MIMEText`和一个HTML格式的`MIMEText`。

## 访问数据库

**关系数据库**

数据库为了便于程序保存和读取数据能够快速查询到指定的数据。在关系数据库中表可以存在一对多的关系。根据某个年级的ID就可以查找所有班级的行，Grade表的每一行对应Class表的多行：

```mysql
SELECT * FROM classes WHERE grade_id = '1';
```

Class表的一行记录又可以关联到Student表的多行记录。

- Oracle，典型的高富帅；
- SQL Server，微软自家产品，Windows定制专款；
- DB2，IBM的产品，听起来挺高端；
- Sybase，曾经跟微软是好基友，后来关系破裂，现在家境惨淡。
- MySQL，普及率最高，开源；
- PostgreSQL，学术气息有点重，其实挺不错，但知名度没有MySQL高；
- sqlite，嵌入式数据库，适合桌面和移动应用。

**非关系数据库NoSQL**

### 使用SQLite

SQLite是一种嵌入式数据库本身是一个文件。由于SQLite本身是C写的而且体积很小，所以经常被集成到各种应用程序中，甚至在iOS和Android的App中都可以集成。Python内置了SQLite3，表是数据的集合，多个表之间通过外键关联。首先要连接到数据库称为`Connection`，然后打开游标称为`Cursor`，通过`Cursor`执行SQL语句，最后获得执行结果。任何数据库只要提供符合Python标准的数据库驱动就能连接Python API操作数据库。

```python
# 导入SQLite驱动:
>>> import sqlite3
# 连接到SQLite数据库
# 数据库文件是test.db
# 如果文件不存在，会自动在当前目录创建:
>>> conn = sqlite3.connect('test.db')
# 创建一个Cursor:
>>> cursor = conn.cursor()
# 执行一条SQL语句，创建user表:
>>> cursor.execute('create table user (id varchar(20) primary key, name varchar(20))')
<sqlite3.Cursor object at 0x10f8aa260>
# 创建表
cursor.execute('''CREATE TABLE COMPANY
       (ID INT PRIMARY KEY     NOT NULL,
       NAME           TEXT    NOT NULL,
       AGE            INT     NOT NULL,
       ADDRESS        CHAR(50),
       SALARY         REAL);''')
# 继续执行一条SQL语句，插入一条记录:
>>> cursor.execute('insert into user (id, name) values (\'1\', \'lsaiah\')')
<sqlite3.Cursor object at 0x10f8aa260>
# INSERT操作
cursor.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (1, 'Paul', 32, 'California', 20000.00 )")
# 通过rowcount获得插入的行数:
>>> cursor.rowcount
1
# 关闭Cursor:
>>> cursor.close()
# 提交事务:
>>> conn.commit()
# 关闭Connection:
>>> conn.close()

# 新连接
>>> conn = sqlite3.connect('test.db')
>>> cursor = conn.cursor()
# 执行查询语句:
>>> cursor.execute('select * from user where id=?', ('1',))
<sqlite3.Cursor object at 0x10f8aa340>
# 获得查询结果集:
>>> values = cursor.fetchall()
>>> values
[('1', 'lsaiah')]
# SELECT操作
cursor = c.execute("SELECT id, name, address, salary  from COMPANY")
for row in cursor:
   print "ID = ", row[0]
   print "NAME = ", row[1]
   print "ADDRESS = ", row[2]
   print "SALARY = ", row[3], "\n"
# UPDATE操作
cursor.execute("UPDATE COMPANY set SALARY = 25000.00 where ID=1")
conn.commit()
print "Total number of rows updated :", conn.total_changes
# DELETE操作
c.execute("DELETE from COMPANY where ID=2;")
conn.commit()
print "Total number of rows deleted :", conn.total_changes
>>> cursor.close()
>>> conn.close()
```

`Connection`和`Cursor`对象打开后一定记得关闭否则资源泄露。确保出错也关闭使用`try:...except:...finally:...`。

`Cursor`对象执行`insert`，`update`，`delete`语句时执行结果由`rowcount`返回影响的行数就可以拿到执行结果。

`Cursor`对象执行`select`语句时，通过`fetchall()`可以拿到结果集。结果集是一个`list`，每个元素都是一个`tuple`，对应一行记录。如果SQL语句带有参数，需要把参数按照位置传递给`execute()`方法，有几个`?`占位符就必须对应几个参数，例如：

```python
cursor.execute('select * from user where name=? and pwd=?', ('abc', 'password'))
```

**练习**

编写函数，在Sqlite中根据分数段查找指定的名字：

```python
import os, sqlite3

db_file = os.path.join(os.path.dirname(__file__), 'test.db')
if os.path.isfile(db_file):
    os.remove(db_file)

# 初始数据:
conn = sqlite3.connect(db_file)
cursor = conn.cursor()
cursor.execute('create table user(id varchar(20) primary key, name varchar(20), score int)')
cursor.execute(r"insert into user values ('A-001', 'Adam', 95)")
cursor.execute(r"insert into user values ('A-002', 'Bart', 62)")
cursor.execute(r"insert into user values ('A-003', 'Lisa', 78)")
conn.commit()
cursor.close()
conn.close()

def get_score_in(low, high):
    ' 返回指定分数区间的名字，按分数从低到高排序 '
    conn = sqlite3.connect(db_file)
    cur = conn.cursor()
    try:
        cur.execute('select * from user where score between ? and ? order by score',(low,high))
        data = cur.fetchall()
        print(data)
        data = sorted(data, key=lambda s: s[2])      # [('id','name','score'),(),()...], 根据score排序
        print(data)
        return list(map(lambda x : x[1], data))
    finally:
        cur.close()
        conn.close()
# 测试:
assert get_score_in(80, 95) == ['Adam'], get_score_in(80, 95)
assert get_score_in(60, 80) == ['Bart', 'Lisa'], get_score_in(60, 80)
assert get_score_in(60, 100) == ['Bart', 'Lisa', 'Adam'], get_score_in(60, 100)
print('Pass')
```

### 使用MySQL

MySQL是为服务器端设计的数据库，能承受高并发，占用内存也远大于SQLite。常用引擎是支持数据库事物的InnoDB。

**安装MySQL**

[下载地址](http://dev.mysql.com/downloads/mysql/5.6.html)，设置UTF-8编码，配置文件在`etc/my.cnf`：

```mysql
[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci
# 检查编码
mysql> show variables like '%char%';
```

**安装MySQL驱动**

MySQL官方提供了mysql-connector-python驱动连接MySQL服务器：

```
pip install mysql-connector
pip install PyMySQL
```

连接到MySQL服务器的testdb数据库：

```python
# 导入MySQL驱动:
>>> import mysql.connector
# host远程IP，没有默认localhost:
>>> conn = mysql.connector.connect(host='xxx', user='admin', password='xxx', database='testdb')
>>> cursor = conn.cursor()
# 创建user表:
>>> cursor.execute('create table user (id varchar(20) primary key, name varchar(20))')
# 插入一行记录，注意MySQL的占位符是%s:
>>> cursor.execute('insert into user (id, name) values (%s, %s)', ['1', 'lsaiah'])
>>> cursor.rowcount
1
# 提交事务:
>>> conn.commit()
>>> cursor.close()
# 运行查询:
>>> cursor = conn.cursor()
>>> cursor.execute('select * from user where id = %s', ('1',))
>>> values = cursor.fetchall()
>>> values
[('1', 'lsaiah')]
# 关闭Cursor和Connection:
>>> cursor.close()
True
>>> conn.close()
```

### 使用SQLAIchemy

ORM（Object-Relational Mapping）框架SQLAlchemy把关系数据库的表结构映射到对象上。

把一个表的内容用Python数据结构表示，用一个list表示多行，list的每一个元素是tuple，用class实例表示就可以看出表结构：

```python
class User(object):
    def __init__(self, id, name):
        self.id = id
        self.name = name

[
    User('1', 'zs'),
    User('2', 'ls'),
    User('3', 'ww')
]
```

**安装SQLAlchemy**

```python
pip install sqlalchemy
```

导入SQLAlchemy，并初始化DBSession：

```python
from sqlalchemy import Column, String, create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 创建对象的基类:
Base = declarative_base()

# 定义User对象:
class User(Base):
    # 表的名字:
    __tablename__ = 'user'

    # 表的结构:
    id = Column(String(20), primary_key=True)
    name = Column(String(20))

# 初始化数据库连接,用一个字符串表示连接信息:
# '数据库类型+数据库驱动名称://用户名:口令@地址:端口号/数据库名'
engine = create_engine('mysql+mysqlconnector://admin:password@remoteip:3306/testdb')
# 创建DBSession类型:
DBSession = sessionmaker(bind=engine)
```

如果有多个表就继续定义class，如School：

```python
class School(Base):
    __tablename__ = 'school'
    id = ...
    name = ..
```

有了ORM，向数据库表中添加一行记录，可以视为添加一个`User`对象：

```
# 创建session对象:
session = DBSession()
# 创建新User对象:
new_user = User(id='2', name='kwaiyu')
# 添加到session:
session.add(new_user)
# 提交即保存到数据库:
session.commit()
# 关闭session:
session.close()
```

有了ORM，查询出的不是tuple，而是`User`对象：

```python
# 创建Session:
session = DBSession()
# 创建Query查询，filter是where条件，最后调用one()返回唯一行，如果调用all()则返回所有行:
user = session.query(User).filter(User.id=='2').one()
# 打印类型和对象的name属性:
print('type:', type(user))
print('name:', user.name)
# 关闭Session:
session.close()
# 运行结果
type: <class '__main__.User'>
name: kwaiyu
```

可见ORM就是把数据库表的行与相应的对象建立关联互相转换。由于关系数据库多个表还可以用外键实现一对多，多对多关联，所以ORM框架也可提供两个对象之间一对多，多对多的功能。

例如一个User拥有多个Book，定义一对多关系如下：

```python
class User(Base):
    __tablename__ = 'user'

    id = Column(String(20), primary_key=True)
    name = Column(String(20))
    # 一对多:
    books = relationship('Book')

class Book(Base):
    __tablename__ = 'book'

    id = Column(String(20), primary_key=True)
    name = Column(String(20))
    # “多”的一方的book表是通过外键关联到user表的:
    user_id = Column(String(20), ForeignKey('user.id'))
```

查询一个User对象时，该对象的books属性将返回一个包含若干个Book对象的list。

## Web开发

1. 静态Web页面：由文本编辑器直接编辑并生成静态的HTML页面，如果要修改Web页面的内容，就需要再次编辑HTML源文件，早期的互联网Web页面就是静态的；
2. CGI：由于静态Web页面无法与用户交互，比如用户填写了一个注册表单，静态Web页面就无法处理。要处理用户发送的动态数据，出现了Common Gateway Interface，简称CGI，用C/C++编写。
3. ASP/JSP/PHP：由于Web应用特点是修改频繁，用C/C++这样的低级语言非常不适合Web开发，而脚本语言由于开发效率高，与HTML结合紧密，因此迅速取代了CGI模式。ASP是微软推出的用VBScript脚本编程的Web开发技术，而JSP用Java来编写脚本，PHP本身则是开源的脚本语言。
4. MVC：为了解决直接用脚本语言嵌入HTML导致的可维护性差的问题，Web应用也引入了Model-View-Controller的模式简化Web开发。ASP发展为ASP.Net，JSP和PHP也有一大堆MVC框架。
5. 异步开发、新的MVVM前端技术

### HTTP协议

HTTP（HyperText Transfer Protocol）超文本传输协议在TCP/IP协议之上，属于应用层协议。

1. 支持客户/服务器模式简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
2. 灵活：*HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。
3. 无连接： 无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
4. 无状态： HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

| 请求方法 | 说明                                                 |
| :------- | :--------------------------------------------------- |
| GET      | 请求获取Request-URI所标识的资源                      |
| POST     | 在Request-URI所标识的资源后附加新的数据              |
| HEAD     | 请求获取由Request-URI所标识的资源的响应消息报头      |
| PUT      | 请求服务器存储一个资源，并用Request-URI作为其标识    |
| DELETE   | 请求服务器删除Request-URI所标识的资源                |
| TRACE    | 请求服务器回送收到的请求信息，主要用于测试或诊断     |
| CONNECT  | 保留将来使用                                         |
| OPTIONS  | 请求查询服务器的性能，或者查询与资源相关的选项和需求 |

- 1xx：指示信息--表示请求已接收，继续处理
- 2xx：成功--表示请求已被成功接收、理解、接受
- 3xx：重定向--要完成请求必须进行更进一步的操作
- 4xx：客户端错误--请求有语法错误或请求无法实现
- 5xx：服务器端错误--服务器未能实现合法的请求

**Get Headers**

```http
# General
Request URL: https://notes.lsaiah.cn/
Request Method: GET
Status Code: 200 
Remote Address: 0.0.0.0:443
Referrer Policy: strict-origin-when-cross-origin
# Response Headers
accept-ranges: bytes
access-control-allow-origin: *
age: 0
cache-control: max-age=600
content-encoding: gzip
content-length: 7093
content-type: text/html; charset=utf-8
date: Fri, 29 Jan 2021 05:35:00 GMT
etag: W/"60128516-5af4"
expires: Fri, 29 Jan 2021 05:31:58 GMT
last-modified: Thu, 28 Jan 2021 09:34:14 GMT
server: GitHub.com
vary: Accept-Encoding
via: 1.1 varnish
x-cache: MISS
x-cache-hits: 0
x-fastly-request-id: cb3c6e8edae88cbf51b4148ba2b91e7dbf491f34
x-github-request-id: 30B4:1AC1:8E293:96599:60139B75
x-proxy-cache: MISS
x-served-by: cache-tyo11974-TYO
x-timer: S1611898501.651423,VS0,VE184
# Request Headers
:authority: notes.lsaiah.cn
:method: GET
:path: /
:scheme: https
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
accept-encoding: gzip, deflate, br
accept-language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
cache-control: no-cache
cookie: ...
pragma: no-cache
referer: https://notes.lsaiah.cn/
sec-fetch-dest: document
sec-fetch-mode: navigate
sec-fetch-site: same-origin
sec-fetch-user: ?1
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36
```

**Post Headers**

```http
# General
Request URL: https://www.lsaiah.cn/login
Request Method: POST
Status Code: 302 
Remote Address: 0.0.0.0:443
Referrer Policy: strict-origin-when-cross-origin
# Response Headers
accept-ranges: bytes
access-control-allow-methods: GET, HEAD, POST, PUT, DELETE, OPTIONS, PATCH
access-control-allow-origin: *
access-control-max-age: 86400
content-type: text/html; charset=UTF-8
date: Fri, 29 Jan 2021 05:47:49 GMT
location: https://www.lsaiah.cn/admin/
server: marco/2.13
set-cookie: ...; path=/
set-cookie: ...; path=/
via: S.mix-js-czx2-045, V.mix-js-czx2-045, T.61.-, M.ctn-zj-jgh-061
x-request-id: 32d2846cbaadca653b9a1864e757d478
x-source: C/302
# Request Headers
:authority: www.lsaiah.cn
:method: POST
:path: /...
:scheme: https
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
accept-encoding: gzip, deflate, br
accept-language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
cache-control: no-cache
content-length: 79
content-type: application/x-www-form-urlencoded
cookie: ...
origin: https://www.lsaiah.cn
pragma: no-cache
referer: https://www.lsaiah.cn/...
sec-fetch-dest: document
sec-fetch-mode: navigate
sec-fetch-site: same-origin
sec-fetch-user: ?1
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36
# Query String Parameters
_=16进制字符串
# Form Data
name=admin&password=...referer=https...
```

### HTML+CSS+JavaScript

[w3school](https://www.w3school.com.cn/h.asp)

### WSGI接口

WSGI：Web Server Gateway Interface，实现底层TCP连接，HTTP请求响应接口。

函数`application()`符合WSGI接口处理HTTP请求，接收参数：

- environ：一个包含所有HTTP请求信息的`dict`对象；
- start_response：一个发送HTTP响应的函数，在函数中调用，接收参数：
  - HTTP响应码
  - 一组`list`表示的HTTP Header，每个Header包含两个`str`的`tuple`。

然后返回HTTP响应body，由WSGI服务器调用，Python内置模块wsgiref。

**运行WSGI服务**

Web应用程序WSGI处理函数：

```python
# hello.py
def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return [b'<h1>Hello, web!</h1>']

# 从environ里读取PATH_INFO显示动态的内容
def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    body = '<h1>Hello, %s!</h1>' % (environ['PATH_INFO'][1:] or 'web')
    return [body.encode('utf-8')]
# 中文UTF-8解码
def application(environ, start_response) :
    print(environ['PATH_INFO'].encode('iso-8859-1').decode('utf8'))
    start_response('200 OK', [('Content-Type', 'text/html;charset=utf-8')])
    body = '<h1>Hello, %s!</h1>'%(environ['PATH_INFO'].encode('iso-8859-1').decode('utf8')[1:] or 'web')
    return [body.encode('utf-8')]
```

启动WSGI服务器，加载`application()`函数：

```python
# server.py
# 从wsgiref模块导入:
from wsgiref.simple_server import make_server
# 导入我们自己编写的application函数:
from hello import application

# 创建一个服务器，IP地址为空，端口是8000，处理函数是application:
httpd = make_server('', 8000, application)
print('Serving HTTP on port 8000...')
# 开始监听HTTP请求:
httpd.serve_forever()
```

### Web框架

处理多个不同请求从`environ`变量里取出HTTP请求的信息逐个判断：

```python
def application(environ, start_response):
    method = environ['REQUEST_METHOD']
    path = environ['PATH_INFO']
    if method=='GET' and path=='/':
        return handle_home(environ, start_response)
    if method=='POST' and path='/signin':
        return handle_signin(environ, start_response)
    ...
```

这样写不好维护，用一个函数处理一个URL对WSGI接口进一步抽象。至于URL到函数的映射交给Web框架。[Flask](http://flask.pocoo.org/)编写Web APP比WSGI接口更简单。先安装：

```python
pip install flask
```

写一个`app.py`处理3个URL，分别是：

- `GET /`：首页，返回`Home`；
- `GET /signin`：登录页，显示登录表单；
- `POST /signin`：处理登录表单，显示登录结果。

同一个URL`/signin`分别有GET和POST两种请求，映射到两个处理函数中。

Flask通过Python的[装饰器](https://www.liaoxuefeng.com/wiki/1016959663602400/1017451662295584)在内部自动地把URL和函数给关联起来：

```python
from flask import Flask
from flask import request

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def home():
    return '<h1>Home</h1>'

@app.route('/signin', methods=['GET'])
def signin_form():
    return '''<form action="/signin" method="post">
              <p><input name="username"></p>
              <p><input name="password" type="password"></p>
              <p><button type="submit">Sign In</button></p>
              </form>'''

@app.route('/signin', methods=['POST'])
def signin():
    # 需要从request对象读取表单内容：
    if request.form['username']=='admin' and request.form['password']=='password':
        return '<h3>Hello, admin!</h3>'
    return '<h3>Bad username or password.</h3>'

if __name__ == '__main__':
    app.run()
```

运行结果：

```python
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
127.0.0.1 - - [29/Jan/2021 15:13:40] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [29/Jan/2021 15:13:41] "GET /favicon.ico HTTP/1.1" 404 -
127.0.0.1 - - [29/Jan/2021 15:16:38] "GET /signin HTTP/1.1" 200 -
127.0.0.1 - - [29/Jan/2021 15:17:59] "POST /signin HTTP/1.1" 200 -
```

进入首页`http://localhost:5000/`，显示Home；进入登录表单`http://localhost:5000/signin`，如果账号密码和预设（实际去数据库查询比对）一致登录成功，否则登录失败。

常见Python Web框架：

- [Django](https://www.djangoproject.com/)：全能型Web框架；
- [web.py](http://webpy.org/)：一个小巧的Web框架；
- [Bottle](http://bottlepy.org/)：和Flask类似的Web框架；
- [Tornado](http://www.tornadoweb.org/)：Facebook的开源异步Web框架。

### 模板

由于在Python代码中拼大量字符串不现实，所以使用模板，在HTML文档中嵌入变量和指令，传入数据替换后得到最终的HTML。

MVC：Model-View-Controller 模型-视图-控制器

Python处理URL的函数就是C：Controller，Controller负责业务逻辑，比如检查用户名是否存在，取出用户信息等等；

包含变量`{{ name }}`的模板就是V：View，View负责显示逻辑，通过简单地替换一些变量，View最终输出的就是用户看到的HTML。

Model是用来传给View的，View在替换变量的时候就可以从Model中取出相应的数据`dict`。

把输出字符串作为HTML的例子用MVC模式改写：

```python
from flask import Flask, request, render_template

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def home():
    return render_template('home.html')

@app.route('/signin', methods=['GET'])
def signin_form():
    return render_template('form.html')

@app.route('/signin', methods=['POST'])
def signin():
    username = request.form['username']
    password = request.form['password']
    if username=='admin' and password=='password':
        return render_template('signin-ok.html', username=username)
    return render_template('form.html', message='Bad username or password', username=username)

if __name__ == '__main__':
    app.run()
```

Flask通过`render_template()`函数实现模板的渲染。渲染模块也有很多种，Flask默认支持的模板是[jinja2](http://jinja.pocoo.org/)，安装jinja2：

```python
pip install jinja2
```

开始编写模板`home.html`：

```jinja2
<html>
<head>
  <title>Home</title>
</head>
<body>
  <h1 style="font-style:italic">Home</h1>
</body>
</html>
```

登录模板`form.html`：

```jinja2
<html>
<head>
  <title>Please Sign In</title>
</head>
<body>
  {% if message %}
  <p style="color:red">{{ message }}</p>
  {% endif %}
  <form action="/signin" method="post">
    <legend>Please sign in:</legend>
    <p><input name="username" placeholder="Username" value="{{ username }}"></p>
    <p><input name="password" placeholder="Password" type="password"></p>
    <p><button type="submit">Sign In</button></p>
  </form>
</body>
</html>
```

登录成功模板`signin-ok.html`：

```jinja2
<html>
<head>
  <title>Welcome, {{ username }}</title>
</head>
<body>
  <p>Welcome, {{ username }}!</p>
</body>
</html>
```

登录失败在`form.html`做判断重用模板。把所有模板放在`templates`目录下，和`app.py`在同级目录下，启动`app.py`。这样就把HTML代码和Python代码分离了，而且每次改完模板刷新就有效果。在Jinja2模板中，用`{{ name }}`表示一个需要替换的变量，需要循环、条件判断等指令语句用`{% ... %}`表示指令，如循环输出页码：

```jinja2
{% for i in page_list %}
    <a href="/page/{{ i }}">{{ i }}</a>
{% endfor %}
```

除了Jinja2，常见的模板还有：

- [Mako](http://www.makotemplates.org/)：用`<% ... %>`和`${xxx}`的一个模板；
- [Cheetah](http://www.cheetahtemplate.org/)：也是用`<% ... %>`和`${xxx}`的一个模板；
- [Django](https://www.djangoproject.com/)：Django是一站式框架，内置一个用`{% ... %}`和`{{ xxx }}`的模板。

## 异步IO

在一个线程中，CPU等待IO完成才继续进行称为同步IO，一个IO阻塞导致当前线程挂起无法继续执行，多线程多进程解决了并发问题但有上限且切换开销大性能下降。另一种解决办法是异步IO，执行耗时的IO操作只发指令不等待结果继续下一个任务，当耗时IO完成后会得到通知。

同步IO：

```python
do_some_code()
f = open('/path/to/file', 'r')
r = f.read() # <== 线程停在此处等待IO操作结果
# IO操作完成后线程才能继续执行:
do_some_code(r)
```

异步IO模型需要一个消息循环，主线程不断地重复读取消息-处理消息的过程：

```python
loop = get_event_loop()
while True:
    event = loop.get_event()
    process_event(event)
```

### 协程

线程和进程的操作是由程序触发系统接口，本质上是操作系统提供的功能。而协程的操作则是程序员指定的，在python中通过yield人为的实现并发处理。

协程存在的意义：对于多线程应用，CPU通过切片的方式来切换线程间的执行，线程切换时需要耗时。协程只使用一个线程，分解一个线程成为多个微线程（Coroutine），在一个线程中规定某个代码块的执行顺序，子程序调用总是一个入口一个返回顺序明确，而协程调用在执行过程中可能内部中断转而执行别的子程序再返回中断位置继续执行。协程比多线程执行效率高，没有锁机制。

```python
def A():
    print('1')
    print('2')
    print('3')

def B():
    print('x')
    print('y')
    print('z')
# 可能结果    
12xy3z
```

Python协程是generator实现的，不但可以通过`for`循环来迭代，还可以不断调用`next()`函数获取由`yield`语句返回的下一个值。`yield`不但可以返回一个值，还可以接收调用者发出的参数。

传统生产者-消费者模型是一个线程写消息，一个线程取消息，通过锁机制控制队列和等待，不小心就可能死锁。改用协程，生产者生产消息后，通过`yield`跳转到消费者开始执行，待消费者执行完毕后，切换回生产者继续生产，效率极高：

```python
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        r = '200 OK'

def produce(c):
    c.send(None)
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()

c = consumer()
produce(c)

# 执行结果：
[PRODUCER] Producing 1...
[CONSUMER] Consuming 1...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 2...
[CONSUMER] Consuming 2...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 3...
[CONSUMER] Consuming 3...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 4...
[CONSUMER] Consuming 4...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 5...
[CONSUMER] Consuming 5...
[PRODUCER] Consumer return: 200 OK
```

`consumer`函数是一个`generator`，把一个`consumer`传入`produce`后：

1. 首先调用`c.send(None)`启动生成器；
2. 然后一旦生产了东西，通过`c.send(n)`切换到`consumer`执行；
3. `consumer`通过`yield`拿到消息，处理，又通过`yield`把结果传回；
4. `produce`拿到`consumer`处理的结果，继续生产下一条消息；
5. `produce`决定不生产了，通过`c.close()`关闭`consumer`，整个过程结束。

整个流程无锁，由一个线程执行，`produce`和`consumer`协作完成任务，所以称为协程而非线程的抢占式多任务。

协程的适用场景：当程序中存在大量不需要CPU的操作时（IO）

常用第三方模块gevent和greenlet（本质上，gevent是对greenlet的高级封装）

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

### asyncio(<3.5)

`asyncio`标准库模型就是一个消息循环，直接获取一个`EventLoop`的引用，把需要执行的协程扔到`EventLoop`中执行就实现了异步IO。

```python
import asyncio

@asyncio.coroutine
def hello():
    print("Hello world!")
    # 异步调用asyncio.sleep(1):
    r = yield from asyncio.sleep(1)
    print("Hello again!")

# 获取EventLoop:
loop = asyncio.get_event_loop()
# 执行coroutine
loop.run_until_complete(hello())
loop.close()
```

`@asyncio.coroutine`把一个generator标记为coroutine类型，然后把这个`coroutine`扔到`EventLoop`中执行。

`hello()`先打印出`Hello world!`，然后`yield from`调用另一个`generator`。由于`asyncio.sleep()`也是一个`coroutine`，所以线程不会等待`asyncio.sleep()`，而是直接中断并执行下一个消息循环。当`asyncio.sleep()`返回时，线程就可以从`yield from`拿到返回值（此处是`None`）接着执行下一行语句。

把`asyncio.sleep(1)`看成是一个耗时1秒的IO操作，期间主线程并未等待，而是去执行`EventLoop`中其他可以执行的`coroutine`实现了并发执行。

### async/await

用Task封装两个`coroutine`（从Python3.5开始引入新的语法async和await）：

```python
import threading
import asyncio

async def hello():
    print('Hello world! (%s)' % threading.currentThread())
    await asyncio.sleep(1)
    print('Hello again! (%s)' % threading.currentThread())

loop = asyncio.get_event_loop()
tasks = [hello(), hello()]
loop.run_until_complete(asyncio.wait(tasks))
loop.close()

# 执行结果：
Hello world! (<_MainThread(MainThread, started 5064)>)
Hello world! (<_MainThread(MainThread, started 5064)>)
暂停约1秒继续输出：
Hello again! (<_MainThread(MainThread, started 5064)>)
Hello again! (<_MainThread(MainThread, started 5064)>)
```

两个`coroutine`是由同一个线程并发执行的，把`asyncio.sleep()`换成真正的IO操作就可以实现同线程协程并发。用`async`和`await`异步协程网络连接获取sina，sohu和163的网站：

```python
import asyncio

async def wget(host):
    print('wget {}'.format(host))
    # 创建 TCP 客户端并连接服务器，或者说创建一个 TCP 连接对象
    # open_connection 接收两个参数：主机和端口号
    # connect 是协程，这步仅是创建协程对象，立即返回，不阻塞
    connect = asyncio.open_connection(host, 80)
    # await 运行协程连接服务器，这步是阻塞操作，释放 CPU
    # 连接创建成功后，asyncio.open_connection 方法的返回值就是读写对象
    # 读写对象分别为 StreamReader 和 StreamWriter 实例
    # 它们也是协程对象，底层调用 socket 模块的 send 和 recv 方法实现读写
    reader, writer = await connect
    # header 是发送给服务器的消息，意为获取页面的 header 信息
    # 这个格式是固定的，见下图
    header = 'GET / HTTP/1.0\r\nHost: {}\r\n\r\n'.format(host)
    # 给服务器发消息，注意消息是二进制的
    writer.write(header.encode())
    # 这是一个与底层 IO 输入缓冲区交互的流量控制方法
    # 当缓冲区达到上限时，drain() 阻塞，待到缓冲区回落到下限时，写操作恢复
    # 当不需要等待时，drain() 会立即返回，例如上面的消息内容较少，不会阻塞
    # 这就是一个控制消息的数据量的控制阀
    await writer.drain()
    # 给服务器发送消息后，就等着读取服务器返回来的消息
    while True:
        # 读取数据是阻塞操作，释放 CPU
        # reader 相当于一个水盆，服务器发来的数据是水流
        # readline 表示读取一行，以 \n 作为换行符
        # 如果在出现 \n 之前，数据流中出现 EOF（End Of File 文件结束符）也会返回
        # 相当于出现 \n 或 EOF 时，拧上水龙头，line 就是这盆水
        line = await reader.readline()
        # 数据接收完毕，会返回空字符串 \r\n ，退出 while 循环，结束数据接收
        if line.decode() == '\r\n':
            break
        # 接收的数据是二进制数据，转换为 UTF-8 格式并打印
        # rstrip 方法删掉字符串的结尾处的空白字符，也就是 \n
        print('{} header > {}'.format(host, line.decode().rstrip()))
    writer.close()   # 关闭数据流，可以省略

host_list = ['www.baidu.com', 'www.douban.com', 'zhihu.com']   # 主机列表
loop = asyncio.get_event_loop()                             # 事件循环
tasks = asyncio.wait([wget(host) for host in host_list])    # 任务收集器
loop.run_until_complete(tasks)                              # 阻塞运行任务
loop.close()
```

执行结果：

```python
wget www.sohu.com...
wget www.sina.com.cn...
wget www.163.com...
www.sina.com.cn header > HTTP/1.1 302 Found
www.sohu.com header > HTTP/1.1 307 Temporary Redirect
www.163.com header > HTTP/1.1 301 Moved Permanently
```

### aiohttp

把`asyncio`用在服务器端单线程+`coroutine`实现多并发TCP、UDP、SSL等协议。`aiohttp`则是基于`asyncio`实现的HTTP框架。安装：

```python
pip install aiohttp
```

编写一个HTTP服务器，分别处理以下URL：

- `/` - 首页返回`b'<h1>Index</h1>'`；
- `/hello/{name}` - 根据URL参数返回文本`hello, %s!`。

`aiohttp`的初始化函数`init()`也是一个`coroutine`，`loop.create_server()`则利用`asyncio`创建TCP服务：

```python
import asyncio

from aiohttp import web

routes = web.RouteTableDef()
@routes.get('/')
async def index(request):
    await asyncio.sleep(0.5)
    return web.Response(body=b'<h1>Index</h1>',headers={'content-type': 'text/html', 'charset': 'utf-8'})

@routes.get('/hello')
async def hello(request):
    await asyncio.sleep(0.5)
    text = '<h1>hello, %s!</h1>' % request.match_info['name']
    return web.Response(body=text.encode('utf-8'),headers={'content-type': 'text/html', 'charset': 'utf-8'})

async def init(loop):
    app = web.Application()
    app.add_routes(routes)
    app.router.add_route('GET', '/', index)
    app.router.add_route('GET', '/hello/{name}', hello)
    # web.run_app(app,host='127.0.0.1',port=8000)
    srv = await loop.create_server(app._make_handler(), '127.0.0.1', 8000)
    print('Server started at http://127.0.0.1:8000...')

loop = asyncio.get_event_loop()
loop.run_until_complete(init(loop))
loop.run_forever()
```

