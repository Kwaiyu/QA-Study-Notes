## re.match

`re.match` 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，`match()`就返回`none`。

**函数语法**：

```
re.match(pattern, string, flags=0)
```

函数参数说明：

| 参数    | 描述                                                         |
| :------ | :----------------------------------------------------------- |
| pattern | 匹配的正则表达式                                             |
| string  | 要匹配的字符串。                                             |
| flags   | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：正则表达式修饰符 - 可选标志 |

匹配成功`re.match`方法返回一个匹配的对象，否则返回`None`。

我们可以使用`group(num)` 或 `groups()` 匹配对象函数来获取匹配表达式。

| 匹配对象方法 | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| group(num=0) | 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。 |
| groups()     | 返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。     |



## re.search

## re.match和re.search的区别

## 检索和替换

### repl参数是一个函数

### compile函数

### findall

### re.finditer

### re.split

## 正则表达式对象

## 正则表达式修饰符 - 可选标志

## 正则表达式模式

## 正则表达式实例

**练习**

提取出带名字的Email地址：

- \<Tom Paris> tom@voyager.org => Tom Paris
- bob@example.com => bob

```python
# -*- coding: utf-8 -*-
import re

str = "<Tom Paris> tom@voyager.org"
def getmidstring(str, start_str, end):
    start = str.find(start_str)
    if start >= 0:
        start += len(start_str)
        end = str.find(end, start)
        if end >= 0:
            return str[start:end].strip()

getmidstring(str,'<Tom Paris> ','@voyager.org')
getmidstring(str,'','@example.com')
```

