## JAVA正则表达式

### 简介

正则表达式是用字符串描述的一个匹配规则，使用正则表达式可以快速判断给定的字符串是否符合匹配规则。Java标准库`java.util.regex`内建了正则表达式引擎。

要判断用户输入的年份是否是`20##`年，先写出规则如下：

一共有4个字符，分别是：`2`，`0`，`0~9任意数字`，`0~9任意数字`。

对应的正则表达式就是：`20\d\d`，其中`\d`表示任意一个数字。

把正则表达式转换为Java字符串就变成了`20\\d\\d`，注意Java字符串用`\\`表示`\`

```java
public class Main {
    public static void main(String[] args) {
        String regex = "20\\d\\d";
        System.out.println("2019".matches(regex)); // true
        System.out.println("2100".matches(regex)); // false
    }
}
```

### 匹配规则

单个字符的匹配规则如下：

| 正则表达式 | 规则                     | 可以匹配                       |
| :--------- | :----------------------- | :----------------------------- |
| `A`        | 指定字符                 | `A`                            |
| `\u548c`   | 指定Unicode字符          | `和`                           |
| `.`        | 任意字符                 | `a`，`b`，`&`，`0`             |
| `\d`       | 数字0~9                  | `0`~`9`                        |
| `\w`       | 大小写字母，数字和下划线 | `a`~`z`，`A`~`Z`，`0`~`9`，`_` |
| `\s`       | 空格、Tab键              | 空格，Tab                      |
| `\D`       | 非数字                   | `a`，`A`，`&`，`_`，……         |
| `\W`       | 非\w                     | `&`，`@`，`中`，……             |
| `\S`       | 非\s                     | `a`，`A`，`&`，`_`，……         |

多个字符的匹配规则如下：

| 正则表达式 | 规则             | 可以匹配                 |
| :--------- | :--------------- | :----------------------- |
| `A*`       | 任意个数字符     | 空，`A`，`AA`，`AAA`，…… |
| `A+`       | 至少1个字符      | `A`，`AA`，`AAA`，……     |
| `A?`       | 0个或1个字符     | 空，`A`                  |
| `A{3}`     | 指定个数字符     | `AAA`                    |
| `A{2,3}`   | 指定范围个数字符 | `AA`，`AAA`              |
| `A{2,}`    | 至少n个字符      | `AA`，`AAA`，`AAAA`，……  |
| `A{0,3}`   | 最多n个字符      | 空，`A`，`AA`，`AAA`     |

编写一个国内电话号码规则的正则表达式，区号3-4位必须以0开头，电话7-8位不能以0开头：

```java
import java.util.*;

public class Main {
    public static void main(String[] args) throws Exception {
        //String re = "\\d{3,4}\\-\\d{7,8}";
          String re = "0\\d{2,3}-[1-9]\\d{6,7}";
        for (String s : List.of("010-12345678", "020-9999999", "0755-7654321")) {
            if (!s.matches(re)) {
                System.out.println("测试失败: " + s);
                return;
            }
        }
        for (String s : List.of("010 12345678", "A20-9999999", "0755-7654.321")) {
            if (s.matches(re)) {
                System.out.println("测试失败: " + s);
                return;
            }
        }
        System.out.println("测试成功!");
    }
}
```

### 复杂匹配规则

| 正则表达式 | 规则                 | 可以匹配                             |
| :--------- | :------------------- | :----------------------------------- |
| ^          | 开头                 | 字符串开头                           |
| $          | 结尾                 | 字符串结束                           |
| [ABC]      | […]内任意字符        | A，B，C                              |
| [A-F0-9xy] | 指定范围的字符       | `A`，……，`F`，`0`，……，`9`，`x`，`y` |
| [^A-F]     | 指定范围外的任意字符 | 非`A`~`F`                            |
| AB\|CD\|EF | AB或CD或EF           | `AB`，`CD`，`EF`                     |

匹配字符串learn java、learn Java、learn python和learn Python。

```java
public class Main {
    public static void main(String[] args) {
        String re = "learn\\s([jJ]ava|[pP]ython)";
        System.out.println("learn java".matches(re));
        System.out.println("learn Java".matches(re));
        System.out.println("learn python".matches(re));
        System.out.println("learn Python".matches(re));
    }
}
```

### 分组匹配

正则表达式用`(...)`分组可以通过`Matcher`对象快速提取子串：

- `group(0)`表示匹配的整个字符串；
- `group(1)`表示第1个子串，`group(2)`表示第2个子串，以此类推。

```java
import java.util.regex.*;
public class Main {
    public static void main(String[] args) {
        //引入java.util.regex包，用Pattern对象匹配，匹配后获得一个Matcher对象，如果匹配成功，就可以直接从Matcher.group(index)返回子串
        Pattern p = Pattern.compile("(\\d{3,4})\\-(\\d{7,8})");
        Matcher m = p.matcher("010-12345678");
        if (m.matches()) {
            String g1 = m.group(1);
            String g2 = m.group(2);
            String g3 = m.group(0);
            System.out.println(g1);
            System.out.println(g2);
            System.out.println(g3);
        } else {
            System.out.println("匹配失败!");
        }
    }
}
```

**Pattern**

`String.matches()`方法和`Pattern`、`Matcher`类的方法本质一致，反复使用`String.matches()`对同一个正则表达式进行多次匹配效率较低，因此创建`Pattern`对象反复使用，可实现编译一次，多次匹配：

```java
import java.util.regex.*;
public class Main {
    public static void main(String[] args) {
        Pattern pattern = Pattern.compile("(\\d{3,4})\\-(\\d{7,8})");
        pattern.matcher("010-12345678").matches(); // true
        pattern.matcher("021-123456").matches(); // true
        pattern.matcher("022#1234567").matches(); // false
        // 获得Matcher对象:
        Matcher matcher = pattern.matcher("010-12345678");
        if (matcher.matches()) {
            String whole = matcher.group(0); // "010-12345678", 0表示匹配的整个字符串
            String area = matcher.group(1); // "010", 1表示匹配的第1个子串
            String tel = matcher.group(2); // "12345678", 2表示匹配的第2个子串
            System.out.println(area);
            System.out.println(tel);
        }
    }
}
```

### 非贪婪匹配

正则表达式匹配默认使用贪婪匹配，可以使用`?`表示对某一规则进行非贪婪匹配。`\d??`中`\d?`表示匹配0个或1个数字，第二个`?`表示非贪婪匹配，尽可能少的匹配。

```java
import java.util.regex.*;
public class Main {
    public static void main(String[] args) {
        //"(\\d+)(0*)"匹配至少1个字符，默认贪婪匹配group1=1230000，group2= 
        Pattern pattern = Pattern.compile("(\\d+?)(0*)");
        Matcher matcher = pattern.matcher("1230000");
        if (matcher.matches()) {
            System.out.println("group1=" + matcher.group(1)); // "123"
            System.out.println("group2=" + matcher.group(2)); // "0000"
        }
    }
}
```

### 搜索和替换

- 分割字符串：`String.split()`

```java
"a b c".split("\\s"); // { "a", "b", "c" }
"a b  c".split("\\s"); // { "a", "b", "", "c" }
"a, b ;; c".split("[\\,\\;\\s]+"); // { "a", "b", "c" }
```

- 搜索子串：`Matcher.find()`

```java
import java.util.regex.*;
public class Main {
    public static void main(String[] args) {
        String s = "the quick brown fox jumps over the lazy dog.";
        //搜索\wo \ \w 中间是o，前后是字符[A-Za-z0-9_]之间的三个字符
        Pattern p = Pattern.compile("\\wo\\w");
        Matcher m = p.matcher(s);
        while (m.find()) {
            String sub = s.substring(m.start(), m.end());
            System.out.println(sub);
        }
    }
}
```

- 替换字符串：`String.replaceAll()`

```java
public class Main {
    public static void main(String[] args) {
        String s = "The     quick\t\t brown   fox  jumps   over the  lazy dog.";
        //\s表示空格和TAB，替换成" "
        String r = s.replaceAll("\\s+", " ");
        System.out.println(r); // "The quick brown fox jumps over the lazy dog."
    }
}
```

- 反向引用

```java
public class Main {
    public static void main(String[] args) {
        String s = "the quick brown fox jumps over the lazy dog.";
        //replaceAll()传入第二个参数中用$1、$2反向引用匹配到的子串。\\s([a-z]{4})\\s 表示空格开头，中间是a-z的4个字母，空格结尾。$1表示匹配到的内容[a-z]{4}
        String r = s.replaceAll("\\s([a-z]{4})\\s", " <b>$1</b> ");
        System.out.println(r);
    }
}
```

**模板引擎练习：**

定义一个字符串作为模板，`${key}`表示变量，传入一个`Map<String, String>`给模板后，通过正则表达式把对应key的替换为Map的Value。

```java
//字符串模板
Hello, ${name}! You are learning ${lang}!
//传入Map
{
    "name":"Bob",
    "lang":"Java"
}
```

```java
package com.lsaiah.test;
import java.util.HashMap;
import java.util.Map;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Template {
    public static void main(String[] args) {
        String s = "Hello, ${name}! You are learning ${lang}!";
        Template template = new Template(s);
        Map<String, String> map = new HashMap<>();
        map.put("name", "Bob");
        map.put("lang", "Java");
        System.out.println(template.render(map));
    }

    final String template;
    final Pattern pattern = Pattern.compile("\\$\\{(\\w+)\\}");

    public Template(String template) {
        this.template = template;
    }

    public String render(Map<String, String> data) {
        Matcher m = pattern.matcher(template);
        StringBuilder sb = new StringBuilder();
        while (m.find()) {
//            String key = template.substring(m.start() + 2, m.end() - 1);
//            m.appendReplacement(sb, data.get(key));
            m.appendReplacement(sb, data.get(m.group(1)).toString());
        }
        m.appendTail(sb);
        return sb.toString();
    }
}
```

