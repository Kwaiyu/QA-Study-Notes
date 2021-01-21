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

```
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

### struct

### hashlib

### hmac

### itertools

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