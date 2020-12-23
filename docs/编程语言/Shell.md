## 简介

Shell 编程需要编辑器和能解释执行的脚本解释器，Linux Shell的常见种类有：

- Bourne Shell（/usr/bin/sh或/bin/sh）
- Bourne Again Shell（/bin/bash）
- C Shell（/usr/bin/csh）
- K Shell（/usr/bin/ksh）
- Shell for Root（/sbin/sh）

主要学习Bash[Bourne Again Shell（/bin/bash）]，`#!` 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell，扩展名为.sh

```shell
#!/bin/bash
echo "Hello World!"
```

#### 运行方法

1. 作为可执行程序，/bin/bash不在PATH里，需要指定当前目录下的Shell文件。
   
   ```shell
   chmod +x ./test.sh  #使脚本具有执行权限
   ./test.sh  #执行脚本
   ```
2. 作为解释器参数，直接运行解释器，参数是Shell脚本文件名，不需要在文件第一行指定解释器信息。
   
   ```shell
   /bin/sh test.sh
   /bin/php test.php
   ```

## 变量

```
your_name="lsaiah.cn"
# 重新定义
your_name="qwq.dog"
```

- 和PHP不一样，不需加 $ 美元符号
- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
- 中间不能有空格，可以使用下划线（_）。
- 不能使用标点符号。
- 不能使用bash里的关键字（可用help命令查看保留关键字）

使用语句将/etc目录下的文件名循环赋值给变量file：

```
for file in `ls /etc`
或
for file in $(ls /etc)
```

### 使用变量

将变量名用`{ }`括好，前面加美元符号即可使用。

```
your_name="qinjx"
echo ${your_name}
```

### 只读变量

使用readonly可以将变量定义为只读变量，只读变量不能被改变。

```
#!/bin/bash
myUrl="https://www.google.com"
readonly myUrl
myUrl="https://www.baidu.com"
# 运行报错
/bin/sh: NAME: This variable is read only.
```

### 删除变量

使用unset可以删除变量，删除后不能再次使用，不能删除只读变量。

```
unset variable_name
```

### 变量类型

- 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
- 环境变量所有的程序都能访问，有些程序需要环境变量来保证其正常运行。也可在Shell脚本中定义环境变量。、
- Shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量。

## 字符串

### 单引号

```
str='this is a string'
```

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符号也不行），但可成对出现，作为字符串拼接使用。

### 双引号

```
your_name='lsaiah'
str="Hello, I know you are \"$your_name\"! \n"
echo -e $str
Hello, I know you are "lsaiah"!
```

- 双引号里可以有变量
- 双引号里可以出现转义字符

### 拼接字符串

```
your_name="lsaiah"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
```

输出结果：

```
hello, lsaiah ! hello, lsaiah !
hello, lsaiah ! hello, ${your_name} !
```

### 获取字符串长度

```
string="abcd"
echo ${#string} #输出4
```

### 提取子字符串

```
# 从字符串第2个字符开始截取4个字符
string="lsaiah is a great site"
echo ${string:1:4} # 输出saia
```

### 查找子字符串

```
# 查找字符i 或s 的位置
string="lsaiah is a great site"
echo `expr index "$string" io` # 输出2
```

## 注释

### 单行注释

以`#`开头的行为注释，运行时会被解释器忽略。

### 多行注释

可以把一段要注释的代码用一对花括号括起来定义成一个函数，没有调用的时候就不会执行。也可使用以下格式：

```
:<<EOF
注释内容...
注释内容...
EOF
或
:<<'
注释内容...
注释内容...
'
或
:<<!
注释内容...
注释内容...
!
```

## 数组

Bash Shell只支持一维数组，初始化不需要定义数组大小，下标从0开始。

```
array_name=(value1 value2 ... valuen)
```

也可使用下标定义数组：

```
array_name[0]=value0
array_name[1]=value1
array_name[2]=value2
```

### 读取数组

```
${array_name[index]}
echo "第一个元素为: ${array_name[0]}"
```

### 获取数组中的所有元素

使用@或*可以获取数组中的所有元素：

```
#!/bin/bash
my_array[0]=A
my_array[1]=B
my_array[2]=C
my_array[3]=D
echo "数组的元素为: ${my_array[*]}"
echo "数组的元素为: ${my_array[@]}"
```

```
$ chmod +x test.sh 
$ ./test.sh
数组的元素为: A B C D
数组的元素为: A B C D
```

### 获取数组长度

```
echo "数组元素个数为: ${#my_array[*]}"
echo "数组元素个数为: ${#my_array[@]}"
$ ./test.sh
数组元素个数为: 4
数组元素个数为: 4
```

## 传递参数

在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：**$n**。**n** 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推。

```
#!/bin/bash
echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

```
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
```

| 参数处理 | 说明                 |
| -------- | -------------------- |
| $#       | 传递到脚本的参数个数 |
| $* | 以一个单字符串显示所有向脚本传递的参数。
如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。 |
| $$ | 脚本运行的当前进程ID号 |
| $! | 后台运行的最后一个进程的ID号 |
| $@ | 与$*相同，但是使用时加引号，并在引号中返回每个参数。
如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。 |
| $- | 显示Shell使用的当前选项，与set功能相同。 |
| $? | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |

**$* 与 $@ 区别：**

- 相同点：都是引用所有参数
- 不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。

```
#!/bin/bash
echo "-- \$* 演示 ---"
for i in "$*"; do
    echo $i
done

echo "-- \$@ 演示 ---"
for i in "$@"; do
    echo $i
done
```

执行输出结果：

```
$ chmod +x test.sh 
$ ./test.sh 1 2 3
-- $* 演示 ---
1 2 3
-- $@ 演示 ---
1
2
3
```

## 运算符

### 基本运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr

```
#!/bin/bash
val=`expr 2 + 2`
echo "两数之和为 : $val"
```

- 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
- 完整的表达式要被``包含。

### 算数运算符

| 运算符 | 说明                                          | 举例                          |
| :----- | :-------------------------------------------- | :---------------------------- |
| +      | 加法                                          | `expr $a + $b` 结果为 30。    |
| -      | 减法                                          | `expr $a - $b` 结果为 -10。   |
| *      | 乘法                                          | `expr $a \* $b` 结果为  200。 |
| /      | 除法                                          | `expr $b / $a` 结果为 2。     |
| %      | 取余                                          | `expr $b % $a` 结果为 0。     |
| =      | 赋值                                          | a=$b 将把变量 b 的值赋给 a。  |
| ==     | 相等。用于比较两个数字，相同则返回 true。     | [$a == $b ] 返回 false。      |
| !=     | 不相等。用于比较两个数字，不相同则返回 true。 | [$a != $b ] 返回 true。       |

### 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                                  | 举例                      |
| :----- | :---------------------------------------------------- | :------------------------ |
| -eq    | 检测两个数是否相等，相等返回 true。                   | [$a -eq $b ] 返回 false。 |
| -ne    | 检测两个数是否不相等，不相等返回 true。               | [$a -ne $b ] 返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | [$a -gt $b ] 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | [$a -lt $b ] 返回 true。  |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [$a -ge $b ] 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [$a -le $b ] 返回 true。  |

### 布尔运算符

假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                                | 举例                                    |
| :----- | :-------------------------------------------------- | :-------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 | [ ! false ] 返回 true。                 |
| -o     | 或运算，有一个表达式为 true 则返回 true。           | [$a -lt 20 -o $b -gt 100 ] 返回 true。  |
| -a     | 与运算，两个表达式都为 true 才返回 true。           | [$a -lt 20 -a $b -gt 100 ] 返回 false。 |

### 逻辑运算符

假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明       | 举例                                      |
| :----- | :--------- | :---------------------------------------- |
| &&     | 逻辑的 AND | [[$a -lt 100 && $b -gt 100 ]] 返回 false  |
| \|\|   | 逻辑的 OR  | [[$a -lt 100 \|\| $b -gt 100 ]] 返回 true |

### 字符串运算符

假定变量 a 为 "abc"，变量 b 为 "efg"：

| 运算符 | 说明                                         | 举例                    |
| :----- | :------------------------------------------- | :---------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。      | [$a = $b ] 返回 false。 |
| !=     | 检测两个字符串是否相等，不相等返回 true。    | [$a != $b ] 返回 true。 |
| -z     | 检测字符串长度是否为0，为0返回 true。        | [ -z $a ] 返回 false。  |
| -n     | 检测字符串长度是否不为 0，不为 0 返回 true。 | [ -n "$a" ] 返回 true。 |
| $      | 检测字符串是否为空，不为空返回 true。        | [ $a ] 返回 true。      |

### 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。

| 操作符  | 说明                                                         | 举例                      |
| :------ | :----------------------------------------------------------- | :------------------------ |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。              | [ -b $file ] 返回 false。 |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            | [ -c $file ] 返回 false。 |
| -d file | 检测文件是否是目录，如果是，则返回 true。                    | [ -d $file ] 返回 false。 |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            | [ -g $file ] 返回 false。 |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  | [ -k $file ] 返回 false。 |
| -p file | 检测文件是否是有名管道，如果是，则返回 true。                | [ -p $file ] 返回 false。 |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            | [ -u $file ] 返回 false。 |
| -r file | 检测文件是否可读，如果是，则返回 true。                      | [ -r $file ] 返回 true。  |
| -w file | 检测文件是否可写，如果是，则返回 true。                      | [ -w $file ] 返回 true。  |
| -x file | 检测文件是否可执行，如果是，则返回 true。                    | [ -x $file ] 返回 true。  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          | [ -e $file ] 返回 true。  |

- **-S**: 判断某文件是否 socket。
- **-L**: 检测文件是否存在并且是一个符号链接。

## echo命令

### 显示普通字符串

```
echo "It is a test"
echo It is a test
```

### 显示转义字符

```
echo "\"It is a test\""
"It is a test"
```

### 显示变量

read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量

```
#!/bin/sh
read name
echo "$name It is a test"
```

```
sh test.sh
OK
Ok It is a test
```

### 显示是否换行

```
echo -e "OK! \n" # -e 开启转义
echo -e "OK! \c" # \C不换行
echo "It is a test"
```

### 显示结果定向到文件

```
echo "It is a test" > myfile
```

### 原样输出字符串

不进行转义或取变量（单引号）

```
echo '$name\"'
```

### 显示命令执行结果

```
echo `date`
```

## printf命令

```
printf  format-string  [arguments...]
```

- **format-string:** 为格式控制字符串
- **arguments:** 为参数列表。

%s %c %d %f代表格式替代符：

```
printf "%-10s %-8s %-4.2f\n" 张三 男 66.1234
```

| 序列  | 说明                                                         |
| :---- | :----------------------------------------------------------- |
| \a    | 警告字符，通常为ASCII的BEL字符                               |
| \b    | 后退                                                         |
| \c    | 抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略 |
| \f    | 换页（formfeed）                                             |
| \n    | 换行                                                         |
| \r    | 回车（Carriage return）                                      |
| \t    | 水平制表符                                                   |
| \v    | 垂直制表符                                                   |
| \\    | 一个字面上的反斜杠字符                                       |
| \ddd  | 表示1到3位数八进制值的字符。仅在格式字符串中有效             |
| \0ddd | 表示1到3位的八进制值字符                                     |

## test命令

用于检查某个条件是否成立，它可以进行数值，字符和文件三方面测试。

```
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
result=$[num1+num2]
echo "result 为： $result"
```

| 参数 | 说明           |
| :--- | :------------- |
| -eq  | 等于则为真     |
| -ne  | 不等于则为真   |
| -gt  | 大于则为真     |
| -ge  | 大于等于则为真 |
| -lt  | 小于则为真     |
| -le  | 小于等于则为真 |

### 字符串测试

| 参数      | 说明                     |
| :-------- | :----------------------- |
| =         | 等于则为真               |
| !=        | 不相等则为真             |
| -z 字符串 | 字符串的长度为零则为真   |
| -n 字符串 | 字符串的长度不为零则为真 |

```
num1="abc"
num2="cba"
if test $num1 = $num2
then
	echo '两个字符串相等'
else
	echo '两个字符串不相等'
fi
```

### 文件测试

```
cd /bin
if test -e ./bash
then
    echo '文件已存在!'
else
    echo '文件不存在!'
fi
```

| 参数      | 说明                                 |
| :-------- | :----------------------------------- |
| -e 文件名 | 如果文件存在则为真                   |
| -r 文件名 | 如果文件存在且可读则为真             |
| -w 文件名 | 如果文件存在且可写则为真             |
| -x 文件名 | 如果文件存在且可执行则为真           |
| -s 文件名 | 如果文件存在且至少有一个字符则为真   |
| -d 文件名 | 如果文件存在且为目录则为真           |
| -f 文件名 | 如果文件存在且为普通文件则为真       |
| -c 文件名 | 如果文件存在且为字符型特殊文件则为真 |
| -b 文件名 | 如果文件存在且为块特殊文件则为真     |

Shell还提供与( -a )、或( -o )、非( ! )三个逻辑操作符用于将测试条件连接起来，其优先级为："!"最高，"-a"次之，"-o"最低。

```
cd /bin
if test -e ./notFile -o -e ./bash
then
    echo '至少有一个文件存在!'
else
    echo '两个文件都不存在'
fi
```

## 逻辑控制

### if

```
if condition
then
	command1
	command2
	...
fi
```

写成一行：

```
if [ $(ps -ef | grep c "ssh") -gt 1]; then echo "true"; fi
```

### if else

```
if condition
then
	command1
	command2
	...
else
	command
fi
```

### if else-if else

```
if condition1
then
	command1
elif condition2
then
	command2
else
	commandN
fi
```

### for循环

```
for var in item1 item2 ... itemN
do
	command1
	command2
	...
done
```

写成一行：

```
for var in item1 item2 ... itemN; do command1; command2 ... done;
```

### while循环

```
while condition
do
	command
done
```

```
echo '按下 <CTRL-D> 退出'
echo -n '输入你最喜欢的网站名: '
while read FILM
do
    echo "是的！$FILM 是一个好网站"
done
```

无限循环：

```
while:
do
	command
done
```

```
while true
do
	command
done
```

```
for (( ; ;))
```

#### break

break命令允许跳出所有循环（终止执行后面的所有循环）

```
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done
```

当输入的数字大于5，跳出所有循环。

#### continue

continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。

```
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue
            echo "游戏结束"
        ;;
    esac
done
```

当输入的数字大于5，结束当前循环，继续下一循环。

### until循环

until循环执行一系列命令直到条件为true时停止，和while循环相反。

```
until condition
do
	command
done
```

```
#!/bin/bash
a=0
until [ ! $a -lt 10 ]
do
   echo $a
   a=`expr $a + 1`
done
```

### case

每个 case 分支用右圆括号开始，用两个分号 **;;** 表示 break当前循环，esac跳出整个循环。

```
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac
```

```
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

## 函数

Shell定义函数可以在shell脚本中调用。

```
[ function ] funname [()]
{
	action;
	[return init;]
}
```

- 可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
- 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255)

```
#!/bin/bash
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```

带有return语句的函数，返回值在调用该函数后通过 $? 来获得

```
#!/bin/bash
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $?"
```

### 函数参数

调用函数时可以传递参数，在函数内部通过$n获取参数值，$1表示第一个参数，$2表示第二个参数，以此类推。当n>=10时，需要使用${n}来获取参数。

```
#!/bin/bash
funWithParam(){
	echo "第一个参数是 $1"
	echo "第二个参数是 $2"
	...
	echo "第十个参数是 ${10}"
	echo "第十一个参数是 ${11}"
}
funWithParam 1 2 3 4 5 6 7 8 9 10 11
```

| 参数处理 | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| $#       | 传递到脚本或函数的参数个数                                   |
| $*       | 以一个单字符串显示所有向脚本传递的参数                       |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。         |
| $-       | 显示Shell使用的当前选项，与set命令功能相同。                 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |

## 输入/输出重定向

linux系统命令从终端接收标准输入将标准输出返回终端。

| 命令            | 说明                                               |
| :-------------- | :------------------------------------------------- |
| command > file  | 将输出重定向到 file。                              |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并。                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

```
# 将stderr重定向到file
$ command 2 > file
# 将stderr追加到file末尾
$ command 2 >> file
# 将stdout和stderr合并后重定向到file
$ command > file 2>$1
# 从file1接收输入，输出到file2
$ command < file1 >file2
```

### Here Document

特殊重定向，将输入重定向到一个交互式的shell脚本或程序

```
command << delimiter
	document
delimiter
```

```
cat << EOF
特殊重定向
here document
EOF
```

### /dev/null文件

可以将输入和输出重定向到 /dev/null文件，写入内容将被丢弃，无法读取。

```
# 屏蔽stdout和stderr
$ command > /dev/null 2>&1
```

## 文件包含

```
. filename
或
source filename
```

```
#!/bin/bash
url="lsaiah.cn"
```

```
#!/bin/bash
. ./test1.sh
# 或source ./test1.sh
echo "博客地址：$url"
```

```
./test2.sh
博客地址：lsaiah.cn
```


## Shell正则表达式

第一类称为基本表达式，基本表达式包括了典型的正则标识符。

^表示开头；

$表示结尾；

[]表示任意匹配项；

*表示0个或多个；

.表示任意字符。

第二类是扩展表达式，它在基础表达式的基础上做了一些扩展，支持了更高级的语法和更复杂的条件。

？表示非贪婪匹配；

+表示一个或多个；

() 表示分组；

{} 表示一个范围的约束；

| 表示匹配多个表达式中的任何一个。

## 常用批处理脚本

批量视频格式转换，需指定ffmpeg位置：%~dp0ffmpeg\bin\ffmpeg.exe 表示当前文件夹下的ffmpeg\bin目录。将文件或文件夹拖入.bat

```shell
@echo off
title 视频转码 - to MP4
mode con cols=71 lines=14
color 5f
cls

setlocal enabledelayedexpansion

set d=%~f1
set ffmpeg=%~dp0ffmpeg\bin\ffmpeg.exe

if "%~f1" equ "" goto error
if /i "%~x1" equ ".ogv" goto single
if /i "%~x1" equ ".ogg" goto single
if /i "%~x1" equ ".wmv" goto single
if /i "%~x1" equ ".flv" goto single
if /i "%~x1" equ ".mp4" goto single
if /i "%~x1" equ ".mov" goto single
if /i "%~x1" equ ".avi" goto single
if /i "%~x1" equ ".mkv" goto single
if /i "%~x1" equ ".mpg" goto single
if /i "%~x1" equ ".vob" goto single
goto multi

:multi
if not exist "%d%\mp4" md "%d%\mp4"
for /R "%d%" %%f in (*.*) do (
set mark=f
if /i "%%~xf" equ ".ogv" set mark=t
if /i "%%~xf" equ ".ogg" set mark=t
if /i "%%~xf" equ ".wmv" set mark=t
if /i "%%~xf" equ ".flv" set mark=t
if /i "%%~xf" equ ".mp4" set mark=t
if /i "%%~xf" equ ".mov" set mark=t
if /i "%%~xf" equ ".avi" set mark=t
if /i "%%~xf" equ ".mkv" set mark=t
if /i "%%~xf" equ ".mpg" set mark=t
if /i "%%~xf" equ ".vob" set mark=t
if "!mark!" == "t" (
%ffmpeg% -i "%%f" -c:v libx264 -crf 19 -preset slow -c:a aac -b:a 192k "%%~df%%~pf\mp4\%%~nf.mp4"
)
)
goto end

:single
if not exist "%~d1%~p1\mp4" md "%~d1%~p1\mp4"
%ffmpeg% -i "%d%" -c:v libx264 -crf 19 -preset slow -c:a aac -b:a 192k "%~d1%~p1\mp4\%~n1.mp4"
goto end

:end
cls
echo.
echo.
echo.
echo.
echo ※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※
echo ※                                                                  ※
echo ※                          ☆★ 提示 ★☆                          ※
echo ※                                                                  ※
echo ※                    处理完成，按任意键退出程序                    ※
echo ※                                                                  ※
echo ※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※
echo.
echo.
echo.
pause > nul
exit

:error
cls
echo.
echo.
echo.
echo ※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※
echo ※                                                                  ※
echo ※                          ☆★ 错误 ★☆                          ※
echo ※                                                                  ※
echo ※     请勿直接运行程序，请将需要处理的视频文件(夹)拖放到程序上     ※
echo ※                                                                  ※
echo ※                         按任意键退出程序                         ※
echo ※                                                                  ※
echo ※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※※
echo.
echo.
pause > nul
exit
```

批量文件重命名：

1.

```shell
@echo off
pushd %~dp0
set /p=回车开始转换
for /f "delims=" %%a in ('dir /a-d /b *.jpeg *.PNG *.TGA *.jpg') do ren "%%a" "%%~na.JPG"
echo;修改扩展名完成
setlocal enabledelayedexpansion
set n=1
for /f "delims=" %%i in ('dir *.jpg /b /a-d') do (
ren %%i !n!.jpg&&call,set /a n+=1
)
echo 修改文件名完成
pause
```

2.

```shell
# create.bat
dir /a-d /b *.jpg>src.txt
echo 收集文件名到src.txt成功！
pause
```

```shell
# trim.bat
@echo off&setlocal enabledelayedexpansion
for /f "delims=" %%i in ('dir /s/b *.*') do (
    set "foo=%%~nxi"
    set foo=!foo: =!
    set foo=!foo: =!
    ren "%%~fi" "!foo!"
)
exit
```

```shell
# rename.bat 按照des.txt重命名
@for /f %%s in (src.txt) do (
if exist %%s for /f %%d in (des.txt) do (rename %%s %%d)
)
echo 操作成功！
pause
```

