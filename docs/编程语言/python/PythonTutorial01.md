## Python简介

### 简介

Python 是一种解释型、面向对象、动态数据类型的高级程序设计语言。由 Guido van Rossum 于 1989 年底发明，第一个公开发行版发行于 1991 年。像 Perl 语言一样, Python 源代码同样遵循 GPL(GNU General Public License) 协议。

### 安装

**在Mac上安装Python**

如果你正在使用Mac，系统是OS X>=10.9，那么系统自带的Python版本是2.7。要安装最新的Python 3.8，有两个方法：

方法一：从Python官网下载Python 3.8的[安装程序](https://www.python.org/downloads/)，下载后双击运行并安装；

方法二：如果安装了[Homebrew](https://brew.sh/)，直接通过命令`brew install python3`安装即可。

**在Linux上安装Python**

```
$ sudo apt-get update
$ sudo apt-get install python3.6
```

```
$ sudo dnf install python3
```

- 打开WEB浏览器访问 https://www.python.org/downloads/source/
- 选择适用于 Unix/Linux 的源码压缩包。
- 下载及解压压缩包 **Python-3.x.x.tgz**，**3.x.x** 为你下载的对应版本号。
- 如果你需要自定义一些选项修改 *Modules/Setup*

```
# tar -zxvf Python-3.8.1.tgz
# cd Python-3.8.1
# ./configure
# make && make install
# python3 -V
Python 3.8.1
```

**在Windows上安装Python**

根据你的Windows版本（64位还是32位）从Python的官方网站下载Python 3.8对应的[64位安装程序](https://www.python.org/ftp/python/3.8.0/python-3.8.0-amd64.exe)或[32位安装程序](https://www.python.org/ftp/python/3.8.0/python-3.8.0.exe)，运行下载的exe安装包。

在Windows上运行Python时，请先启动命令行，然后运行`python`。

在Mac和Linux上运行Python时，请打开终端，然后运行`python3`。

### 解释器

Python的解释器很多，但使用最广泛的还是CPython。如果要和Java或.Net平台交互，最好的办法不是用Jython或IronPython，而是通过网络调用来交互，确保各程序之间的独立性。

**CPython**

当我们从[Python官方网站](https://www.python.org/)下载并安装好Python 3.x后，我们就直接获得了一个官方版本的解释器：CPython。这个解释器是用C语言开发的，所以叫CPython。在命令行下运行`python`就是启动CPython解释器。CPython是使用最广的Python解释器。教程的所有代码也都在CPython下执行。

**IPython**

IPython是基于CPython之上的一个交互式解释器，也就是说，IPython只是在交互方式上有所增强，但是执行Python代码的功能和CPython是完全一样的。好比很多国产浏览器虽然外观不同，但内核其实都是调用了IE。

CPython用`>>>`作为提示符，而IPython用`In [序号]:`作为提示符。

**PyPy**

PyPy是另一个Python解释器，它的目标是执行速度。PyPy采用[JIT技术](http://en.wikipedia.org/wiki/Just-in-time_compilation)，对Python代码进行动态编译（注意不是解释），所以可以显著提高Python代码的执行速度。

绝大部分Python代码都可以在PyPy下运行，但是PyPy和CPython有一些是不同的，这就导致相同的Python代码在两种解释器下执行可能会有不同的结果。如果你的代码要放到PyPy下执行，就需要了解[PyPy和CPython的不同点](http://pypy.readthedocs.org/en/latest/cpython_differences.html)。

**Jython**

Jython是运行在Java平台上的Python解释器，可以直接把Python代码编译成Java字节码执行。

**IronPython**

IronPython和Jython类似，只不过IronPython是运行在微软.Net平台上的Python解释器，可以直接把Python代码编译成.Net的字节码。

### 运行

用文本编辑器写Python程序，然后保存为后缀为`.py`的文件，就可以用Python直接运行这个程序了。

Python的交互模式和直接运行`.py`文件有什么区别呢？

直接输入`python`进入交互模式，相当于启动了Python解释器，等待你一行一行地输入源代码，每输入一行就执行一行。

直接运行`.py`文件相当于启动了Python解释器，然后一次性把`.py`文件的源代码给执行了，你是没有机会以交互的方式输入源代码的。

用Python开发程序，完全可以一边在文本编辑器里写代码，一边开一个交互式命令窗口，在写代码的过程中，把部分代码粘到命令行去验证，事半功倍！

**注释**

```python
# 这是单行注释
def a():
    '''这是文档注释'''
    pass
print(a.__doc__)
```

**直接运行py文件**

在Windows上是不行的，但是，在Mac和Linux上是可以的，方法是在`.py`文件的第一行加上一个特殊的注释：

```python
#!/usr/bin/env python3
print('hello, world')
```

```python
$ chmod a+x hello.py
./hello.py
```

**输出**

用`print()`在括号中加上字符串，就可以向屏幕上输出指定的文字。比如输出`'hello, world'`，用代码实现如下：

```python
>>> print('hello, world')
```

`print()`函数也可以接受多个字符串，用逗号“,”隔开，就可以连成一串输出，遇到逗号会输出一个空格：

```python
>>> print('The quick brown fox', 'jumps over', 'the lazy dog')
The quick brown fox jumps over the lazy dog
```

**输入**

Python提供了一个`input()`，可以让用户输入字符串，并存放到一个变量里。比如输入用户的名字：

```python
>>> name = input()
Lsaiah
```

输入完成后，不会有任何提示，Python交互式命令行又回到`>>>`状态了。那我们刚才输入的内容到哪去了？答案是存放到`name`变量里了。可以直接输入`name`查看变量内容：

```python
>>> name
'Lsaiah'
```

要打印出`name`变量的内容，除了直接写`name`然后按回车外，还可以用`print()`函数：

```python
>>> print(name)
Lsaiah
```

`input()`可以让你显示一个字符串来提示用户，于是我们把代码改成：

```python
name = input('please enter your name: ')
print('hello,', name)
```

## Python基础

Python使用缩进来组织代码块，请务必遵守约定俗成的习惯，坚持使用4个空格的缩进。在文本编辑器中需要设置把Tab自动转换为4个空格。

```python
# print absolute value of an integer:
a = 100
if a >= 0:
    print(a)
else:
    print(-a)
```

当语句以冒号`:`结尾时，缩进的语句视为代码块。使用*4个空格*的缩进，大小写敏感。

### 数据类型和变量

对变量赋值`x = y`是把变量`x`指向真正的对象，该对象是变量`y`所指向的。随后对变量`y`的赋值不影响变量`x`的指向。

注意：Python的整数没有大小限制，而某些语言的整数根据其存储长度是有大小限制的，例如Java对32位整数的范围限制在`-2147483648`-`2147483647`。Python的浮点数也没有大小限制，但是超出一定范围就直接表示为`inf`（无限大）。

#### 数据类型

**整数**

Python可以处理任意大小的整数，当然包括负整数，在程序中的表示方法和数学上的写法一模一样，例如：`1`，`100`，`-8080`，`0`，等等。

计算机由于使用二进制，所以，有时候用十六进制表示整数比较方便，十六进制用`0x`前缀和0-9，a-f表示，例如：`0xff00`，`0xa5b4c3d2`，等等。

对于很大的数，例如`10000000000`，很难数清楚0的个数。Python允许在数字中间以`_`分隔，因此，写成`10_000_000_000`和`10000000000`是完全一样的。十六进制数也可以写成`0xa1b2_c3d4`。

**浮点数**

浮点数也就是小数，之所以称为浮点数，是因为按照科学记数法表示时，一个浮点数的小数点位置是可变的，比如，1.23x109和12.3x108是完全相等的。浮点数可以用数学写法，如`1.23`，`3.14`，`-9.01`，等等。但是对于很大或很小的浮点数，就必须用科学计数法表示，把10用e替代，1.23x109就是`1.23e9`，或者`12.3e8`，0.000012可以写成`1.2e-5`，等等。

整数和浮点数在计算机内部存储的方式是不同的，整数运算永远是精确的（除法难道也是精确的？是的！），而浮点数运算则可能会有四舍五入的误差。

**字符串**

字符串是以单引号`'`或双引号`"`括起来的任意文本，比如`'abc'`，`"xyz"`等等。请注意，`''`或`""`本身只是一种表示方式，不是字符串的一部分，因此，字符串`'abc'`只有`a`，`b`，`c`这3个字符。如果`'`本身也是一个字符，那就可以用`""`括起来，比如`"I'm OK"`包含的字符是`I`，`'`，`m`，空格，`O`，`K`这6个字符。

如果字符串内部既包含`'`又包含`"`怎么办？可以用转义字符`\`来标识，比如：

```python
'I\'m \"OK\"!'
```

表示的字符串内容是：

```python
I'm "OK"!
```

转义字符`\`可以转义很多字符，比如`\n`表示换行，`\t`表示制表符，字符`\`本身也要转义，所以`\\`表示的字符就是`\`，可以在Python的交互式命令行用`print()`打印字符串看看：

```python
>>> print('I\'m ok.')
I'm ok.
>>> print('I\'m learning\nPython.')
I'm learning
Python.
>>> print('\\\n\\')
\
\
```

如果字符串里面有很多字符都需要转义，就需要加很多`\`，为了简化，Python还允许用`r''`表示`''`内部的字符串默认不转义，可以自己试试：

```python
>>> print('\\\t\\')
\       \
>>> print(r'\\\t\\')
\\\t\\
```

如果字符串内部有很多换行，用`\n`写在一行里不好阅读，为了简化，Python允许用`'''...'''`的格式表示多行内容，可以自己试试：

```python
>>> print('''line1
... line2
... line3''')
line1
line2
line3
```

如果写成程序并存为`.py`文件，就是：

```python
print('''line1
line2
line3''')
```

多行字符串`'''...'''`还可以在前面加上`r`使用，不转义。

**布尔值**

布尔值和布尔代数的表示完全一致，一个布尔值只有`True`、`False`两种值，要么是`True`，要么是`False`，在Python中，可以直接用`True`、`False`表示布尔值（请注意大小写）。布尔值可以用`and`、`or`和`not`运算。

`and`运算是与运算，只有所有都为`True`，`and`运算结果才是`True`；

`or`运算是或运算，只要其中有一个为`True`，`or`运算结果就是`True`；

`not`运算是非运算，它是一个单目运算符，把`True`变成`False`，`False`变成`True`。

**空值**

空值是Python里一个特殊的值，用`None`表示。`None`不能理解为`0`，因为`0`是有意义的，而`None`是一个特殊的空值。

此外，Python还提供了列表、字典等多种数据类型，还允许创建自定义数据类型。

#### 变量

变量在程序中就是用一个变量名表示了，变量名必须是大小写英文、数字和`_`的组合，且不能用数字开头，比如：

```python
a = 1
```

变量`a`是一个整数。

```python
t_007 = 'T007'
```

变量`t_007`是一个字符串。

```python
Answer = True
```

变量`Answer`是一个布尔值`True`。

在Python中，等号`=`是赋值语句，可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量。

Python和静态语言Java不同，Java定义变量后不能将变量赋值不同数据类型，而动态语言更灵活。

```python
x = 10
x = x + 2
```

赋值语句和数据等号不一样，如果从数学上理解`x = x + 2`那无论如何是不成立的，在程序中，赋值语句先计算右侧的表达式`x + 2`，得到结果`12`，再赋给变量`x`。由于`x`之前的值是`10`，重新赋值后，`x`的值变成`12`。

```python
a = 'ABC'
```

在计算机内存中，Python解释器创建了一个`'ABC'`的字符串和一个名为`a`的变量，并把它指向`'ABC'`。

也可以把一个变量`a`赋值给另一个变量`b`，这个操作实际上是把变量`b`指向变量`a`所指向的数据。

```python
a = 'ABC'
b = a
a = 'XYZ'
print(b)
```

执行`a = 'ABC'`，解释器创建了字符串`'ABC'`和变量`a`，并把`a`指向`'ABC'`；

执行`b = a`，解释器创建了变量`b`，并把`b`指向`a`指向的字符串`'ABC'`；

执行`a = 'XYZ'`，解释器创建了字符串'XYZ'，并把`a`的指向改为`'XYZ'`，但`b`并没有更改；

最后打印变量`b`的结果自然是`'ABC'`了。

#### 常量

所谓常量就是不能变的变量，比如常用的数学常数π就是一个常量。在Python中，通常用全部大写的变量名表示常量，但事实上`PI`仍然是一个变量。

```python
PI = 3.14159265359
```

在Python中，有两种除法，一种除法是`/`，即使两个数整除结果也是浮点数：

```python
>>> 10 / 3
3.3333333333333335
>>> 9 /3
3.0
```

还有一种除法是`//`，称为地板除，两个整数的除法仍然是整数：

```python
>>> 10 // 3
3
```

`//`除法只取结果的整数部分，所以Python还提供一个余数运算，可以得到两个整数相除的余数：

```python
>>> 10 % 3
1
```

### 字符串和编码

Python 3的字符串使用Unicode，直接支持多语言。

当`str`和`bytes`互相转换时，需要指定编码。最常用的编码是`UTF-8`。Python当然也支持其他编码方式，比如把Unicode编码成`GB2312`：

```python
>>> '中文'.encode('gb2312')
b'\xd6\xd0\xce\xc4'
```

但这种方式纯属自找麻烦，如果没有特殊业务要求，请牢记仅使用`UTF-8`编码。格式化字符串的时候，可以用Python的交互式环境测试，方便快捷。

#### 字符编码

最早只有127个字符被编码到计算机里，也就是大小写英文字母、数字和一些符号，这个编码表被称为`ASCII`编码，比如大写字母`A`的编码是`65`，小写字母`z`的编码是`122`。

但要处理中文显然一个字节是不够的，至少需要两个字节，而且还不能和ASCII编码冲突，所以，中国制定了`GB2312`编码，用来把中文编进去。

全世界有上百种语言，日本把日文编到`Shift_JIS`里，韩国把韩文编到`Euc-kr`里，各国有各国的标准，就会不可避免地出现冲突，但在多语言混合的文本中，显示出来会有乱码。

因此，Unicode字符集应运而生，ASCII编码是1个字节，而Unicode编码通常是2个字节。

字母`A`用ASCII编码是十进制的`65`，二进制的`01000001`；

字符`0`用ASCII编码是十进制的`48`，二进制的`00110000`，注意字符`'0'`和整数`0`是不同的；

汉字`中`已经超出了ASCII编码的范围，用Unicode编码是十进制的`20013`，二进制的`01001110 00101101`。

你可以猜测，如果把ASCII编码的`A`用Unicode编码，只需要在前面补0就可以，因此，`A`的Unicode编码是`00000000 01000001`。

乱码问题解决了但是如果你写的文本基本上全部是英文的话，用Unicode编码比ASCII编码需要多一倍的存储空间，在存储和传输上就十分不划算。

为了节省空间，又出现了把Unicode编码转化为“可变长编码”的`UTF-8`编码。UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间。

| 字符 | ASCII    | Unicode           | UTF-8                      |
| :--- | :------- | :---------------- | :------------------------- |
| A    | 01000001 | 00000000 01000001 | 01000001                   |
| 中   | x        | 01001110 00101101 | 11100100 10111000 10101101 |

从上面的表格还可以发现，UTF-8编码有一个额外的好处，就是ASCII编码实际上可以被看成是UTF-8编码的一部分，所以，大量只支持ASCII编码的历史遗留软件可以在UTF-8编码下继续工作。

在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。

#### Python字符串

在最新的Python 3版本中，字符串是以Unicode编码的，也就是说，Python的字符串支持多语言：

```python
>>> print('包含中文的str')
包含中文的str
```

对于单个字符的编码，Python提供了`ord()`函数获取字符的整数表示，`chr()`函数把编码转换为对应的字符：

```python
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

如果知道字符的整数编码，还可以用十六进制这么写`str`：

```python
>>> '\u4e2d\u6587'
'中文'
```

由于Python的字符串类型是`str`，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把`str`变为以字节为单位的`bytes`。

Python对`bytes`类型的数据用带`b`前缀的单引号或双引号表示：

```python
x = b'ABC'
```

要注意区分`'ABC'`和`b'ABC'`，前者是`str`，后者虽然内容显示得和前者一样，但`bytes`的每个字符都只占用一个字节。

以Unicode表示的`str`通过`encode()`方法可以编码为指定的`bytes`，例如：

```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

纯英文的`str`可以用`ASCII`编码为`bytes`，内容是一样的，含有中文的`str`可以用`UTF-8`编码为`bytes`。含有中文的`str`无法用`ASCII`编码，因为中文编码的范围超过了`ASCII`编码的范围，Python会报错。

在`bytes`中，无法显示为ASCII字符的字节，用`\x##`显示。

反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是`bytes`。要把`bytes`变为`str`，就需要用`decode()`方法：

```python
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

如果`bytes`中包含无法解码的字节，`decode()`方法会报错：

```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8')
Traceback (most recent call last):
  ...
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
```

如果`bytes`中只有一小部分无效的字节，可以传入`errors='ignore'`忽略错误的字节：

```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
'中'
```

要计算`str`包含多少个字符，可以用`len()`函数：

```python
>>> len('ABC')
3
>>> len('中文')
2
```

`len()`函数计算的是`str`的字符数，如果换成`bytes`，`len()`函数就计算字节数：

```python
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```

可见，1个中文字符经过UTF-8编码后通常会占用3个字节，而1个英文字符只占用1个字节。

在操作字符串时，我们经常遇到`str`和`bytes`的互相转换。为了避免乱码问题，应当始终坚持使用UTF-8编码对`str`和`bytes`进行转换。

由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；

第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

申明了UTF-8编码并不意味着你的`.py`文件就是UTF-8编码的，必须并且要确保文本编辑器正在使用UTF-8 without BOM编码。

如果`.py`文件本身使用UTF-8编码，并且也申明了`# -*- coding: utf-8 -*-`，打开命令提示符测试就可以正常显示中文。

#### 格式化

我们经常会输出类似`'亲爱的xxx你好！你xx月的话费是xx，余额是xx'`之类的字符串，而xxx的内容都是根据变量变化的，所以，需要一种简便的格式化字符串的方式。

你可能猜到了，`%`运算符就是用来格式化字符串的。在字符串内部，`%s`表示用字符串替换，`%d`表示用整数替换，有几个`%?`占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个`%?`，括号可以省略。

常见的占位符有：

| 占位符 | 替换内容     |
| :----- | :----------- |
| %d     | 整数         |
| %f     | 浮点数       |
| %s     | 字符串       |
| %x     | 十六进制整数 |

格式化整数和浮点数还可以指定是否补0和整数与小数的位数：

```python
# -*- coding: utf-8 -*-
print('%2d-%02d' % (3, 1))
print('%.2f' % 3.1415926)
```

如果你不太确定应该用什么，`%s`永远起作用，它会把任何数据类型转换为字符串：

```python
>>> 'Age: %s. Gender: %s' % (25, True)
'Age: 25. Gender: True'
```

有些时候，字符串里面的`%`是一个普通字符怎么办？这个时候就需要转义，用`%%`来表示一个`%`：

```python
>>> 'growth rate: %d %%' % 7
'growth rate: 7 %'
```

**format()**

另一种格式化字符串的方法是使用字符串的`format()`方法，它会用传入的参数依次替换字符串内的占位符`{0}`、`{1}`……，不过这种方式写起来比%要麻烦得多：

```python
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```

**f-string**

最后一种格式化字符串的方法是使用以`f`开头的字符串，称之为`f-string`，它和普通字符串不同之处在于，字符串如果包含`{xxx}`，就会以对应的变量替换：

```python
>>> r = 2.5
>>> s = 3.14 * r ** 2
>>> print(f'The area of a circle with radius {r} is {s:.2f}')
The area of a circle with radius 2.5 is 19.62
```

上述代码中，`{r}`被变量`r`的值替换，`{s:.2f}`被变量`s`的值替换，并且`:`后面的`.2f`指定了格式化参数（即保留两位小数），因此，`{s:.2f}`的替换结果是`19.62`。

### 使用list和tuple

#### list可变集合

Python内置的一种数据类型列表list是一种有序的集合，可以随时添加和删除其中的元素。

列出班里所有同学的名字，就可以用一个list表示：

```python
>>> classmates = ['Lsaiah', 'Bob', 'Tracy']
>>> classmates
['Lsaiah', 'Bob', 'Tracy']
```

变量`classmates`就是一个list。用`len()`函数可以获得list元素的个数：

```python
>>> len(classmates)
3
```

用索引来访问list中每一个位置的元素，记得索引是从`0`开始的：

```python
>>> classmates[0]
'Lsaiah'
>>> classmates[1]
'Bob'
>>> classmates[2]
'Tracy'
>>> classmates[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

当索引超出了范围时，Python会报一个`IndexError`错误，所以，要确保索引不要越界，记得最后一个元素的索引是`len(classmates) - 1`。

如果要取最后一个元素，除了计算索引位置外，还可以用`-1`做索引，直接获取最后一个元素：

```python
>>> classmates[-1]
'Tracy'
```

以此类推，可以获取倒数第2个、倒数第3个：

```python
>>> classmates[-2]
'Bob'
>>> classmates[-3]
'Lsaiah'
>>> classmates[-4]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

当然，倒数第4个就越界了。

list是一个**可变**的有序表，所以，可以往list中追加元素到末尾：

```python
>>> classmates.append('Adam')
>>> classmates
['Lsaiah', 'Bob', 'Tracy', 'Adam']
```

也可以把元素插入到指定的位置，比如索引号为`1`的位置：

```python
>>> classmates.insert(1, 'Jack')
>>> classmates
['Lsaiah', 'Jack', 'Bob', 'Tracy', 'Adam']
```

要删除list末尾的元素，用`pop()`方法：

```python
>>> classmates.pop()
'Adam'
>>> classmates
['Lsaiah', 'Jack', 'Bob', 'Tracy']
```

要删除指定位置的元素，用`pop(i)`方法，其中`i`是索引位置：

```python
>>> classmates.pop(1)
'Jack'
>>> classmates
['Lsaiah', 'Bob', 'Tracy']
```

要把某个元素替换成别的元素，可以直接赋值给对应的索引位置：

```python
>>> classmates[1] = 'Sarah'
>>> classmates
['Lsaiah', 'Sarah', 'Tracy']
```

list里面的元素的数据类型也可以不同，比如：

```python
>>> L = ['Apple', 123, True]
```

list元素也可以是另一个list，比如：

```python
>>> s = ['python', 'java', ['asp', 'php'], 'scheme']
>>> len(s)
4
```

要注意`s`只有4个元素，其中`s[2]`又是一个list，如果拆开写就更容易理解了：

```python
>>> p = ['asp', 'php']
>>> s = ['python', 'java', p, 'scheme']
```

要拿到`'php'`可以写`p[1]`或者`s[2][1]`，因此`s`可以看成是一个二维数组，类似的还有三维、四维……数组，不过很少用到。

如果一个list中一个元素也没有，就是一个空的list，它的长度为0：

```python
>>> L = []
>>> len(L)
0
```

#### tuple不可变集合

tuple序列表叫元组，tuple和list非常类似，但是tuple一旦初始化就不能修改。如列出同学的名字：

```python
>>> classmates = ('Lsaiah', 'Bob', 'Tracy')
```

没有append()，insert()这样的方法。其他获取元素的方法和list是一样的，你可以正常地使用`classmates[0]`，`classmates[-1]`，但不能赋值成另外的元素。因为tuple不可变，所以代码更安全。如果可能能用tuple代替list就尽量用tuple。

```python
# 定义时必须确定元素
>>> t = (1, 2)
>>> t
(1, 2)
# 定义一个空的tuple
>>> t = ()
>>> t
()
# 定义一个只有1个元素的tuple
>>> t = (1)
>>> t
1
# 因为括号()既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是1。所以，只有1个元素的tuple定义时必须加一个逗号,，来消除歧义：
>>> t = (1,)
>>> t
(1,)
# Python在显示只有1个元素的tuple时，也会加一个逗号,，以免你误解成数学计算意义上的括号。
# “可变的”tuple，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```



### 条件判断

比如，输入用户年龄，根据年龄打印不同的内容，在Python程序中，用`if`语句实现：

```python
age = 20
if age >= 18:
    print('your age is', age)
    print('adult')
```

根据Python的缩进规则，如果`if`语句判断是`True`，就把缩进的两行print语句执行了，否则，什么也不做。

也可以给`if`添加一个`else`语句，意思是，如果`if`判断是`False`，不要执行`if`的内容，去把`else`执行了：

```python
age = 3
if age >= 18:
    print('your age is', age)
    print('adult')
else:
    print('your age is', age)
    print('teenager')
```

当然上面的判断是很粗略的，完全可以用`elif`做更细致的判断：

```
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```

`elif`是`else if`的缩写，完全可以有多个`elif`，所以`if`语句的完整形式就是：

```python
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```

`if`语句执行有个特点，它是从上往下判断，如果在某个判断上是`True`，把该判断对应的语句执行后，就忽略掉剩下的`elif`和`else`。

`if`判断条件还可以简写，比如写：

```python
if x:
    print('True')
```

只要`x`是非零数值、非空字符串、非空list等，就判断为`True`，否则为`False`。

**从input获取条件**

```python
birth = input('birth: ')
if birth < 2000:
    print('00前')
else:
    print('00后')
```

输入`1982`，结果报错：

```python
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unorderable types: str() > int()
```

这是因为`input()`返回的数据类型是`str`，`str`不能直接和整数比较，必须先把`str`转换成整数。Python提供了`int()`函数来完成这件事情：

```python
s = input('birth: ')
birth = int(s)
if birth < 2000:
    print('00前')
else:
    print('00后')
```

再次运行，就可以得到正确地结果。但是，如果输入`abc`又会得到一个错误信息。因为`int()`函数发现一个字符串并不是合法的数字时就会报错，程序就退出了。

### 循环

`break`语句可以在循环过程中直接退出循环，而`continue`语句可以提前结束本轮循环，并直接开始下一轮循环。这两个语句通常都*必须*配合`if`语句使用。

*要特别注意*，不要滥用`break`和`continue`语句。`break`和`continue`会造成代码执行逻辑分叉过多，容易出错。大多数循环并不需要用到`break`和`continue`语句，上面的两个例子，都可以通过改写循环条件或者修改循环逻辑，去掉`break`和`continue`语句。

有些时候，如果代码写得有问题，会让程序陷入“死循环”，也就是永远循环下去。这时可以用`Ctrl+C`退出程序，或者强制结束Python进程。

Python的循环有两种，一种是for...in循环，依次把list或tuple中的每个元素迭代出来，看例子：

```python
names = ['Lsaiah', 'Bob', 'Tracy']
for name in names:
    print(name)
```

执行这段代码，会依次打印`names`的每一个元素。

所以`for x in ...`循环就是把每个元素代入变量`x`，然后执行缩进块的语句。

再比如我们想计算1-10的整数之和，可以用一个`sum`变量做累加：

```python
sum = 0
for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    sum = sum + x
print(sum)
```

如果要计算1-100的整数之和，从1写到100有点困难，幸好Python提供一个`range()`函数，可以生成一个整数序列，再通过`list()`函数可以转换为list。比如`range(5)`生成的序列是从0开始小于5的整数：

```python
>>> list(range(5))
[0, 1, 2, 3, 4]
>>>
sum = 0
for x in range(101):
    sum = sum + x
print(sum)
```

第二种循环是while循环，只要条件满足，就不断循环，条件不满足时退出循环。比如我们要计算100以内所有奇数之和，可以用while循环实现：

```python
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```

在循环内部变量`n`不断自减，直到变为`-1`时，不再满足while条件，循环退出。

**练习**

利用循环依次对list中的每个名字打印出Hello, xxx!

```python
# -*- coding: utf-8 -*-
L = ['Bart','Lisa','Adam']
for i in L:
    print('Hello, %s!' %i)
```

**break**

如果要提前结束循环，可以用`break`语句：

```python
n = 1
while n <= 100:
    if n > 10: # 当n = 11时，条件满足，执行break语句
        break # break语句会结束当前循环
    print(n)
    n = n + 1
print('END')
```

执行上面的代码可以看到，打印出1~10后，紧接着打印`END`，程序结束。

**continue**

在循环过程中，也可以通过`continue`语句，跳过当前的这次循环，直接开始下一次循环。

```python
n = 0
while n < 10:
    n = n + 1
    print(n)
```

上面的程序可以打印出1～10。但是，如果我们想只打印奇数，可以用`continue`语句跳过某些循环：

```python
n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue语句
        continue # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)
```

执行上面的代码可以看到，打印的不再是1～10，而是1，3，5，7，9。

### 使用dict和set

#### dict

Python内置了字典dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。

假设要根据同学名字查找对应的成绩，如果用list实现，需要两个list：

```python
names = ['Lsaiah', 'Bob', 'Tracy']
scores = [95, 75, 85]
```

给定一个名字，要查找对应的成绩，就先要在names中找到对应的位置，再从scores取出对应的成绩，list越长，耗时越长。

如果用dict实现，只需要一个{'名字' : 成绩}对照表。直接根据名字查找成绩，无论这个表有多大，查找速度都不会变慢。

```python
>>> d = {'Lsaiah': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Lsaiah']
95
```

还可以通过key把数据放入dict：

```python
>>> d['Adam'] = 67
>>> d['Adam']
67
```

由于一个key只能对应一个value，所以对一个key放入value，后面的值会把前面的值冲掉，如果key不存在，dict报错：

```python
>>> d['Jack'] = 90
>>> d['Jack']
90
>>> d['Jack'] = 88
>>> d['Jack']
88
>>> d['Thomas']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'Thomas'
```

要避免key不存在的错误，一是通过`in`判断key是否存在：

```python
>>> 'Thomas' in d
False
```

二是通过dict提供的`get()`方法，如果key不存在返回`None`，或者返回指定的value：

注意：返回`None`的时候交互环境不显示结果。

```python
>>> d.get('Thomas')
>>> d.get('Thomas', -1)
-1
```

要删除一个key，用`pop(key)`方法，对应的value也会从dict中删除：

```python
>>> d.pop('Bob')
75
>>> d
{'Lsaiah': 95, 'Tracy': 85}
```

list（时间换空间）：

1. 查找和插入的速度极快，不会随着key的增加而变慢；
2. 需要占用大量的内存，内存浪费多。

dict（空间换时间）：

1. 查找和插入的时间随着元素的增加而增加；
2. 占用空间小，浪费内存很少。

因为dict根据key来计算value的存储位置称为Hash哈希算法，作为key的对象必须是不可变对象。在Python中字符串，整数都是不可变的，而list是可变的。

```python
>>> key = [1, 2, 3]
>>> d[key] = 'a list'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

#### set

set和dict类似，也是一组key的集合，但不存储value。

要创建一个set，需要提供一个list [1,2,3] 作为输入集合，返回元素不一定有序：

```python
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
```

重复的元素在set中自动被过滤：

```python
>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
{1, 2, 3}
```

通过`add(key)`方法可以添加元素到set中，可以重复添加，但不会有效果：

```python
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}
```

通过`remove(key)`方法可以删除元素：

```python
>>> s.remove(4)
>>> s
{1, 2, 3}
```

set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：

```python
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
{2, 3}
>>> s1 | s2
{1, 2, 3, 4}
```

对于可变对象，比如list，对list进行操作，list内部的内容是会变化的，比如：

```python
>>> a = ['c', 'b', 'a']
>>> a.sort()
>>> a
['a', 'b', 'c']
```

而对于不可变对象，比如str，对str进行操作虽然字符串有个`replace()`方法，也确实变出了`'Abc'`，但变量`a`最后仍是`'abc'`。：

```python
>>> a = 'abc'
>>> a.replace('a', 'A')
'Abc'
>>> a
'abc'
```

`a`本身是一个变量，它指向的对象的内容才是`'abc'`，调用`a.replace('a', 'A')`时，实际上调用方法`replace`是作用在字符串对象`'abc'`上的，而这个方法虽然名字叫`replace`，但却没有改变字符串`'abc'`的内容。相反，`replace`方法创建了一个新字符串`'Abc'`并返回，如果我们用变量`b`指向该新字符串，变量`a`仍指向原有的字符串`'abc'`。

```python
>>> a = 'abc'
>>> b = a.replace('a', 'A')
>>> b
'Abc'
>>> a
'abc'
```

所以对于不变对象来说，调用对象自身的任意方法不会改变该对象自身的内容。相反这些方法会创建新的对象并返回，这样就保证了不可变对象本身永远是不可变的。

## 函数

如圆的面积计算公式为：S=πr²

有了函数，我们就不再每次写`s = 3.14 * x * x`，而是写成更有意义的函数调用`s = area_of_circle(x)`，而函数`area_of_circle`本身只需要写一次，就可以多次调用。Python不但能非常灵活地定义函数，而且本身内置了很多有用的函数可以直接调用。

函数就是最基本的一种代码抽象的方式，借助抽象才能不关心底层的具体计算过程，而直接在更高的层次上思考问题。

### 调用函数

要调用一个函数，需要知道函数的名称和参数，比如求绝对值的函数`abs`，只有一个参数。可以直接从Python的官方网站查看文档：[](http://docs.python.org/3/library/functions.html#abs)。也可以在交互式命令行通过`help(abs)`查看`abs`函数的帮助信息。

```python
# 调用abs函数
>>> abs(-20)
20
# 如果传入的参数数量不对，会报TypeError的错误
>>> abs(1, 2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: abs() takes exactly one argument (2 given)
# 如果传入的参数数量是对的，但参数类型不能被函数所接受，也会报TypeError的错误
>>> abs('a')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: bad operand type for abs(): 'str'
# 函数max()可以接收任意多个参数，并返回最大的。
>>> max(2, 3, 1, -5)
3
```

#### 数据类型转换

Python内置的常用函数还包括数据类型转换函数，比如`int()`函数可以把其他数据类型转换为整数：

```python
>>> int('123')
123
>>> int(12.34)
12
>>> float('12.34')
12.34
>>> str(1.23)
'1.23'
>>> str(100)
'100'
>>> bool(1)
True
>>> bool('')
False
```

函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个别名：

```python
>>> a = abs # 变量a指向abs函数
>>> a(-1) # 所以也可以通过a调用abs函数
1
```

### 定义函数

定义一个函数要使用`def`语句，依次写出函数名、括号、括号中的参数和冒号`:`，然后，在缩进块中编写函数体，函数的返回值用`return`语句返回。

定义一个求绝对值的`my_abs`函数为例：

```python
# -*- coding: utf-8 -*-
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
print(my_abs(-99))
```

如果没有`return`语句，函数执行完毕后也会返回结果，只是结果为`None`。`return None`可以简写为`return`。

把`my_abs()`的函数定义保存到`abstest.py`文件，可以在该文件当前目录启动解释器，用`from abstest import my_abs`来导入`my_abs()`函数。

#### 空函数

定义一个什么事也不做的空函数，可以用`pass`语句：

```python
def nop():
    pass
```

实际上`pass`可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个`pass`，让代码能运行起来，缺少了`pass`，代码运行就会有语法错误。

`pass`还可以用在其他语句里，比如：

```python
if age >= 18:
    pass
```

#### 参数检查

调用函数时，如果参数个数不对，Python解释器会自动检查出来，并抛出`TypeError`。但是如果参数类型不对，Python解释器就无法帮我们检查。试试`my_abs`和内置函数`abs`的差别：

```python
>>> my_abs('A')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in my_abs
TypeError: unorderable types: str() >= int()
>>> abs('A')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: bad operand type for abs(): 'str'
```

修改`my_abs`的定义，对参数类型做检查，只允许整数和浮点数类型的参数。数据类型检查可以用内置函数`isinstance()`实现：

```python
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```

添加了参数类型检查后，传入错误的参数就会抛出错误：

```python
>>> my_abs('A')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in my_abs
TypeError: bad operand type
```

#### 多个返回值

比如在游戏中需要从一个点移动到另一个点，给出坐标、位移和角度，就可以计算出新的坐标：

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny
```

```python
>>> x, y = move(100, 100, 60, math.pi / 6)
>>> print(x, y)
151.96152422706632 70.0
# Python函数返回的仍然是单一值
>>> r = move(100, 100, 60, math.pi / 6)
>>> print(r)
(151.96152422706632, 70.0)
```

返回值是一个tuple不可变集合，可以省略括号。

**练习**

请定义一个函数`quadratic(a, b, c)`，接收3个参数，返回

ax²+bx+c=0一元二次方程的两个解。一元二次方程的求根公式：

$$x=\frac {-b±\sqrt{b^2-4ac}}{2a}$$

```python
# -*- coding: utf-8 -*-
import math

def quadratic(a, b, c):
    if not isinstance(a,(int, float)):
        raise TypeError('bad operand Type of a')
    elif not isinstance(b,(int, float)):
        raise TypeError('bad operand Type of b')
    elif not isinstance(c,(int, float)):
        raise TypeError('bad operand Type of c')
    else:
        d = b**2-4*a*c
        if d < 0:
            return "no solution"
        elif d == 0:
            return "d=0根相等"
        else:
            sqrt = math.sqrt(d)
            x1 = (-b + sqrt)/(2*a)
            x2 = (-b - sqrt)/(2*a)
            return(x1,x2)
# 测试:
print('quadratic(2, 3, 1) =', quadratic(2, 3, 1))
print('quadratic(1, 3, -4) =', quadratic(1, 3, -4))

if quadratic(2, 3, 1) != (-0.5, -1.0):
    print('测试失败')
elif quadratic(1, 3, -4) != (1.0, -4.0):
    print('测试失败')
else:
    print('测试成功')
```

### 函数的参数

#### 位置参数

计算x²的函数：

```python
def power(x):
    return x * x
```

对于`power(x)`函数，参数`x`就是一个位置参数。

当我们调用`power`函数时，必须传入有且仅有的一个参数`x`：

```python
>>> power(5)
25
```

但是如果要计算$x^n$，改为`power(x, n)`：

```python
def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
```

这两个参数都是位置参数，调用函数时，传入的两个值按照位置顺序依次赋给参数`x`和`n`。

#### 默认参数

新的函数有两个参数，旧的调用报错：

```python
>>> power(5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: power() missing 1 required positional argument: 'n'
```

由于经常计算 $x^2$，所以把第二个参数n默认值设定成2：

```python
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
# 默认n=2计算
>>> power(5)
25
```

注意必选参数在前，默认参数在后，否则Python的解释器会报错；

当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数。使用默认参数能降低调用函数的难度。

如学生注册函数，传入`name`和`gender`两个参数：

```python
def enroll(name, gender):
    print('name:', name)
    print('gender:', gender)
# 调用只需传2个参数
>>> enroll('Sarah', 'F')
name: Sarah
gender: F
```

把年龄和城市设为默认参数：

```python
def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)
```

这样，大多数学生注册时不需要提供年龄和城市。只有与默认参数不符的学生才需要提供额外的信息：

```python
# 按顺序提供参数
enroll('Bob', 'M', 7)
# 不按顺序提供参数需要写参数名
enroll('Adam', 'M', city='Tianjin')
```

> [!WARNING]
>
> 默认参数必须指向不变对象，可变对象调用后再次调用会保留之前的结果。

如果定义一个函数传入可变对象list，添加一个`END`再返回：

```python
def add_end(L=[]):
    L.append('END')
    return L
```

```python
# 正常调用
>>> add_end([1, 2, 3])
[1, 2, 3, 'END']
# 使用默认参数调用
>>> add_end()
['END']
# 再次使用默认参数调用
>>> add_end()
['END', 'END']
```

Python函数在定义的时候，默认参数`L`的值就被计算出来了，即`[]`，因为默认参数`L`也是一个变量，它指向对象`[]`，每次调用该函数，如果改变了`L`的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的`[]`了。

用`None`不可变对象修改上面的例子：

```python
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```

这样无论调用多少次都不会保留上一次的值。使用不变对象`str`、`None`内部数据不能修改，减少了由于修改数据导致的错误。此外多任务环境下同时读取对象不需要加锁。

#### 可变参数

可变参数就是传入的参数个数是可变的，给定一组数字a，b，c……，请计算a2 + b2 + c2 + ……。

要定义函数，必须确定输入的参数，但参数的个数不确定，可以吧它们作为一个list或tuple传进来：

```python
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

调用时需要先组装出一个list或tuple：

```python
>>> calc([1, 2, 3])
14
>>> calc((1, 3, 5, 7))
84
```

把函数的参数改为可变参数：

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

可变参数调用函数简化：

```python
>>> calc(1, 2, 3)
14
>>> calc(1, 3, 5, 7)
84
```

参数`*numbers`收到tuple，调用时可以传入任意个参数：

```python
>>> calca(1,2)
5
>>> calc()
0
```

如果已经有一个list或者tuple时，调用一个可变参数，把list或tuple的元素变成可变参数传进去：

```python
>>> nums = [1, 2, 3]
>>> calc(*nums)
14
```

#### 关键字参数

关键字参数允许传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
```

除了必选参数外还接受关键字参数`kw`。在调用时可以只传必选参数。

```python
>>> person('Lsaiah', 30)
name: Lsaiah age: 30 other: {}
```

也可传入任意个数的关键字参数：

```python
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

关键字参数可以扩展函数的功能。和可变参数类似，也可以先组装出一个dict，然后把这个dict转换为关键字参数传进去：

```python
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, city=extra['city'], job=extra['job'])
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

上面复杂的调用可以简化写法：

```python
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

`**extra`表示把`extra`这个dict的所有key-value用关键字参数传入到函数的`**kw`参数，`kw`将获得一个dict，注意`kw`获得的dict是`extra`的一份拷贝，对`kw`的改动不会影响到函数外的`extra`。

#### 命名关键字参数

对于关键字参数，调用者可以传入不受限制的关键字参数，可以在函数内部通过`kw`检查：

```python
def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数
        pass
    if 'job' in kw:
        # 有job参数
        pass
    print('name:', name, 'age:', age, 'other:', kw)
```

但仍然可以传入不受限制的关键字参数：

```python
>>> person('Jack', 24, city='Beijing', addr='Chaoyang', zipcode=123456)
```

如果限制参数就可以使用命名关键字参数，例如只接收`city`和`job`作为关键字参数。`*`分隔符后面的参数被视为命名关键字参数。

```python
def person(name, age, *, city, job):
	print(name, age, city, job)
```

如果函数定义中已经有了一个可变参数，后面跟的命名关键字参数就不需要分隔符了：

```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```

命名关键字参数必须传入参数名，这和位置参数不同，不传报错：

```python
>>> person('Jack', 24, 'Beijing', 'Engineer')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: person() takes 2 positional arguments but 4 were given
```

因为person()只接收2个位置参数，实际传入4个位置参数。

命名关键字参数可以有缺省值，简化调用：

```python
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
```

由于`city`有默认值，调用时可不用传入`city`参数：

```python
>>> person('Jack', 24, job='Engineer')
Jack 24 Beijing Engineer
```

#### 参数组合

在Python中定义函数，可以按顺序必选参数、默认参数、可变参数、命名关键字参数和关键字参数组合使用。

比如一个函数包含若干种参数：

```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```

在调用的时候，Python解释器自动按照参数位置和参数名把对应的参数传进去。

```python
>>> f1(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}
>>> f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
>>> f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
>>> f1(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
>>> f2(1, 2, d=99, ext=None)
a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}
```

另外也可以通过一个tuple和一个dict调用上面的函数：

```python
>>> args = (1, 2, 3, 4)
>>> kw = {'d': 99, 'x': '#'}
>>> f1(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
>>> args = (1, 2, 3)
>>> kw = {'d': 88, 'x': '#'}
>>> f2(*args, **kw)
a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
```

对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。

**练习**

可接收一个或多个数计算两个数乘积：

```python
# -*- coding:utf-8 -*-
def product(x,*numbers):
    multi = x
    for y in numbers:
        multi = multi * y
    return multi
# 测试
print('product(5) =', product(5))
print('product(5, 6) =', product(5, 6))
print('product(5, 6, 7) =', product(5, 6, 7))
if product(5) != 5:
    print('测试失败!')
elif product(5, 6) != 30:
    print('测试失败!')
elif product(5, 6, 7) != 210:
    print('测试失败!')
else:
    try:
        product()
        print('测试失败!')
    except TypeError:
        print('测试成功!')
```

**小结**

默认参数一定要用不可变对象，如果是可变对象，程序运行时会有逻辑错误，保留上次的结果。

要注意定义可变参数和关键字参数的语法：

`*args`是可变参数，args接收的是一个tuple；

`**kw`是关键字参数，kw接收的是一个dict。

以及调用函数时如何传入可变参数和关键字参数的语法：

可变参数既可以直接传入：`func(1, 2, 3)`，又可以先组装list或tuple，再通过`*args`传入：`func(*(1, 2, 3))`；

关键字参数既可以直接传入：`func(a=1, b=2)`，又可以先组装dict，再通过`**kw`传入：`func(**{'a': 1, 'b': 2})`。

使用`*args`和`**kw`是Python的习惯写法，当然也可以用其他参数名，但最好使用习惯用法。

命名的关键字参数是为了限制调用者可以传入的参数名，同时可以提供默认值。

定义命名的关键字参数在没有可变参数的情况下不要忘了写分隔符`*`，否则定义的将是位置参数。

### 递归函数

如果一个函数在内部调用自身本身，这个函数就是递归函数。

例如：计算阶乘`n! = 1 x 2 x 3 x ... x n`，用函数`fact(n)`表示，可以看出：
$$
fact(n) = n! = 1×2×3× ... × (n-1)×n = (n-1)! × n = fact(n-1) × n
$$
只有当n=1时需要特出处理。

用递归函数写：

```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n-1)
```

```python
>>> fact(1)
1
>>> fact(5)
120
```

理论上所有的递归都可以写成循环，但递归定义简单，逻辑清晰。

函数的调用是通过栈实现的，每当进入一个函数的调用，栈会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以递归调用次数过多会造成栈溢出。如`fact(1000)`：

```python
>>> fact(1000)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 4, in fact
  ...
  File "<stdin>", line 4, in fact
RuntimeError: maximum recursion depth exceeded in comparison
```

解决栈溢出的方法是通过尾递归优化，和循环效果是一样的。在函数返回时调用自身本身，并且return语句不能包含表达式。这样无论调用多少次都只占用一个栈帧，不会造成栈溢出。

把`fact(n)`改成尾递归方式，把每一步的乘积传入到递归函数中：

```python
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
```

函数`fact_iter`只返回函数本身，`num - 1`和`num * product`在函数`fact`调用函数`fact_iter`之前就会被计算。

```python
===> fact_iter(5, 1)
===> fact_iter(4, 5)
===> fact_iter(3, 20)
===> fact_iter(2, 60)
===> fact_iter(1, 120)
===> 120
```

但是Python解释器没有对尾递归做优化，所以上面的尾递归函数也会导致栈溢出。

**练习**

汉诺塔游戏，编写`move(n, a, b, c)`函数，它接收参数`n`，表示3个柱子A、B、C中第1个柱子A的盘子数量，然后打印出把所有盘子从A借助B移动到C的方法。

```python
# -*- coding: utf-8 -*-
def move(n, a, b, c):
    if n == 1:
        print('move', a, '-->', c)
    else:
        move(n-1, a, c, b)
        move(1, a, b, c)
        move(n-1, b, a, c)
        
# 期待输出:
# A --> C
# A --> B
# C --> B
# A --> C
# B --> A
# B --> C
# A --> C
```



## 高级特性

在Python中，代码越少越简单开发效率越高。

### 切片

取一个list或tuple的部分元素非常常见，如取list的前3个元素：

```python
>>> L = ['Lsaiah', 'Sarah', 'Tracy', 'Bob', 'Jack']
>>> [L[0], L[1], L[2]]
```

取list的前n个元素，索引为0 - (N-1)，可以用循环：

```python
>>> r = []
>>> n = 3
>>> for i in range(n):
...     r.append(L[i])
... 
>>> r
```

指定索引范围循环复杂，因此python提供了切片（Slice）操作符，简化操作。如取前3个元素：

```python
>>> L[0:3]
['Lsaiah', 'Sarah', 'Tracy']
```

`L[0:3]`表示，从索引`0`开始取，直到索引`3`为止，但不包括索引`3`。即索引`0`，`1`，`2`。如果第一个索引是`0`还可以省略。支持`L[-1]`取倒数第一个元素。

先创建一个0-99的数列：

```python
>>> L = list(range(1000))
>>> L
[0,1,2,3,...,99]
```

```python
# 取前10个：
>>> L[:10]
# 取后10个：
>>> L[-10:]
# 前11-20个
>>> L[10:20]
[10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
# 前10个数每2个取1个：
>>> L[:10:2]
[0, 2, 4, 6, 8]
# 所有数每5个取1个：
>>> L[::5]
[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]
# 复制原list
>>> L[:]
```

tuple不可变集合也可以用切片操作：

```python
>>> (0, 1, 2, 3, 4, 5)[:3]
(0, 1, 2)
```

字符串也可以看成一种list，每个元素就是一个字符，因此也可以用切片操作：

```python
>>> 'ABCDEFG'[:3]
'ABC'
```

**练习**

利用切片操作实现函数trim()，去除字符串首尾的空格，不要调用str的strip()方法。

```python
# -*- coding: utf-8 -*-
def trim(s):
    while s = None:
        return '字符串空'
    while ' ' == s[:1]:
        s = s[1:]
    while ' ' == s[-1:]:
        s = s[:-1]
    return s
# 测试:
if trim('hello  ') != 'hello':
    print('测试失败!')
elif trim('  hello') != 'hello':
    print('测试失败!')
elif trim('  hello  ') != 'hello':
    print('测试失败!')
elif trim('  hello  world  ') != 'hello  world':
    print('测试失败!')
elif trim('') != '':
    print('测试失败!')
elif trim('    ') != '':
    print('测试失败!')
else:
    print('测试成功!')
```



### 迭代

通过`for`循环遍历list或tuple称为迭代（iteration），在Python中迭代是通过`for ... in`来完成的。而很多语言迭代是通过下标完成的。所以python的`for`循环还可以作用在其他可迭代对象上，如迭代dict。

因为dict的存储不是按照list的方式顺序排列，所以迭代出的结果很可能不一样。默认dict迭代key，迭代value可以用`for value in d.values()`，如果同时迭代key和value可以用`for k, v in d.items()`。

字符串也是可迭代对象：

```python
>>> for ch in 'ABC':
		print(ch)
A
B
C
```

所以在使用`for`循环时，不用关心对象的数据类型，而是判断对象是否可迭代。通过collections模块的Iterable类型判断：

```python
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```

在Java中可以实现对list迭代下标索引，而在Python中内置的`enumerate`函数可以把一个list变成索引-元素对。

```
>>> for i, value in enumerate(['A'], ['B'], ['C'])
...     print(i, value)
...
0 A
1 B
2 C
```

**练习**

迭代查找一个list中最大和最小值，返回一个tuple：

```python
# -*- coding: utf-8 -*-
def findMinAndMax(L):
    if L == []:
        return (Nonoe, None)
    else:
        Min = Max = L[0]
        for i in L:
            if i <= Min:
                Min = i
            elif i >= Max:
                Max = i
    return(Min,Max)
# 测试
if findMinAndMax([]) != (None, None):
    print('测试失败!')
elif findMinAndMax([7]) != (7, 7):
    print('测试失败!')
elif findMinAndMax([7, 1]) != (1, 7):
    print('测试失败!')
elif findMinAndMax([7, 1, 3, 9, 5]) != (1, 9):
    print('测试失败!')
else:
    print('测试成功!')
```



### 列表生成式

列表生成式即List Comprehensions，是Python内置的简单却强大创建list生成式。如生成list`[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`可以用`list(range(1, 11))`。

但要生成`[1x1, 2x2, 3x3, ..., 10x10]`，使用循环：

```python
>>> L = []
>>> for x in range(1, 11):
...    L.append(x * x)
```

使用列表生成式：

```python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

把要生成的元素`x * x`放到前面，后面跟`for`循环，就可以把list创建出来，for循环后面还可以加上if判断，这样我们就可以筛选出仅偶数的平方：

```python
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```

还可以使用两层循环，生成全排列。

```python
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

运用列表生成式可写出非常简洁的代码，如列出当前目录下的所有文件和目录名：

```python
>>> import os
>>> [d for d in os.listdir('.')] # 列出文件和目录
['.emacs.d', '.ssh', '.Trash', 'Adlm', 'Applications', 'Desktop', 'Documents', 'Downloads', 'Library', 'Movies', 'Music', 'Pictures', 'Public', 'VirtualBox VMs', 'Workspace', 'XCode']
```

`for`循环可以同时使用两个甚至更多变量，比如`dict`的`items()`可以同时迭代key和value：

```python
>>> d = {'x': 'A', 'y': 'B', 'z': 'C'}
>>> for k,v in d.items():
		print(k, '=', v)
y = B
x = A
z = C
```

列表生成式也可以使用两个变量来生成list：

```python
>>> d = {'x': 'A', 'y': 'B', 'z': 'C'}
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```

把list中所有字符串变成小写：

```python
>>> L = ['Hello', 'World', 'IBM', 'Apple']
>>> [s.lower() for s in L]
```

**if...else**

如正常输出偶数，在列表生成式后加`if`条件筛选

```python
>>> [x for x in range(1, 11) if x % 2 == 0]
[2, 4, 6, 8, 10]
```

但是在后面加上`else`报错`SyntaxError: invalid syntax`

当把`if`写在`for`前面，不加`else`报错。

```python
>>> [x if x % 2 == 0 for x in range(1, 11)]
  File "<stdin>", line 1
    [x if x % 2 == 0 for x in range(1, 11)]
                       ^
SyntaxError: invalid syntax
```

这是因为`for`前面的部分是一个表达式，它必须根据`x`计算出一个结果。因此，考察表达式：`x if x % 2 == 0`，它无法根据`x`计算出结果，因为缺少`else`，必须加上`else`：

```python
>>> [x if x % 2 == 0 else -x for x in range(1, 11)]
[-1, 2, -3, 4, -5, 6, -7, 8, -9, 10]
```

可见，在一个列表生成式中，`for`前面的`if ... else`是表达式，而`for`后面的`if`是过滤条件，不能带`else`。

**练习**

如果list中既包含字符串，又包含整数，由于非字符串类型没有`lower()`方法，所以列表生成式会报错：

```python
>>> L = ['Hello', 'World', 18, 'Apple', None]
>>> [s.lower() for s in L]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 1, in <listcomp>
AttributeError: 'int' object has no attribute 'lower'
```

使用内建的`isinstance`函数可以判断一个变量是不是字符串：

```python
>>> x = 'abc'
>>> y = 123
>>> isinstance(x, str)
True
>>> isinstance(y, str)
False
```

请修改列表生成式，通过添加`if`语句保证列表生成式能正确地执行：

```python
# -*- coding: utf-8 -*-
L1 = ['Hello', 'World', 18, 'Apple', None]
L2 = [s.lower() for s in L1 if isinstance(s,str)]
```

### 生成器

受内存限制，列表生成式创建的列表容量有限，如果创建很多个元素只访问个别元素，占用存储空间大且浪费。所以通过生成器（generator）一边循环一边计算，这样就不必创建完整的list，从而节省大量空间。

第一种方法很简单，只要把一个列表生成式的`[]`改成`()`就创建了一个generator：

```python
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```

可以直接打印出list的每一个元素，但是打印出generator需要通过`next()`函数获取返回值。generator保存的是算法，每次调用`next(g)`，就计算出`g`的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出`StopIteration`的错误。

```python
>>> next(g)
0
>>> next(g)
1
>>> next(g)
4
>>> next(g)
9
>>> next(g)
16
>>> next(g)
25
>>> next(g)
36
>>> next(g)
49
>>> next(g)
64
>>> next(g)
81
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

可以使用`for`循环，因为generator是可迭代对象。

```python
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print(n)
... 
0
1
4
9
16
25
36
49
64
81
```

创建了一个generator后，基本上永远不会调用`next()`，而是通过`for`循环来迭代它，并且不需要关心`StopIteration`的错误。

如果推算比较复杂，用列表生成式无法实现，还可以用函数实现。比如斐波拉契数列（Fibonacci）除第一个和第二个数外，任意一个数都可由前两个数相加得到：1, 1, 2, 3, 5, 8, 13, 21, 34, ...

```python
def fib(max):
    n,a,b = 0,0,1
    while n < max:
        print(b)
        a, b = b, a+b
        n = n + 1
    return 'done'
```

上面的赋值语句a, b = b, a+b相当于：

```python
t = (b, a+b)
a = t[0]
b = t[1]
```

```python
>>> fib(6)
1
1
2
3
5
8
'done'
```

`fib`函数定义了斐波那契数列的推算规则，要把它变成generator，只要把`print(b)`改成`yield(b)`就可以了。

```python
def fib(max):
    n, a ,b = 0,0,1
    while n < amx:
        yield b
        a, b = b, a+b
        n = n+1
    return 'done'
```

函数是顺序执行，遇到`return`语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的`yield`语句处继续执行。

如定义一个generator，依次返回数字1，3，5：

```python
def odd():
    print('step 1')
    yield 1
    print('step 2')
    yield(3)
    print('step')
    yield(5)
```

调用该generator时，先要生成一个generator对象，然后用`next()`函数不断获得下一个返回值。

```python
>>> o = odd()
>>> next(o)
step 1
1
>>> next(o)
step 2
3
>>> next(o)
step 3
5
>>> next(o)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

generator在执行过程中，遇到`yield`就中断，下次又继续执行。执行3次`yield`后，已经没有`yield`可以执行了，所以，第4次调用`next(o)`就报错。

把函数改成generator后不会用`next()`获取下一个返回值，而是使用循环迭代。

```python
>>> for n in fib(6):
...     print(n)
...
1
1
2
3
5
8
```

但是用`for`循环调用generator时，发现拿不到generator的`return`语句的返回值。如果想要拿到返回值，必须捕获`StopIteration`错误，返回值包含在`StopIteration`的`value`中：

```python
>>> g = fib(6)
>>> while True:
...     try:
...         x = next(g)
...         print('g:', x)
...     except StopIteration as e:
...         print('Generator return value:', e.value)
...         break
...
g: 1
g: 1
g: 2
g: 3
g: 5
g: 8
Generator return value: done
```

**练习**

杨辉三角定义如下：

```ascii
          1
         / \
        1   1
       / \ / \
      1   2   1
     / \ / \ / \
    1   3   3   1
   / \ / \ / \ / \
  1   4   6   4   1
 / \ / \ / \ / \ / \
1   5   10  10  5   1
```

把每一行看做一个list，写一个generator，不断输出下一行list：

```python
# -*- coding: utf-8 -*-
def triangles():
    t = [1]
    while True:
        yield t
        s = [0] + t
        t = t + [0]
        for i in range(len(s)):
            t[i] = s[i] + t[i]
n = 0
results = []
for t in triangles():
    results.append(t)
    n = n + 1
    if n == 10:
        break
for t in results:
    print(t)

if results == [
    [1],
    [1, 1],
    [1, 2, 1],
    [1, 3, 3, 1],
    [1, 4, 6, 4, 1],
    [1, 5, 10, 10, 5, 1],
    [1, 6, 15, 20, 15, 6, 1],
    [1, 7, 21, 35, 35, 21, 7, 1],
    [1, 8, 28, 56, 70, 56, 28, 8, 1],
    [1, 9, 36, 84, 126, 126, 84, 36, 9, 1]
]:
    print('测试通过!')
else:
    print('测试失败!')
```

把第i组列表分表前后+[0]，这样会形成2个长度为i+1的列表，这两个列表对应的元素相加即可。例如[1, 2, 1]前后分别+[0]为[0, 1, 2, 1]和[1, 2, 1, 0]，这两个列表相加即为[1, 3, 3, 1]。

### 迭代器

可以直接作用于`for`循环的数据类型：

一类是集合数据类型：如`list`、`tuple`、`dict`、`set`、`str`等；

一类是`generator`，包括生成器和带`yield`的generator function.

这些称为可迭代对象`Iterable`，可使用`isinstance()`判断一个对象是否是`Iterable`对象：

```python
>>> from collections.abc import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False
```

而生成器不但可以作用于`for`循环，还可以被`next()`函数不断调用返回值，直到抛出`StopIteration`错误表示无法继续返回下一个值。可以被`next()`函数调用不断返回下一个值的对象称为迭代器：`Iterator`，可使用`isinstance()`判断一个对象是否是`Iterator`对象。

```python
>>> from collections.abc import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance('abc', Iterator)
False
```

生成器都是`Iterator`对象，但是`list`、`dict`、`str`虽然是`Iterable`，却不是`Iterator`。把`list`、`dict`、`str`等`Iterable`变成`Iterator`可以使用`iter()`函数：

```python
>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True
```

因为Python的`Iterator`对象表示的是一个数据流，Iterator对象可以被`next()`函数调用并不断返回下一个数据，直到没有数据时抛出`StopIteration`错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过`next()`函数实现按需计算下一个数据，所以`Iterator`的计算是惰性的，只有在需要返回下一个数据时它才会计算。`Iterator`甚至可以表示一个无限大的数据流，例如全体自然数。而使用list是永远不可能存储全体自然数的。

Python的`for`循环本质上就是通过不断调用`next()`函数实现的，例如：

```python
for x in [1, 2, 3, 4, 5]:
    pass
```

实际上完全等价于：

```python
# 首先获得Iterator对象:
it = iter([1, 2, 3, 4, 5])
# 循环:
while True:
    try:
        # 获得下一个值:
        x = next(it)
    except StopIteration:
        # 遇到StopIteration就退出循环
        break
```

## 函数式编程

### 高阶函数

**变量可以指向函数**

```python
>>> abs(-10)
10
>>> abs
<built-in function abs>
>>> x = abs(-10)
>>> x
10
>>> f = abs
>>> f
<built-in function abs>
>>> f(-10)
10
```

说明变量`f`现在已经指向了`abs`函数本身。直接调用`abs()`函数和调用变量`f()`完全相同。

**函数名也是变量**

```python
>>> abs = 10
>>> abs(-10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not callable
```

把`abs`指向`10`后，就无法通过`abs(-10)`调用该函数了。因为`abs`这个变量已经不指向求绝对值函数而是指向一个整数`10`。说明函数名也是变量。

注：由于`abs`函数实际上是定义在`import builtins`模块中的，所以要让修改`abs`变量的指向在其它模块也生效，要用`import builtins; builtins.abs = 10`。

**传入函数**

既然变量可以指向函数，函数的参数能接收变量，那么一个函数可以作为另一个函数的参数。

```python
def add(x,y,f):
    return f(x) + f(y)
```

当我们调用`add(-5, 6, abs)`时，参数`x`，`y`和`f`分别接收`-5`，`6`和`abs`，根据函数定义，我们可以推导计算过程为：

```python
x = -5
y = 6
f = abs
f(x) + f(y) ==> abs(-5) + abs(6) ==> 11
return 11
```

#### map/reduce

`map()`函数接收两个参数，一个是函数，一个是`Iterable`，将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回。比如函数f(x)=x2作用在list`[1, 2, 3, 4, 5, 6, 7, 8, 9]`上，用`map()`实现：

```python
>>> def f(x):
    	return x * x
>>> r = map(f,[1,2,3,4,5,6,7,8,9])
# 第一个参数f是函数本身，第二个是Iterable，结果r是Iterator，惰性序列
>>> list(r)
# 通过list函数把整个惰性序列计算出来
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

也可通过循环实现：

```python
L = []
for n in [1,2,3,4,5,6,7,8,9]:
    L.append(f(n))
print(L)
```

`map()`作为高阶函数把运算规则抽象了，因此还可以计算复杂的函数，如把list所有数字转为字符串：

```python
>>> list(map(str, [1,2,3,4,5,6,7,8,9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

`reduce`把一个函数作用在一个序列[x1, x2, x3, ...]上，必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算，其效果就是：

```python
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1,x2), x3), x4)
```

比如对一个序列求和就可以用`reduce`实现：

```python
>>> from functools import reduce
>>> def add(x, y):
    	return x + y
>>> reduce(add, [1,3,5,7,9])
25
```

求和也可以直接用Python内置函数`sum()`。

把`str`转换为`int`函数：

```python
from functools import reduce
DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
def str2int(s):
    def fn(x,y):
        return x * 10 + y
    def char2num(s):
        return DIGITS(s)
    return reduce(fn, map(char2num, s))
```

可以用lambda函数进一步简化：

```python
from functools import reduce

DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def char2num(s):
    return DIGITS[s]

def str2int(s):
    return reduce(lambda x, y: x * 10 + y, map(char2num, s))
```

不使用python提供的`int()`函数，完全可以自己写一个字符串转证书的函数。

**练习**

利用`map()`函数把用户输入不规范的英文名变为首字母大写，其他小写。输入`['adam', 'LISA', 'barT']`，输出：`['Adam', 'Lisa', 'Bart']`：

```python
# -*- coding: utf-8 -*-
def normalize(name):
    return name.capitalize()
L1 = ['adam', 'LISA', 'barT']
L2 = list(map(normalize, L1))
print(L2)
```

请编写一个`prod()`函数，可以接受一个list并利用`reduce()`求积：

```python
# -*- coding: utf-8 -*-
from functools import reduce
def prod(L):
    return reduce(lambda x,y: x * y, L)
print('3 * 5 * 7 * 9 =', prod([3, 5, 7, 9]))
if prod([3, 5, 7, 9]) == 945:
    print('测试成功!')
else:
    print('测试失败!')
```

利用`map()`和`reduce()`写一个`str2float`的函数，把字符串`'123.456'`转换成浮点数`123.456`。

```python
# -*- coding: utf-8 -*-
from functools import reduce
def str2float(s)
	d = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
    s1,s2 = s.split('.')
    s = s.replace('.','')
    inte = reduce(lambda x, y: 10*x+y, map(lambda x: d[x], s))
    return inte/(10**len(s2))
```

#### filter

`filter()`函数用于过滤序列，接收一个函数和一个序列，与`map()`不同的是把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。

在一个list中删掉偶数，保留奇数：

```python
def is_odd(n):
    return n % 2 == 1
list(filter(is_odd, [1,2,4,5,6,9,10,15]))
# 结果：[1,5,9,15]
```

把一个序列中的空字符串删掉：

```python
def not_empty(s):
    return s and s.strip()
list(filter(not_empty, ['A','','B',None,'C',' ']))
# 结果：['A','B','C']
```

注意刀`filter()`函数返回一个`Iterator`惰性序列，所以要强迫`filter()`完成计算结果，需要用`list()`函数获得所有结果并返回。

**用filter求素数**

又叫质数大于等于2，不能被它本身和1以外的数整除。

计算素数的一个方法是埃氏筛法：

首先，列出从2开始的所有自然数构造一个序列：

2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...

取序列的第一个数2，一定是素数，然后用2把序列的2的倍数筛掉：

3, ~~4~~, 5, ~~6~~, 7, ~~8~~, 9, ~~10~~, 11, ~~12~~, 13, ~~14~~, 15, ~~16~~, 17, ~~18~~, 19, ~~20~~, ...

取新序列第一个数3，一定是素数，然后用3把序列3的倍数筛掉：

5, ~~6~~, 7, ~~8~~, ~~9~~, ~~10~~, 11, ~~12~~, 13, ~~14~~, ~~15~~, ~~16~~, 17, ~~18~~, 19, ~~20~~, ...

取新序列第一个数5，一定是素数，然后用5把序列5的倍数筛掉：

7, ~~8~~, ~~9~~, ~~10~~, 11, ~~12~~, 13, ~~14~~, ~~15~~, ~~16~~, 17, ~~18~~, 19, ~~20~~, ...

不断筛选就可以得到所有素数。

用Python实现，先构造一个从3开始的奇数序列：

```python
def _odd_iter():
    n = 1
    while True:
        n = n+2
        yield n
```

合适一个无限序列生成器。然后定义筛选函数：

```python
def _not_divisible(n):
    return lambda x: x % n > 0
```

最后定义一个生成器不断返回下一个素数：

```python
def primes():
    yield 2
    it = _odd_iter() # 初始序列
    while True:
        n = next(it) # 返回序列的第一个数
        yield n
        it = filter(_not_divisible(n), it) # 构造新序列
```

这个生成器先返回第一个素数`2`，然后，利用`filter()`不断产生筛选后的新的序列。

由于`primes()`也是一个无限序列，所以调用时需要设置一个退出循环的条件：

```python
# 打印1000以内的素数:
for n in primes():
    if n < 1000:
        print(n)
    else:
        break
```

注意到`Iterator`是惰性计算的序列，所以我们可以用Python表示“全体自然数”，“全体素数”这样的序列，而代码非常简洁。

**练习**

回数是指从左向右和从右向左读都一样的数，如`12321`，用`filter()`筛选出回数。

```python
# -*- coding: utf-8 -*-
def is_palindrome(n):
    nv = list(map(int, str(n)))
    nv1 = list(map(int,str(n)))
    nv.reverse()
    return nv == nv1
output = filter(is_palindrome, range(1, 1000))
print('1~1000:', list(output))
if list(filter(is_palindrome, range(1, 200))) == [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 22, 33, 44, 55, 66, 77, 88, 99, 101, 111, 121, 131, 141, 151, 161, 171, 181, 191]:
    print('测试成功!')
else:
    print('测试失败!')
```

#### sorted

**排序算法**

Python内置的`sorted()`函数可以对list进行排序：

```python
>>> sorted([36,5,-12,9,-21])
[-21,-12,5,9,36]
```

此外，`sorted()`函数也是一个高阶函数，可以接收一个`key`函数实现自定义排序。如按绝对值大小排序：

```python
>>> sorted([36,5,-12,9,-21], key=abs)
[5, 9, -12, -21, 36]
```

默认情况对字符串排序按照ASCII大小比较，对字符串排序：

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'])
['Credit', 'Zoo', 'about', 'bob']
```

现在排序要忽略大小写，按字母排序。只要把key函数映射一个忽略大小写的函数即可。忽略大小写比较实际就是先把字符串都变成大写或者都变成小写再比较。

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
['about', 'bob', 'Credit', 'Zoo']
```

要进行反向排序，不比改动key函数，可以传入第三个参数`reverse=True`：

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```

**练习**

假设我们用一组tuple表示学生名字和成绩：

```python
L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
```

请用`sorted()`对上述列表分别按名字排序：

```python
# -*- coding: utf-8 -*-

L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
def by_name(t):
    return t[0]
L2 = sorted(L, key=by_name)
print(L2)
```

按分数从高到低排序：

```python
def by_score(t):
    return -t[1]
L2 = sorted(L, key=by_score)
print(L2)
```

```python
def by_score(t):
    return t[1]
L3 = sorted(L, key=by_score, reverse=True)
print(L3)
```

**小结**

- tuple是用  ()  来表示的，比如（1,2,3），它是不可变的，但如果里面有list，list是可变的，适合平常存放数据；
- list是用  []  来表示的，比如['c','b'.'a']，它是一个有序集合，可以添加删除元素，适合平常的增删改查使用；
- dict是用 {} 来表示的，比如{'b': 12, 'a' : 23}，它是key-value组合，特性与list类似，但它查找速度快，占用内存高，key必须是不可变的，适合用来快速查找数据；
- set也是用 {} 来表示的，比如{1,2,4}，它是key的组合，无value，key是唯一的，所以set中的元素都不相同，而且元素也是不可变的，适合用来做不同set间的或与非判断；

凡是可作用于`for`循环的对象都是`Iterable`类型；

凡是可作用于`next()`函数的对象都是`Iterator`类型，它们表示一个惰性计算的序列；

集合数据类型如`list`、`dict`、`str`等是`Iterable`但不是`Iterator`，不过可以通过`iter()`函数获得一个`Iterator`对象。

### 返回函数

**函数作为返回值**

高阶函数除了可以接受函数作为参数外，还可以把函数作为结果返回。

实现一个可变参数的求和函数：

```python
def calc_sum(*args):
    ax = 0
    for n in args:
        ax = ax + n
    return ax
```

如果不需要立刻求和，而是在后面的代码中根据需要计算，可以不返回结果而是返回求和函数：

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```

调用`lazy_sum()`时返回的并不是求和结果，而是求和函数：

```python
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
>>> f()
25
```

调用函数`f`时才是真正计算求和的结果。

内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，相关参数和变量都保存在返回的函数中，这种称为闭包（Closure）。

注意当调用`lazy_sum()`时，每次调用都会返回一个新的函数，即使传入相同的参数，`f1()`和`f2()`调用结果互不影响。

```python
>>> f1 = lazy_sum(1, 3, 5, 7, 9)
>>> f2 = lazy_sum(1, 3, 5, 7, 9)
>>> f1==f2
False
```

**闭包**

```python
def count():
    fs = []
    for i in range(1,4):
        def f():
            return i*i
        fs.append(f)
    return fs
f1,f2,f3 = count()
```

```python
>>> f1()
9
>>> f2()
9
>>> f3()
9
```

调用`f1()`，`f2()`和`f3()`结果不是`1`，`4`，`9`。因为返回的函数引用了变量`i`，不是立刻执行。等到3个函数都返回时，变量`i`已经变成了3，最终结果9。

> [!ATTENTION]
>
> 返回闭包时，返回函数不要引用任何循环变量或者后续会发生变化的变量。

如果一定要引用循环变量，方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：

```python
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
```

```python
>>> f1, f2, f3 = count()
>>> f1()
1
>>> f2()
4
>>> f3()
9
```

**练习**

利用闭包返回一个计数器函数，每次调用它返回递增整数：

```python
# -*- coding：utf-8 -*-
# 方法一：
# 声明静态变量的关键字是nonlocal作用对象是外层变量
# global声明代码块中的变量使用外部全局的同名变量
def createCounter():
    a = 0
    def counter():
        nonlocal a
        a += 1
        return a
    return counter

# 测试:
counterA = createCounter()
print(counterA(), counterA(), counterA(), counterA(), counterA()) # 1 2 3 4 5
counterB = createCounter()
if [counterB(), counterB(), counterB(), counterB()] == [1, 2, 3, 4]:
    print('测试通过!')
else:
    print('测试失败!')


# 方法二
def createCounter():
    c = [0]
    def counter():
        c[0] += 1
        return c[0]
    return counter

# 测试:
counterA = createCounter()
print(counterA(), counterA(), counterA(), counterA(), counterA()) # 1 2 3 4 5
counterB = createCounter()
if [counterB(), counterB(), counterB(), counterB()] == [1, 2, 3, 4]:
    print('测试通过!')
else:
    print('测试失败!')


# 方法三
def createCounter():
	def iterator():	#定义一个生成器
		i = 0
		while True:
			i += 1
			yield i
	g = iterator()
	
	def counter():
		return next(g)
	return counter

# 测试:
counterA = createCounter()
print(counterA(), counterA(), counterA(), counterA(), counterA()) # 1 2 3 4 5
counterB = createCounter()
if [counterB(), counterB(), counterB(), counterB()] == [1, 2, 3, 4]:
    print('测试通过!')
else:
    print('测试失败!')
```

### 匿名函数

当传入函数时，有些时候不需要显示定义函数，直接传入匿名函数更方便。在Python中关键字`lambda`表示匿名函数，以`map()`函数为例，计算f(x)=x2时，除了定义一个`f(x)`的函数外，还可以直接传入匿名函数：

```python
>>> list(map(lambda x: x*x, [1,2,3,4,5,6,7,8,9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

匿名函数`lambda x: x*x`，冒号前面的`x`表示函数参数，冒号后面表示表达式，不用写`return`，返回值就是表达式的结果。实际上就是：

```python
def f(x):
	return x*x
```

匿名函数没有名字不会函数名冲突，且匿名函数是一个对象可以赋值给一个变量，通过变量调用。

```python
>>> f = lambda x: x * x
>>> f
<function <lambda> at 0x101c6ef28>
>>> f(5)
25
```

同样也可以把匿名函数作为返回值返回：

```python
def build(x,y):
    return lambda: x * x + y * y
```

**练习**

将下面的代码改成匿名函数：

```python
# -*- coding: utf-8 -*-
def is_odd(n):
    return n % 2 == 1
L = list(filter(is_odd, range(1,20)))
L1 = list(filter(lambda x: x % 2 == 1, range(1,20)))
print(L,L1)
```



### 装饰器

函数也是一个对象，可以赋值给变量，通过变量调用该函数。

```python
>>> def now():
    	print('2020-12-21')
>>> f = now
>>> f()
2015-3-25
```

函数对象有一个`__name__`属性，可以拿到函数的名字：

```python
>>> now.__name__
'now'
>>> f.__name__
'now'
```

假设现在要增强`now()`函数的功能，比如在调用前后自动打印日志，但又不修改`now()`函数的定义，这种在代码运行期间动态增加功能的方式成为装饰器（Decorator）。本质上decorator是一个返回函数的高阶函数，定义一个能打印日志的decorator：

```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

`log`是一个decorator，接收一个函数作为参数，并返回一个函数。借助Python的@语法把装饰器置于函数定义处：

```python
@log
def now():
    print('2020-12-21')
```

当调用`now()`函数，会在运行函数前打印一行日志。

```python
>>> now()
call now():
2020-12-21
```

把`@log`放到`now()`函数定义处就相当于：

```python
now = log(now)
```

调用`now()`将执行新函数，即在`log()`函数中返回的`wrapper()`函数。`wrapper()`函数的参数定义是`(*args, **kw)`，因此，`wrapper()`函数可以接受任意参数的调用。在`wrapper()`函数内，首先打印日志，再紧接着调用原始函数。

```
>>> now()
call wrapper:
call now():
2020-12-21
```

如果decorator本身需要传入参数，那就需要编写一个返回decorator的高阶函数。比如自定义log文本：

```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```

这个三层签到的decorator用法如下：

```python
@log('execute')
def now():
    print('2020-12-21')
```

执行结果如下：

```python
>>> now()
execute now():
2020-12-21
```

把`log(text)`放在`now()`函数定义处就相当于：

```python
>>> now = log('execute')(now)
```

分析上面的语句，首先执行`log('execute')`，返回`decorator`函数，再调用返回的`wrapper`函数，参数是`now`，返回值是`wrapper`函数。由于函数也是对象，它有`__name__`属性，经过decorator装饰后的函数已经从原来的`'now'`变成了`'wrapper'`：

```python
>>> now.__name__
'wrapper'
```

因为最终返回值`wrapper()`函数的名字就是`'wrapper'`，所以需要把原始函数的`__name__`等属性复制到`wrapper()`函数中，否则有些依赖函数的代码就会出错。

不需要编写`wrapper.__name__ = func.__name__`这样的代码，Python内置的`functools.wraps`就是干这个事的，所以一个完整的decorator的写法如下：

```python
import functools
def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
    return wrapper
```

带参数的decorator：

```python
import functools
def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('call %s():' % func.__name__)
            return func(**args, **kw)
        return wrapper
    return decorator
```

**练习**

设计一个decorator，可以作用于任何函数并且打印该函数的执行事件：

```python
# -*- coding: utf-8 -*-
import time, functools
def metric(fn):
    @functools.wraps(fn)
    def wrapper(*args, **kw):
        startTime = time.time()
        fn(*args, **kw)
        endTime = time.time()
        print("%s execute in %.2f time" % (fn.__name__,endTime - startTime))
        return fn(*args, **kw)
    return wrapper
# 测试
@metric
def fast(x, y):
    time.sleep(0.0012)
    return x + y;

@metric
def slow(x, y, z):
    time.sleep(0.1234)
    return x * y * z;

f = fast(11, 22)
s = slow(11, 22, 33)
if f != 33:
    print('测试失败!')
elif s != 7986:
    print('测试失败!')
```

**小结**

在面向对象（OOP）的设计模式中，decorator被称为装饰模式。OOP的装饰模式需要通过继承和组合来实现，而Python除了能支持OOP的decorator外，直接从语法层次支持decorator。Python的decorator可以用函数实现，也可以用类实现。

decorator可以增强函数的功能，定义起来虽然有点复杂，但使用起来非常灵活和方便。

请编写一个decorator，能在函数调用的前后打印出`'begin call'`和`'end call'`的日志。

再思考一下能否写出一个`@log`的decorator，使它既支持：

```python
@log
def f():
    pass
```

又支持：

```python
@log('execute')
def f():
    pass
```

同时适用是否带参数：

```python
import functools
def log(x): 
    if callable(x):
        @functools.wraps(x)        
        def wrapper(*args,**kw):            
            print('call %s():' % x.__name__)            
        return x(*args,**kw)        
    return wrapper    
    else:        
        def decorator(func):            
        @functools.wraps(func)            
        def wrapper1(*args1,**kw):                
            print('%s %s():' % (x, func.__name__))                
        return func(*args1,**kw)            
    return wrapper1        
return decorator
```

```python
# 同时适用于是否带参数
import functools
def log(text=None): #修饰器存在默认值：为没有参数
    def de(func):
        @functools.wraps(func)
        def first(*args, **kw):
            if text: # 当text为True，即修饰器携带参数
                print('begin call', func.__name__, 'input is', text) # 函数开始前打印日志，并显示修饰器携带的参数
            else: # 当为False时，text为None，即修饰器不携带参数
                print('begin call', func.__name__) #函数开始前打印日志
            rs = func(*args, **kw)
            print('end call') #函数结束打印日志
            return rs
        return first
    return de


@log
def f():
    pass

@log('execute')
def f():
    pass
```



### 偏函数

Python的`functools`模块还提供了很多功能，其中包含偏函数（Partial function）。函数可以通过设定默认参数降低调用难度，而偏函数也可以。举例：

`int()`函数默认按十进制将传入的字符串转为整数。

```python
>>> int('12345')
12345
```

但`int()`函数还提供额外的`base`参数，传入参数可做N进制转换：

```python
>>> int('12345', base=8)
5349
>>> int('12345', 16)
74565
```

假设要转换大量的二进制字符串，每次都传入`int(x,base=2)`非常麻烦，可以定义一个`int2()`函数默认把`base=2`传进去：

```python
def int2(x, base=2):
    return int(x,base)
```

这样调用`int2()`就可以实现默认转换二进制了：

```python
>>> int2('1000000')
64
```

`functools.partial`就是创建偏函数的，不需要自己定义`int2()`，可以直接使用下面的代码创建一个新的`int2`：

```python
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
```

所以`functools.partial()`的作用就是设置某个函数的默认参数值，并返回一个新的函数。

上面的`int2`函数把`base`参数重新设定为2，但也可以在调用时传入其它值：

```python
>>> int2('1000000', base=10)
1000000
```

创建偏函数时，可以接收函数对象、`*args`、`**kw`三个参数，

```python
int2 = functools.partial(int, base=2)
```

实际上固定了`int()`函数的关键字参数`base`，也就是

```python
int2('10010')
```

相当于

```python
kw = {'base': 2}
int('10010', **kw)
```

```python
max2 = functools.partial(max,10)
```

实际上会把`10`作为`*args`的一部分自动加到左边：

```
max2(5,6,7)
args = (10,5,6,7)
max(*args)
```



## 模块

将函数分组，一个.py文件就称为一个模块。

- 模块名遵循Python变量命名规范，不要使用中文、特殊字符；
- 模块名不能和系统模块名冲突，通过执行`import abc`检查系统是否存在此模块。不同包名下的同名模块不同。

### 使用模块

通过内置的`sys`模块编写一个`hello`模块：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

' a test module '

__author__ = 'Lsaiah Liao'

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```

第1和第2行是标准注释，1表示可以让这个模块直接在Unix/Linux/Mac上运行，2表示.py文件使用UTF-8编码；

第4行是一个字符串，表示模块的文档注释，任何模块的第一个字符串都被视为模块的文档注释。第6行`__author__`表示作者名。

通过`import`关键字导入`sys`模块后，变量sys指向该模块，通过这个变量就可以使用`sys`模块的功能。`sys.argv`变量表示用list存储了命令行的所有参数，`argv`至少有一个参数是该.py文件的名称。例如运行`python3 hello.py`获得`sys.argv`就是`['hello.py']`。

```python
if __name__=='__main__':
    test()
```

当在命令行运行`hello`模块时，Python解释器把一个特殊变量`__name__`置为`__main__`，而如果在其他地方导入该`hello`模块时，`if`判断将失败，因此，这种`if`测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。

```python
$ python3 hello.py
Hello, world!
```

```python
>>> import hello
>>>
```

导入时没有打印是因为没有执行`test()`函数，调用`hello.test()`才能打印。而在命令行直接执行`hello.py`会调用`if __name__=='__main__'`方法。

**作用域**

在一个模块中，通过`_`前缀实现模块非公开private，不应该被直接引用。正常的函数和变量名都是公开的public，可以直接被引用。因为Python不能完全限制访问private的函数或变量。

```python
def _private_1(name):
    return 'Hello, %s' % name

def _private_2(name):
    return 'Hi, %s' % name

def greeting(name):
    if len(name) > 3:
        return _private_1(name)
    else:
        return _private_2(name)
```

在模块里公开`greeting()`函数，把内部逻辑用private函数隐藏了，这样调用`greeting()`函数不用关心内部private的细节，这是一种非常有用的代码封装和抽象的方法。即外部不需要引用的函数全部定义成private，只有外部需要引用的函数才定义为public。

### 安装第三方模块

 在Mac或Linux系统上安装Python自带pip且已经添加到环境变量，在Windows安装时需要勾选。区分python3和python2使用pip3命令。

通过[第三方库官网](https://pypi.org/)查找该库的名称进行安装：

```python
pip install Pillow
```

当需要安装很多个常用库时，推荐使用[Anaconda](https://www.anaconda.com/)，内置集成了许多非常有用的第三方库。

**模块搜索路径**

当试图导入加载一个模块时，Python会在指定路径搜索对应的.py文件，找不到就会报错`ImportError: No module named mymodule`，默认情况python解释器会搜索当前目录，所有已安装的内置和第三方模块，搜索路径放在`sys`模块的`path`变量中：

```python
>>> import sys
>>> sys.path
['', 'D:\\python\\python38.zip', 'D:\\python\\DLLs', 'D:\\python\\lib', 'D:\\pyt
hon', 'D:\\python\\lib\\site-packages']
```

如果要添加自己的搜索目录，一是修改`sys.path`添加要搜索的目录。

```python
>>> import sys
>>> sys.path.append('/Users/lsaiah/my_py_lib')
```

这种方法是在运行时修改，运行后失效。

二是设置环境变量`PYTHONPATH`，该环境变量的内容会被自动添加到模块搜索路径中。

## 面向对象编程

面向对象编程——Object Oriented Programming，是一种程序设计思想，把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。在Python中，所有数据类型都可以视为对象，也可以自定义对象。自定义的对象数据类型就是面向对象中的类（Class）的概念。

通过一个例子来说明面向过程和面向对象在程序流程上的不同处，假设要处理学生的成绩表，为了表示一个学生的成绩，面向过程的程序可以用一个dict表示：

```python
std1 = {'name': 'Lsaiah', 'score': 98}
std2 = {'name': 'Bob', 'score': 81}
```

而处理学生成绩可以通过函数实现，比如打印学生成绩：

```python
def print_score(std):
    print('%s: %s' % (std['name'], std['score']))
```

如果是面向对象的程序设计思想，首先考虑的不是程序的执行流程，而是`Student`这种数据类型应该被视为一个对象，这个对象有`name`和`score`两个属性（Property），如果要打印一个学生的成绩，首先必须创建出这个学生对应的对象，然后调用对象的`print_score`方法（Method）让对象自己把自己的数据打印出来。

```python
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score
        
    def print_score(self):
        print('%s : %s' % (self.name, self.score))
```

```python
bart = Student('Bart Simpson', 59)
lisa = Student('Lisa Simpson', 87)
bart.print_score()
lisa.print_score()
```

面向对象的设计思想是从自然界中来的，因为在自然界中，Class是一种抽象概念，比如我们定义的Class——Student，是指学生这个概念，而实例（Instance）则是一个个具体的Student。所以面向对象的设计思想是抽象出Class，根据Class创建Instance。数据封装、继承和多态是面向对象的三大特点。

###  类和实例

面向对象最重要的概念就是类（Class）和实例（Instance），类是抽象的模板，比如Student。而实例时根据类创建出来的一个个具体对象，每个对象都拥有相同的方法，数据可能不同。

```python
class Student(object):
```

class关键字定义类名，通常大写开头，接着是`(object)`，表示该类是从哪个类继承下来的，如果没有合适的继承类就使用`object`类，这是所有类最终都会继承的类。定义好类后就可以根据`Student`类创建出`Student`的实例，通过类名+()实现：

```python
>>> bart = Student()
>>> bart
<__main__.Student object at 0x10a67a590>
>>> Student
<class '__main__.Student'>
```

变量`bart`指向的就是一个`Student`实例，后面`0x10a67a590`是内存地址，每个object的地址都不一样。

可以给实例变量绑定属性：

```python
>>> bart.name = 'Bart Simpson'
```

通过定义一个特殊的`__init__`方法，在创建实例的时候把`name`，`score`等属性绑上去：

```python
class Student(object):
    def __init__(self,name,score):
        self.name = name
        self.score = score
```

`__init__`方法的第一个参数永远是`self`，表示创建的实例本身，因此，在`__init__`方法内部，就可以把各种属性绑定到`self`。有了`__init__`方法，在创建实例的时候，就不能传入空的参数了，必须传入与`__init__`方法匹配的参数，但`self`不需要传，Python解释器自己会把实例变量传进去：

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.name
'Bart Simpson'
>>> bart.score
59
```

在类中定义的函数和普通函数相比，第一个参数永远是实例变量`self`，并且在调用时不用传该参数。除此之外没什么区别，所以类函数仍然可以使用默认参数，可变参数，关键字参数和命名关键字参数。

**数据封装**

在`Student`类中，每个实例都有各自的`name`和`score`，可以通过函数访问这些数据：

```python
>>> def print_score(std):
...     print('%s: %s' % (std.name, std.score))
...
>>> print_score(bart)
Bart Simpson: 59
```

在`Student`内部可以直接定义访问数据的函数，这样就把数据封装起来了，这些封装数据的函数和类关联称为方法：

```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))
```

定义方法第一个参数是`self`和其它普通函数一样，在实例变量上直接调用，除了`self`不用传递，其它参数正常传入：

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.print_score()
Bart Simpson: 59
```

封装调用容易，不用知道内部实现的细节，另外还可以给`Student`类增加新的方法，比如`get_grade()`：

```python
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))

    def get_grade(self):
        if self.score >= 90:
            return 'A'
        elif self.score >= 60:
            return 'B'
        else:
            return 'C'
```

同样`get_grade()`可以直接调用，不需要知道实现细节：

```python
lisa = Student('Lisa', 99)
bart = Student('Bart', 59)
print(lisa.name, lisa.get_grade())
print(bart.name, bart.get_grade())
```

和静态语言不同，Python允许对实例变量绑定任何数据，也就是说，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同：

```python
>>> bart = Student('Bart Simpson', 59)
>>> lisa = Student('Lisa Simpson', 87)
>>> bart.age = 8
>>> bart.age
8
>>> lisa.age
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'age'
```

bart对象的实例增加了自己的属性而不影响类，因此同一个类的不同对象lisa实例不能调用bart对象的属性。

### 访问限制

Class内部的属性和方法可以直接调用，还可以自由修改。如果让内部属性不被外部访问，可以把属性名称前面加两个下划线`__`，在Python中实例变量如果以`__`开头就变成了一个私有变量（private），只有在class内部可以访问，外部不能访问。

```python
class Student(object):
    def __init__(self, name, score):
        self.__name = name
        self.__score = score
    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
```

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
```

这样就确保了外部代码不能随意修改对象内部状态，访问限制使得代码更健壮。如果外部代码要获取name和score，可以给Student类增加`get_name`和`get_score`方法：

```python
class Student(object):
    def get_name(self):
        return self.__name
    def get_score(self):
        return self.__score
```

如果还要允许外部代码修改score，可以给Student类增加`set_score`方法：

```python
class Student(object):
    def set_score(self, score):
        self.__score = score
```

没有`set_score`方法时也可以直接修改，但是不能检查传入的参数，为了避免传入无效参数：

```python
class Student(object):
    def set_score(self, score):
        if 0 <= score <= 100:
            self.__score = score
        else:
            raise ValueError('bad score')
```

在Python中变量名类似`__xx__`双下划线开头和结尾属于特殊变量，不是private变量可以直接访问。类似`_xx`单下划线开头的变量外部是可以访问的，但是一般视为私有变量。`__name`不能直接访问，但是Python解释器把`__name`变量改成了`_Student__name`仍然可以通过`_Student__name`来访问`__name`变量，不同版本的解释器可能会把`__name`改成不同的变量名，所以不推荐这样访问。

```python
>>> bart._Student__name
'Bart Simpson'
```

> [!ATTENTION]
>
> 错误写法：外部代码设置了`__name`变量，但实际上这个`__name`变量和class内部的`__name`变量*不是*一个变量！内部的`__name`变量已经被Python解释器自动改成了`_Student__name`，而外部代码给`bart`新增了一个`__name`变量。

```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.get_name()
'Bart Simpson'
>>> bart.__name = 'New Name' # 设置__name变量！
>>> bart.__name
'New Name'
```

```python
>>> bart.get_name() # get_name()内部返回self.__name
'Bart Simpson'
```

**练习**

```python
# -*- coding: utf-8 -*-
class Student(object):
    def __init__(self, name, gender):
        self.name = name
        self.__gender = gender
    def get_gender(self):
        return self.__gender
    def set_gender(self, gender):
        self.__gender = gender
# 测试:
bart = Student('Bart', 'male')
if bart.get_gender() != 'male':
    print('测试失败!')
else:
    bart.set_gender('female')
    if bart.get_gender() != 'female':
        print('测试失败!')
    else:
        print('测试成功!')
```

### 继承和多态

在面向对象编程OOP中，定义一个class的时候可以从某个现有的class中继承，新的class称为子类，被继承的类称为基类、父类或超类。

比如编写一个`Animal`的class有一个`run()`方法：

```python
class Animal(object):
	def run(self):
        print('Animal is running ...')
```

当编写`Dog`和`Cat`类时，可以从`Animal`类中直接继承`run()`方法。

```python
class Dog(Animal):
    pass
class Cat(Animal):
    pass
```

子类继承父类可以获得父类的全部功能：

```python
dog = Dog()
dog.run()
Animal is running...
cat = Cat()
cat.run()
Animal is running...
```

子类继承父类可以重写父类的方法，运行时重写的方法覆盖了父类的方法，实现了多态性。也可以增加单独的方法。

```python
class Dog(Animal):
    def run(self):
        print('Dog is running...')
    def eat(self):
        print('Eating meat...')
class Cat(Animal):
    def run(self):
        print('Cat is running...')
```

```python
dog = Dog()
dog.run()
dog.eat()
Dog is running...
Eating meat...
cat = Cat()
cat.run()
Cat is running...
```

当定义了一个class的时候，实际上定义了一种数据类型：

```python
a = list() # a是list类型
b = Animal() # b是Animal类型
c = Dog() # c是Dog类型
```

判断一个变量是否是某个类型可以用`isinstance()`判断：

```python
>>> isinstance(a, list)
True
>>> isinstance(b, Animal)
True
>>> isinstance(c, Dog)
True
```

同时`Dog`是从`Animal`继承下来的，所以`c`也是`Animal`类：

```python
>>> isinstance(c, Animal)
True
```

在继承关系中，如果一个实例的数据类型是某个子类，那它的数据类型也可以被看做是父类。反过来`Animal`不可以看成`Dog`：

```python
>>> b = Animal()
>>> isinstance(b, Dog)
False
```

多态的好处：

```python
class Animal(object):   #编写Animal类
    def run(self):
        print("Animal is running...")

class Dog(Animal):  #Dog类继承Amimal类，没有run方法
    pass

class Cat(Animal):  #Cat类继承Animal类，有自己的run方法
    def run(self):
        print('Cat is running...')
    pass

class Car(object):  #Car类不继承，有自己的run方法
    def run(self):
        print('Car is running...')

class Stone(object):  #Stone类不继承，也没有run方法
    pass

def run_twice(animal):
    animal.run()
    animal.run()

run_twice(Animal())
run_twice(Dog())
run_twice(Cat())
run_twice(Car())
run_twice(Stone())
```

输出结果如下：

```python
Animal is running...
Animal is running...
Animal is running...
Animal is running...
Cat is running...
Cat is running...
Car is running...
Car is running...

AttributeError: 'Stone' object has no attribute 'run'
```

除石头吃了不会跑的亏外，其余的都能run，都是“鸭子”。

对于静态语言（例如Java）来说，如果需要传入`Animal`类型，则传入的对象必须是`Animal`类型或者它的子类，否则，将无法调用`run()`方法。对于Python这样的动态语言来说，则不一定需要传入`Animal`类型。我们只需要保证传入的对象有一个`run()`方法就可以调用`run_twice`。

### 获取对象信息

**使用type()**

使用`type()`判断对象的基本类型：

```python
>>> type(123)
<class 'int'>
>>> type('str')
<class 'str'>
>>> type(None)
<type(None) 'NoneType'>
```

如果一个变量指向函数或者类也可以用`type()`判断：

```python
>>> type(abs)
<class 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>
```

`type()`函数返回对应的class类型，比较两个变量的`type()`是否相同：

```python
>>> type(123)==type(456)
True
>>> type(123)==int
True
>>> type('abc')==type('123')
True
>>> type('abc')==str
True
>>> type('abc')==type(123)
False
```

判断基本数据类型可以直接写`int`，`str`。判断一个对象是否是函数，可以使用`types`模块中定义的常量：

```python
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```

**使用isinstance()**

判断class类型，可以使用`isinstance()`，如下class继承关系：

```python
object -> Animal -> Dog -> Husky
```

先创建三种类型的对象：

```python
>>> a = Animal()
>>> d = Dog()
>>> h = Husky()
```

然后判断：

```python
>>> isinstance(h, Husky)
True
>>> isinstance(h, Dog)
True
>>> isinstance(h, Animal)
True
>>> isinstance(d, Dog) and isinstance(d, Animal)
True
>>> isinstance(d, Husky)
False
```

`isinstance()`判断的是一个对象是否是该类型本身，或者位于该类型的父继承链上。

能用`type()`判断的基本类型也可以用`isinstance()`判断：

```python
>>> isinstance('a', str)
True
>>> isinstance(123, int)
True
>>> isinstance(b'a', bytes)
True
```

还可以判断一个变量是否是某些类型中的一种：

```python
>>> isinstance([1, 2, 3], (list, tuple))
True
>>> isinstance((1, 2, 3), (list, tuple))
True
```

所以优先使用`isinstance()`判断类型。

**使用dir()**

获得一个对象的所有属性和方法，可以使用`dir()`函数，返回一个包含字符串的list：

```python
>>> dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
```

类似`__xxx__`的属性和方法都有特殊用途，比如`__len__`方法返回长度，当调用`len()`函数获取对象长度时，在函数内部自动调用该对象的`__len__()`方法。

```python
>>> len('ABC')
3
>>> 'ABC'.__len__()
3
```

自己写的类中想用`len(myObj)`，可以自己写一个`__len__()`方法：

```python
>>> class MyDog(object):
...     def __len__(self):
...         return 100
...
>>> dog = MyDog()
>>> len(dog)
100
```

只列出属性和方法不够，可以配合`getattr()`、`setattr()`以及`hasattr()`直接操作一个对象的状态：

```python
>>> class MyObject(object):
...     def __init__(self):
...         self.x = 9
...     def power(self):
...         return self.x * self.x
...
>>> obj = MyObject()
```

获取对象的属性：

```python
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
>>> getattr(obj, 'z') # 获取不存在的属性'z'抛出AttributeError的错误
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'MyObject' object has no attribute 'z'
>>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
404
```

获取对象的方法：

```python
>>> hasattr(obj, 'power') # 有方法'power'吗？
True
>>> getattr(obj, 'power') # 获取方法'power'
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn = getattr(obj, 'power') # 获取方法'power'并赋值到变量fn
>>> fn # fn指向obj.power
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn() # 调用fn()与调用obj.power()是一样的
81
```

只有在不知道对象信息的时候才会获取对象信息。正确示例：

```python
def readImage(fp):
    if hasattr(fp, 'read'):
        return readData(fp)
    return None
```

从文件流fp中读取图像，首先要判断fp对象是否存在read方法，如果存在则对象时一个流，如果不存在则无法读取。注意根据鸭子类型，有`read()`方法，不代表该fp对象就是一个文件流，它也可能是网络流，也可能是内存中的一个字节流，但只要`read()`方法返回的是有效的图像数据，就不影响读取图像的功能。

### 实例属性和类属性

Python是动态语言，根据类创建的实例可以任意绑定属性。

```python
class Student(object):
    def __init__(self, name):
        self.name = name

s = Student('Bob')
s.score = 90
```

如果`Student`类本身需要绑定一个属性，可以直接在class中定义属性，这种属性是类属性，归classs所有，类的所有实例都可以访问。

```python
class Student(object):
    name = 'Student'
```

```python
>>> class Student(object):
...     name = 'Student'
...
>>> s = Student() # 创建实例s
>>> print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
Student
>>> print(Student.name) # 打印类的name属性
Student
>>> s.name = 'Lsaiah' # 给实例绑定name属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会覆盖掉类的name属性
Lsaiah
>>> print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
Student
>>> del s.name # 如果删除实例的name属性
>>> print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
Student
```

**练习**

为了统计学生人数，给Student类增加一个类属性，每创建一个实例，该属性自动增加：

```python
class Student(object):
    count = 0
    def __init__(self, name):
        self.name = name
        Student.count += 1
```

## 面向对象高级编程

### 使用`__slots__`

动态语言Python定义一个class，创建class的实例后可以给该实例绑定任何属性和方法：

```python
class Student(object):
    pass

>>> s = Student()
>>> s.name = 'Lsaiah' # 动态给实例绑定一个属性
>>> print(s.name)
Lsaiah
>>> def set_age(self, age): # 定义一个函数作为实例方法
...     self.age = age
...
>>> from types import MethodType
>>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
>>> s.set_age(25) # 调用实例方法
>>> s.age # 测试结果
25
# 但是对另外一个实例不起作用
>>> s2 = Student() # 创建新的实例
>>> s2.set_age(25) # 尝试调用方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'
# 给class绑定方法，所有实例均可调用：
>>> def set_score(self, score):
...     self.score = score
...
>>> Student.set_score = set_score
>>> s.set_score(100)
>>> s.score
100
>>> s2.set_score(99)
>>> s2.score
99
```

如果要限制实例的属性，比如只允许对Student实例添加`name`和`age`属性。Python允许在定义class的时候，定义一个特殊的`__slots__`变量，来限制该class实例能添加的属性：

```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
    
>>> s = Student() # 创建新的实例
>>> s.name = 'Lsaiah' # 绑定属性'name'
>>> s.age = 25 # 绑定属性'age'
>>> s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```

使用`__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：

```python
>>> class GraduateStudent(Student):
...     pass
...
>>> g = GraduateStudent()
>>> g.score = 9999
```

除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`。

### 使用`@property`

在绑定属性时，不能检查参数，可以随便改，不符合逻辑。为了限制属性的范围，可以通过一个`set_score()`方法来设置成绩，再通过一个`get_score()`来获取成绩，这样，在`set_score()`方法里，就可以检查参数：

```python
class Student(object):

    def get_score(self):
         return self._score

    def set_score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value

>>> s = Student()
>>> s.set_score(60) # ok!
>>> s.get_score()
60
>>> s.set_score(9999)
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

上面的方法比较复杂，通过装饰器（decorator）可以给函数动态加上功能，对于类方法内置`@property`装饰器可以把一个方法变成属性调用：

```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value

>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.set_score(60)
>>> s.score # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

`@property`还可以定义只读属性，只定义getter方法，不定义setter方法就是一个只读属性，`age`就是一个*只读*属性，因为`age`可以根据`birth`和当前时间计算出来：

```python
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2015 - self._birth
```

**练习**

请利用`@property`给一个`Screen`对象加上`width`和`height`属性，以及一个只读属性`resolution`：

```python
# -*- coding: utf-8 -*-
class Screen(object):
    @property
    def width(self):
        return self._width

    @width.setter
    def width(self, value):
        self._width = value

    @property
    def height(self):
        return self._height

    @height.setter
    def height(self, value):
        self._height = value

    @property
    def resolution(self):
        return self._width * self._height

# 测试:
s = Screen()
s.width = 1024
s.height = 768
print('resolution =', s.resolution)
if s.resolution == 786432:
    print('测试通过!')
else:
    print('测试失败!')
```



### 多重继承

通过继承，子类就可以扩展父类的功能。`Animal`类层次的设计，假设要实现以下4种动物：

- Dog - 狗狗；
- Bat - 蝙蝠；
- Parrot - 鹦鹉；
- Ostrich - 鸵鸟。

按照哺乳动物和鸟类归类，也可以按照能飞和能跑归类，如果两种都包含就要更多的层次：

- 哺乳类：能跑的哺乳类，能飞的哺乳类；
- 鸟类：能跑的鸟类，能飞的鸟类。

```ascii
           │    Animal     │
                └───────────────┘
                        │
           ┌────────────┴────────────┐
           │                         │
           ▼                         ▼
    ┌─────────────┐           ┌─────────────┐
    │   Mammal    │           │    Bird     │
    └─────────────┘           └─────────────┘
           │                         │
     ┌─────┴──────┐            ┌─────┴──────┐
     │            │            │            │
     ▼            ▼            ▼            ▼
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
│  MRun   │  │  MFly   │  │  BRun   │  │  BFly   │
└─────────┘  └─────────┘  └─────────┘  └─────────┘
     │            │            │            │
     │            │            │            │
     ▼            ▼            ▼            ▼
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
│   Dog   │  │   Bat   │  │ Ostrich │  │ Parrot  │
```

如果继续增加宠物类和非宠物类，那么类的数量会呈指数增长，正确做法是采用多重继承。首先，主要的类层次仍按照哺乳类和鸟类设计：

```python
class Animal(object):
    pass

# 大类:
class Mammal(Animal):
    pass

class Bird(Animal):
    pass

# 各种动物:
class Dog(Mammal):
    pass

class Bat(Mammal):
    pass

class Parrot(Bird):
    pass

class Ostrich(Bird):
    pass
```

给动物加上`Runnable`和`Flyable`的功能，定义类：

```python
class Runnable(object):
    def run(self):
        print('Running...')

class Flyable(object):
    def fly(self):
        print('Flying...')
```

对于需要`Runnable`功能的动物，就多继承一个`Runnable`，例如`Dog`；对于需要`Flyable`功能的动物，就多继承一个`Flyable`，例如`Bat`。

```python
class Dog(Mammal, Runnable):
    pass
    
class Bat(Bird, Flyable):
	pass
```

**Mixln**

多重继承，比如让`Ostrich`除了继承自`Bird`外，再同时继承`Runnable`。这种设计通常称之为MixIn。把`Runnable`和`Flyable`改为`RunnableMixIn`和`FlyableMixIn`，类似的还可以定义出肉食动物`CarnivorousMixIn`和植食动物`HerbivoresMixIn`，让某个动物同时拥有好几个MixIn：

```python
class Dog(Mammal, RunnableMixIn, CarnivorousMixIn):
    pass
```

在设计类时优先考虑通过多重继承来组合多个Mixln的功能，而不是设计多层次复杂的继承关系。内置的很多库也使用了Mixln，如`TCPServer`和`UDPServer`这两类网络服务，而要同时服务多个用户就必须使用多进程或多线程模型，这两种模型由`ForkingMixIn`和`ThreadingMixIn`提供。通过组合就可以创造出合适的服务来：

```python
# 编写一个多进程模式的TCP服务
class MyTCPServer(TCPServer, ForkingMixIn):
    pass
# 编写一个多线程模式的UDP服务
class MyUDPServer(UDPServer, ThreadingMixIn):
    pass
# 更先进的协程模型
class MyTCPServer(TCPServer, CoroutineMixIn):
    pass
```

这样就不需要复杂而庞大的继承链，只要选择组合不同的类的功能就可以快速构造出所需的子类。

### 定制类

类似`__xxx__`的变量或者函数名是有特殊用途的，如`__slots__`限制class的实例添加的属性，`__len__()`方法是为了能让class作用于`len()`函数。除此之外Python的class中还有许多这样有特殊用途的函数，可以帮助我们定制类。

**`__str__`**

定义一个`Student`类，打印实例：

```python
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...
>>> print(Student('Lsaiah'))
<__main__.Student object at 0x109afb190>
```

定义`__str__()`方法，返回一个有名字的字符串：

```python
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...     def __str__(self):
...         return 'Student object (name: %s)' % self.name
...
>>> print(Student('Lsaiah'))
Student object (name: Lsaiah)
```

当直接调用变量不用`print`时，打印出来的实例还是`<__main__.Student object at 0x109afb310>`，这是因为直接显示变量调用的不是`__str__()`，而是`__repr__()`，`__repr__()`是为调试服务的，`__str__()`返回用户看到的字符串。解决方法就是再定义一个和`__str__()`一样的`__repr__()`：

```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name
    __repr__ = __str__
```

**`__iter__`**

一个类被用于`for...in`循环，类似list和tuple。必须实现`__iter__()`方法，该方法返回一个迭代对象，python的for循环会不断调用该对象的`__next__()`方法拿到循环的下一个值，直到遇到`StopIteration`错误时退出循环。以斐波那契数列为例写一个Fib类可用于for循环：

```python
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration()
        return self.a # 返回下一个值
    
>>> for n in Fib():
...     print(n)
...
1
1
2
3
5
...
46368
75025
```

**`__getitem__`**

Fib实例可以用作for循环，但是不能当成list使用，比如获取第5个元素：

```python
>>> Fib()[5]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'Fib' object does not support indexing
```

按照下标获取元素，要实现`__getitem__()`方法：

```python
class Fib(object):
    def __getitem__(self, n):
        a, b = 1, 1
        for x in range(n):
            a, b = b, a + b
        return a

>>> f = Fib()
>>> f[0]
1
>>> f[1]
1
>>> f[2]
2
>>> f[3]
3
>>> f[10]
89
>>> f[100]
573147844013817084101
```

但是list的切片方法对于Fib报错，因为`__getitem__()`传入的参数可能是一个int，也可能是一个切片对象`slice`，所以要做判断：

```python
>>> list(range(100))[5:10]
[5, 6, 7, 8, 9]

class Fib(object):
    def __getitem__(self, n):
        if isinstance(n, int): # n是索引
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a
        if isinstance(n, slice): # n是切片
            start = n.start
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1, 1
            L = []
            for x in range(stop):
                if x >= start:
                    L.append(a)
                a, b = b, a + b
            return L

>>> f = Fib()
>>> f[0:5]
[1, 1, 2, 3, 5]
>>> f[:10]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

但是没有对step参数做处理，也没有对负数做处理，还需再实现`__getitem__()`：

```python
>>> f[:10:2]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
>>> f[-10:]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 15, in __getitem__
TypeError: 'NoneType' object cannot be interpreted as an integer
```

此外如果把对象看成`dict`，`__getitem__()`的参数可能是一个可以作key的object，如`str`，与之对应的是`__setitem__()`方法，把对象视为list或dict对集合赋值，还有一个`__delitem__()`方法，用于删除某个元素。通过上面的方法定义的类表现得和Python自带的list、tuple、dict没什么区别，这完全归功于动态语言的鸭子类型，不需要强制继承某个接口。

**`__getattr__`**

正常情况调用类的方法或属性，如果不存在就会报错：

```python
class Student(object):
    
    def __init__(self):
        self.name = 'Lsaiah'

>>> s = Student()
>>> print(s.name)
Lsaiah
>>> print(s.score)
Traceback (most recent call last):
  ...
AttributeError: 'Student' object has no attribute 'score'
```

要避免这个错误，除了可以加一个`score`属性外，还可以写一个方法`__getattr__()`动态返回一个属性：

```python
class Student(object):
    def __init__(self):
        self.name = 'Lsaiah'

    def __getattr__(self, attr):
        if attr=='score':
            return 99

# 也可以返回函数
    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25

>>> s = Student()
>>> s.name
'Lsaiah'
>>> s.score
99
>>> s.age()
25
```

class没有的属性才会调用`__getattr__`，如果调用没有返回的attr，如`s.abc`都会返回`None`，要让class只响应特定的几个属性，按照约定抛出`AttributeError`的错误：

```python
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
        raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)
```

把一个类的所有属性和方法调用全部动态化处理的作用是可以针对完全动态的情况调用，如新浪微博、豆瓣很多网站都搞REST API，调用API的URL类似：

- http://api.server/user/friends
- http://api.server/user/timeline/list

如果要写SDK，给每个URL对应的API都写一个方法，那得累死，而且API一旦改动，SDK也要改。

利用完全动态的`__getattr__`可以写出一个链式调用：

```python
class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__
    
>>> Chain().status.user.timeline.list
'/status/user/timeline/list'
```

无论API怎么变，SDK都可以根据URL实现完全动态的调用，而且不随API的增加而改变，还有些REST API会把参数放到URL中，比如github api：

```python
GET /users/:user/repos
# 调用时需要把:user替换成实际的用户名
Chain().users('Lsaiah').repos
# 输出/users/Lsaiah/repos
class Chain(object):
    def __init__(self, path=''):
       self.__path = path

   def __getattr__(self, path):
       return Chain('%s/%s' % (self.__path, path))

   def __call__(self, path):
       return Chain('%s/%s' % (self.__path, path))

   def __str__(self):
       return self.__path

   __repr__ = __str__

   print(Chain().users('Lsaiah').repos)
```

**`__call__`**

一个对象的实例可以有自己的属性和方法，用`instance.method()`可以直接在实例本身上调用，任何类只需定义一个`__call__()`方法，就可以直接对实例进行调用：

```python
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)
        
>>> s = Student('Lsaiah')
>>> s() # self参数不要传入
My name is Lsaiah.
```

`__call__()`还可以定义参数，对实例调用就是对函数调用，完全可以把对象看成函数，把函数看成对象。判断一个变量是对象还是函数，能被调用的对象就是一个`Callable`对象：

```python
>>> callable(Student())
True
>>> callable(max)
True
>>> callable([1, 2, 3])
False
>>> callable(None)
False
>>> callable('str')
False
```

### 使用枚举类

Python提供`Enum`类实现枚举类型，每个常量都是class的唯一实例：

```python
# 大写变量通过整数定义，如月份
JAN = 1
FEB = 2
MAR = 3
# 枚举类型定义
from enum import Enum
Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
# 直接使用Month.Jan来引用一个常量
>>> Month.Jan
<Month.Jan: 1>
# 枚举所有成员
for name, member in Month.__members__.items():
    print(name, '=>', member, ',', member.value)

Jan => Month.Jan , 1
Feb => Month.Feb , 2
Mar => Month.Mar , 3
Apr => Month.Apr , 4
May => Month.May , 5
Jun => Month.Jun , 6
Jul => Month.Jul , 7
Aug => Month.Aug , 8
Sep => Month.Sep , 9
Oct => Month.Oct , 10
Nov => Month.Nov , 11
Dec => Month.Dec , 12
```

`value`属性则是自动赋给成员的`int`常量，默认从`1`开始计数。如果要更精确控制枚举类型，可以从`Enum`派生出自定义类：

```python
from enum import Enum, unique
@unique
class Weekday(Enum):
    Sun = 0 # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6
```

`@unique`装饰器可以帮助我们检查保证没有重复值。访问这些枚举类型可以有若干种方法：

```python
>>> day1 = Weekday.Mon
>>> print(day1)
Weekday.Mon
>>> print(Weekday.Tue)
Weekday.Tue
>>> print(Weekday['Tue'])
Weekday.Tue
>>> print(Weekday.Tue.value)
2
>>> print(day1 == Weekday.Mon)
True
>>> print(day1 == Weekday.Tue)
False
>>> print(Weekday(1))
Weekday.Mon
>>> print(day1 == Weekday(1))
True
>>> Weekday(7)
Traceback (most recent call last):
  ...
ValueError: 7 is not a valid Weekday
>>> for name, member in Weekday.__members__.items():
...     print(name, '=>', member)
...
Sun => Weekday.Sun
Mon => Weekday.Mon
Tue => Weekday.Tue
Wed => Weekday.Wed
Thu => Weekday.Thu
Fri => Weekday.Fri
Sat => Weekday.Sat
```

**练习**

把`Student`的`gender`属性改造为枚举类型，可以避免使用字符串：

```python
# -*- coding: utf-8 -*-
from enum import Enum, unique
class Gender(Enum):
    Male = 0
    Female = 1
# Gender = Enum('Gender', {'Male': 0, 'Female': 1})
class Student(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender
# 测试:
bart = Student('Bart', Gender.Male)
if bart.gender == Gender.Male:
    print('测试通过!')
else:
    print('测试失败!')
```



### 使用元类

**type()**

静态语言函数和类的定义不是编译时定义的，而是运行时动态创建的。比如要定义一个`Hello`的class，写一个`hello.py`模块：

```python
class Hello(object):
    def hello(self, name='world'):
        print('Hello, %s.' % name)
# 当解释器赞如hello模块时，依次执行该模块的所有语句，执行结果就是动态创建出一个Hello的class对象
>>> from hello import Hello
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<class 'type'>
>>> print(type(h))
<class 'hello.Hello'>
```

`type()`函数可以查看一个类型或变量的类型，`Hello`是一个class，它的类型就是`type`，而`h`是一个实例，它的类型就是class `Hello`。class的定义时运行时创建的，而class的方法就是使用`type()`函数，可以返回一个对象的类型，又可以创建出新的类型，比如通过`type()`函数创建出`Hello`类，而无需通过`class Hello(object)...`的定义：

```python
>>> def fn(self, name='world'): # 先定义函数
...     print('Hello, %s.' % name)
...
>>> Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<class 'type'>
>>> print(type(h))
<class '__main__.Hello'>
```

要创建一个class对象，`type()`函数依次传入3个参数：

1. class的名称；
2. 继承的父类集合，注意Python支持多重继承，如果只有一个父类，别忘了tuple的单元素写法；
3. class的方法名称与函数绑定，这里我们把函数`fn`绑定到方法名`hello`上。

正常情况下，我们都用`class Xxx...`来定义类，但是，`type()`函数也允许我们动态创建出类来，通过`type()`函数创建的类和直接写class是完全一样的。

**metaclass**

除了使用`type()`动态创建类以外，要控制类的创建行为，还可以使用metaclass，直译为元类。当我们定义了类以后，就可以根据这个类创建出实例，所以先定义类，然后创建实例。但是如果要先创建出类，就必须根据metaclass创建出类，所以先定义metaclass，然后创建类，最后创建实例。metaclass允许创建类或者修改类，换句话说类是metaclass创建出来的“实例”。

如使用metaclass给自定义的MyList增加一个`add`方法：

```python
# metaclass是类的模板，所以必须从`type`类型派生：
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value)
        return type.__new__(cls, name, bases, attrs)
# 有了ListMetaclass，在定义类的时候还要指示使用ListMetaclass来定制类，传入关键字参数metaclass
class MyList(list, metaclass=ListMetaclass):
    pass
```

它指示Python解释器在创建`MyList`时，要通过`ListMetaclass.__new__()`来创建，在此可以修改类的定义，比如，加上新的方法，然后返回修改后的定义。

`__new__()`方法接收到的参数依次是：

1. 当前准备创建的类的对象；
2. 类的名字；
3. 类继承的父类集合；
4. 类的方法集合。

```python
# MyList可以调用add()方法，而list没有add()方法
>>> L = MyList()
>>> L.add(1)
>> L
[1]
>>> L2 = list()
>>> L2.add(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'add'
```

一般正常情况直接在`MyList`定义中写上`add()`方法，但是也会遇到要通过metaclass修改类定义的，如ORM全称Object Relational Mapping，即对象-关系映射，就是把关系数据库的一行映射为一个对象，也就是一个类对应一个表，这样写代码更简单，不用直接操作SQL语句。编写ORM框架，第一步先把调用的接口写出来，比如使用者要定义一个`User`类操作对应的数据库表`User`，父类`Model`和属性类型`StringField`、`IntegerField`是由ORM框架提供的，剩下的魔术方法比如`save()`全部由metaclass自动完成。：

```python
class User(Model):
    # 定义类的属性到列的映射：
    id = IntegerField('id')
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')

# 创建一个实例：
u = User(id=12345, name='Lsaiah', email='test@orm.org', password='my-pwd')
# 保存到数据库：
u.save()
```

下面就按照上面的接口实现该ORM，定义`Field`类负责保存数据库表的字段名和字段类型：

```python
class Field(object):

    def __init__(self, name, column_type):
        self.name = name
        self.column_type = column_type

    def __str__(self):
        return '<%s:%s>' % (self.__class__.__name__, self.name)
```

在`Field`的基础上，定义各种类型的`Field`，比如`StringField`，`IntegerField`等等：

```python
class StringField(Field):

    def __init__(self, name):
        super(StringField, self).__init__(name, 'varchar(100)')

class IntegerField(Field):

    def __init__(self, name):
        super(IntegerField, self).__init__(name, 'bigint')
```

然后就编写最复杂的`ModelMetaclass`了：

```python
class ModelMetaclass(type):

    def __new__(cls, name, bases, attrs):
        if name=='Model':
            return type.__new__(cls, name, bases, attrs)
        print('Found model: %s' % name)
        mappings = dict()
        for k, v in attrs.items():
            if isinstance(v, Field):
                print('Found mapping: %s ==> %s' % (k, v))
                mappings[k] = v
        for k in mappings.keys():
            attrs.pop(k)
        attrs['__mappings__'] = mappings # 保存属性和列的映射关系
        attrs['__table__'] = name # 假设表名和类名一致
        return type.__new__(cls, name, bases, attrs)
```

以及基类：

```python
class Model(dict, metaclass=ModelMetaclass):

    def __init__(self, **kw):
        super(Model, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

    def save(self):
        fields = []
        params = []
        args = []
        for k, v in self.__mappings__.items():
            fields.append(v.name)
            params.append('?')
            args.append(getattr(self, k, None))
        sql = 'insert into %s (%s) values (%s)' % (self.__table__, ','.join(fields), ','.join(params))
        print('SQL: %s' % sql)
        print('ARGS: %s' % str(args))
```

当用户定义`class User(Model)`时，Python解释器首先在当前类`User`的定义中查找`metaclass`，如果没有找到，就继续在父类`Model`中查找`metaclass`，找到了，就使用`Model`中定义的`metaclass`的`ModelMetaclass`来创建`User`类，也就是说，metaclass可以隐式地继承到子类，但子类自己却感觉不到。

在`ModelMetaclass`中，一共做了几件事情：

1. 排除掉对`Model`类的修改；
2. 在当前类（比如`User`）中查找定义的类的所有属性，如果找到一个Field属性，就把它保存到一个`__mappings__`的dict中，同时从类属性中删除该Field属性，否则，容易造成运行时错误（实例的属性会遮盖类的同名属性）；
3. 把表名保存到`__table__`中，这里简化为表名默认为类名。

在`Model`类中，就可以定义各种操作数据库的方法，比如`save()`，`delete()`，`find()`，`update`等等。

`save()`方法把一个实例保存到数据库中。因为有表名，属性到字段的映射和属性值的集合，就可以构造出`INSERT`语句：

```python
u = User(id=12345, name='Lsaiah', email='test@orm.org', password='my-pwd')
u.save()

Found model: User
Found mapping: email ==> <StringField:email>
Found mapping: password ==> <StringField:password>
Found mapping: id ==> <IntegerField:uid>
Found mapping: name ==> <StringField:username>
SQL: insert into User (password,email,username,id) values (?,?,?,?)
ARGS: ['my-pwd', 'test@orm.org', 'Lsaiah', 12345]
```

打印出可执行的SQL语句以及参数，只需要链接数据库执行就可以实现功能。

**理解和注释**

第一部分 - 数据类型Fiedl类的定义

\# 定义Field类，包含两个属性，分别是字段的名称name与类型column_type

```python
class Field(object):
    # 类Fiedl的构造函数有两个属性: name, column_type
    def __init__(self, name, column_type):
        self.name = name
        self.column_type = column_type
    def __str__(self):
        return '<%s:%s>' % (self.__class__.__name__, self.name)
```

\# 类StringField继承自类Field，同样有两个属性name与类型column_type

```python
class StringField(Field):
    # 此处仅属性name是强制属性
    def __init__(self, name):
        # 通过super()函数调用parent类的构造函数
        # 其中name就直接传递给Field, column_type传递固定值'varchar(100)'
        # 所以StringField('username').__dict__ == Field('username', 'varchar(100)').__dict__
        super(StringField, self).__init__(name, 'varchar(100)')
```

\# 同理类IntegerField继承自类Field，同样有两个属性name与类型column_type

```python
class IntegerField(Field):
    def __init__(self, name):
        super(IntegerField, self).__init__(name, 'bigint')
```

第二部分 - 元类ModelMetaclass的定义

```python
class ModelMetaclass(type):
    # 此处说明下__new__和__init__的区别：
    # __new__是用来创造一个类对象的构造函数，而__init__是用来初始化一个实例对象的构造函数
    # 类似于__init__，__new__接收的第一个参数cls（类对象）其实就是相当于__init__的self（实例对象）
    # 在初始化__init__之前，类是通过__new__创建的，所以在__init__前一定有__new__来构造类cls，之后__init__才能初始化对象self
    def __new__(cls, name, bases, attrs):
        # 对于名称为Model的类不做其他操作，直接通过type()函数生成类对象Model
        if name == 'Model':
            return type.__new__(cls, name, bases, attrs)
        print('Found model: %s' % name)
        # 对于名称不是Model的类，如User类，通过下面代码过滤类对象User的属性
        # 因为类属性以dict格式存在attrs中，所以是对dict格式的操作：
        # 第一步，过滤出满足条件的属性
        # 先新建一个空dict，如果对象的属性值是Field类的实例对象(如StringField('username'))，则将这些属性放入dict格式的mappings变量中
        mappings = dict()
        for k, v in attrs.items():
            # 下面判断属性的值是否是Field类格式，满足Field类格式形态如下：
            # isinstance(StringField('username'), Field)
            # 或者isinstance(Field('name', StringField('username')), Field)
            # 注意此处是属性的值，不是属性，如：属性name的值为StringField('username')
            if isinstance(v, Field):
                print('Found mapping: %s ==> %s' % (k, v))
                # 将过滤出的dict数据写入mappings变量
                mappings[k] = v
        # 其实上面的这几行可以简化为一行 mappings = {k:v for k, v in attrs.items() if isinstance(v, Field)} 吧？

        # 第二步，把上一步过滤出满足条件的属性从类对象User的属性dict中移除
        for k in mappings.keys():
            attrs.pop(k)

        # 第三步，把dict格式的mappings变量添加到类对象User的属性__mapping__中
        # 即__mapping__成为了类对象User的一个属性，该属性值为dict格式，内容为满足Field类格式的原类对象的属性值
        attrs['__mappings__'] = mappings
        # 到目前为止其实就是把实例对象User的一些属性（满足Field格式）移了个位置
        # 下面再新建一个属性__table__，并且赋值为该类对象的名字，如User
        attrs['__table__'] = name  # 假设表名和类名一致
        # 将修改后的类对象User的属性值返回给type().__new__
        return type.__new__(cls, name, bases, attrs)
```

其实定义好的元类ModelMetaclass，仍然是调用`type.__new__(cls, name, bases, attrs)`函数去构造类对象，可以试下如下代码

不同的是`type.__new__()`构造出的类的属性就在`__dict__`下，但是ModelMetaclass将属性移到`__mappings__`下了

```python
type.__new__(type, 'Model', (dict, ), {'id': IntegerField('id')}).__dict__
ModelMetaclass('Table1', (object, ), {'name': StringField('username')}).__mappings__
```

第三部分 - 生成Model的实例对象

核心在于：在运行`__init__`来生成实例对象前，调用元函数ModelMetaclass来生成类对象，用这个类对象再去生成实例对象。 根据ModelMetaclass代码可以知道，当类名称为Model时，直接返回原始的`type.__new__(cls, name, bases, attrs)`， 如：`type.__new__(type, 'Model', (dict, ), Model(id=12345, name='Michael'))`

此处对于super()函数的理解还是不清楚

```python
class Model(dict, metaclass=ModelMetaclass):
    # 在运行__init__来生成实例对象前，调用元函数ModelMetaclass来生成类对象，用这个类对象再去生成实例对象
    # 根据ModelMetaclass代码可以知道，当类名称为Model时，直接返回原始的type.__new__(cls, name, bases, attrs)
    # 如：type.__new__(type, 'Model', (dict, ), Model(id=12345, name='Michael'))
    # 接下来定义从类对象生成实例对象的__init__函数
    def __init__(self, **kw):
        # 通过super调用父类初始化函数，__init__()动态函数无需self
        # 此处先调用dict
        # ModelMetaclass('User', (type, ), {'id': IntegerField('id')})
        super(Model, self).__init__(**kw)
    # 重新定义getattr函数，可以匹配dict格式输入的{属性:属性值}
    # 如：Model(id = IntegerField('id')).__getattr__
    def __getattr__(self, key):
        '''
        重载__getattr__, __setattr__方法使子类可以像正常的类使用
        '''
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)
    # 个人觉得可以不定义__setattr__，没什么影响
    def __setattr__(self, key, value):
        self[key] = value
    # 定义save方法，用于动态生成SQL
    def save(self):
        # 定义3个数组
        fields = []
        params = []
        args = []
        # 此处引入了之前ModelMetaclass元类定义的dict格式变量__mappings__
        # 假设__mappings__ = {'name': StringField('username')}，则'name'就是k, StringField('username')是v, 'username'是v.name
        for k, v in self.__mappings__.items():
            # 把__mappings__的v.name（即'username'）写入fields变量
            fields.append(v.name)
            params.append('?')
            # fields变量('username')对应的数值通过重新定义的__getattr__函数获取
            # __mappings__中的k为{'name': StringField('username')}中的'name'
            args.append(getattr(self, k, None))
        # __table__在之前ModelMetaclass元类定义为类对象的名称，即'User'表
        # 通过'sep'.join(seq)函数（','.join(['id', 'username'])）生成表字段和字段值
        sql = 'insert into %s (%s) values (%s)' % (self.__table__, ','.join(fields), ','.join(params))
        print('SQL: %s' % sql)
        print('ARGS: %s' % str(args))
```

第四部分 - 生成User实例对象

```python
class User(Model):
    # 因为User继承了Model，而Model继承了dict类型    # 所以dict(id=12345, name='Michael')直接转化为{'id': 12345, 'name': 'Michael'}格式    id = IntegerField('id')
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')
    test = str('abc')
```

总结User实例对象的创建步骤：

1. User类的创建1: User类继承了Model类，而Model类由元类ModelMetaclass定义类的生成，所以User类对象首先通过ModelMetaclass的`type.__new__()`构造生成

2. User类的创建2: 在ModelMetaclass的`type.__new__()`构造函数中，name和bases已经确认，而属性attr是根据输入的属性内容，由`__new__`构造出

3. User实例的创建1：User类继承了Model类，所以User通过Model的构造函数`__init__`生成属性的时候会把输入的(id=12345, name='Michael')转化为dict格式{'id': 12345, 'name': 'Michael'}

4. User实例的创建2: User实例的生成中要按照元类ModelMetaclass的定义生成，即生成`__mappings__`和`__Table__`等属性

## 错误、调试和测试

### 错误处理

如打开文件函数`open()`成功时返回文件描述符，出错时返回-1，用错误码表示是否出错不方便，调用者必须用大量的代码判断，一旦出错还要一级一级上报，直到某个函数可以处理该错误：

```python
def foo():
    r = some_function()
    if r==(-1):
        return (-1)
    # do something
    return r

def bar():
    r = foo()
    if r==(-1):
        print('Error')
    else:
        pass
```

Python内置`try...except...finally...`错误处理机制：

```python
try:
    print('try...')
    r = 10/0
    print('result:', r)
except ZeroDivisionError as e:
    print('except:', e)
finally:
    print('finally...')
print('END')
```

当认为某些代码可能会出错时，就可以用`try`来运行这段代码，如果执行出错，则后续代码不会继续执行，而是直接跳转至错误处理代码，即`except`语句块，执行完`except`后，如果有`finally`语句块，则执行`finally`语句块，没有`finally`语句块则不执行。

```python
try...
except: division by zero
finally...
END
# 把除数0改成2
try...
result: 5
finally...
END
```

由于错误有很多种类，可以有多个`except`来捕获不同类型的错误，`int()`函数可能会抛出`ValueError`和`ZeroDivisionError`：

```python
try:
    print('try...')
    r = 10 / int('a')
    print('result:', r)
except ValueError as e:
    print('ValueError:', e)
except ZeroDivisionError as e:
    print('ZeroDivisionError:', e)
finally:
    print('finally...')
print('END')
```

此外，如果没有错误发生，也可以在`except`语句块后面加一个`else`，没有错误发生执行：

```python
try:
    print('try...')
    r = 10 / int('2')
    print('result:', r)
except ValueError as e:
    print('ValueError:', e)
except ZeroDivisionError as e:
    print('ZeroDivisionError:', e)
else:
    print('no error!')
finally:
    print('finally...')
print('END')
```

Python的错误也是class，所有的错误类型都继承自`BaseException`，所以它不但捕获该类型的错误，还会捕获子类的错误。因为`UnicodeError`是`ValueError`的子类，所以第二个`except`永远也捕获不到`UnicodeError`，如果有`UnicodeError`也被第一个`except`捕获了。

```python
try:
    foo()
except ValueError as e:
    print('ValueError')
except UnicodeError as e:
    print('UnicodeError')
```

[常见的错误类型和继承关系](https://docs.python.org/3/library/exceptions.html#exception-hierarchy)

使用`try...except`不需要在每个可能出错的地方都去捕获，只要在合适的层次去捕获错误就可以了。比如函数`main()`调用`bar()`，`bar()`调用`foo()`，结果`foo()`出错了，这时，只要`main()`捕获到了，就可以处理：

```python
def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    try:
        bar('0')
    except Exception as e:
        print('Error:', e)
    finally:
        print('finally...')
```

如果错误没有被捕获就会一直往上抛，最后被Python解释器捕获，打印一个错误信息，然后程序退出：

```python
# err.py:
def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    bar('0')

main()
$ python3 err.py
Traceback (most recent call last):
  File "err.py", line 11, in <module>
    main()
  File "err.py", line 9, in main
    bar('0')
  File "err.py", line 6, in bar
    return foo(s) * 2
  File "err.py", line 3, in foo
    return 10 / int(s)
ZeroDivisionError: division by zero
```

根据错误类型`ZeroDivisionError`判断，`int(s)`本身并没有出错，但是`int(s)`返回`0`，在计算`10 / 0`时出错。

**记录错误**

Python内置的`logging`模块可以把捕获到的错误堆栈打印出来，同时程序不会结束，而是继续执行下去。`logging`还可以把错误记录到日志文件：

```python
# err_logging.py

import logging

def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    try:
        bar('0')
    except Exception as e:
        logging.exception(e)

main()
print('END')

$ python3 err_logging.py
ERROR:root:division by zero
Traceback (most recent call last):
  File "err_logging.py", line 13, in main
    bar('0')
  File "err_logging.py", line 9, in bar
    return foo(s) * 2
  File "err_logging.py", line 6, in foo
    return 10 / int(s)
ZeroDivisionError: division by zero
END
```

**抛出错误**

因为错误是class，捕获一个错误就是捕获该class的一个实例，因此错误是创建并抛出的，必要时可以自己定义错误，定义一个错误的class，选择好继承关系，然后，用`raise`语句抛出一个错误的实例：

```python
# err_raise.py
class FooError(ValueError):
    pass

def foo(s):
    n = int(s)
    if n==0:
        raise FooError('invalid value: %s' % s)
    return 10 / n

foo('0')

$ python3 err_raise.py 
Traceback (most recent call last):
  File "err_throw.py", line 11, in <module>
    foo('0')
  File "err_throw.py", line 8, in foo
    raise FooError('invalid value: %s' % s)
__main__.FooError: invalid value: 0
```

另一种错误处理的方式：

```python
# err_reraise.py
def foo(s):
    n = int(s)
    if n==0:
        raise ValueError('invalid value: %s' % s)
    return 10 / n

def bar():
    try:
        foo('0')
    except ValueError as e:
        print('ValueError!')
        raise
bar()
```

`except`捕获错误只是记录，不知道该怎么处理该错误，所以`raise`不带参数，就会把当前错误原样抛出，让顶层调用处理。此外，在`except`中`raise`一个Error，还可以把一种类型的错误转化成另一种类型，但是不应该把一个错误转化成另一个毫不相干的错误：

```python
try:
    10 / 0
except ZeroDivisionError:
    raise ValueError('input error!')
```

**练习**

```python
from functools import reduce

def str2num(s):
    try:
        return int(s)
    except Exception as e:
        return float(s)
'''
def str2num(s):
    if s.isdigit():
        return int(s)
    return float(s)
'''
'''
def str2num(s):
    if s.find('.') == -1
        return int(s)
    else:
        return float(s)
'''
'''
def str2num(s):
    if '.' in s:
        return float(s)
    else:
        return int(s)
'''
def calc(exp):
    ss = exp.split('+')
    ns = map(str2num, ss)
    return reduce(lambda acc, x: acc + x, ns)

def main():
    r = calc('100 + 200 + 345')
    print('100 + 200 + 345 =', r)
    r = calc('99 + 88 + 7.6')
    print('99 + 88 + 7.6 =', r)

main()
```



### 调试

用`print()`把肯能有问题的变量打印出来：

```python
def foo(s):
    n = int(s)
    print('>>> n = %d' % n)
    return 10 / n

def main():
    foo('0')

main()
$ python err.py
>>> n = 0
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in main
  File "<stdin>", line 4, in foo
ZeroDivisionError: division by zero
```

但是调试完还得删掉`print()`，否则运行结果包含大量垃圾信息。

**断言**

用`print()`辅助调试的地方都可以用断言assert替代：

```python
def foo(s):
    n = int(s)
    assert n != 0, 'n is zero!'
    return 10 / n

def main():
    foo('0')
    
$ python err.py
Traceback (most recent call last):
  ...
AssertionError: n is zero!

$ python -O err.py
Traceback (most recent call last):
  ...
ZeroDivisionError: division by zero
```

意思是表达式`n != 0`应该是`True`，否则报错，如果断言失败`assert`语句本身就会抛出`AssertionError`。启动Python解释器时可用`-O`参数关闭`assert`。

**logging**

使用`logging`替换`print()`，和`assert`相比，`logging`不会抛出错误，可以输出一段文本到console和文件。

```python
import logging
logging.basicConfig(level=logging.INFO)
s = '0'
n = int(s)
logging.info('n = %d' % n)
print(10 / n)

$ python err.py
INFO:root:n = 0
Traceback (most recent call last):
  File "err.py", line 8, in <module>
    print(10 / n)
ZeroDivisionError: division by zero
```

`logging`允许记录日志信息的级别，有`deubug`，`info`，`warning`，`error`等几个级别，当指定一个级别后，其它级别就不起作用了。

**pdb**

使用Python调试器pdb，让程序单步运行，随时查看运行状态：

```python
# err.py
s = '0'
n = int(s)
print(10 / n)

# 启动
$ python -m pdb err.py
> /Users/michael/Github/learn-python3/samples/debug/err.py(2)<module>()
-> s = '0'
# -m pdb启动后定位到下一步要执行的代码，输入l查看：
(Pdb) l
  1     # err.py
  2  -> s = '0'
  3     n = int(s)
  4     print(10 / n)
# 输入n单步执行：
(Pdb) n
> c:\users\lsaiah\documents\err.py(3)<module>()
-> n = int(s)
(Pdb) n
> c:\users\lsaiah\documents\err.py(4)<module>()
-> print(10 / n)
# 任何时候都可以输入p 变量名来查看变量:
(Pdb) p s
'0'
(Pdb) p n
0
# 输入q结束调试
(Pdb) q
```

如果代码有很多行，这样调试太麻烦。

**pdb.set_trace()**

这个方法也是用pdb，但不需要单步执行，`import pdb`在可能出错的地方加`pdb.set_trace()`就可以设置一个断点。

```python
# err.py
import pdb

s = '0'
n = int(s)
pdb.set_trace() # 运行到这里会自动暂停
print(10 / n)

# 可以用命令p查看变量，或者用命令c继续运行：
C:\Users\Administrator.PC-20190504PBOJ>python C:\Users\Administrator.PC-20190504
PBOJ\Documents\err.py
> c:\users\administrator.pc-20190504pboj\documents\err.py(6)<module>()
-> print(10 / n)
(Pdb) p n
0
(Pdb) c
Traceback (most recent call last):
  File "C:\Users\Administrator.PC-20190504PBOJ\Documents\err.py", line 6, in <mo
dule>
    print(10 / n)
ZeroDivisionError: division by zero
```

这种方法调试效率比单步高，但没有使用IDE调试方便。

Visual Studio Code：https://code.visualstudio.com/，需要安装Python插件。

PyCharm：http://www.jetbrains.com/pycharm/

另外，[Eclipse](http://www.eclipse.org/)加上[pydev](http://pydev.org/)插件也可以调试Python程序。

### 单元测试

测试驱动开发（TDD：Test-Driven Development）模式确保一个程序模块的行为符合设计的测试用例，单元测试是用来对一个模块，一个函数或者一个类来进行正确性检验的测试工作。

比如函数`abs()`编写用例：

1. 输入正数，比如`1`、`1.2`、`0.99`期望返回值与输入值相同；
2. 输入负数，比如`-1`、`-1.2`、`-0.99`期望返回值与输入值相反；
3. 输入`0`，期望返回`0`；
4. 输入非数值类型，比如`None`、`[]`、`{}`，期望抛出`TypeError`。

编写一个`Dict`类，行为和`dict`一致，但是可以通过属性访问：

```python
class Dict(dict):

    def __init__(self, **kw):
        super().__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

>>> d = Dict(a=1, b=2)
>>> d['a']
1
>>> d.a
1
```

编写单元测试，需要引入Python的`unittest`模块，编写一个测试类，从`unittest.TestCase`继承，以`test`开头的方法就是测试方法，不以`test`开头的方法不被认为是测试方法，测试的时候不会被执行：

```python
import unittest

from mydict import Dict

class TestDict(unittest.TestCase):

    def test_init(self):
        d = Dict(a=1, b='test')
        self.assertEqual(d.a, 1)
        self.assertEqual(d.b, 'test')
        self.assertTrue(isinstance(d, dict))

    def test_key(self):
        d = Dict()
        d['key'] = 'value'
        self.assertEqual(d.key, 'value')

    def test_attr(self):
        d = Dict()
        d.key = 'value'
        self.assertTrue('key' in d)
        self.assertEqual(d['key'], 'value')

    def test_keyerror(self):
        d = Dict()
        with self.assertRaises(KeyError):
            value = d['empty']

    def test_attrerror(self):
        d = Dict()
        with self.assertRaises(AttributeError):
            value = d.empty
```

`unittest.TestCase`提供了很多[内置的条件判断](https://docs.python.org/zh-cn/3/library/unittest.html?highlight=unittest#unittest.TestCase.assertEqual)，最常用`assertEqual()`，`assertRaises()`：

```python
self.assertEqual(abs(-1), 1) # 断言函数返回的结果与1相等
# 断言期望抛出错误的指定类型
with self.assertRaises(KeyError):
    value = d['empty']
with self.assertRaises(AttributeError):
    value = d.empty
```

**运行单元测试**

在`mydict_test.py`的最后加上：

```python
if __name__ == '__main__':
    unittest.main()
```

另一种推荐方法是在命令行通过参数`-m unittest`直接批量运行单元测试：

```python
$ python -m unittest mydict_test
.....
----------------------------------------------------------------------
Ran 5 tests in 0.000s

OK
```

**setUP和tearDown**

`setUp()`和`tearDown()`方法会分别在每调用一个测试方法的前后分别被执行。比如在测试前启动数据库，可以在`setUp()`方法中连接数据库，在测试后关闭数据库，在`tearDown()`方法中关闭。这样不比在每个测试方法中重复相同的代码：

```python
class TestDict(unittest.TestCase):

    def setUp(self):
        print('setUp...')

    def tearDown(self):
        print('tearDown...')
```

**练习**

对Student类编写单元测试，结果测试不通过，修改Student类让测试通过：

```python
# -*- coding: utf-8 -*-
import unittest
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score
    def get_grade(self):
        if self.score < 0 or self.score > 100:
            raise ValueError
        if self.score >= 80:
            return 'A'
        if self.score >= 60:
            return 'B'
        if self.score >= 0:
            return 'C'

class TestStudent(unittest.TestCase):
    def test_80_to_100(self):
        s1 = Student('Bart', 80)
        s2 = Student('Lisa', 100)
        self.assertEqual(s1.get_grade(), 'A')
        self.assertEqual(s2.get_grade(), 'A')
    def test_60_to_80(self):
        s1 = Student('Bart', 60)
        s2 = Student('Lisa', 79)
        self.assertEqual(s1.get_grade(), 'B')
        self.assertEqual(s2.get_grade(), 'B')
    def test_0_to_60(self):
        s1 = Student('Bart', 0)
        s2 = Student('Lisa', 59)
        self.assertEqual(s1.get_grade(), 'C')
        self.assertEqual(s2.get_grade(), 'C')
    def test_invalid(self):
        s1 = Student('Bart', -1)
        s2 = Student('Lisa', 101)
        with self.assertRaises(ValueError):
            s1.get_grade()
        with self.assertRaises(ValueError):
            s2.get_grade()

if __name__ == '__main__':
    unittest.main()
```



### 文档测试

Python官方文档的示例代码可以写在注释中，由一些工具自动生成文档。文档中的代码可以使用内置的文档测试（doctest）模块自动执行，严格按照Python交互式命令行的输入和输出来判断测试结果是否正确：

```python
def abs(n):
    '''
    Function to get absolute value of number.
    
    Example:
    
    >>> abs(1)
    1
    >>> abs(-1)
    1
    >>> abs(0)
    0
    '''
    return n if n >= 0 else (-n)
```

使用doctest测试`Dict`类，可以用`...`表示中间一大段输出：

```python
# mydict2.py
class Dict(dict):
    '''
    Simple dict but also support access as x.y style.

    >>> d1 = Dict()
    >>> d1['x'] = 100
    >>> d1.x
    100
    >>> d1.y = 200
    >>> d1['y']
    200
    >>> d2 = Dict(a=1, b=2, c='3')
    >>> d2.c
    '3'
    >>> d2['empty']
    Traceback (most recent call last):
        ...
    KeyError: 'empty'
    >>> d2.empty
    Traceback (most recent call last):
        ...
    AttributeError: 'Dict' object has no attribute 'empty'
    '''
    def __init__(self, **kw):
        super(Dict, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

if __name__=='__main__':
    import doctest
    doctest.testmod()
```

运行`python mydict2.py`什么输出也没有说明编写的doctest运行正确，如果有问题再运行就会报错，比如把`__getattr__()`方法注释掉：

```python
C:\Users\Administrator.PC-20190504PBOJ>python C:\Users\Administrator.PC-20190504
PBOJ\Documents\mydict2.py
**********************************************************************
File "C:\Users\Administrator.PC-20190504PBOJ\Documents\mydict2.py", line 8, in _
_main__.Dict
Failed example:
    d1.x
Exception raised:
    Traceback (most recent call last):
      File "D:\python\lib\doctest.py", line 1329, in __run
        exec(compile(example.source, filename, "single",
      File "<doctest __main__.Dict[2]>", line 1, in <module>
        d1.x
    AttributeError: 'Dict' object has no attribute 'x'
**********************************************************************
File "C:\Users\Administrator.PC-20190504PBOJ\Documents\mydict2.py", line 14, in
__main__.Dict
Failed example:
    d2.c
Exception raised:
    Traceback (most recent call last):
      File "D:\python\lib\doctest.py", line 1329, in __run
        exec(compile(example.source, filename, "single",
      File "<doctest __main__.Dict[6]>", line 1, in <module>
        d2.c
    AttributeError: 'Dict' object has no attribute 'c'
**********************************************************************
# 不必担心doctest在非测试环境运行，模块导入不会执行，只有在命令行直接运行时才执行doctest
1 items had failures:
   2 of   9 in __main__.Dict
***Test Failed*** 2 failures.

```

**练习**

对函数`fact(n)`编写doctest并执行：

```python
def fact(n):
    '''
    Calculate 1*2*...*n
    
    >>> fact(1)
    1
    >>> fact(10)
    3628800
    >>> fact(-1)
    Traceback (most recent call last):
        ...
    ValueError
    '''
    if n < 1:
        raise ValueError()
    if n == 1:
        return 1
    return n * fact(n - 1)

if __name__ == '__main__':
    import doctest
    doctest.testmod()
```



## IO编程

1. 基本概念：input， output，stream
2. 存在问题：输入和接收速度不匹配
3. 解决方法：同步、异步(回调--好了叫我，轮询---好了没...好了没)
4. 编程语言都会把操作系统提供的低级C接口封装起来方便使用

### 文件读写

在磁盘上读写文件的功能都是由操作系统提供的，读写文件就是请求操作系统打开一个文件对象（通常称为文件描述符），通过操作系统提供的接口从这个文件对象中读取数据（读文件），或者把数据写入这个文件对象（写文件）。

**读文件**

使用Python内置`open()`函数，传入文件名和标识符：

```python
>>> f = open('/Users/lsaiah/test.txt', 'r')
```

如果文件不存在抛出`IOError`：

```python
>>> f=open('/Users/lsaiah/notfound.txt', 'r')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [Errno 2] No such file or directory: '/Users/lsaiah/notfound.txt'
```

文件打开成功，调用`read()`方法一次读取文件全部内容`str`到内存，最后调用`close()`关闭文件：

```python
>>> f.read()
'Hello, world!'
>>> f.close()
```

因为文件读写可能产生`IOError`，出错后不会调用`f.close()`，保证正确关闭文件，使用`try ... finally`实现：

```python
try:
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```

由于每次这么写太繁琐，引入`with`语句代码简洁且自动调用`close()`方法：

```python
with open('/path/to/file', 'r') as f:
    print(f.read())
```

调用`read()`一次性读取文件全部内容，如果文件过大内存就爆了，保险起见可以反复调用`read(size)`，每次读取size个字节的内容。电泳`readline()`每次读取一行内容，调用`readlines()`一次读取所以内容按行返回`list`。

```python
for line in f.readlines():
    print(line.strip()) # 把末尾的'\n'删掉
```

**file-like Object**

像`open()`函数返回的这种有个`read()`方法的对象，在Python中统称为file-like Object。file还可以是内存的字节流，网络流，自定义流等等。`StringIO`就是在内存中创建的file-like Object，常用作临时缓冲。

**二进制文件**

默认读取UTF-8编码的文本文件，要读取二进制文件如图片，视频，用`'rb'`模式打开文件：

```python
>>> f = open('/Users/lsaiah/test.jpg', 'rb')
>>> f.read()
b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节
```

**字符编码**

读取非UTF-8编码文本文件，要给`open()`函数传入`encoding`参数，遇到编码不规范的文件可能会遇到`UnicodeDecodeError`，这种情况`open()`函数还接收一个`errors`参数，表示如果遇到编码错误后如何处理。最简单的方式是直接忽略：

```python
>>> f = open('/Users/lsaiah/gbk.txt', 'r', encoding='gbk')
>>> f.read()
'测试'
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
```

**写文件**

写文件调用`open()`函数时，传入标识符`'w'`或者`'wb'`表示写文本文件或写二进制文件：

```python
>>> f = open('/Users/lsaiah/test.txt', 'w')
>>> f.write('Hello, world!')
>>> f.close()
```

`write()`不是立刻写入，而是先放到内存中空闲时写入，只有调用`close()`方法才能保证没有写入的数据全部写入磁盘，忘记`close()`可能造成数据丢失，所以还是用`with`语句保险：

```python
with open('/User/lsaiah/test.txt', 'w') as f:
    f.write('Hello, world!')
```

写入特定编码文本文件给`open()`函数传入`encoding`参数。以`w`模式写入文件会直接覆盖，追加要传入参数`a`写入。

| 字符  | 含义                                       |
| :---- | :----------------------------------------- |
| `'r'` | 打开阅读（默认）                           |
| `'w'` | 打开进行写入，先截断文件                   |
| `'x'` | 打开以进行独占创建，如果文件已经存在则失败 |
| `'a'` | 打开进行写入，如果存在则追加到文件末尾     |
| `'b'` | 二进制模式                                 |
| `'t'` | 文字模式（默认）                           |
| `'+'` | 开放进行更新（读写）                       |

 **练习**

将本地一个文本文件读为一个str并打印出来，注意文件路径不能用反斜杠“\”，在python中“\”表示转义，需要在路径前加r即保持字符原始值，或者替换为双反斜杠“\ \“，或者替换为正斜杠”/“。

```python
>>> fpath = 'C:/Users/lsaiah/test.txt'
>>>
>>> with open(fpath, 'r') as f:
...     s = f.read()
...     print(s)
...
hello
```



### StringIO和BytesIO

**StringIO**

StringIO是在内存中读取str。把str写入StringIO，要先创建一个StringIO，然后像文件一样写入即可，`getvalue()`用于获取写入后的str：

```python
>>> from io import StringIO
>>> f = StringIO()
>>> f.write('hello')
5
>>> f.write('')
1
>>> f.write('world!')
6
>>> print(f.getvalue())
hello world!
```

要读取StringIO，可以用一个str初始化StringIO，然后像读文件一样读取：

```python
>>> from io import StringIO
>>> f = StringIO('Hello!\nHi!\nGoodbye!')
>>> while True:
...     s = f.readline()
...     if s == '':
...         break
...     print(s.strip())
...
Hello!
Hi!
Goodbye!
```

**BytesIO**

StringIO只能操作str，如果操作二进制需要使用BytesIo。创建一个BytesIO，然后写入一些bytes，经过UTF-8编码的bytes：

```python
>>> from io import BytesIO
>>> f = BytesIO()
>>> f.write('中文'.encode('utf-8'))
6
>>> print(f.getvalue())
b'\xe4\xb8\xad\xe6\x96\x87'
```

要读取BytesIO和读StringIO类似，初始化，读文件：

```python
>>> from io import BytesIO
>>> f = BytesIO(b'\xe4\xb8\xad\xe6\x96\x87')
>>> f.read()
b'\xe4\xb8\xad\xe6\x96\x87'
```

初始化和`write`读取区别：

```python
f=StringIO('abc')
f.read() #返回'abc'
f.read() #返回'' 因为使用过一次read之后指针会发生移动
f.getvalue() #返回'abc' 因为getvalue不受指针影响

f=StringIO('')
f.write('abc')
f.read() #返回'' 因为write已经使指针发生了移动
f.getvalue() #返回'abc' 因为getvalue不受指针影响
f.seek(0) #解决方法：用seek将指针归零
f.read() #返回'abc'
```



### 操作文件和目录

Python内置的`os`模块可以直接调用操作系统提供的接口函数。`os.name`查看操作系统类型，Linux、Unix或Mac OS X是`posix`，Windows是`nt`，获取详细系统信息可以调用`os.uname()`。

```python
>>> import os
>>> os.name
'posix'
>>> os.uname()
posix.uname_result(sysname='Linux', nodename='VM-0-16-centos', release='4.18.0-193.6.3.el8_2.x86_64', version='#1 SMP Wed Jun 10 11:09:32 UTC 2020', machine='x86_64')
```

**环境变量**

操作系统定义的环境变量全部保存在`os.environ`变量中，要获取某个环境变量的值，可以调用`os.environ.get('key')`：

```python
>>> os.environ
...
>>> os.environ.get('PATH')
'/root/node-v15.5.1-linux-x64/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin'
>>> os.environ.get('x', 'default')
'default'
```

**操作文件和目录**

查看，创建，删除目录：

```python
# 查看当前目录的绝对路径:
>>> os.path.abspath('.')
'/Users/lsaiah'
# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
>>> os.path.join('/Users/lsaiah', 'testdir')
'/Users/lsaiah/testdir'
# 然后创建一个目录:
>>> os.mkdir('/Users/lsaiah/testdir')
# 删掉一个目录:
>>> os.rmdir('/Users/lsaiah/testdir')
```

合并路径不用拼接字符串，通过`os.path.join()`函数就可以处理不同操作系统的路径分隔符：

```python
# linux
part-1/part-2
# windows
part-1\part-2
```

同样拆分路径不用拆字符串，通过`os.path.split()`函数把一个路径拆分为两部分，最后一部分总是目录或文件名，`os.path.splitext()`可以直接得到文件扩展名：

```python
>>> os.path.split('/Users/lsaiah/testdir/file.txt')
('/Users/michael/testdir', 'file.txt')
>>> os.path.splitext('/path/to/file.txt')
('/path/to/file', '.txt')
```

合并拆分并不要求目录或文件真实存在，只对字符串操作。操作文件使用下面的函数：

```python
# 对文件重命名
>>> os.rename('test.txt', 'test.py')
# 删掉文件
>>> os.remove('test.py')
# 复制文件非操作系统提供，可以通过读写文件复制，或者使用shutil模块提供的copyfile()函数，它是os模块的补充。
shutil.copyfile(src, dst, *, follow_symlinks=True)
```

利用python特性过滤文件：

```python
# 如列出当前目录下的所有目录
>>> [x for x in os.listdir('.') if os.path.isdir(x)]
['.ssh', 'node-v15.5.1-linux-x64', '.pip', 'drone', '.forever', '.cache', 'UnblockNeteaseMusic', '.npm']
# 要列出所有的.py文件：
>>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
['test.py']
```

**练习**

1. 利用`os`模块编写一个能实现`dir -l`输出的程序。

   ```python
   #!/usr/bin/env python3
   # -*- coding: utf-8 -*-
   
   from datetime import datetime
   import os
   
   pwd = os.path.abspath('.')
   
   print('      Size     Last Modified  Name')
   print('------------------------------------------------------------')
   
   for f in os.listdir(pwd):
       fsize = os.path.getsize(f)
       mtime = datetime.fromtimestamp(os.path.getmtime(f)).strftime('%Y-%m-%d %H:%M')
       flag = '/' if os.path.isdir(f) else ''
       print('%10d  %s  %s%s' % (fsize, mtime, f, flag))
   ```

   

2. 编写一个程序，能在当前目录以及当前目录的所有子目录下查找文件名包含指定字符串的文件，并打印出相对路径。

   ```python
   import os
   def find(path,key):
       count_dirs = count_files = 0
       for root, dirs, files in os.walk(path):
           for x in files:
               if key in x:
                   print(os.path.join(root,x),'文件')
                   count_files += 1
           for y in dirs:
               if key in y:
                   print(os.path.join(root,y),'目录')
                   count_dirs += 1
   #
   import os
   def main():
       paths = '.'       # 具体查找路径在此更改
       os.chdir(paths)   # 查找路径为默认路径可忽略此行
       keyword = input('请输入要查找的关键字: ').lower()
       search_file(paths, keyword)
   def search_file(pat, kwd):
       all_files = [x for x in os.listdir(pat)]
       for file in all_files:
           if kwd in file.lower():
               print(os.path.join(pat, file))
       folders = [x for x in os.listdir(pat) if os.path.isdir(x)]
       for folder in folders:
           new_path = os.path.join(pat, folder)
           search_file(new_path, kwd)
   if __name__ == '__main__':
       main()
   #
   import os
   def search(path,key):
       for x in os.listdir(path):
           #如果是文件，打印
           if os.path.isfile(os.path.join(path,x)):
               if key in x:
                print(os.path.join(path,x))
           #如果是目录，递归
           if os.path.isdir(os.path.join(path,x)):
               new_path = os.path.join(path,x)
               search(new_path,key)
   ```
   
   

### 序列化

程序运行过程中，所有变量都在内存中，一旦程序结束变量所占用的内存就被操作系统全部回收。把变量从内存中变成可存储或可传输的过程称为序列化，在Python中叫pickling，其它语言中被称之为serialization，marshalling，flattening等。序列化之后就可以把内容写入磁盘，或者通过网络传输到别的机器上。反过来把变量内容从序列化的对象重新读到内存里称之为反序列化unpickling。

Python通过`pickle`模块实现序列化，首先把一个对象序列化并写入文件，`pickle.dumps()`方法把任意对象序列化成一个`bytes`：

```python
>>> import pickle
>>> d = dict(name = 'Bob', age = 20, score = 88)
>>> pickle.dumps(d)
b'\x80\x04\x95$\x00\x00\x00\x00\x00\x00\x00}\x94(\x8c\x04name\x94\x8c\x03Bob\x94
\x8c\x03age\x94K\x14\x8c\x05score\x94KXu.'
```

或者用另一个方法`pickle.dump()`直接把对象序列化后写入一个file-like Object：

```python
>>> f = open('dump.txt', 'wb')
>>> pickle.dump(d, f)
>>> f.close()
```

写入的`dump.txt`文件就是Python保存的对象内部信息。当把对象从磁盘读到内存时，可以先把内容读到一个`bytes`，然后用`pickle.loads()`方法反序列化出对象，也可以直接用`pickle.load()`方法从一个`file-like Object`中直接反序列化出对象。打开另一个Python命令行来反序列化刚才保存的对象：

```python
>>> f = open('dump.txt', 'rb')
>>> d = pickle.load(f)
>>> f.close()
>>> d
{'age': 20, 'score': 88, 'name': 'Bob'}
```

这个变量和原来的变量是完全不相干的对象，只是内容相同。

**JSON**

如果在不同编程语言之间传递对象，就必须把对象序列化成标准格式，如XML，更好的方法是序列化成JSON，因为JSON表示出来就是一个字符串，可以被所有语言读取，也可以方便的存储或传输。JSON不但是标准格式，并且比XML更快。JSON标准规定编码是UTF-8，所以总能正确地将Python的`str`与JSON字符串转换。

JSON表示的对象就是标准的JavaScript语言的对象，JSON和Python内置的数据类型对应如下：

| JSON类型   | Python类型 |
| :--------- | :--------- |
| {}         | dict       |
| []         | list       |
| "string"   | str        |
| 1234.56    | int或float |
| true/false | True/False |
| null       | None       |

Python内置的`json`模块提供了非常完善的Python对象到JSON格式的转换。把Python对象变成一个JSON：

```python
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```

`dumps()`方法返回一个`str`，内容就是标准的JSON。类似的，`dump()`方法可以直接把JSON写入一个`file-like Object`。

要把JSON反序列化为Python对象，用`loads()`把JSON字符串反序列化，或者用的`load()`方法从`file-like Object`中读取字符串并反序列化：

```python
>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}
```

**JSON进阶**

Python的`dict`对象可以直接序列化成JSON的`{}`，不过一般用`class`表示对象，比如定义`Student`类序列化：

```python
import json

class Student(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score

s = Student('Bob', 20, 88)
print(json.dumps(s))
```

运行报错：

```python
>>> import json
>>>
>>> class Student(object):
...     def __init__(self, name, age, score):
...         self.name = name
...         self.age = age
...         self.score = score
...
>>> s = Student('Bob', 20, 88)
>>> print(json.dumps(s))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "D:\python\lib\json\__init__.py", line 231, in dumps
    return _default_encoder.encode(obj)
  File "D:\python\lib\json\encoder.py", line 199, in encode
    chunks = self.iterencode(o, _one_shot=True)
  File "D:\python\lib\json\encoder.py", line 257, in iterencode
    return _iterencode(o, 0)
  File "D:\python\lib\json\encoder.py", line 179, in default
    raise TypeError(f'Object of type {o.__class__.__name__} '
TypeError: Object of type Student is not JSON serializable
```

错误原因是`Student`对象不是一个可序列化为JSON的对象，查看`dumps()`[方法的参数列表](https://docs.python.org/zh-cn/3/library/json.html#json.dumps)发现除了第一个`obj`参数外，还有可选参数用来定制JSON序列化，前面报错是因为默认情况下，`dumps()`方法不知道如何将`Student`实例变为一个JSON的`{}`对象。可选参数`default`就是把任意一个对象变成一个可序列为JSON的对象，为`Student`专门写一个转换函数`student2dict`传入即可。

```python
def student2dict(std):
    return {
        'name': std.name,
        'age': std.age,
        'score': std.score
    }
```

这样`Student`实例首先被`student2dict`函数转换为`dict`，然后再被顺利序列化为JSON。

```python
>>> print(json.dumps(s, default=student2dict))
{"age": 20, "name": "Bob", "score": 88}
```

不过下次如果遇到一个`Teacher`类的实例，照样无法序列化成JSON。通常`class`的实例都有一个`__dict__`属性就是一个`dict`用来存储变量，也有少数例外比如定义了`__slots__`的class。所以可以这样写：

```python
print(json.dumps(s, default=lambda obj: obj.__dict__))
```

同样如果要把JSON反序列化为一个`Student`对象实例，先用`loads()`方法转换出一个`dict`对象，然后传入的`object_hook`函数负责把`dict`转换为`Student`实例：

```python
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])
```

```python
>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> print(json.loads(json_str, object_hook=dict2student))
<__main__.Student object at 0x10cd3c190>
```

**练习**

对中文进行JSON序列化，`json.dumps()`提供了一个`ensure_ascii`参数。如果 *ensure_ascii* 是 true （即默认值），输出保证将所有输入的非 ASCII 字符转义。如果 *ensure_ascii* 是 false，这些字符会原样输出。

```python
import json
obj = dict(name='小明', age=20)
s = json.dumps(obj, ensure_ascii=True)
```



## 进程和线程

真正的并行执行多任务只能在多核CPU上实现，单核CPU交替执行速度快感觉像是同时执行。在操作系统上一个任务就启动一个进程Process，子任务就启动线程。每个进程至少有一个线程，多个线程和多进程执行方式一样交替运行。如果要执行多个任务，有三种方式：

1. 多进程模式（启动多个进程，每个进程只有一个线程）
2. 多线程模式（启动一个进程，在一个进程里启动多个线程）
3. 多进程+多线程模式（启动多个进程，每个进程启动多个线程）

同时执行多个任务，任务之间没有关联，而是需要相互通信和协调。

### 多进程

Python实现多进程（multiprocessing），在Unix/Linux操作系统提供了一个`fork()`系统调用，普通函数调用一次返回一次，而`fork()`调用一次返回两次，因为操作系统自动把当前进程（父进程）复制了一分（子进程），然后在父进程返回子进程ID，在子进程内返回`0`，一个父进程可以`fork()`出很多子进程，所以父进程要记下每个子进程的ID，子进程调用`getppid()`就可以拿到父进程的ID。

Python的`os`模块封装了常见的系统调用其中就包含`fork()`：

```python
import os
print('Process (%s) start...' % os.getpid())
# Only works on Unix/Linux/Mac:
pid = os.fork()
if pid == 0:
    print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid()))
else:
    print('I (%s) just created a child process (%s).' % (os.getpid(), pid))
```

```python
Process (1007716) start...
I (1007716) just created a child process (1007725).
I am child process (1007725) and my parent is 1007716.
```

由于Windows没有`fork()`调用无法运行，在Unix/Linux系统中可以调用，而Mac系统是基于BSD（Unix的一种内核）所以也可以调用。常见的Apache服务器就是由父进程监听端口，当有新的http请求时就fork出子进程处理新的http请求。

**multiprocessing**

由于Python是跨平台的，Windows没有`fork()`调用，但可以使用`multiprocessing`模块实现跨平台多进程。`multiprocessing`模块提供了一个`Process`类来代表一个进程对象，创建一个子进程传入一个执行函数和函数的参数，用`start()`方法启动一个子进程，`join()`方法等待子进程结束再继续往下运行，用于进程同步：

```python
from multiprocessing import Process
import os

# 子进程要执行的代码
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('Child process will start.')
    p.start()
    p.join()
    print('Child process end.')
```

```python
Parent process 928.
Child process will start.
Run child process test (929)...
Process end.
```

**Pool**

如果要启动大量子进程，可以用进程池的方式批量创建子进程：

```python
from multiprocessing import Pool
import os, time, random

def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        p.apply_async(long_time_task, args=(i,))
    print('Waiting for all subprocesses done...')
    p.close()
    p.join()
    print('All subprocesses done.')
```

```python
Parent process 669.
Waiting for all subprocesses done...
Run task 0 (671)...
Run task 1 (672)...
Run task 2 (673)...
Run task 3 (674)...
Task 2 runs 0.14 seconds.
Run task 4 (673)...
Task 1 runs 0.27 seconds.
Task 3 runs 0.86 seconds.
Task 0 runs 1.41 seconds.
Task 4 runs 1.91 seconds.
All subprocesses done.
```

对`Pool`对象调用`join()`方法会等待所有子进程执行完毕，调用`join()`之前必须先调用`close()`，调用`close()`之后就不能继续添加新的`Process`了。task `0`，`1`，`2`，`3`是立刻执行的，而task `4`要等待前面某个task完成后才执行，这是因为`Pool`的默认大小是CPU核数，这里指定大小`Pool(4)`因此最多同时执行4个进程。如果要跑满8核CPU，要提交至少9个子进程。

**子进程是外部进程**

很多时候子进程并不是自身而是一个外部进程，创建子进程还需要控制子进程的输入和输出，使用`subprocess`模块，在Python代码中运行命令`nslookup www.python.org`：

```python
import subprocess
print('$ nslookup www.python.org')
r = subprocess.call(['nslookup', 'www.python.org'])
print('Exit code:', r)
```

```python
$ nslookup www.python.org
Server:		192.168.19.4
Address:	192.168.19.4#53

Non-authoritative answer:
www.python.org	canonical name = python.map.fastly.net.
Name:	python.map.fastly.net
Address: 199.27.79.223

Exit code: 0
```

如果子进程还需要输入，可以通过`communicate()`方法输入，如在命令行执行命令`nslookup`后手动输入：

```python
set q=mx
python.org
exit
```

```python
import subprocess

print('$ nslookup')
p = subprocess.Popen(['nslookup'], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
output, err = p.communicate(b'set q=mx\npython.org\nexit\n')
print(output.decode('utf-8'))
print('Exit code:', p.returncode)
```

```python
$ nslookup
Server:		192.168.19.4
Address:	192.168.19.4#53

Non-authoritative answer:
python.org	mail exchanger = 50 mail.python.org.

Authoritative answers can be found from:
mail.python.org	internet address = 82.94.164.166
mail.python.org	has AAAA address 2001:888:2000:d::a6

Exit code: 0
```

**进程间通信**

Python的`multiprocessing`模块包装了底层的机制，提供了`Queue`、`Pipes`等多种方式来交换数据。

以`Queue`为例，在父进程中创建两个子进程，一个往`Queue`里写数据，一个从`Queue`里读数据：

```python
from multiprocessing import Process, Queue
import os, time, random

# 写数据进程执行的代码:
def write(q):
    print('Process to write: %s' % os.getpid())
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

# 读数据进程执行的代码:
def read(q):
    print('Process to read: %s' % os.getpid())
    while True:
        value = q.get(True)
        print('Get %s from queue.' % value)

if __name__=='__main__':
    # 父进程创建Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    # 启动子进程pw，写入:
    pw.start()
    # 启动子进程pr，读取:
    pr.start()
    # 等待pw结束:
    pw.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止:
    pr.terminate()
```

```python
Process to write: 50563
Put A to queue...
Process to read: 50564
Get A from queue.
Put B to queue...
Get B from queue.
Put C to queue...
Get C from queue.
```

在Windows中没有`fork`调用，因此`multiprocessing`需要模拟出`fork`的效果，父进程所有Python对象都必须通过pickle序列化再传到子进程去。如果multiprocessing在Windows下调用失败了，要先考虑是不是pickle失败了。

以`Pipe()`为例，本质是进程之间的数据传递而不是数据共享，这和socket有点像。`pipe()`返回两个连接对象分别表示管道的两端，每端都有`send()`和`recv()`方法。如果两个进程试图在同一时间的同一端进行读取和写入那么可能会损坏管道中的数据。

```python
from multiprocessing import Process, Pipe
 
def f(conn):
    conn.send([42, None, 'hello'])
    conn.close()
 
if __name__ == '__main__':
    parent_conn, child_conn = Pipe() 
    p = Process(target=f, args=(child_conn,))
    p.start()
    print(parent_conn.recv())   # prints "[42, None, 'hello']"
    p.join()
```



### 多线程

| 方法            | 注释                                                         |
| --------------- | ------------------------------------------------------------ |
| start()         | 线程准备就绪，等待CPU调度                                    |
| setName()       | 为线程设置名称                                               |
| getName()       | 获取线程名称                                                 |
| setDaemon(True) | 设置为守护线程                                               |
| join()          | 逐个执行每个线程，执行完毕后继续往下执行                     |
| run()           | 线程被cpu调度后自动执行线程对象的run方法，如果想自定义线程类，直接重写run方法就行了 |

线程是操作系统直接执行的单元，Python线程是Posix Thread，标准库提供两个模块：`_thread`低级模块和`threading`高级模块，高级模块对低级模块进行了封装，启动一个线程就是把一个函数传入并创建`Thread`实例，然后调用`start()`开始执行：

```python
import time, threading

# 新线程执行的代码:
def loop():
    print('thread %s is running...' % threading.current_thread().name)
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s ended.' % threading.current_thread().name)

print('thread %s is running...' % threading.current_thread().name)
t = threading.Thread(target=loop, name='LoopThread')
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name)
```

```python
thread MainThread is running...
thread LoopThread is running...
thread LoopThread >>> 1
thread LoopThread >>> 2
thread LoopThread >>> 3
thread LoopThread >>> 4
thread LoopThread >>> 5
thread LoopThread ended.
thread MainThread ended.
```

任何进程默认启动一个线程称为主线程，主线程又可以启动新的线程，Python的`threading`模块有个`current_thread()`函数，它可以永远返回当前线程的实例，主线程实例的名字叫`MainThread`，子线程的名字在创建时指定，用`LoopThread`命名子线程。名字仅仅在打印时用来显示没有其他意义，如果不起名字Python就自动给线程命名为`Thread-1`，`Thread-2`……

**Lock**

多进程中同一个变量各自在各自的进程中互不影响，而在多线程中，多个线程共享变量，任何一个变量都可以被任何一个线程修改，因此多个线程改一个变量内容就乱了：

```python
import time, threading

# 假定这是你的银行存款:
balance = 0

def change_it(n):
    # 先存后取，结果应该为0:
    global balance
    balance = balance + n
    balance = balance - n

def run_thread(n):
    for i in range(2000000):
        change_it(n)

t1 = threading.Thread(target=run_thread, args=(5,))
t2 = threading.Thread(target=run_thread, args=(8,))
t1.start()
t2.start()
t1.join()
t2.join()
print(balance)
```

定义了一个共享变量`balance`，初始值是0，并且启动了两个线程，先存后取，理论结果应该是0。但是由于线程调度是操作系统决定的，当t1和t2交替执行时，只要循环次数足够多，结果就不一定是0。因为高级语言的一条语句在CPU执行时是若干语句，即使一个简单的语句：

```python
balance = balance + n
```

分为两步：

1. 计算`balance + n`存入临时变量中
2. 将临时变量赋值给`balance`

```python
x = balance + n
balance = x
```

由于x是局部变量，两个线程都有各自的x，正常执行：

```python
初始值 balance = 0

t1: x1 = balance + 5 # x1 = 0 + 5 = 5
t1: balance = x1     # balance = 5
t1: x1 = balance - 5 # x1 = 5 - 5 = 0
t1: balance = x1     # balance = 0

t2: x2 = balance + 8 # x2 = 0 + 8 = 8
t2: balance = x2     # balance = 8
t2: x2 = balance - 8 # x2 = 8 - 8 = 0
t2: balance = x2     # balance = 0
    
结果 balance = 0
```

但是t1和t2交替运行，如果操作系统以下面的顺序执行t1、t2。修改`balance`需要多条语句，线程可能中断，从而导致多个线程把一个对象的内容改乱了，两个线程一存一取就可能造成余额不足。

```python
初始值 balance = 0

t1: x1 = balance + 5  # x1 = 0 + 5 = 5

t2: x2 = balance + 8  # x2 = 0 + 8 = 8
t2: balance = x2      # balance = 8

t1: balance = x1      # balance = 5
t1: x1 = balance - 5  # x1 = 5 - 5 = 0
t1: balance = x1      # balance = 0

t2: x2 = balance - 8  # x2 = 0 - 8 = -8
t2: balance = x2      # balance = -8

结果 balance = -8
```

所以要确保一个线程修改`balance`时别的线程一定不能改。给`change_it()`上一把锁，当某个线程开始执行`change_it()`时，该线程因为获得了锁因此其它线程不能同时执行，只能等待直到锁被释放后，获得该锁以后才能改。由于锁只有一个，无论多少线程，同一时刻最多只有一个线程持有该锁，所以不会造成修改的冲突。创建一个锁就是通过`threading.Lock()`来实现：

```python
balance = 0
lock = threading.Lock()
def run_thread(n):
    for i in range(100000):
        # 先获取锁
        lock.acquire()
        try:
            # 锁了之后只能有一个线程改
            change_it(n)
        finally：
        	# 改完了释放锁，别的线程才能改
        	lock.release()
```

多个线程同时执行`lock.acquire()`时，只有一个线程能成功获取锁，其它线程只能继续等待直到获取锁，所以用`try...finally`一定释放锁，避免死线程。锁的好处就是确保某段关键代码只能由一个线程执行，不会被其它线程影响。坏处就是单线程模式，不能并发，效率降低，可能存在多个锁的时候，不同线程不同锁，获取锁的时候可能造成死锁导致线程全部挂起，不能执行无法结束。

**多核CPU**

一个死循环线程会100%占用一个CPU，如果有两个死循环在多核CPU中可能占用200%CPU。N核CPU全部跑满，必须启动N个死循环。

```python
import threading, multiprocessing

def loop():
    x = 0
    while True:
        x = x ^ 1

for i in range(multiprocessing.cpu_count()):
    t = threading.Thread(target=loop)
    t.start()
```

启动与CPU核心数量相同的N个线程死循环，在4核CPU上可以监控到CPU占用率仅有102%，也就是仅使用了一核。但是用C、C++或Java改写相同的死循环可以把全部核心跑满，4核400%占用。因为Python的解释器有GIL锁（Global Interpreter Lock），每执行100条字节码自动释放GIL锁，所以在Python中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核。不能完全利用多核，除非写一个不带GIL的解释器。虽然不能利用多线程实现多核任务，但可以通过多进程实现多核任务，各自进程的GIL锁互不影响。

### ThreadLocal

每个线程使用自己的局部变量不会影响其它线程，而全局变量的修改必须加锁。但是局部变量在函数调用时传递很麻烦：

```python
def process_student(name):
    std = Student(name)
    # std是局部变量，但是每个函数都要用它，因此必须传进去：
    do_task_1(std)
    do_task_2(std)

def do_task_1(std):
    do_subtask_1(std)
    do_subtask_2(std)

def do_task_2(std):
    do_subtask_2(std)
    do_subtask_2(std)
```

每个函数一层一层调用传参数很麻烦，用一个全局`dict`存放所有`Student`对象，然后以`thread`自身作为`key`获得线程对应的`Student`对象。[`threading.current_thread()`返回当前线程的对象](https://docs.python.org/zh-cn/3/library/threading.html#threading.current_thread)：

```python
global_dict = {}

def std_thread(name):
    std = Student(name)
    # 把std放到全局变量global_dict中：
    global_dict[threading.current_thread()] = std
    do_task_1()
    do_task_2()

def do_task_1():
    # 不传入std，而是根据当前线程查找：
    std = global_dict[threading.current_thread()]
    ...

def do_task_2():
    # 任何函数都可以查找出当前线程的std变量：
    std = global_dict[threading.current_thread()]
    ...
```

这样就解决了`std`在每层函数中的传递问题，但还不够简单，使用`ThreadLocal`不用查找`dict`：

```python
import threading
    
# 创建全局ThreadLocal对象:
local_school = threading.local()

def process_student():
    # 获取当前线程关联的student:
    std = local_school.student
    print('Hello, %s (in %s)' % (std, threading.current_thread().name))

def process_thread(name):
    # 绑定ThreadLocal的student:
    local_school.student = name
    process_student()

t1 = threading.Thread(target= process_thread, args=('Alice',), name='Thread-A')
t2 = threading.Thread(target= process_thread, args=('Bob',), name='Thread-B')
t1.start()
t2.start()
t1.join()
t2.join()
# 执行结果：
Hello, Alice (in Thread-A)
Hello, Bob (in Thread-B)
```

全局变量`local_school`就是一个`ThreadLocal`对象，每个`Thread`对它都可以读写`student`属性互不影响。可以把`local_school`看成全局变量是一个`dict`，每个属性如`local_school.student`都是线程的局部变量，还可以绑定其他变量，如`local_school.teacher`等等。可以任意读写而互不干扰也不用管理锁的问题，`ThreadLocal`内部会处理。`ThreadLocal`最常用的地方就是为每个线程绑定一个数据库连接，HTTP请求，用户身份信息等，这样一个线程的所有调用到的处理函数都可以非常方便地访问这些资源。

### 进程和线程对比

**区别**

1. 同一个进程中的线程共享同一内存空间，但是进程之间是独立的。
2. 同一个进程中的所有线程的数据是共享的（进程通讯），进程之间的数据是独立的。
3. 对主线程的修改可能会影响其他线程的行为，但是父进程的修改（除了删除以外）不会影响其他子进程。
4. 线程是一个上下文的执行指令，而进程则是与运算相关的一簇资源。
5. 同一个进程的线程之间可以直接通信，但是进程之间的交流需要借助中间代理来实现。
6. 创建新的线程很容易，但是创建新的进程需要对父进程做一次复制。
7. 一个线程可以操作同一进程的其他线程，但是进程只能操作其子进程。
8. 线程启动速度快，进程启动速度慢（但是两者运行速度没有可比性）

**优缺点**

要实现多个任务，通常设计Master-Worker模式，Master负责分配任务，Worker负责执行任务。通常是一个主Master和多个次Worker。

多进程的优点就是稳定性高，一个子进程挂了不会影响主进程和其它子进程。缺点是创建进程代价大，Windows的multiprocessing模拟`fork`，另外操作系统同时运行的进程有限受内存和CPU限制。如最早的Apache服务器就是多进程。

多线程模式通常比多进程快点，缺点就是任何一个线程挂掉可能直接造成进程崩溃，因为所有线程共享进程的内存，在windwos下多线程的效率比多进程要高，但稳定性差，如IIS服务器。

为了解决效率问题和稳定性问题，IIS和Apache现在用多进程+多线程混合模式。

**线程切换**

任务多效率低，在操作系统中切换进程或者线程需要先保存当前执行的现场环境（CPU寄存器状态、内存页等），然后把新任务的执行环境准备好（恢复上次的寄存器状态，切换内存页等）才能开始执行。这个切换过程虽然很快，但是也需要耗费时间。如果有几千个任务同时进行，操作系统可能就主要忙着切换任务，没有多少时间去执行任务了。

**计算密集型和IO密集型**

多任务还要考虑任务的类型，计算密集型任务主要消耗CPU资源，任务越多，花在任务切换的时间就越多，CPU执行任务的效率就越低，高效利用CPU计算密集型任务同时进行的数量应当等于CPU的核心数。Python运行效率低不适合，最好用C语言。

IO密集型设计到网络、磁盘IO消耗CPU资源少，任务大多等待IO完成，任务越多CPU效率越高，最合适的是开发效率高的脚本语言如Python，C语言最差。

**异步IO协程**

考虑到CPU和IO之间巨大的速度差异，一个任务在执行的过程中大部分时间都在等待IO操作，单进程单线程模型会导致别的任务无法并行执行，因此才需要多进程模型或者多线程模型来支持多任务并发执行。

现代操作系统对IO操作已经做了巨大的改进，最大的特点就是支持异步IO。如果充分利用操作系统提供的异步IO支持，就可以用单进程单线程模型来执行多任务，这种全新的模型称为事件驱动模型，Nginx就是支持异步IO的Web服务器，它在单核CPU上采用单进程模型就可以高效地支持多任务。在多核CPU上可以运行多个进程（数量与CPU核心数相同）充分利用多核CPU。由于系统总的进程数量十分有限，因此操作系统调度非常高效。用异步IO编程模型来实现多任务是一个主要的趋势。Python单线程的异步编程模型称为协程，有了协程的支持，就可以基于事件驱动编写高效的多任务程序。

### 分布式进程

进程比线程更稳定，并且可以分布到多台机器，而线程只能在一台机器的多个CPU上。Python的`multiprocessing`模块不但支持多进程，其中`managers`子模块还支持依靠网络通信把多进程分布到多台机器上。如通过`Queue`通信的多进程程序在同一台机器运行，由于处理任务的进程任务繁重，要把发送任务的进程和处理任务的进程分布到两台机器上，用分布式实现，原有的`Queue`可以继续使用，通过`managers`模块把`Queue`通过网络通信传输就可以让其他机器的进程访问`Queue`了。

服务进程负责启动`Queue`，把`Queue`注册到网络上，然后往`Queue`写入任务：

```python
# task_master.py

import random, time, queue
from multiprocessing.managers import BaseManager

# 发送任务的队列:
task_queue = queue.Queue()
# 接收结果的队列:
result_queue = queue.Queue()

# 从BaseManager继承的QueueManager:
class QueueManager(BaseManager):
    pass

# 把两个Queue都注册到网络上, callable参数关联了Queue对象:
QueueManager.register('get_task_queue', callable=lambda: task_queue)
QueueManager.register('get_result_queue', callable=lambda: result_queue)
# 绑定端口5000, 设置验证码'abc':
manager = QueueManager(address=('', 5000), authkey=b'abc')
# 启动Queue:
manager.start()
# 获得通过网络访问的Queue对象:
task = manager.get_task_queue()
result = manager.get_result_queue()
# 放几个任务进去:
for i in range(10):
    n = random.randint(0, 10000)
    print('Put task %d...' % n)
    task.put(n)
# 从result队列读取结果:
print('Try get results...')
for i in range(10):
    r = result.get(timeout=10)
    print('Result: %s' % r)
# 关闭:
manager.shutdown()
print('master exit.')
```

Windows，pickle模块不能序列化lambda function，所以需要自定义函数实现序列化，修改`task_manger.py`：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import random, time, queue
from multiprocessing.managers import BaseManager
from multiprocessing import freeze_support

# 发送任务的队列:
task_queue = queue.Queue()
# 接收结果的队列:
result_queue = queue.Queue()

def return_task_queue():
    global task_queue
    return task_queue

def return_result_queue():
    global result_queue
    return result_queue

# 从BaseManager继承的QueueManager:
class QueueManager(BaseManager):
    pass

def test():
# 把两个Queue都注册到网络上, callable参数关联了Queue对象:
    QueueManager.register('get_task_queue', callable=return_task_queue)
    QueueManager.register('get_result_queue', callable=return_result_queue)
# 绑定端口5000, 设置验证码'abc':
    manager = QueueManager(address=('127.0.0.1', 5000), authkey=b'abc')
# 启动Queue:
    manager.start()
# 获得通过网络访问的Queue对象:
    task = manager.get_task_queue()
    result = manager.get_result_queue()
# 放几个任务进去:
    for i in range(10):
        n = random.randint(0, 10000)
        print('Put task %d...' % n)
        task.put(n)
# 从result队列读取结果:
    print('Try get results...')
    for i in range(10):
        r = result.get(timeout=10)
        print('Result: %s' % r)
# 关闭:
    manager.shutdown()
    print('master exit.')

if __name__ == '__main__':
    freeze_support()
    print("start")
    test()
```

在一台机器上写多进程程序时创建`Queue`可以直接拿来用，但是在分布式多进程下，添加任务不能对原始的`task_queue`进行操作，必须通过`manager.get_task_queue()`获得的`Queue`接口添加。

然后在另一台机器上启动任务进程（本机启动也可以）：

```python
# task_worker.py

import time, sys, queue
from multiprocessing.managers import BaseManager

# 创建类似的QueueManager:
class QueueManager(BaseManager):
    pass

# 由于这个QueueManager只从网络上获取Queue，所以注册时只提供名字:
QueueManager.register('get_task_queue')
QueueManager.register('get_result_queue')

# 连接到服务器，也就是运行task_master.py的机器:
server_addr = '127.0.0.1'
print('Connect to server %s...' % server_addr)
# 端口和验证码注意保持与task_master.py设置的完全一致:
m = QueueManager(address=(server_addr, 5000), authkey=b'abc')
# 从网络连接:
m.connect()
# 获取Queue的对象:
task = m.get_task_queue()
result = m.get_result_queue()
# 从task队列取任务,并把结果写入result队列:
for i in range(10):
    try:
        n = task.get(timeout=1)
        print('run task %d * %d...' % (n, n))
        r = '%d * %d = %d' % (n, n, n*n)
        time.sleep(1)
        result.put(r)
    except Queue.Empty:
        print('task queue is empty.')
# 处理结束:
print('worker exit.')
```

任务进程通过网络连接到服务进程，先启动`task_master.py`服务进程：

```python
$ python task_master.py 
start
Put task 433...
Put task 1446...
Put task 1540...
Put task 7959...
Put task 9413...
Put task 6516...
Put task 7613...
Put task 54...
Put task 6842...
Put task 3245...
Try get results...
```

`task_master.py`进程发送完任务后，开始等待`result`队列的结果。现在启动`task_worker.py`进程：

```python
$ python task_worker.py
Connect to server 127.0.0.1...
run task 433 * 433...
run task 1446 * 1446...
run task 1540 * 1540...
run task 7959 * 7959...
run task 9413 * 9413...
run task 6516 * 6516...
run task 7613 * 7613...
run task 54 * 54...
run task 6842 * 6842...
run task 3245 * 3245...
worker exit.
```

`task_worker.py`进程结束，在`task_master.py`进程中会继续打印出结果：

```python
Result: 433 * 433 = 187489
Result: 1446 * 1446 = 2090916
Result: 1540 * 1540 = 2371600
Result: 7959 * 7959 = 63345681
Result: 9413 * 9413 = 88604569
Result: 6516 * 6516 = 42458256
Result: 7613 * 7613 = 57957769
Result: 54 * 54 = 2916
Result: 6842 * 6842 = 46812964
Result: 3245 * 3245 = 10530025
master exit.
```

这就是Master/Worker模型，简单的分布式计算，比如把`n*n`换成发邮件，启动多个Worker就可以把任务分布到多个机器上，实现了邮件队列异步发送。

Queue对象存储在`task_master.py`进程中：

```ascii
                                             │
┌─────────────────────────────────────────┐     ┌──────────────────────────────────────┐
│task_master.py                           │  │  │task_worker.py                        │
│                                         │     │                                      │
│  task = manager.get_task_queue()        │  │  │  task = manager.get_task_queue()     │
│  result = manager.get_result_queue()    │     │  result = manager.get_result_queue() │
│              │                          │  │  │              │                       │
│              │                          │     │              │                       │
│              ▼                          │  │  │              │                       │
│  ┌─────────────────────────────────┐    │     │              │                       │
│  │QueueManager                     │    │  │  │              │                       │
│  │ ┌────────────┐ ┌──────────────┐ │    │     │              │                       │
│  │ │ task_queue │ │ result_queue │ │<───┼──┼──┼──────────────┘                       │
│  │ └────────────┘ └──────────────┘ │    │     │                                      │
│  └─────────────────────────────────┘    │  │  │                                      │
└─────────────────────────────────────────┘     └──────────────────────────────────────┘
                                             │

                                          Network
```

而`Queue`之所以能通过网络访问，就是通过`QueueManager`实现的。由于`QueueManager`管理的不止一个`Queue`，所以要给每个`Queue`的网络调用接口起个名字，比如`get_task_queue`。要确保IP，端口，authkey一致才能连接。