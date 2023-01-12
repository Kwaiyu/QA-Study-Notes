Java是SUN公司的詹姆斯·高斯林在90年代初开发的一种编程语言后来被Oracle收购，随着互联网的高速发展，Java逐渐成为最重要的编程语言。对于软件测试来说，掌握一两门编程语言已是必要条件，具体学习哪门编程语言还需根据自己的工作需求以及兴趣爱好来考虑。下图是TIOBE统计的编程语言长期历史排名：

![img](https://qwq.lsaiah.cn/usr/uploads/Picture/tiobe.png)

## 基础语法

### 流程控制

#### if

`if ... else`可以做条件判断，`else`是可选的；

不推荐省略花括号`{}`；

多个`if ... else`串联要特别注意判断顺序；

要注意`if`的边界条件；

要注意浮点数常常无法精确表示，判断相等不能直接用`==`运算符；

引用类型判断内容相等要使用`equals()`，注意避免`NullPointerException`。

#### switch

`switch`语句可以做多重选择，然后执行匹配的`case`语句后续代码；

`switch`的计算结果必须是整型、字符串或枚举类型；

注意千万不要漏写`break`，建议打开`fall-through`警告；

总是写上`default`，建议打开`missing default`警告；

从Java 14开始，`switch`语句正式升级为表达式，不再需要`break`，并且允许使用`yield`返回值。

例如，在游戏中，让用户选择选项：

1. 单人模式
2. 多人模式
3. 退出游戏

这时，`switch`语句就派上用场了。

`switch`语句根据`switch (表达式)`计算的结果，跳转到匹配的`case`结果，然后继续执行后续语句，直到遇到`break`结束执行。

修改`option`的值分别为`1`、`2`、`3`，观察执行结果。

如果`option`的值没有匹配到任何`case`，例如`option = 99`，那么，`switch`语句不会执行任何语句。这时，可以给`switch`语句加一个`default`，当没有匹配到任何`case`时，执行`default`：

```java
public class Main {
    public static void main(String[] args) {
        int option = 99;
        switch (option) {
        case 1:
            System.out.println("Selected 1");
            break;
        case 2:
            System.out.println("Selected 2");
            break;
        case 3:
            System.out.println("Selected 3");
            break;
        default:
            System.out.println("Not selected");
            break;
        }
    }
}

```

**Switch表达式**

使用`switch`时，如果遗漏了`break`，就会造成严重的逻辑错误，而且不易在源代码中发现错误。从Java 12开始，`switch`语句升级为更简洁的表达式语法，使用类似模式匹配（Pattern Matching）的方法，保证只有一种路径会被执行，并且不需要`break`语句：

```java
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        switch (fruit) {
        case "apple" -> System.out.println("Selected apple");
        case "pear" -> System.out.println("Selected pear");
        case "mango" -> {
            System.out.println("Selected mango");
            System.out.println("Good choice!");
        }
        default -> System.out.println("No fruit selected");
        }
    }
}

```

注意新语法使用`->`，如果有多条语句，需要用`{}`括起来。不要写`break`语句，因为新语法只会执行匹配的语句，没有穿透效应。

很多时候，我们还可能用`switch`语句给某个变量赋值。例如：

```java
int opt;
switch (fruit) {
case "apple":
    opt = 1;
    break;
case "pear":
case "mango":
    opt = 2;
    break;
default:
    opt = 0;
    break;
}
```

使用新的`switch`语法，不但不需要`break`，还可以直接返回值。把上面的代码改写如下：

```java
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        int opt = switch (fruit) {
            case "apple" -> 1;
            case "pear", "mango" -> 2;
            default -> 0;
        }; // 注意赋值语句要以;结束
        System.out.println("opt = " + opt);
    }
}

```

**yield**

```java
public class Main {
    public static void main(String[] args) {
        String fruit = "orange";
        int opt = switch (fruit) {
            case "apple" -> 1;
            case "pear", "mango" -> 2;
            default -> {
                int code = fruit.hashCode();
                yield code; // switch语句返回值
            }
        };
        System.out.println("opt = " + opt);
    }
}

```



#### while

`while`循环先判断循环条件是否满足，再执行循环语句；

`while`循环可能一次都不执行；

编写循环时要注意循环条件，并避免死循环。

```java
while (条件表达式) {
    循环语句
}
// 继续执行后续代码
```

#### do while

`do while`循环先执行循环，再判断条件；

`do while`循环会至少执行一次。

```java
do {
    执行循环语句
} while (条件表达式);
```

#### for

`for`循环通过计数器可以实现复杂循环；

`for each`循环可以直接遍历数组的每个元素；

最佳实践：计数器变量定义在`for`循环内部，循环体内部不修改计数器；

```java
for (初始条件; 循环检测条件; 循环后更新计数器) {
    // 执行语句
}
```

```java
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        int sum = 0;
        for (int i=0; i<ns.length; i++) {
            System.out.println("i = " + i + ", ns[i] = " + ns[i]);
            sum = sum + ns[i];
        }
        System.out.println("sum = " + sum);
    }
}
```

**灵活使用**

`for`循环还可以缺少初始化语句、循环条件和每次循环更新语句，例如：

```java
// 不设置结束条件:
for (int i=0; ; i++) {
    ...
}
// 不设置结束条件和更新语句:
for (int i=0; ;) {
    ...
}
// 什么都不设置:
for (;;) {
    ...
}
```

通常不推荐这样写，但是，某些情况下，是可以省略`for`循环的某些语句的。

**for each循环**

`for`循环经常用来遍历数组，因为通过计数器可以根据索引来访问数组的每个元素：

```java
int[] ns = { 1, 4, 9, 16, 25 };
for (int i=0; i<ns.length; i++) {
    System.out.println(ns[i]);
}
```

但是，很多时候，我们实际上真正想要访问的是数组每个元素的值。Java还提供了另一种`for each`循环，它可以更简单地遍历数组：

```java
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
    }
}

```

和`for`循环相比，`for each`循环的变量n不再是计数器，而是直接对应到数组的每个元素。`for each`循环的写法也更简洁。但是，`for each`循环无法指定遍历顺序，也无法获取数组的索引。

除了数组外，`for each`循环能够遍历所有“可迭代”的数据类型，包括后面会介绍的`List`、`Map`等。

#### break和continue

`break`语句可以跳出当前循环；

`break`语句通常配合`if`，在满足条件时提前结束整个循环；

`break`语句总是跳出最近的一层循环；

`continue`语句可以提前结束本次循环；

`continue`语句通常配合`if`，在满足条件时提前结束本次循环。

**break**

在循环过程中，可以使用`break`语句跳出所在那一层的当前循环。我们来看一个例子：

```java
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        for (int i=1; ; i++) {
            sum = sum + i;
            if (i == 100) {
                break;
            }
        }
        System.out.println(sum);
    }
}
```

**continue**

`break`会跳出当前循环，也就是整个循环都不会执行了。而`continue`则是提前结束本次循环，直接继续执行下次循环。我们看一个例子：

```java
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        for (int i=1; i<=10; i++) {
            System.out.println("begin i = " + i);
            if (i % 2 == 0) {
                continue; // continue语句会结束本次循环
            }
            sum = sum + i;
            System.out.println("end i = " + i);
        }
        System.out.println(sum); // 25
    }
}

```

注意观察`continue`语句的效果。当`i`为奇数时，完整地执行了整个循环，因此，会打印`begin i=1`和`end i=1`。在i为偶数时，`continue`语句会提前结束本次循环，因此，会打印`begin i=2`但不会打印`end i = 2`。

在多层嵌套的循环中，`continue`语句同样是结束本次自己所在的循环。

### 数组

#### 数组遍历

遍历数组可以使用`for`循环，`for`循环可以访问数组索引，`for each`循环直接迭代每个数组元素，但无法获取索引；

使用`Arrays.toString()`可以快速获取数组内容。

```java
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int i=0; i<ns.length; i++) {
            int n = ns[i];
            System.out.println(n);
        }
    }
}
```

第二种方式是使用`for each`循环，直接迭代数组的每个元素：

```java
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
    }
}

```

使用`for each`循环打印也很麻烦。幸好Java标准库提供了`Arrays.toString()`，可以快速打印数组内容：

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 1, 2, 3, 5, 8 };
        System.out.println(Arrays.toString(ns));
    }
}

```



#### 数组排序

常用的排序算法有冒泡排序、插入排序和快速排序等；

冒泡排序使用两层`for`循环实现排序；

交换两个变量的值需要借助一个临时变量。

可以直接使用Java标准库提供的`Arrays.sort()`进行排序；

对数组排序会直接修改数组本身。

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
        // 排序前:
        System.out.println(Arrays.toString(ns));
        for (int i = 0; i < ns.length - 1; i++) {
            for (int j = 0; j < ns.length - i - 1; j++) {
                if (ns[j] > ns[j+1]) {
                    // 交换ns[j]和ns[j+1]:
                    int tmp = ns[j];
                    ns[j] = ns[j+1];
                    ns[j+1] = tmp;
                }
            }
        }
        // 排序后:
        System.out.println(Arrays.toString(ns));
    }
}

```

Java的标准库已经内置了排序功能，我们只需要调用JDK提供的`Arrays.sort()`就可以排序：

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
        Arrays.sort(ns);
        System.out.println(Arrays.toString(ns));
    }
}

```

#### 二维数组

二维数组就是数组的数组，三维数组就是二维数组的数组；

多维数组的每个数组元素长度都不要求相同；

打印多维数组可以使用`Arrays.deepToString()`；

最常见的多维数组是二维数组，访问二维数组的一个元素使用`array[row][col]`。

二维数组遍历：

```java
import java.util.Arrays;

public class Myclass {
    public static int [][] ns = {
            {1,3,4,2},
            {5,6},
            {7,9,8}
    };
    public void forMethod(){
        for(int i = 0; i < ns.length; i++){
            for (int j = 0; j < ns[i].length; j++){
                System.out.print(ns[i][j] + ",");
            }
        }
    }
    public void foreachMethod(){
        for (int arr[]: ns) {
            for(int n: arr){
                System.out.print(n + ",");
            }
        }
    }

    public static void main(String[] args) {
        Myclass m = new Myclass();
//        m.forMethod();
//        m.foreachMethod();
        System.out.println(Arrays.deepToString(ns));
    }
}
```

## 面向对象基础

### 方法参数绑定

- 基本类型参数的传递，是调用方值的复制。双方各自的后续修改，互不影响。

- 引用类型参数的传递，调用方的变量，和接收方的参数变量，指向的是同一个对象。双方任意一方对这个对象的修改，都会影响对方，因为指向同一个对象。

- this关键字的用法：指向当前实例对象的引用

  1. `this`关键字可用来引用当前类的实例变量。
  2. `this`关键字可用于调用当前类方法(隐式)。
  3. `this()`可以用来调用当前类的构造函数。
  4. `this`关键字可作为调用方法中的参数传递。
  5. `this`关键字可作为参数在构造函数调用中传递。
  6. `this`关键字可用于从方法返回当前类的实例。

  static关键字用法：

  java中的`static`关键字主要用于内存管理。我们可以应用java `static`关键字在变量，方法，块和嵌套类中。 `static`关键字属于类，而不是类的实例。方便在没有创建对象的情况下调用变量/方法。

  静态(`static`)可以是：

  1. 变量(也称为类变量)

     静态变量可以用于引用所有对象的公共属性(对于每个对象不是唯一的)。如：员工公司名称，学生所在的大学名称。

  2. 方法(也称为类方法)

     静态方法属于类，而不属于类的对象。

     可以直接调用静态方法，而无需创建类的实例。

     静态方法可以访问静态数据成员，并可以更改静态数据成员的值。

  3. 代码块

     用于初始化静态数据成员。

     它在类加载时在main方法之前执行。

  4. 嵌套类

```java
package com.lsaiah.java;
public class MethodDemo {
    public static void main(String[] args) {
        Person p = new Person();
        int n = 10;
        p.setAge(n); //传入n的值10
        n = 15; //n的值改为15
        p.setName("张三");
        //基本类型参数绑定：输出p.getAge()还是10
        //当set传递数组时，get指向同一个对象，当修改传递时对象改变
        System.out.println("姓名：" + p.getName() + " 年龄：" + p.getAge());
    }
}

class Person {
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    private String name;
    private int age;
}
```

static访问特点： 

非静态成员方法： 

能够访问静态/非静态的成员变量/成员方法

静态成员方法：

能够访问静态成员变量/成员方法，但是不能直接访问非静态成员变量/成员方法，可以通过类名.静态成员变量/静态成员方法访问。

5. static修饰常量（constant）
```java
public class Constant{
    public static final int CODE_200 = 200;
}
public class TestConstant{
    public static void main(String[] args) {
        if (Constant.CODE_200 == 200){
            System.out.println("成功");
        }
    }
}
```


**Java 程序设计语言总是采用按值调用。也就是说，方法得到的是所有参数值的一个拷贝，也就是说，方法不能修改传递给它的任何参数变量的内容。**

> **example 1**

```java
public static void main(String[] args) {
    int num1 = 10;
    int num2 = 20;

    swap(num1, num2);

    System.out.println("num1 = " + num1);
    System.out.println("num2 = " + num2);
}

public static void swap(int a, int b) {
    int temp = a;
    a = b;
    b = temp;

    System.out.println("a = " + a);
    System.out.println("b = " + b);
}
```

**结果：**

```
a = 20
b = 10
num1 = 10
num2 = 20
```

在 swap 方法中，a、b 的值进行交换，并不会影响到 num1、num2。因为，a、b 中的值，只是从 num1、num2 的复制过来的。也就是说，a、b 相当于 num1、num2 的副本，副本的内容无论怎么修改，都不会影响到原件本身。

**通过上面例子，我们已经知道了一个方法不能修改一个基本数据类型的参数，而对象引用作为参数就不一样，请看 example2.**

> **example 2**

```java
	public static void main(String[] args) {
		int[] arr = { 1, 2, 3, 4, 5 };
		System.out.println(arr[0]);
		change(arr);
		System.out.println(arr[0]);
	}

	public static void change(int[] array) {
		// 将数组的第一个元素变为0
		array[0] = 0;
	}
```

**结果：**

```
1
0
```

array 被初始化 arr 的拷贝也就是一个对象的引用，也就是说 array 和 arr 指向的是同一个数组对象。 因此，外部对引用对象的改变会反映到所对应的对象上。

**通过 example2 我们已经看到，实现一个改变对象参数状态的方法并不是一件难事。理由很简单，方法得到的是对象引用的拷贝，对象引用及其他的拷贝同时引用同一个对象。**

**很多程序设计语言（特别是，C++和 Pascal)提供了两种参数传递的方式：值调用和引用调用。有些程序员（甚至本书的作者）认为 Java 程序设计语言对对象采用的是引用调用，实际上，这种理解是不对的。由于这种误解具有一定的普遍性，所以下面给出一个反例来详细地阐述一下这个问题。**

> **example 3**

```java
public class Test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Student s1 = new Student("小张");
		Student s2 = new Student("小李");
		Test.swap(s1, s2);
		System.out.println("s1:" + s1.getName());
		System.out.println("s2:" + s2.getName());
	}

	public static void swap(Student x, Student y) {
		Student temp = x;
		x = y;
		y = temp;
		System.out.println("x:" + x.getName());
		System.out.println("y:" + y.getName());
	}
}
```

**结果：**

```
x:小李
y:小张
s1:小张
s2:小李
```

通过上面两张图可以很清晰的看出： **方法并没有改变存储在变量 s1 和 s2 中的对象引用。swap 方法的参数 x 和 y 被初始化为两个对象引用的拷贝，这个方法交换的是这两个拷贝**

> **总结**

Java 程序设计语言对对象采用的不是引用调用，实际上，对象引用是按 值传递的。

下面再总结一下 Java 中方法参数的使用情况：

- 一个方法不能修改一个基本数据类型的参数（即数值型或布尔型）。
- 一个方法可以改变一个对象参数的状态。
- 一个方法不能让对象参数引用一个新的对象。

### 方法（函数）类型

**1.无参数无返回值的方法**

```
// 无参数无返回值的方法(如果方法没有返回值，不能不写，必须写void，表示没有返回值)
public void f1() {
    System.out.println("无参数无返回值的方法");
}
```

**2.有参数无返回值的方法**

```java
/**
* 有参数无返回值的方法
* 参数列表由零组到多组“参数类型+形参名”组合而成，多组参数之间以英文逗号（,）隔开，形参类型和形参名之间以英文空格隔开
*/
public void f2(int a, String b, int c) {
    System.out.println(a + "-->" + b + "-->" + c);
}
```

**3.有返回值无参数的方法**

```java
// 有返回值无参数的方法（返回值可以是任意的类型,在函数里面必须有return关键字返回对应的类型）
public int f3() {
    System.out.println("有返回值无参数的方法");
    return 2;
}
```

**4.有返回值有参数的方法**

```java
// 有返回值有参数的方法
public int f4(int a, int b) {
    return a * b;
}
```

**5.return 在无返回值方法的特殊使用**

```java
// return在无返回值方法的特殊使用
public void f5(int a) {
    if (a > 10) {
        return;//表示结束所在方法 （f5方法）的执行,下方的输出语句不会执行
    }
    System.out.println(a);
}
```



### 构造方法、封装

- 实例在创建时通过`new`操作符会调用其对应的构造方法，构造方法用于初始化实例，方法名就是类名，没有返回值；
- 没有定义构造方法时，编译器会自动创建一个默认的无参数构造方法；
- 可以定义多个构造方法，编译器根据参数自动判断；
- 可以在一个构造方法内部调用另一个构造方法，便于代码复用。

```java
package com.lsaiah.java;

public class MethodDemo02 {
    public static void main(String[] args) {
    //TODO:请给Person02类增加(String, int)的构造方法：
        Person02 p = new Person02("张三", 10);
        System.out.println("姓名：" + p.getName() + " 年龄：" + p.getAge());
    }
}

class Person02 {
    public Person02(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    private String name;
    private int age;
}
```

### 方法重载

- 方法重载是指多个方法的方法名相同，但各自的参数不同；
- 重载方法应该完成类似的功能，参考`String`的`indexOf()`；
- 重载方法返回值类型应该相同。

```java
public class MethodDemo03 {
    public static void main(String[] args) {
        // TODO: 给Person增加重载方法setName(String, String):
        Person03 ming = new Person03();
        Person03 hong = new Person03();
        ming.setName("Xiao Ming");
        hong.setName("Xiao", "Hong");
        System.out.println(ming.getName());
        System.out.println(hong.getName());
    }
}

class Person03 {
    public String getName() {
        return name;
    }
    //方法重载：方法名、返回值类型相同，参数类型数量不同
    public void setName(String name) {
        this.name = name;
    }
    public void setName(String name, String name1) {
        this.name = name + " " + name1;
    }
    private String name;
}
```

### 继承

- 继承是面向对象编程的一种强大的代码复用方式；
- Java只允许单继承，所有类最终的根类是`Object`；
- `protected`允许子类访问父类的字段和方法；
- 子类的构造方法可以通过`super()`调用父类的构造方法；
- 可以安全地向上转型为更抽象的类型；
- 可以强制向下转型，最好借助`instanceof`判断；
- 子类和父类的关系是is，has关系不能用继承。

```java
//instanceof variable: 从Java 14开始支持
//在不支持的版本中编译：javac --enable-preview -source 14 Main.java 
public class Main {
    public static void main(String[] args) {
        Object obj = "hello";
        if (obj instanceof String s) {
            // 可以直接使用变量s:
            System.out.println(s.toUpperCase());
        }
    }
}

public class ExtendsDemo {
    public static void main(String[] args) {
        Person p = new Person("张三", 20);
        Student s = new Student("李四", 15, 60);
        PrimaryStudent ps = new PrimaryStudent("张三", 10, 100, 5);
        System.out.println("姓名"+ ps.getName() + "年龄"+ps.getAge() + "分数"+ ps.getScore() + "年级"+ps.getGrade());
    }
}

class Person {
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }
    public int getAge() {
        return age;
    }
    protected String name;
    protected int age;
}

class Student extends Person{
    public int getScore() {
        return score;
    }
    public Student(String name, int age, int score){
        super(name,age);
        this.score = score;
    }

    protected int score;
}

class PrimaryStudent extends Student{
    public int getGrade() {
        return grade;
    }
    protected int grade;

    public PrimaryStudent(String name, int age, int score,int grade) {
        super(name, age, score);
        this.grade = grade;
    }
}
```
```java
public class Test01 {
    public static void main(String[] args) {
        AnimalParent cat = new Cat();
//        instanceof判断对象引用是否为该类型（自身类、自身类的父类、Object）返回true,其他情况返回false
//        对象的引用(cat) instanceof 具体类型(Dog)类或者接口
        if (cat instanceof Dog){
            System.out.println("cat是Dog类型");
        }else {
            System.out.println("cat不是Dog类型");
        }
        if (cat instanceof AnimalParent){
            System.out.println("cat是AnimalParent类型");
        }else {
            System.out.println("cat不是AnimalParent类型");
        }
//        类型转换异常：猫类不能强制转换成狗类
//        Dog dog = (Dog) cat;
    }
}
```


### 多态

- 子类可以重写父类的方法（Override），重写在子类中改变了父类方法的行为；
- Java的方法调用总是作用于运行期对象的实际类型，这种行为称为多态；
- `final`修饰符有多种作用：
  - `final`修饰的方法可以阻止被重写；
  - `final`修饰的class可以阻止被继承；
  - `final`修饰的field必须在创建对象时初始化，随后不可修改。

```java
package com.lsaiah.java;

//TODO:给一个有工资收入和稿费收入的小伙伴算税。
public class PolymorphicDemo {
    public static void main(String[] args) {
        Income[] incomes = new Income[] {
                new Income(3000),
                new Salary(7500),
                new Royalty(15000)
        };
        double totalTax = 0;
        for (Income income : incomes){
            totalTax+=income.getTax();
        }
        System.out.println(totalTax);
    }
}

class Income {
    //Income构造方法，传递参数income
    public Income(double income) {
        this.income = income;
    }
    //Income类计算税率的方法
    public double getTax(){
        return income * 0.1;
    }
    protected double income;
}

class Salary extends Income{

    public Salary(double income) {
        super(income);
    }
    @Override
    public double getTax() {
        if (income < 5000){
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}

class Royalty extends Income{

    public Royalty(double income) {
        super(income);
    }
    @Override
    public double getTax() {
        return 0;
    }
}
```

### 抽象类

- 通过`abstract`定义的方法是抽象方法，它只有定义，没有实现。抽象方法定义了子类必须实现的接口规范；
- 定义了抽象方法的class必须被定义为抽象类，从抽象类继承的子类必须实现抽象方法；
- 如果不实现抽象方法，则该子类仍是一个抽象类；
- 面向抽象编程使得调用者只关心抽象方法的定义，不关心子类的具体实现。

```java
package com.lsaiah.java;

//TODO:用抽象类给一个有工资收入和稿费收入的小伙伴算税。
public class abstractDemo {
    public static void main(String[] args) {
        Income01[] incomes01 = new Income01[]{
          new Income01(3000) {
              @Override
              double getTax() {
                  return income * 0.1;
              }
          },
          new Salary01(7500),
          new Royalty01(15000)
        };
        double totalTax = 0;
        for (Income01 income01:incomes01) {
            totalTax+=income01.getTax();
        }
        System.out.println(totalTax);
    }
}

abstract class Income01{
    protected double income;
    public Income01(double income){
        this.income = income;
    };
    abstract double getTax();
}

class Salary01 extends Income01{
    public Salary01(double income) {
        super(income);
    }
    @Override
    double getTax() {
        if (income < 5000){
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}

class Royalty01 extends Income01{
    public Royalty01(double income) {
        super(income);
    }
    @Override
    double getTax() {
        return 0;
    }
}
```

### 接口

Java的接口（interface）定义了纯抽象规范，一个类可以实现多个接口；

接口也是数据类型，适用于向上转型和向下转型；

接口的所有方法都是抽象方法，接口不能定义实例字段；

接口可以定义`default`方法。


|            | abstract class       | interface                   |
| :--------- | :------------------- | :-------------------------- |
| 继承       | 只能extends一个class | 可以implements多个interface |
| 字段       | 可以定义实例字段     | 不能定义实例字段            |
| 抽象方法   | 可以定义抽象方法     | 可以定义抽象方法            |
| 非抽象方法 | 可以定义非抽象方法   | 可以定义default方法         |

```java
package com.lsaiah.java;

//TODO:用接口给一个有工资收入和稿费收入的小伙伴算税。
public class InterfaceDemo {
    public static void main(String[] args) {
        Income02[] incomes02 = new Income02[]{
                new Salary02(7500),
                new Royalty02(15000)
        };
        double totalTax = 0;
        for (Income02 income02: incomes02) {
            totalTax+=income02.getTax();
        }
        System.out.println(totalTax);
    }
}

interface Income02 {
    double getTax();
}

class Salary02 implements Income02{
    private double income;
    public Salary02(double income){
        this.income = income;
    }
    @Override
    public double getTax() {
        if (income < 5000){
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}
class Royalty02 implements Income02{
    private double income;
    public Royalty02(double income){
        this.income = income;
    }
    @Override
    public double getTax() {
        return 0;
    }
}
```

### 静态字段和静态方法

在外部调用静态方法时，可以使用 `类名.方法名` 的方式，也可以使用 `对象.方法名` 的方式，而实例方法只有后面这种方式。也就是说**调用静态方法可以无需创建对象** 。一般建议使用 `类名.方法名` 的方式来调用静态方法。

```java
public class Person {
    public void method() { 
      //......
    }
 
    public static void staicMethod(){
      //......
    }
    public static void main(String[] args) {
        Person person = new Person();
        // 调用实例方法
        person.method(); 
        // 调用静态方法
        Person.staicMethod()
    }
}
```

在一个静态方法内调用一个非静态成员为什么是非法的?

因为静态方法是属于类的，在类加载的时候就会分配内存，可以通过类名直接访问。而非静态成员属于实例对象，只有在对象实例化之后才存在，然后通过类的实例对象去访问。在类的非静态成员不存在的时候静态成员就已经存在了，此时调用在内存中还不存在的非静态成员，属于非法操作。

静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），不允许访问实例成员（即实例成员变量和实例方法），而实例方法不存在这个限制。

- 静态字段属于所有实例“共享”的字段，实际上是属于`class`的字段；
- 调用静态方法不需要实例，无法访问`this`，但可以访问静态字段和其他静态方法；
- 静态方法常用于工具类和辅助方法，如Arrays.sort();。

```java
package com.lsaiah.java;

//TODO:给Person04类增加一个静态字段count和静态方法getCount，统计实例创建的个数
public class StaticDemo {
    public static void main(String[] args) {
        Person04 p4 = new Person04("张三");
        System.out.println(Person04.getCount());
        System.out.println(Person04.getCount());
        Person04 p5 = new Person04("李四");
        System.out.println(Person04.getCount());
        Person04 p6 = new Person04("王五");
        System.out.println(Person04.getCount());
    }
}

class Person04 {
    public Person04(String name) {
        this.name = name;
        count++;
    }

    public static int getCount() {
        return count;
    }

    static int count;
    static String name;
}
```

### 包/API

- Java内建的`package`机制是为了避免`class`命名冲突；
- JDK的核心类使用`java.lang`包，编译器会自动导入；官方类库在rt.jar包
- JDK的其它常用类定义在`java.util.*`，`java.math.*`，`java.text.*`，……；
- 包名推荐使用倒置的域名，例如`org.apache`。
- 使用完整类名或使用import语句导入
- JAVA JDK提供API：https://docs.oracle.com/javase/8/docs/api/


### 作用域

- Java内建的访问权限包括`public`、`protected`、`private`和`package`权限；
- Java在方法内部定义的变量是局部变量，局部变量的作用域从变量声明开始，到一个块结束；
- `final`修饰符不是访问权限，它可以修饰`class`、`field`和`method`；
- 一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。

#### 访问修饰符

修饰类：

- public：该类可以被任何其他类访问
- default： 该类只能由同一包中的类访问。当不指定修饰符时使用。

修饰属性，方法和构造函数：

- public：该属性方法构造函数可用于所有类
- private： 该属性方法构造函数只能在声明的类中访问
- default： 该属性方法构造函数只能在同一程序包中访问。当不指定修饰符时使用。
- protected：该属性方法构造函数可在相同的包和子类中访问。

#### 非访问修饰符

修饰类：

- final: 该类不能被其他类继承
- abstract: 该类不能用于创建对象（要访问抽象类，它必须从另一个类继承。

修饰属性，方法和构造函数：

- final: 属性和方法不能被覆盖/修改
- static: 属性和方法属于类，而不是对象，调用需要先对象实例化
- abstract: 只能在抽象类中使用，并且只能在方法上使用。该方法没有主体，例如abstract void run(); 主体由子类提供（从继承）。
- transient: 序列化包含属性和方法的对象时，将跳过这些属性和方法。
- volatile：属性的值不在线程本地缓存，并且始终从“主内存”中读取

### classpath和jar

`classpath`是JVM用到的一个环境变量，它用来指示JVM如何搜索`class`。Jar包的第一层目录不能是`bin`目录。

- JVM通过环境变量`classpath`决定搜索`class`的路径和顺序；
- 不推荐设置系统环境变量`classpath`，始终建议通过`-cp`命令传入；
- jar包相当于目录，可以包含很多`.class`文件，方便下载和使用；
- `MANIFEST.MF`文件可以提供jar包的信息，如`Main-Class`，这样可以直接运行jar包。

### 模块

- Java 9引入的模块目的是为了管理依赖；
- 使用模块可以按需打包JRE；
- 使用模块对类的访问权限有了进一步限制。

打包Jar包：

```
$ jar --create --file hello.jar --main-class com.itranswarp.sample.Main -C bin .
```

运行Jar包：

```
java -jar hello.jar
```

打包模块：

```
$ jmod create --class-path hello.jar hello.jmod
```

运行模块：

```
java --module-path hello.jar --module hello.world
```

打包JRE：

```
$ jlink --module-path hello.jmod --add-modules java.base,java.xml,hello.world --output jre/
```

运行JRE：

```
$ jre/bin/java --module hello.world
```

## 核心类

### 字符串和编码

- Java字符串`String`是不可变对象；
- 字符串操作不改变原字符串内容，而是返回新字符串；
- 常用的字符串操作：提取子串、查找、替换、大小写转换等；
- Java使用Unicode编码表示`String`和`char`；
- 转换编码就是将`String`和`byte[]`转换，需要指定编码；
- 转换为`byte[]`时，始终优先考虑`UTF-8`编码。

### String
```java
public class Login {
    public static void main(String[] args) {
        String userName = "admin";
        String userPwd = "123456";

        Scanner scanner = new Scanner(System.in);
        int pwdCount = 3;
        for (int i = 1; i <= pwdCount; i++) {
            System.out.println("请输入用户名：");
            String user = scanner.nextLine();
            System.out.println("请输入密码：");
            String pwd = scanner.nextLine();
//        正确的.equals(输入的)，如果用户输入null也不会报错
//        输入的.equals(正确的)，如果用户输入null则抛异常NOP
            if (userName.equals(user) && userPwd.equals(pwd)) {
                System.out.println("用户名和密码正确，登录成功");
                break;
            }else{
                if (i==pwdCount){
                    System.out.println("账户和密码输入错误达到"+ pwdCount +"次，已被冻结");
                    return;
                }
            System.out.println("用户名或密码错误，登录失败. 剩余机会：" + (pwdCount - i));
            }
        }
    }
}
```

### StringBuilder

- `StringBuilder`是可变对象，用来高效拼接字符串；
- `StringBuilder`可以支持链式操作，实现链式操作的关键是返回实例本身；
- `StringBuffer`是`StringBuilder`的线程安全版本，现在很少使用。

```java
package com.lsaiah.java;

//TODO:请使用StringBuilder构造一个INSERT语句
public class StringBuilderDemo {
    public static void main(String[] args) {
        String[] fields = { "name", "position", "salary" };
        String table = "employee";
        String insert = buildInsertSql(table, fields);
        System.out.println(insert);
        String s = "INSERT INTO employee (name, position, salary) VALUES (?, ?, ?)";
        System.out.println(s.equals(insert) ? "测试成功" : "测试失败");
    }
    static String buildInsertSql(String table, String[] fields) {
        StringBuilder sb = new StringBuilder(1024);
        sb.append("INSERT INTO ").append(table).append(" (");
        for (int i = 0; i < fields.length; ++i) {
            if (i == fields.length - 1) {
                sb.append(fields[i]);
                continue;
            }
            sb.append(fields[i] + ", ");
        }
        sb.append(") VALUES (?, ?, ?)");
        return sb.toString();
    }
}

```

String：适用于少量字符串操作的情况

StringBuilder：适用于单线程下大量字符串操作的情况。非线程安全的，适合单线程处理字符串。

StringBuffer：适用于多线程下大量字符串操作的情况。是线程安全的，适合多线程处理字符串。

### StringJoiner

- 用指定分隔符拼接字符串数组时，使用`StringJoiner`或者`String.join()`更方便；
- 用`StringJoiner`拼接字符串时，还可以额外附加一个开头和结尾。

```java
package com.lsaiah.java;
import java.util.StringJoiner;

public class StringJoinerDemo {
    public static void main(String[] args) {
        String[] fields = { "name", "position", "salary" };
        String table = "employee";
        String select = buildSelectSql(table, fields);
        System.out.println(select);
        System.out.println("SELECT name, position, salary FROM employee".equals(select) ? "测试成功" : "测试失败");
    }

    private static String buildSelectSql(String table, String[] fields) {
        var sj = new StringJoiner(", ", "SELECT "," FROM " + table);
        for (String s : fields){
            sj.add(s);
        }
        return sj.toString();
    }
}
```

### 包装类型

Java是一种面向对象的编程语言，而基本数据类型的值不是对象，将简单数据类型的数据进行封装而得到的类就是包装类。
下表显示了原始类型和等效的包装器类：

包装类型不赋值就是 `Null` ，而基本类型有默认值且不是 `Null`。

局部变量表主要存放了编译期可知的基本数据类型 **（boolean、byte、char、short、int、float、long、double）**占用的空间非常小，而**对象引用**（reference 类型，它不同于对象本身，存在于堆中，可能是一个指向对象起始地址的引用指针，也可能是指向一个代表对象的句柄或其他与此对象相关的位置）。


| 基础数据类型 | 引用类型            | 基本类型默认值（包装类型默认null） |
| :----------- | :------------------ | ---------------------------------- |
| byte         | java.lang.Byte      | 0                                  |
| short        | java.lang.Short     | 0                                  |
| int          | java.lang.Integer   | 0                                  |
| long         | java.lang.Long      | 0L                                 |
| float        | java.lang.Float     | 0f                                 |
| double       | java.lang.Double    | 0d                                 |
| boolean      | java.lang.Boolean   | false                              |
| char         | java.lang.Character | 'u0000'                            |

装箱：将基本类型用它们对应的引用类型包装起来；`Integer i = 10;` 在字节码中，等价于`Integer i = Integer.valueOf(10)`

拆箱：将包装类型转换为基本数据类型；`int n = i` 在字节码中，等价于`int n = i.intValue();`

- Java核心库提供的包装类型可以把基本类型包装为`class`；
- 自动装箱和自动拆箱都是在编译期完成的（JDK>=1.5）；
- 装箱和拆箱会影响执行效率，装箱是将基础数据类型变为引用类型的赋值，拆箱是将引用类型变为基础数据类型可能发生`NullPointerException`；
- 包装类型的比较必须使用`equals()`；
- 整数和浮点数的包装类型都继承自`Number`；
- 包装类型提供了大量实用方法。

```
Integer i1 = 33;

Integer i2 = 33;

System.out.println(i1 == i2);// 输出 true

Float i11 = 333f;

Float i22 = 333f;

System.out.println(i11 == i22);// 输出 false

Double i3 = 1.2;

Double i4 = 1.2;

System.out.println(i3 == i4);// 输出 false

Integer i1 = 40;// 这一行代码会发生装箱，也就是这行代码等价于 Integer i1=Integer.valueOf(40) 。因此，i1 直接使用的是常量池中的对象。
Integer i2 = new Integer(40);// 而Integer i1 = new Integer(40) 会直接创建新的对象。
System.out.println(i1==i2);// 因此输出 false 
```

**==比较的是值，对于引用数据类型来说==比较的是对象的内存地址。**

- **类没有覆盖 `equals()`方法** ：通过`equals()`比较该类的两个对象时，等价于通过“==”比较这两个对象，使用的默认是 `Object`类`equals()`方法。
- **类覆盖了 `equals()`方法** ：一般我们都覆盖 `equals()`方法来比较两个对象中的属性是否相等；若它们的属性相等，则返回 true(即，认为这两个对象相等)。

**所有整型包装类对象之间值的比较，全部使用 equals 方法比较**。说明：对于`Integer var= ?`在-128至127之间的赋值， `Integer`对象是在`IntegerCache.cache`产生，会复用已有对象，这个区间内的`Integer`值可以直接使用`==`进行判断，但是这个区间之外的所有数据，都会在堆上产生，并不会复用已有对象，这是一个大坑，推荐使用`equals`方法进行判断。

### JavaBean

- JavaBean是一种符合命名规范的`class`，它通过`getter`和`setter`来定义属性；
- 属性是一种通用的叫法，并非Java语法规定；
- 可以利用IDE快速生成`getter`和`setter`；
- 使用`Introspector.getBeanInfo()`可以获取属性列表。

```java
package com.lsaiah.java;
import java.beans.*;
public class Main {
    public static void main(String[] args) throws Exception {
        BeanInfo info = Introspector.getBeanInfo(Person.class);
        for (PropertyDescriptor pd : info.getPropertyDescriptors()) {
            System.out.println(pd.getName());
            System.out.println("  " + pd.getReadMethod());
            System.out.println("  " + pd.getWriteMethod());
        }
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

### 枚举类

枚举类是一种特殊类，它和普通类一样可以使用构造器、定义成员变量和方法，也能实现一个或多个接口,但枚举类不能继承其他类。

- Java使用`enum`定义枚举类型，它被编译器编译为`final class Xxx extends Enum { … }`；
- 通过`name()`获取常量定义的字符串，注意不要使用`toString()`；
- 通过`ordinal()`返回常量定义的顺序（无实质意义）；
- 可以为`enum`编写构造方法、字段和方法
- `enum`的构造方法要声明为`private`，字段强烈建议声明为`final`；
- `enum`适合用在`switch`语句中。

```java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
//        if (day.dayValue == 6 || //day.dayValue == 0) {
//            System.out.println("Today is //" + day + ". Work at home!");
//        } else {
//            System.out.println("Today is //" + day + ". Work at office!");
//        }
       switch(day){
           case MON:
           case TUE:
           case WED:
           case FRI:                      		System.out.println("Today is " + day + ". Work at office!");
            break;
           case SAT:
           case SUN:
      System.out.println("Today is " + day + ". Work at home!");
            break;
           default:
               throw new RuntimeException("cannot process " + day);
       }
    }
}

enum Weekday {
    MON(1, "星期一"), TUE(2, "星期二"), WED(3, "星期三"), THU(4, "星期四"), FRI(5, "星期五"), SAT(6, "星期六"), SUN(0, "星期日");

    public final int dayValue;
    private final String chinese;

    private Weekday(int dayValue, String chinese) {
        this.dayValue = dayValue;
        this.chinese = chinese;
    }

    @Override
    public String toString() {
        return this.chinese;
    }
}
```

### 记录类

从Java 14开始，提供新的`record`关键字，可以非常方便地定义Data Class：

- 使用`record`定义的是不变类；
- 可以编写Compact Constructor对参数进行验证；
- 可以定义静态方法。

### BigInteger

- `BigInteger`用于表示任意大小的整数；
- `BigInteger`是不变类，并且继承自`Number`；
- 将`BigInteger`转换成基本类型时可使用`longValueExact()`等方法保证结果准确。

### BigDecimal

- `BigDecimal`用于表示精确的小数，常用于财务计算，`scale()`表示小数位数，`setScale`传递`(小数位数,RoundingMode.HALF_UP)`表示四舍五入，`(小数位数, RoundingMode.DOWN)`表示直接截断，`stripTrailingZeros()` 方法将去除末尾的0，`divideAndRemainder()`方法返回商和余数的数组；
- 比较`BigDecimal`的值是否相等，必须使用`compareTo()`而不能使用`equals()`。返回负数、正数和0，分别表示大于、小于和等于。

### 常用工具类

Java提供的常用工具类有：

- Math.abs()：绝对值计算
- Math.pow()：次方计算
- Math.sqrt()：根号计算
- Math.log()：底数计算
- Random：生成伪随机数
- SecureRandom：生成安全的随机数

实际上真正的真随机数只能通过量子力学原理来获取，`SecureRandom`无法指定种子，它使用RNG（random number generator）算法。JDK的`SecureRandom`实际上有多种不同的底层实现，有的使用安全随机种子加上伪随机数算法来产生安全的随机数，有的使用真正的随机数生成器。实际使用的时候，可以优先获取高强度的安全随机数生成器，如果没有提供，再使用普通等级的安全随机数生成器，`SecureRandom`的安全性是通过操作系统提供的安全的随机种子来生成随机数。这个种子是通过CPU的热噪声、读写磁盘的字节、网络流量等各种随机事件产生的“熵”。在密码学中，安全的随机数非常重要。如果使用不安全的伪随机数，所有加密体系都将被攻破。因此，时刻牢记必须使用`SecureRandom`来产生安全的随机数。

```java
import java.util.Arrays;
import java.security.SecureRandom;
import java.security.NoSuchAlgorithmException;
public class Main {
    public static void main(String[] args) {
        SecureRandom sr = null;
        try {
            sr = SecureRandom.getInstanceStrong(); // 获取高强度安全随机数生成器
        } catch (NoSuchAlgorithmException e) {
            sr = new SecureRandom(); // 获取普通的安全随机数生成器
        }
        byte[] buffer = new byte[16];
        sr.nextBytes(buffer); // 用安全随机数填充buffer
        System.out.println(Arrays.toString(buffer));
    }
}
```

### 内部类

嵌套类（类中的一个类），要访问内部类需要先创建外部类的对象，然后创建内部类的对象。
**成员内部类（定义在方法外面）示例：**

```java
class OuterClass {
    int x = 10;
    class InnerClass {
        int y = 5;
        public int myInnerMethod() {
            return x;
        }
    }
}
public class MyMainClass {
    public static void main(String[] args) {
        
        OuterClass myOuter = new OuterClass();
//        在外界访问内部类格式：外部类.内部类 = new 外部类.new 内部类();
        OuterClass.InnerClass = new OuterClass.new InnerClass();
        
        OuterClass.InnerClass myInner = myOuter.new InnerClass();
        System.out.println(myInner.y + myOuter.x);
        System.out.println(myInner.myInnerMethod());
    }
}
```
当Private修饰内部类时，外部对象将无法访问内部类。
当Static修饰内部类时，可以不创建外部类对象访问内部类，无法访问外部类的成员。

方法内部类（定义在方法里面）
静态内部类（定义在方法外面，且被static修饰）
```java

public class TestA {

    private static int code = 200;
    public static void show(){
        System.out.println("外部类中的show方法");
    }

    /**
     * 在静态内部类中访问的成员属性和方法必须是静态的
     */
    public static class TestB{
        public void testB(){
            show();
            System.out.println(code);
            System.out.println("静态内部类中的testB方法");
        }
    }

    public static void main(String[] args) {
        TestB testB = new TestB();
    }
}


public class Test01 {
    public static void main(String[] args) {
//        外界访问静态内部类格式：外部类.内部类 = new 外部类.内部类
        TestA.TestB testB = new TestA.TestB();
        testB.testB();
    }
}

```

匿名内部类
```java

public interface TestA {
    void TestA();
}

public class Test01 {
    public static void main(String[] args) {
        /**
         * 匿名内部类
         * 使用匿名内部类不需要创建实现类和子类，可以通过new 内部类的形式创建实现类和子类
         */
        TestA testA = new TestA() {
            @Override
            public void TestA() {
                System.out.println("匿名内部类TestA方法");
            }
        };
        testA.TestA();
    }
}
```
```java
public abstract class AnimalParent {

    public abstract void eat();
}
public class Dog extends AnimalParent{
    /**
     * 正常实现抽象类是需要继承父类重写父类方法
     * 如果新增一个Cat类，也是需要继承父类重写父类方法
     * 而直接使用匿名内部类不需要创建子类或者实现类
     */
    @Override
    public void eat() {
        System.out.println("我是狗类，吃骨头");
    }
}
public class Test01 {
    public static void main(String[] args) {
//        正常实现抽象类是需要继承父类重写父类方法
        AnimalParent dog = new Dog();
        dog.eat();
//        使用匿名内部类不需要创建子类或者实现类，底层在编译阶段自动创建一个null子类继承抽象类/接口
        AnimalParent cat = new AnimalParent() {
            @Override
            public void eat() {
                System.out.println("我是猫类，吃鱼");
            }
        };
        cat.eat();
    }
}

```

### Object类
所有类默认继承Object类，如果该类已继承则间接继承Object类
1. 无参构造方法
2. clone()
3. equals(Object obj)
如果自定义对象没有重写Object中的equals()，则比较两个对象的内存地址
如果重写Object中的equals()，则比较两个对象的值
```java
public class Student {
    public int age;
    public String name;

    public Student(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }

    @Override
    public boolean equals(Object object) {
//        如果两个对象的内存地址相同，返回true
        if(this == object){
            return true;
        }
//        如果比较对象为null或者两个对象的类型不一致，返回false
        if (object == null || this.getClass()!= object.getClass()){
            return false;
        }
//        如果两个对象的类型一致，则比较两个对象的值
//        多态强转类型；String类型重写了Object中的equals(),所以比较两个字符串的值
        Student s2 = (Student) object;
        return this.age == s2.age && this.name.equals(s2.name);
    }
}
```
4. finalize() 过时
5. getClass()
6. hashCode()
7. notify()、notifyAll()、wait()
8. synchronized()
9. toString()

## 异常处理

- 必须捕获的异常，包括`Exception`及其子类，但不包括`RuntimeException`及其子类，这种类型的异常称为Checked Exception，或者用throws声明；
- 不需要捕获的异常，包括`Error`及其子类，`RuntimeException`及其子类；
- Java使用异常来表示错误，并通过`try ... catch`捕获异常；
- Java的异常是`class`，并且从`Throwable`继承；
- 不推荐捕获了异常但不进行任何处理。

### 捕获异常

使用try语句定义可能出错的代码块，如果发生错误则执行catch语句定义的代码块，存在多个catch的时，按照顺序执行，当父类异常在前则不执行后面的子类异常，不管结果如何都会执行finally代码块，finally是可选的，一个catch语句也可以匹配多个非继承关系的异常。

```java
package com.lsaiah.java;

public class ExceptionDemo01 {
    public static void main(String[] args) {
        try {
            int[] numbers = {1,2,3};
            System.out.println(numbers[10]);
        }catch (Exception e) {
            e.printStackTrace();
            System.out.println("ArrayIndexOutOfBoundsException");
        }finally {
            System.out.println("The 'try catch' is finished.");
        }
    }
}
```

### 抛出异常

- 调用`printStackTrace()`可以打印异常的传播栈，对于调试非常有用；
- 捕获异常并再次抛出新的异常时，应该持有原始异常信息；
- 通常不要在`finally`中抛出异常。如果在`finally`中抛出异常，应该原始异常加入到原有异常中。调用方可通过`Throwable.getSuppressed()`获取所有添加的`Suppressed Exception`。

throw关键字与异常类型（在Java中可用的许多异常类型：ArithmeticException，FileNotFoundException，ArrayIndexOutOfBoundsException，SecurityException等）自定义错误：

```java
package com.lsaiah.java;

public class ExceptionDemo02 {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
//            throw new IllegalArgumentException();   //丢失原始异常NullPointerException
            throw new IllegalArgumentException(e);  //把原始的Exception实例传进去
        }
    }
    static void process2() {
        throw new NullPointerException();
    }
}

```

在`try`或`catch`语句块中抛出异常，不会影响`finally`执行，JVM会先执行`finally`后抛出异常。如果再`finally`语句中抛出异常，那么`catch`语句的异常被屏蔽（Suppressed Exception）。

### 自定义异常

Java的异常是`class`，它的继承关系如下：

```
                     ┌───────────┐
                     │  Object   │
                     └───────────┘
                           ▲
                           │
                     ┌───────────┐
                     │ Throwable │
                     └───────────┘
                           ▲
                 ┌─────────┴─────────┐
                 │                   │
           ┌───────────┐       ┌───────────┐
           │   Error   │       │ Exception │
           └───────────┘       └───────────┘
                 ▲                   ▲
         ┌───────┘              ┌────┴──────────┐
         │                      │               │
┌─────────────────┐    ┌─────────────────┐┌───────────┐
│OutOfMemoryError │... │RuntimeException ││IOException│...
└─────────────────┘    └─────────────────┘└───────────┘
                                ▲
                    ┌───────────┴─────────────┐
                    │                         │
         ┌─────────────────────┐ ┌─────────────────────────┐
         │NullPointerException │ │IllegalArgumentException │...
         └─────────────────────┘ └─────────────────────────┘
```

Java标准库定义的常用异常包括：

```
Exception
│
├─ RuntimeException
│  │
│  ├─ NullPointerException
│  │
│  ├─ IndexOutOfBoundsException
│  │
│  ├─ SecurityException
│  │
│  └─ IllegalArgumentException
│     │
│     └─ NumberFormatException
│
├─ IOException
│  │
│  ├─ UnsupportedCharsetException
│  │
│  ├─ FileNotFoundException
│  │
│  └─ SocketException
│
├─ ParseException
│
├─ GeneralSecurityException
│
├─ SQLException
│
└─ TimeoutException
```

- 抛出异常时，尽量复用JDK已定义的异常类型；
- 自定义异常体系时，推荐从`RuntimeException`派生“根异常”，再派生出业务异常；
- 自定义异常时，应该提供多种构造方法。

```java
package com.lsaiah.java;
//根异常从RuntimeException派生并创建构造方法：
public class BaseException extends RuntimeException {
    public BaseException() {
        super();
    }
    public BaseException(String message) {
        super(message);
    }

    public BaseException(String message, Throwable cause) {
        super(message, cause);
    }

    public BaseException(Throwable cause) {
        super(cause);
    }
}
//其它业务异常从BaseException派生：
class UserNotFoundException extends BaseException{

    public UserNotFoundException(String error_message) {
        super(error_message);
    }
}

class LoginFailedException extends BaseException{
    public LoginFailedException(String error_mess) {
        super(error_mess);
    }
}
package com.lsaiah.java;
//在主类中调用异常
public class Main {
    public static void main(String[] args) {
        try {
            String token = login("admin", "pass");
            System.out.println("Token: " + token);
        }catch (LoginFailedException | UserNotFoundException e){
            e.printStackTrace();
        }
    }
    static String login(String username, String password){
        if (username.equals("admin")){
            if (password.equals("password")){
                return "xxx";
            }else {
                throw new LoginFailedException("登录失败：用户名或密码错误");
            }
        }else{
            throw new UserNotFoundException("用户不存在");
        }
    }
}

```

### NullPointerException

- `NullPointerException`是Java代码常见的逻辑错误，应当早暴露，早修复；
- `java -XX:+ShowCodeDetailsInExceptionMessages Main.java` 可以启用Java 14的增强异常信息来查看`NullPointerException`的详细错误信息。

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        System.out.println(p.address.city.toLowerCase());
    }
}

class Person {
    String[] name = new String[2];
    Address address = new Address();
}

class Address {
    String city;
    String street;
    String zipcode;
}
```

### 断言

- 断言是一种调试方式，断言失败会抛出`AssertionError`，只能在开发和测试阶段启用断言。JVM默认关闭断言指令，要执行`assert`语句需要在执行时传递参数`-enableassertions`可简写`-ea`启用断言，还可以有选择地对特定地类`-ea:com.itranswarp.sample.Main`或特定的包`-ea:com.itranswarp.sample...`启用断言；
- 对可恢复的错误不能使用断言，而应该抛出异常；
- 断言很少被使用，更好的方法是编写单元测试。

```java
public static void main(String[] args) {
    double x = Math.abs(-123.45);
    assert x >= 0 : "x must >= 0";
    System.out.println(x);
}
```

## 日志

### 使用JDK Logging

Java标准库内置了日志包`java.util.logging`使用JDK Loggin，定义了7个日志级别：

- SEVERE
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST

默认级别INFO，INFO以下的日志不会被打印出来，需要在main()方法运行之前修改配置，在JVM启动时传递参数`-Djava.util.logging.config.file=`。

- 日志是为了替代`System.out.println()`，可以定义格式，重定向到文件等；
- 日志可以存档，便于追踪问题；
- 日志记录可以按级别分类，便于打开或关闭某些级别；
- 可以根据配置文件调整日志，无需修改代码；
- Java标准库提供了`java.util.logging`来实现日志功能。

```java
package com.lsaiah.java;

import java.io.UnsupportedEncodingException;
import java.util.logging.Logger;

public class LoggingDemo {
    public static void main(String[] args) {
        Logger logger = Logger.getLogger(LoggingDemo.class.getName());
        logger.info("Start Process ...");
        try {
            "".getBytes("invalidCharsetName");
        } catch (UnsupportedEncodingException e) {
            // TODO: 使用logger.severe()打印异常
            logger.severe(e.toString());
        }
        logger.info("Process end ...");
    }
}
```

### 使用Commons Logging

- Commons Logging是使用最广泛的日志模块；
- Commons Logging的API非常简单；
- Commons Logging是一个第三方提供的库可以自动检测并使用其他日志模块，默认使用Log4j，如果没有再使用JDK Logging，使用前先导入`commons-logging-x.x.jar`，在编译`javac -cp commons-logging-x.x.jar Main.java`和执行`java -cp .;commons-logging-x.x.jar Main`时需要指定classpath。

Commons Logging定义了6个日志级别：

- FATAL
- ERROR
- WARNING
- INFO
- DEBUG
- TRACE

```java
// 在静态方法中引用Log:
public class Main {
    static final Log log = LogFactory.getLog(Main.class);

    static void foo() {
        log.info("foo");
    }
}

// 在实例方法中引用Log:
public class Person {
    protected final Log log = LogFactory.getLog(getClass());

    void foo() {
        log.info("foo");
    }
}

// 在子类中使用父类实例化的log:
public class Student extends Person {
    void bar() {
        log.info("bar");
    }
}
//使用重载方法打印出异常：error(String, Throwable)
try {
    ...
} catch (Exception e) {
    log.error("got exception!", e);
}
```

### 使用Log4j

- 导入Log4j的Jar包，通过Commons Logging实现日志，不需要修改代码即可使用Log4j；
- 使用Log4j只需要把log4j2.xml和相关jar放入classpath；
- 如果要更换Log4j，只需要移除log4j2.xml和相关jar；
- 只有扩展Log4j时，才需要引用Log4j的接口（例如，将日志加密写入数据库的功能，需要自己开发）。

Log4j参考配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
    <Properties>
        <!-- 定义日志格式 -->
        <Property name="log.pattern">%d{MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36}%n%msg%n%n</Property>
        <!-- 定义文件名变量 -->
        <Property name="file.err.filename">log/err.log</Property>
        <Property name="file.err.pattern">log/err.%i.log.gz</Property>
    </Properties>
    <!-- 定义Appender，即目的地 -->
    <Appenders>
        <!-- 定义输出到屏幕 -->
        <Console name="console" target="SYSTEM_OUT">
            <!-- 日志格式引用上面定义的log.pattern -->
            <PatternLayout pattern="${log.pattern}" />
        </Console>
        <!-- 定义输出到文件,文件名引用上面定义的file.err.filename -->
        <RollingFile name="err" bufferedIO="true" fileName="${file.err.filename}" filePattern="${file.err.pattern}">
            <PatternLayout pattern="${log.pattern}" />
            <Policies>
                <!-- 根据文件大小自动切割日志 -->
                <SizeBasedTriggeringPolicy size="1 MB" />
            </Policies>
            <!-- 保留最近10份 -->
            <DefaultRolloverStrategy max="10" />
        </RollingFile>
    </Appenders>
    <Loggers>
        <Root level="info">
            <!-- 对info级别的日志，输出到console -->
            <AppenderRef ref="console" level="info" />
            <!-- 对error级别的日志，输出到err，即上面定义的RollingFile -->
            <AppenderRef ref="err" level="error" />
        </Root>
    </Loggers>
</Configuration>
```

```java
public class Log4JTest {
    private static  final Logger LOGGER = Logger.getLogger(Log4JTest.class);
    public static void  main(String[] agrs){
        // 记录debug级别的信息
        LOGGER.debug("This is debug message.");
        // 记录info级别的信息
        LOGGER.info("This is info message.");
        // 记录warn级别的信息
        LOGGER.info("This is warn message.");
        // 记录error级别的信息
        LOGGER.error("This is error message.");
    }
```

### 使用SL4J和Logback

- SLF4J和Logback可以取代Commons Logging和Log4j，SLF4接口更方便，Logback性能更好；
- 始终使用SLF4J的接口写入日志，使用Logback只需要配置，不需要修改代码。


| Commons Logging                       | SLF4J                   |
| :------------------------------------ | :---------------------- |
| org.apache.commons.logging.Log        | org.slf4j.Logger        |
| org.apache.commons.logging.LogFactory | org.slf4j.LoggerFactory |

Logback配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <file>log/output.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
            <fileNamePattern>log/output.log.%i</fileNamePattern>
        </rollingPolicy>
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>1MB</MaxFileSize>
        </triggeringPolicy>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
    </root>
</configuration>
```

## 反射

反射为了解决在运行期，对某个实例一无所知的情况下调用方法。由于JVM为每个加载的`class`创建了对应的`Class`实例，并在实例中保存了该`class`的所有信息，包括类名、包名、父类、实现的接口、所有方法、字段等，因此，如果获取了某个`Class`实例，我们就可以通过这个`Class`实例获取到该实例对应的`class`的所有信息。这种通过`Class`实例获取`class`信息的方法称为反射（Reflection）

### Class类

- JVM为每个加载的`class`及`interface`创建了对应的`Class`实例来保存`class`及`interface`的所有信息；
- 获取一个`class`对应的`Class`实例后，就可以获取该`class`的所有信息；
- 通过Class实例获取`class`信息的方法称为反射（Reflection）；
- JVM总是动态加载`class`，可以在运行期根据条件来控制加载class。

获取`class`的`Class`实例有三种方法：

方法一：直接通过一个`class`的静态变量`class`获取：

```java
Class cls = String.class;
```

方法二：如果我们有一个实例变量，可以通过该实例变量提供的`getClass()`方法获取：

```java
String s = "Hello";
Class cls = s.getClass();
```

方法三：如果知道一个`class`的完整类名，可以通过静态方法`Class.forName()`获取：

```java
Class cls = Class.forName("java.lang.String");
```

因为`Class`实例在JVM中是唯一的，所以上述方法获取的`Class`实例是同一个实例。

- 用`instanceof`不但匹配指定类型，还匹配指定类型的子类。而用`==`判断`class`实例可以精确地判断数据类型，而不能对子类型比较。

```java
Integer n = new Integer(123);
boolean b1 = n instanceof Integer; //true，因为n是Integer类型
boolean b2 = n instanceof Number; //true，因为n是Number类型的子类
//当获取实例n时，通过n.getClass()反射获取Integer的class信息
boolean b3 = n.getClass() == Integer.class; //true，因为n.getClass()返回Integer.class
boolean b4 = n.getClass() == Number.class; //false，因为返回的Integer.class != Number.class
```

- 如果获取到一个`Class`实例，可以通过该`Class`实例来创建对应类型`class`的实例。相当于`new String()`，通过`cls.newInstance()`创建的实例具有局限性，只能调用`public`的无参构造方法，对于带参数和非`public`的构造方法无法被调用。

```java
//获取String的Class实例
Class cls = String.class;
//创建String的实例
String s = (String)cls.newInstance();
```

- 利用JVM动态加载`class`的特性，我们才能在运行期根据条件加载不同的实现类。例如，Commons Logging总是优先使用Log4j，只有当Log4j不存在时，才使用JDK的logging。利用JVM动态加载特性，大致的实现代码如下：

```java
// Commons Logging优先使用Log4j:
LogFactory factory = null;
if (isClassPresent("org.apache.logging.log4j.Logger")) {
    factory = createLog4j();
} else {
    factory = createJdkLog();
}

boolean isClassPresent(String name) {
    try {
        Class.forName(name);
        return true;
    } catch (Exception e) {
        return false;
    }
}
```

### 访问字段

- Java的反射API提供的`Field`类封装了字段的所有信息：
- 通过`Class`实例的方法可以获取`Field`实例：`getField()`，`getFields()`，`getDeclaredField()`，`getDeclaredFields()`；
- 通过Field实例可以获取字段信息：`getName()`，`getType()`，`getModifiers()`；
- 通过Field实例可以读取或设置某个对象的字段，如果存在访问限制，要首先调用`setAccessible(true)`来访问非`public`字段。
- 通过反射读写字段是一种非常规方法，它会破坏对象的封装。更多地是给工具或者底层框架来使用，目的是在不知道目标实例任何信息的情况下获取特定字段的值。JVM运行期存在`SecurityManager`，那么它会根据规则进行检查，有可能阻止`setAccessible(true)`。

> Field getField(name)：根据字段名获取某个public的field（包括父类）
>
> Field getDeclaredField(name)：根据字段名获取当前类的某个field（不包括父类）
>
> Field[] getFields()：获取所有public的field（包括父类）
>
> Field[] getDeclaredFields()：获取当前类的所有field（不包括父类）

```java
package com.lsaiah.java;
import java.lang.reflect.Field;

public class FieldDemo {
    public static void main(String[] args) throws Exception {
        Person05 p = new Person05("张三");    //创建class的实例p
        System.out.println(p.getName());    //通过实例.方法直接调用
        Class c = p.getClass(); //通过反射获取实例p的Class
        Field f = c.getDeclaredField("name"); //创建Field的实例f，通过p的Class调用getDeclaredField方法根据字段名获取当前类的某个field
        f.setAccessible(true); //设置private字段可访问
        Object value = f.get(p); //获取字段的值
        System.out.println(value);
        f.set(p,"李四");  //设置字段的值
        Object newValue = f.get(p);
        System.out.println(newValue);
    }
}
class Person05{
    public Person05(String name) {
        this.name = name;
    }
    public String getName() {
        return this.name;
    }
    private String name;
}


```

> Method getMethod(name, Class...)：获取某个public的Method（包括父类） Method getDeclaredMethod(name, Class...)：获取当前类的某个Method（不包括父类） Method[] getMethods()：获取所有public的Method（包括父类） Method[] getDeclaredMethods()：获取当前类的所有Method（不包括父类）

- Java的反射API提供的Method对象封装了方法的所有信息：
- 通过`Class`实例的方法可以获取`Method`实例：`getMethod()`，`getMethods()`，`getDeclaredMethod()`，`getDeclaredMethods()`；
- 通过`Method`实例可以获取方法信息：`getName()`，`getReturnType()`，`getParameterTypes()`，`getModifiers()`；
- 通过`Method`实例可以调用某个对象的方法：`Object invoke(Object instance, Object... parameters)`；
- 通过设置`setAccessible(true)`来访问非`public`方法；
- 通过反射调用方法时，仍然遵循多态原则，即总是调用实际类型的覆写方法（如果存在）。

```java
//通过获取到Method对象调用非静态public方法
import java.lang.reflect.Method;
public class Main{
    public static void main(String[] args){
        String s = "Hello world";
        Method m = String.class.getMethod("substring", int.class);  //获取String substring(int)方法，参数为int
        String r = (String) m.invoke(s,6);  //调用该方法并获取结果
        System.out.println(r);
    }
}

//调用静态方法
import java.lang.reflect.Method;
public class Main{
    public static void main(String[] args){
        Method m = Integer.class.getMethod("parseInt", String.class);   //获取Integer.parseInt(String)方法，参数为String
        Integer n = (Integer) m.invoke(null,"12345");   //调用该静态方法并获取结果
        System.out.println(n);
    }
}

//调用非public方法
import java.lang.reflect.Method;
public class Main{
    public static void main(String[] args) {
        Person p = new Person();
        Method m = p.getClass().getDeclaredMethod("setName", String.class);
        m.setAccessible(true);
        m.invoke(p, "Bob");
        System.out.println(p.name)
    }
}
class Person{
    String name;
    private void setName(String name){
        this.name = name;
    }
}

```

### 调用构造方法

- `Constructor`对象封装了构造方法的所有信息；
- 通过`Class`实例的方法可以获取`Constructor`实例：`getConstructor()`，`getConstructors()`，`getDeclaredConstructor()`，`getDeclaredConstructors()`；
- 通过`Constructor`实例可以创建一个实例对象：`newInstance(Object... parameters)`； 通过设置`setAccessible(true)`来访问非`public`构造方法。

通常调用方法使用`new`创建新的实例：

```java
Person p = new Person();
```

通过反射创建新的实例，可以通过Class提供的newInstance()方法：

```java
Person p = Person.class.newInstance();
```

但是通过反射调用构造方法具有局限性，只能调用该类public无参构造方法。有参数或者不是public的构造方法就无法使用class.newInstance()调用。

为了调用任意构造方法，Java反射的API提供了Constructor对象，它包含一个构造方法的所有信息，可以创建一个实例。

通过Class实例获取Constructor的方法如下：

- `getConstructor(Class...)`：获取某个`public`的`Constructor`；
- `getDeclaredConstructor(Class...)`：获取某个`Constructor`；
- `getConstructors()`：获取所有`public`的`Constructor`；
- `getDeclaredConstructors()`：获取所有`Constructor`。

注意`Constructor`总是当前类定义的构造方法，和父类无关，因此不存在多态的问题。

调用非`public`的`Constructor`时，必须首先通过`setAccessible(true)`设置允许访问。`setAccessible(true)`可能会失败。

```java
import java.lang.reflect.Constructor;
publci class Main{
    public static void main(String[] args){
        //获取构造方法Integer(int):
        Constructor cons1 = Integer.class.getConstructor(int.class);
        //调用构造方法：
        Integer n1 = (Integer) cons1.newInstance(123);
        System.out.print(n1);
        //获取构造方法Integer(String):
        Constructor cons2 = Integer.class.getConstructor(String.class);
        Integer n2 = (Integer) cons2.newInstance("456");
        System.out.println(n2);
    }
}

```

### 获取继承关系

通过`Class`对象可以获取继承关系：

- `Class getSuperclass()`：获取父类类型；
- `Class[] getInterfaces()`：获取当前类实现的所有接口。
- 通过`Class`对象的`isAssignableFrom()`方法可以判断一个向上转型是否可以实现。

### 动态代理

所有`interface`类型的变量总是通过向上转型并指向某个实例，静态代码：

```java
//定义接口：
public interface Hello {
    void morning(String name);
}

//编写实现类：
public class HelloWorld implements Hello {
    public void morning(String name){
        System.out.println("Good morning," + name);
    }
}

//创建实例，HelloWorld向上转型为Hello接口并调用：
Hello helo = new HelloWorld();
hello.morning("Bob");

```

Java标准库提供了动态代理功能，允许在运行期动态创建一个接口的实例；

动态代理是通过`Proxy`创建代理对象，然后将接口方法“代理”给`InvocationHandler`完成的。

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class Main{
    public static void main(String[] args) {
        //1. 定义一个InvocationHandler实例，它负责实现接口的方法调用
        InvocationHandler handler = new InvocationHandler(){
            //3. 将返回的Object强制转换为接口
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throw Throwable {
                System.out.println(method);
                if(method.getName().equals("morning")){
                    System.out.println("Good morning, " + args[0]);
                }
                return null;
            }
        };
        /*
         * 2. 通过Proxy.newProxyInstance()创建interface实例，它需要3个参数
         *  a. Hello.class.getClassLoader()，使用的ClassLoader，通常就是接口类的ClassLoader。
         *  b. new Class[] { Hello.class }，需要实现的接口数组，至少需要传入一个接口进去
         *  c. handler，用来处理接口方法调用的InvocationHandler实例
         */
        Hello hello = (Hello) Proxy.newProxyInstance(
            Hello.class.getClassLoader(),
            new Class[] { Hello.class },
            handler);
        hello.morning("Bob");
    }
}

interface Hello {
    void morning(String name);
}

```

## 注解

### 使用注解

- 注解（Annotation）是Java语言用于工具处理的标注：
- 注解可以配置参数，没有指定配置的参数使用默认值；
- 如果参数名称是`value`，且只有一个参数，那么可以省略参数名称。

```java
public class Hello {
    @Check(min=0, max=100, value=55)
    public int n;

    @Check(value=99)
    public int p;

    @Check(99) // @Check(value=99)
    public int x;

    @Check
    public int y;
}

```

`@Check`就是一个注解。第一个`@Check(min=0, max=100, value=55)`明确定义了三个参数，第二个`@Check(value=99)`只定义了一个`value`参数，它实际上和`@Check(99)`是完全一样的。最后一个`@Check`表示所有参数都使用默认值。

### 定义注解

- Java使用`@interface`定义注解：
- 可定义多个参数和默认值`default`，核心参数使用`value`名称；
- 必须设置`@Target`来指定`Annotation`可以应用的范围；
- 应当设置`@Retention(RetentionPolicy.RUNTIME)`便于运行期读取该`Annotation`。
- 非必须要元注解：`@Inherited`定义子类是否可继承父类定义的注解，仅针对`@Target(ElementType.TYPE)`类型的类注解有效，对`interface`无效。`@Repeatable`可定义注解是否可重复，经过`@Repatable`修饰的注解在某个类型声明处可以添加多个`@Report`注解。注解`@Retention`定义了注解的生命周期，仅编译期：`RetentionPolicy.SOURCE`，仅class文件：`RetentionPolicy.CLASS`，运行期：`RetentionPolicy.RUNTIME`

### 处理注解

可以在运行期通过反射读取`RUNTIME`类型的注解，注意不要漏写`@Retention(RetentionPolicy.RUNTIME)`，否则运行期无法读取到该注解。

判断某个注解是否存在于`Class`、`Field`、`Method`或`Constructor`：

- `Class.isAnnotationPresent(Class)`
- `Field.isAnnotationPresent(Class)`
- `Method.isAnnotationPresent(Class)`
- `Constructor.isAnnotationPresent(Class)`

使用反射API读取Annotation：

- `Class.getAnnotation(Class)`
- `Field.getAnnotation(Class)`
- `Method.getAnnotation(Class)`
- `Constructor.getAnnotation(Class)`

使用反射API读取`Annotation`有两种方法。方法一是先判断`Annotation`是否存在，如果存在，就直接读取：

```java
Class cls = Person.class;
if (cls.isAnnotationPresent(Report.class)) {
    Report report = cls.getAnnotation(Report.class);
    ...
}

```

第二种方法是直接读取`Annotation`，如果`Annotation`不存在，将返回`null`：

```java
Class cls = Person.class;
Report report = cls.getAnnotation(Report.class);
if (report != null) {
   ...
}

```

读取方法参数的`Annotation`可以将参数看成一个数组，而每个参数又可以定义多个注解，所以，一次获取方法参数的所有注解就必须用一个二维数组来表示。例如，对于以下方法定义的注解：

```java
public void hello(@NotNull @Range(max=5) String name, @NotNull String prefix) {
}

```

要读取方法参数的注解，我们先用反射获取`Method`实例，然后读取方法参数的所有注解：

```java
// 获取Method实例:
Method m = ...
// 获取所有参数的Annotation:
Annotation[][] annos = m.getParameterAnnotations();
// 第一个参数（索引为0）的所有Annotation:
Annotation[] annosOfName = annos[0];
for (Annotation anno : annosOfName) {
    if (anno instanceof Range) { // @Range注解
        Range r = (Range) anno;
    }
    if (anno instanceof NotNull) { // @NotNull注解
        NotNull n = (NotNull) anno;
    }
}

```

如何使用注解由程序决定，如`@Range`注解，用来定义一个`String`字段的长度满足`@Range`参数定义：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Range {
    int min() default 0;
    int max() default 255;
}

```

在JavaBean中，可以使用该注解：

```java
public class Person {
    @Range(min=1, max=20)
    public String name;

    @Range(max=10)
    public String city;
}

```

编写一个`Person`实例的检查方法，它可以检查`Person`实例的`String`字段长度是否满足`@Range`的定义：

```java
void check(Person person) throws IllegalArgumentException, ReflectiveOperationException {
    // 遍历所有Field:
    for (Field field : person.getClass().getFields()) {
        // 获取Field定义的@Range:
        Range range = field.getAnnotation(Range.class);
        // 如果@Range存在:
        if (range != null) {
            // 获取Field的值:
            Object value = field.get(person);
            // 如果值是String:
            if (value instanceof String) {
                String s = (String) value;
                // 判断值是否满足@Range的min/max:
                if (s.length() < range.min() || s.length() > range.max()) {
                    throw new IllegalArgumentException("Invalid field: " + field.getName());
                }
            }
        }
    }
}

```

### 注解练习

使用`@Range`注解来检查Java Bean的字段。如果字段类型是`String`，就检查`String`的长度，如果字段是`int`，就检查`int`的范围。

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Retention;
import java.lang.annotation.Target;
//定义注解
@Retention(RetentionPolicy.RUNTIME) //定义注解声明周期为运行时（类型的注解会被加载进JVM，并且在运行期可以被程序读取。）
@Target(ElementType.FIELD)  //定义注解的位置为字段
public @interface Range {
    int min() default 0;
    int max() default 255;
}


public class Person {
    @Range(min = 1, max = 20)
    public String name;
    @Range(max = 10)
    public String city;
    @Range(min = 1, max = 100)
    public int age;

    public Person(String name, String city, int age) {
        this.name = name;
        this.city = city;
        this.age = age;
    }
    @Override
    public String toString() {
        return String.format("{Person: name=%s, city=%s, age=%d}", name, city, age);
    }
}


import java.lang.reflect.Field;

public class Main {
    public static void main(String[] args) throws Exception {
        Person p1 = new Person("Bob", "Beijing", 20);
        Person p2 = new Person("", "Shanghai", 20);
        Person p3 = new Person("Alice", "Shanghai", 199);
        for (Person p : new Person[] { p1, p2, p3 }) {
            try {
                check(p);
                System.out.println("Person " + p + " checked ok.");
            } catch (IllegalArgumentException e) {
                System.out.println("Person " + p + " checked failed: " + e);
            }
        }
    }

    static void check(Person person) throws IllegalArgumentException, ReflectiveOperationException {
        //通过 foreach循环，依次获取person这个实例的public的字段的字段名称（数据类型为Field）
        for (Field field : person.getClass().getFields()) {
            //通过Filed类的 .getAnnotation 方法来获得注解
            Range range = field.getAnnotation(Range.class);
            //判断注解是否为null
            if (range != null) {
                //使用了注解的情况下，通过Filed的 .get方法，来获取指定字段的字段值
                Object value = field.get(person);
                //如果是对 String字段使用的注解
                if (value instanceof String) {
                    String s = (String) value;
                    if (s.length()<range.min() || s.length()>range.max()) {
                        throw new IllegalArgumentException("Invalid filed: "+field.getName());
                    }
                }
                //如果是对 int字段使用的注解
                if (value instanceof Integer) {
                    int i = (int) value;
                    if (i<range.min() || i>range.max()) {
                        throw new IllegalArgumentException("Invalid filed: "+field.getName());
                    }
                }
            }
        }
    }
}

```

## 泛型

### 介绍

- 泛型就是编写模板代码来适应任意类型；
- 泛型的好处是使用时不必对类型进行强制转换，它通过编译器对类型进行检查；
- 注意泛型的继承关系：可以把`ArrayList`向上转型为`List`（`T`不能变！），但不能把`ArrayList`向上转型为`ArrayList`（`T`不能变成父类）。

当数组的类型被定义后，添加不同类型的元素需要强制类型转换，可能发生误转型出现`ClassCastException`，对不同类型的编写不同的`ArrayList`不全面且麻烦，为了解决问题需要使用泛型将`ArrayList`变成一种新的模板`ArrayList`

```java
public class ArrayList<T> {
    private T[] array;
    private int size;
    public void add(T e) {...}
    public void remove(int index) {...}
    public T get(int index) {...}
}

T`可以是任何类型，这样只要编写一种模板就可以创建任意类型的`ArrayList
//创建可存储String的ArrayList
ArrayList<String> strList = new ArrayList<String>();
strList.add("hello"); // OK
String s = strList.get(0); // OK
strList.add(new Integer(123)); // compile error!
Integer n = strList.get(0); // compile error!
//创建可存储Float的ArrayList
ArrayaList<Float> floatList = new ArrayList<Float>();
//创建可存储Person的ArrayList
ArrayList<Person> personList = new ArrayList<Person>();

```

### 使用泛型

- 使用泛型时，把泛型参数``替换为需要的class类型，例如：`ArrayList`，`ArrayList`等；
- 可以省略编译器能自动推断出的类型，例如：`List list = new ArrayList<>();`；
- 不指定泛型参数类型时，编译器会给出警告，且只能将``视为`Object`类型；
- 可以在接口中定义泛型类型，实现此接口的类必须实现正确的泛型类型。

```java
//自定义的类使用泛型接口实现Array.sort();
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Person[] ps = new Person[] {
            new Person("Bob", 61),
            new Person("Alice", 88),
            new Person("Lily", 75),
        };
        Arrays.sort(ps);
        System.out.println(Arrays.toString(ps));
    }
}
class Person implements Comparable<Person> {
    String name;
    int score;
    Person(String name, int score) {
        this.name = name;
        this.score = score;
    }
    public int compareTo(Person other) {
        return this.name.compareTo(other.name);
    }
    public String toString() {
        return this.name + "," + this.score;
    }
}

```

### 编写泛型

- 编写泛型时，需要定义泛型类型``；
- 静态方法不能引用泛型类型``，必须定义其他类型（例如``）来实现静态泛型方法；
- 泛型可以同时定义多种类型，例如`Map`。

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public T getLast() { ... }

    // 静态泛型方法应该使用其他类型区分:
    public static <K> Pair<K> create(K first, K last) {
        return new Pair<K>(first, last);
    }
}

```

### 擦拭法

擦拭法决定了泛型``：

- 不能是基本类型，例如：`int`，因为实际类型是`Object`无法持有基本类型；
- 不能获取带泛型类型的`Class`，例如：`Pair.class`，因为`T`类型`getClass()`返回同一个`Class`实例，在编译后都是`类`；
- 不能判断带泛型类型的类型，例如：`x instanceof Pair`，因为不存在实际的类型.class，只有唯一的类.class；
- 不能实例化`T`类型，例如：`new T()`擦拭后变成`new Object()`，需要借助`Class`参数，通过反射来实例化`T`类型，使用的时候也必须传入`Class`。

泛型方法要防止重复定义方法，例如：`public boolean equals(T obj)`；

子类可以获取父类的泛型类型``。

### extends通配符

使用类似``通配符作为方法参数时表示：

- 方法内部可以调用获取`Number`引用的方法，例如：`Number n = obj.getFirst();`；
- 方法内部无法调用传入`Number`引用的方法（`null`除外），例如：`obj.setFirst(Number n);`。

即一句话总结：使用`extends`通配符表示可以读，不能写。

使用类似``定义泛型类时表示：

- 泛型类型限定为`Number`以及`Number`的子类。

### super通配符

使用类似``通配符作为方法参数时表示：

- 方法内部可以调用传入`Integer`引用的方法，例如：`obj.setFirst(Integer n);`；
- 方法内部无法调用获取`Integer`引用的方法（`Object`除外），例如：`Integer n = obj.getFirst();`。

即使用`super`通配符表示只能写不能读。

使用`extends`和`super`通配符要遵循PECS（Producer Extends Consumer Super）原则。

无限定通配符``很少使用，可以用``替换，同时它是所有``类型的超类。

### 泛型和反射

- 部分反射API是泛型，例如：`Class`，`Constructor`；
- 可以声明带泛型的数组，但不能直接创建带泛型的数组，必须强制转型；
- 可以通过`Array.newInstance(Class, int)`创建`T[]`数组，需要强制转型；

同时使用泛型和可变参数时需要注意：

```java
import java.util.Arrays;

public class Main {  
    public static void main(String[] args) {
        String[] arr = asArray("one", "two", "three");
        System.out.println(Arrays.toString(arr));
        // ClassCastException:
        String[] firstTwo = pickTwo("one", "two", "three");
        System.out.println(Arrays.toString(firstTwo));
    }
    //在pickTwo()方法内部，编译器无法检测K[]的正确类型，因此返回了Object[]
    static <K> K[] pickTwo(K k1, K k2, K k3) {
        return asArray(k1, k2);
    }
    @SafeVarargs
    static <T> T[] asArray(T... objs) {
        return objs;
    }
```

## Java集合简介

Java的集合类定义在`java.util`包中，支持泛型，主要提供了3种集合类，包括`List`，`Set`和`Map`。

- `List`：一种有序列表的集合，例如，按索引排列的`Student`的`List`；
- `Set`：一种保证没有重复元素的集合，例如，所有无重复名称的`Student`的`Set`；
- `Map`：一种通过键值（key-value）查找的映射表集合，例如，根据`Student`的`name`查找对应`Student`的`Map`。

Java集合使用统一的`Iterator`遍历，尽量不要使用遗留接口。

- `Hashtable`：一种线程安全的`Map`实现；
- `Vector`：一种线程安全的`List`实现；
- `Stack`：基于`Vector`实现的`LIFO`的栈。

### 使用List

- `List`是按索引顺序访问的长度可变的有序表，允许`null`元素和重复元素。优先使用`ArrayList`（底层数据结构为数组，可以自动扩容，查询快，增删慢）而不是`LinkedList`（底层数据结构为双向链表，查询慢，增删快），；
- 可以直接使用`for each`遍历`List`，它会自动把`for each`循环变成`Iterator`的调用，原因就在于`Iterable`接口定义了一个`Iterator<E> iterator()`方法，强迫集合类必须返回一个`Iterator`实例；
- `List`可以和`Array`相互转换

`List`转`Array`：

```java
Integer[] array = list.toArray(new Integer[list.size()]);

```

通过`List`接口定义的`T[] toArray(IntFunction<T[]> generator)`方法：

```java
Integer[] array = list.toArray(Integer[]::new);

```

`Array`转`List`：

```java
Integer[] array = { 1, 2, 3 };
List<Integer> list = List.of(array);
//List接口调用List.of()方法返回只读的List,调用add()、remove()方法会抛出UnsupportedOperationException
list.add(999); // UnsupportedOperationException

```

> 练习：给定一组连续的整数，例如：10，11，12，......，20 但其中缺失一个数字，找出缺失的数字。

```java
package com.lsaiah.java;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class ListDemo {
    public static void main(String[] args) {
        // 构造从start到end的序列：
        final int start = 10;
        final int end = 20;
        List<Integer> list = new ArrayList<>();
        for (int i = start; i <= end; i++) {
            list.add(i);
        }
        // 洗牌算法suffle可以随机交换List中的元素位置:
//        Collections.shuffle(list);
        // 随机删除List中的一个元素:
        int removed = list.remove((int) (Math.random() * list.size()));
        int found = findMissingNumber(start, end, list);
        System.out.println(list.toString());
        System.out.println("missing number: " + found);
        System.out.println(removed == found ? "测试成功" : "测试失败");
    }

    static int findMissingNumber(int start, int end, List<Integer> list) {
        //List转Array:
        Integer[] nums = list.toArray(Integer[]::new);
        //数组上升排序
//        Arrays.sort(nums);
        //判断20是否在末位,数组索引从0开始
        if(nums[nums.length-1] != end){
            return end;
        }
        //判断10是否在首位
        else if(nums[0] != start){
            return start;
        }
        //此时缺失的数字一定在(10,20)中
        for (int i=1; i<nums.length;i++){
            int expectedNum = nums[i-1] + 1;
            if (nums[i] != expectedNum) {
                return expectedNum;
            }
        }
        //未缺失任何数字
        return 0;
    }
}

```

### 编写List的equals方法

- 在`List`中查找元素时，`List`的实现类通过元素的`equals()`方法比较两个元素是否相等，因此放入的元素必须正确覆写`equals()`方法，Java标准库提供的`String`、`Integer`等已经覆写了`equals()`方法；
  - 先确定实例“相等”的逻辑，即哪些字段相等，就认为实例相等；
  - 用`instanceof`判断传入的待比较的`Object`是不是当前类型，如果是，继续比较，否则，返回`false`；
  - 对引用类型用`Objects.equals()`比较，对基本类型直接用`==`比较。
- 如果不在`List`中查找元素，就不必覆写`equals()`方法。

```java
//给Person类增加equals方法，使得IndexOf()方法返回正常
package com.lsaiah.java;
import java.util.List;
import java.util.Objects;

public class ListDemo02 {
    public static void main(String[] args) {
        List<Person06> list = List.of(
                new Person06("Xiao", "Ming", 18),
                new Person06("Xiao", "Hong", 25),
                new Person06("Bob", "Smith", 20)
        );
        boolean exist = list.contains(new Person06("Bob", "Smith", 20));
        System.out.println(exist ? "测试成功!" : "测试失败!");
    }
}

class Person06 {
    String firstName;
    String lastName;
    int age;
    public Person06(String firstName, String lastName, int age) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
    }
    @Override
    public boolean equals(Object o) {
        if (o instanceof Person06) {
            Person06 p = (Person06) o;
            return Objects.equals(this.firstName, p.firstName) && Objects.equals(this.lastName, p.lastName) && this.age == p.age;
        }
        return false;
    }
}

```

### 使用Map

- `Map`是一种映射表，可以通过`key`快速查找`value`。
- 可以通过`for each`遍历`keySet()`，也可以通过`for each`遍历`entrySet()`，直接获取`key-value`。
- 最常用的一种`Map`实现是`HashMap`。

```java
//编写一个根据name查找score的程序，并利用Map充当缓存以提高查找效率：
package com.lsaiah.java;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class MapDemo {
    public static void main(String[] args) {
        List<Student01> list = List.of(
                new Student01("Bob", 78),
                new Student01("Alice", 85),
                new Student01("Brush", 66),
                new Student01("Newton", 99));
        var holder = new Students(list);
        System.out.println(holder.getScore("Bob") == 78 ? "测试成功!" : "测试失败!");
        System.out.println(holder.getScore("Alice") == 85 ? "测试成功!" : "测试失败!");
        System.out.println(holder.getScore("Tom") == -1 ? "测试成功!" : "测试失败!");
    }
}
class Students {
    List<Student01> list;
    Map<String, Integer> cache;

    Students(List<Student01> list) {
        this.list = list;
        cache = new HashMap<>();
    }
    /**
     * 根据name查找score，找到返回score，未找到返回-1
     */
    int getScore(String name) {
        // 先在Map中查找:
        Integer score = this.cache.get(name);
        if (score == null) {
            // 如果找不到，就在list中找
            score = findInList(name);
        }
        // 如果找到了就加入到Map缓存中
        else {
            cache.put(name,score);
            score = this.cache.get(name);
        }
        return score == null ? -1 : score.intValue();
    }

    Integer findInList(String name) {
        for (var ss : this.list) {
            if (ss.name.equals(name)) {
                return ss.score;
            }
        }
        return null;
    }
}

class Student01 {
    String name;
    int score;

    Student01(String name, int score) {
        this.name = name;
        this.score = score;
    }
}

```

### 编写Map的equals和hashCode

要正确使用`HashMap`，作为`key`的类必须正确覆写`equals()`和`hashCode()`方法，频繁自动扩容对性能影响很大，最好创建`HashMap`时指定容量；

一个类如果覆写了`equals()`，就必须覆写`hashCode()`，并且覆写规则是：

- 如果`equals()`返回`true`，则`hashCode()`返回值必须相等；
- 如果`equals()`返回`false`，则`hashCode()`返回值尽量不要相等。

实现`hashCode()`方法可以通过`Objects.hashCode()`辅助方法实现。

```java
public class Person {
    String firstName;
    String lastName;
    int age;
	@Override
    boolean equals(Object o){
        if(o instanceof Person){
            Person p = (Person) o;
            return Object.equals(firstName,lastName,age);
        }
        return false;
    }
    @Override
    int hashCode() {
		return Objects.hash(firstName, lastName, age);
    }
}

```

### EnumMap

如果`Map`的key是`enum`类型，推荐使用`EnumMap`，既保证速度，也不浪费空间。

使用`EnumMap`的时候，根据面向抽象编程的原则，应持有`Map`接口。

```java
import java.time.DayOfWeek;
import java.util.*;
public class EnumDemo {
    public static void main(String[] args) {
        Map<DayOfWeek, String> map = new EnumMap<>(DayOfWeek.class);
        map.put(DayOfWeek.MONDAY, "星期一");
        map.put(DayOfWeek.TUESDAY, "星期二");
        map.put(DayOfWeek.WEDNESDAY, "星期三");
        map.put(DayOfWeek.THURSDAY, "星期四");
        map.put(DayOfWeek.FRIDAY, "星期五");
        map.put(DayOfWeek.SATURDAY, "星期六");
        map.put(DayOfWeek.SUNDAY, "星期日");
        System.out.println(map);
        System.out.println(map.get(DayOfWeek.MONDAY));
    }
}

```

### TreeMap

`Map`在遍历时严格按照Key的顺序遍历，最常用的实现父类`SortedMap`的子类`TreeMap`；

作为`SortedMap`的Key必须实现`Comparable`接口，`String`、`Integer`这些类已经实现了`Comparable`接口，因此可以直接作为Key使用。或者传入`Comparator`；

要严格按照`compare()`规范实现比较逻辑，否则`TreeMap`将不能正常工作，注意到`Comparator`接口要求实现一个比较方法，它负责比较传入的两个元素`a`和`b`，如果`a<b`，则返回负数，通常是`-1`，如果`a==b`，则返回`0`，如果`a>b`，则返回正数，通常是`1`。`TreeMap`内部根据比较结果对Key进行排序。。

```java
package com.lsaiah.java;
import java.util.Comparator;
import java.util.Map;
import java.util.TreeMap;

public class TreeMapDemo {
    public static void main(String[] args) {
        Map<Student02, Integer> map = new TreeMap<>(new Comparator<Student02>() {
            public int compare(Student02 p1, Student02 p2) {
                if (p1.score == p2.score) {
                    return 0;
                }
                return p1.score > p2.score ? -1 : 1;
            }
        });
        map.put(new Student02("Tom", 77), 1);
        map.put(new Student02("Bob", 66), 2);
        map.put(new Student02("Lily", 99), 3);
        for (Student02 key : map.keySet()) {
            System.out.println(key);
        }
        System.out.println(map.get(new Student02("Bob", 66)));
    }
}

class Student02 {
    public String name;
    public int score;
    Student02(String name, int score) {
        this.name = name;
        this.score = score;
    }
    public String toString() {
        return String.format("{%s: score=%d}", name, score);
    }
}

```

### Properties

Java集合库提供的`Properties`用于读写配置文件`.properties`。`.properties`文件可以使用UTF-8编码。

可以从文件系统、classpath或其他任何地方读取`.properties`文件。

读写`Properties`时，注意仅使用`getProperty()`和`setProperty()`方法，不要调用继承而来的`get()`和`put()`等方法。

```java
# setting.properties
last_open_file=/data/hello.txt
auto_save_interval=60
//从文件读取
String f = "setting.properties";
//1. 创建Properties实例
Properties props = new Properties();
//2. 调用load()读取文件
props.load(new java.io.FileInputStream(f));
//3. 调用getProperty()读取文件配置
String filepath = props.getProperty("last_open_file");
String interval = props.getProperty("auto_save_interval", "120");

```

```java
//从classpath读取字节流
Properties props = new Properties();
props.load(getClass().getResourceAsStream("/common/setting.properties"));

```

```java
//从内存读取字节流
String settings = "# test" + "\n" + "course=Java" + "\n" + "last_open_date=2019-08-07T12:35:01";
ByteArrayInputStream input = new ByteArrayInputStream(settings.getBytes("UTF-8"));
Properties props = new Properties();
props.load(input);
System.out.println(props.getProperty("course"));

```

```java
//写入配置文件
Properties props = new Properties();
props.setProperty("url", "http://www.lsaiah.cn");
props.setProperty("language", "Java");
props.store(new FileOutputStream("C:\\conf\\setting.properties"), "这是写入的properties注释");

```

```java
//load(InputStream)默认以ASCII编码读取字节流乱码，需要使用load(Reader)读取为UTF-8编码字符流
Properties props = new Properties();
props.load(new FileReader("settings.properties", StandardCharsets.UTF_8));

```

### Set

`Set`用于存储不重复的元素集合：

- 放入`HashSet`的元素与作为`HashMap`的key要求相同；
- 放入`TreeSet`的元素与作为`TreeMap`的Key要求相同；

利用`Set`可以去除重复元素；

遍历`SortedSet`按照元素的排序顺序遍历，也可以自定义排序算法。

> 练习：在聊天软件中，发送方发送消息时，遇到网络超时后就会自动重发，因此接收方可能会收到重复的消息，在显示给用户看的时候，需要首先去重。请练习使用`Set`去除重复的消息：

```java
package com.lsaiah.java;
import java.util.*;

public class SetDemo {
    public static void main(String[] args) {
        List<Message> received = List.of(
                new Message(1, "Hello!"),
                new Message(2, "发工资了吗？"),
                new Message(2, "发工资了吗？"),
                new Message(3, "去哪吃饭？"),
                new Message(3, "去哪吃饭？"),
                new Message(4, "Bye")
        );
        List<Message> displayMessages = process(received);
        for (Message message : displayMessages) {
            System.out.println(message.text);
        }
    }
    static List<Message> process(List<Message> received) {
        // TODO: 按sequence去除重复消息
        Set<Message> treeSet = new TreeSet<>(new Comparator<Message>() {
            @Override
            public int compare(Message m1,Message m2) {
                return Integer.compare(m1.sequence, m2.sequence);
            }
        });
        treeSet.addAll(received);
        //将received重定向为一个可修改的新创建的空的ArrayList
        received = new ArrayList<>();
        //将Set的内容全数添加给received
        received.addAll(treeSet);
        return received;
    }
}
class Message {
    public final int sequence;
    public final String text;
    public Message(int sequence, String text) {
        this.sequence = sequence;
        this.text = text;
    }
}

```

### Queue

队列`Queue`实现了一个先进先出（FIFO）的数据结构：

- 通过`add()`/`offer()`方法将元素添加到队尾；
- 通过`remove()`/`poll()`从队首获取元素并删除；
- 通过`element()`/`peek()`从队首获取元素但不删除。

要避免把`null`添加到队列。

### Priority Queue

`PriorityQueue`实现了一个优先队列：从队首获取元素时，总是获取优先级最高的元素；

`PriorityQueue`默认按元素比较的顺序排序（必须实现`Comparable`接口），也可以通过`Comparator`自定义排序算法（元素就不必实现`Comparable`接口）。

```java
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;

public class Main {
    public static void main(String[] args) {
        Queue<User> q = new PriorityQueue<>(new UserComparator());
        // 添加3个元素到队列:
        q.offer(new User("Bob", "A10"));
        q.offer(new User("Alice", "A2"));
        q.offer(new User("Boss", "V1"));
        System.out.println(q.poll()); // Boss/V1
        System.out.println(q.poll()); // Bob/A1
        System.out.println(q.poll()); // Alice/A2
        System.out.println(q.poll()); // null,因为队列为空
    }
}

class UserComparator implements Comparator<User> {
    public int compare(User u1, User u2) {
        if (u1.number.charAt(0) == u2.number.charAt(0)) {
            // 如果两人的号都是A开头或者都是V开头,比较号的大小:
            int num1 = Integer.parseInt(u1.number.substring(1, u1.number.length()));
            int num2 = Integer.parseInt(u2.number.substring(1, u2.number.length()));
            return Integer.compare(num1, num2);
            //return u1.number.compareTo(u2.number);
        }
        if (u1.number.charAt(0) == 'V') {
            // u1的号码是V开头,优先级高:
            return -1;
        } else {
            return 1;
        }
    }
}

class User {
    public final String name;
    public final String number;

    public User(String name, String number) {
        this.name = name;
        this.number = number;
    }

    public String toString() {
        return name + "/" + number;
    }
}

```

### Deque

`Deque`实现了一个双端队列（Double Ended Queue），它可以：

- 将元素添加到队尾或队首：`addLast()`/`offerLast()`/`addFirst()`/`offerFirst()`；
- 从队首／队尾获取元素并删除：`removeFirst()`/`pollFirst()`/`removeLast()`/`pollLast()`；
- 从队首／队尾获取元素但不删除：`getFirst()`/`peekFirst()`/`getLast()`/`peekLast()`；
- 总是调用`xxxFirst()`/`xxxLast()`以便与`Queue`的方法区分开；
- 避免把`null`添加到队列。

```java
Deque<String> deque = new LinkedList<>();

```

### Stack

栈（Stack）是一种后进先出（LIFO）的数据结构，操作栈的元素的方法有：

- 把元素压栈：`push(E)`；
- 把栈顶的元素“弹出”：`pop(E)`；
- 取栈顶元素但不弹出：`peek(E)`。

在Java中，我们用`Deque`可以实现`Stack`的功能，注意只调用`push()`/`pop()`/`peek()`方法，避免调用`Deque`的其他方法。

最后，不要使用遗留类`Stack`。

> 利用Stack把一个给定的整数转换为十六进制

```java
package com.lsaiah.java;
import java.util.ArrayDeque;
import java.util.Deque;

public class StackDemo {
    public static void main(String[] args) {
        String hex = toHex(12500);
        if (hex.equalsIgnoreCase("30D4")){
            System.out.println("测试通过");
        }else {
            System.out.println("测试失败");
        }
    }

    static String toHex(int n) {
        //创建一个空栈，通过Deque实现
        Deque<Character> dq = new LinkedList<>();
        //循环计算12500%16=0压入栈
        for (int i=n; i !=0; i= i/16){
            int remainder = i%16;
            int ch;
            if (remainder >= 10){
                ch = 65 + (remainder-10);
            }else {
                ch = remainder + 48;
            }
            dq.push(Character.valueOf((char) ch));
        }
        StringBuilder result = new StringBuilder(dq.size());
        while (dq.peek() != null){
            result.append(dq.pop());
        }
        return result.toString();
    }
}

```

> 利用Stack把字符串中缀表达式编译为后缀表达式，然后再利用栈执行后缀表达式获得计算结果

> 把带变量的中缀表达式编译为后缀表达式，执行后缀表达式时，传入变量的值并获得计算结果

### Iterator

`Iterator`是一种抽象的数据访问模型。使用`Iterator`模式进行迭代的好处有：

- 对任何集合都采用同一种访问模型；
- 调用者对集合内部结构一无所知；
- 集合类返回的`Iterator`对象知道如何迭代。

Java提供了标准的迭代器模型，即集合类实现`java.util.Iterable`接口，返回`java.util.Iterator`实例。

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ReverseList<String> rlist = new ReverseList<>();
        rlist.add("Apple");
        rlist.add("Orange");
        rlist.add("Pear");
        for (String s : rlist) {
            System.out.println(s);
        }
    }
}

class ReverseList<T> implements Iterable<T> {

    private List<T> list = new ArrayList<>();

    public void add(T t) {
        list.add(t);
    }

    @Override
    public Iterator<T> iterator() {
        return new ReverseIterator(list.size());
    }

    class ReverseIterator implements Iterator<T> {
        int index;

        ReverseIterator(int index) {
            this.index = index;
        }

        @Override
        public boolean hasNext() {
            return index > 0;
        }

        @Override
        public T next() {
            index--;
            return ReverseList.this.list.get(index);
        }
    }
}

```

### Collections

`Collections`类提供了一组工具方法来方便使用集合类：

- 创建空集合；
  - 创建空的List：List<T> emptyList()
  - 创建空的Map：Map<K, V> emptyMap()
  - 创建空的Set：Set<T> emptySet()
- 创建单元素集合；
  - 创建一个元素的List：List<T> singletonList(T o)
  - 创建一个元素的Map：Map<K, V> singletonMap(K key, V value)
  - 创建一个元素的Set：Set<T> singleton(T o)
- 创建不可变集合；
  - 封装成不可变List：List<T> unmodifiableList(List<? extends T> list)
  - 封装成不可变Set：Set<T> unmodifiableSet(Set<? extends T> set)
  - 封装成不可变Map：Map<K, V> unmodifiableMap(Map<? extends K, ? extends V> m)
- 排序／洗牌等操作；
  - Collections.sort(list);
  - Collections.shuffle(list);
- 线程安全集合。
  - 变为线程安全的List：List<T> synchronizedList(List<T> list)
  - 变为线程安全的Set：Set<T> synchronizedSet(Set<T> s)
  - 变为线程安全的Map：Map<K,V> synchronizedMap(Map<K,V> m)

## IO

IO流是一种流式的数据输入/输出模型：

- 二进制数据以`byte`为最小单位在`InputStream`/`OutputStream`中单向流动；
- 字符数据以`char`为最小单位在`Reader`/`Writer`中单向流动。
  - 同步IO是指，读写IO时代码必须等待数据返回后才继续执行后续代码，它的优点是代码编写简单，缺点是CPU执行效率低。
  - 异步IO是指，读写IO时仅发出请求，然后立刻执行后续代码，它的优点是CPU执行效率高，缺点是代码编写复杂。

  Java标准库的`java.io`包提供了同步IO功能，`java.nio`提供异步功能：
- 字节流接口：`InputStream`/`OutputStream`；
- 字符流接口：`Reader`/`Writer`。

### File对象

Java标准库的`java.io.File`对象表示一个文件或者目录：

- 创建`File`对象本身不涉及IO操作；
- 可以获取路径／绝对路径／规范路径：`getPath()`/`getAbsolutePath()`/`getCanonicalPath()`；
- 可以获取目录的文件和子目录：`list()`/`listFiles()`；
- 可以创建或删除文件和目录。

> 利用`File`对象列出指定目录下所有子目录和文件，并按层次打印

> Documents/  word/
>      1.docx
>      2.docx
>      work/
>        abc.doc
>
>   ppt/
>   other/

```java
package com.lsaiah.io;
import java.io.File;
import java.io.IOException;

public class FileDemo {
    public static void main(String[] args) throws IOException {
        File currentDir = new File("C:\\Users\\Administrator.PC-20190504PBOJ\\Documents");
        listDir(currentDir.getCanonicalFile(), 0);
    }

    public static String getSpace(int level) {
        String temp = "";
        for (int i = 0; i < level; i++) {
            temp += "   ";
        }
        return temp;
    }

    static void listDir(File dir, int dir_level) {
        //先把当前目录打印出来（根据传入的目录级别打印空格）
        System.out.println(getSpace(dir_level)+dir+"\\");
        //列出所有文件和子目录
        File[] fs = dir.listFiles();
        if (fs != null) {
            for (File f : fs) {
                //判断f，如果是文件，先打印文件
                if (f.isFile()) {
                    System.out.println(getSpace(dir_level+1)+f.getName());
                }else {
                    //如果是目录，继续递归执行
                    listDir(f,dir_level+1);
                }
            }
        }
    }
}

```

### InputStream

Java标准库的`java.io.InputStream`定义了所有输入流的超类：

- `FileInputStream`实现了文件流输入；
- `ByteArrayInputStream`在内存中模拟一个字节流输入。

总是使用`try(resource)`来保证`InputStream`正确关闭。

```java
public void readFile() throws IOException {
    InputStream input = null;
    try {
        input = new FileInputStream("src/readme.txt");
        int n;
        while ((n = input.read()) != -1) { // 利用while同时读取并判断
            System.out.println(n);
        }
    } finally {
        if (input != null) { input.close(); }
    }
}

//使用Java 7引入的新的try(resource)的语法自动关闭资源
public void readFile() throws IOException {
    try (InputStream input = new FileInputStream("src/readme.txt")) {
        int n;
        while ((n = input.read()) != -1) {
            System.out.println(n);
        }
    } // 编译器在此自动为我们写入finally并调用close()
}
```

**缓冲：**

- `int read(byte[] b)`：读取若干字节并填充到`byte[]`数组，返回读取的字节数
- `int read(byte[] b, int off, int len)`：指定`byte[]`数组的偏移量和最大填充数

```java
public void readFile() throws IOException {
    try (InputStream input = new FileInputStream("src/readme.txt")) {
        // 定义1000个字节大小的缓冲区:
        byte[] buffer = new byte[1024];
        int n;
        while ((n = input.read(buffer)) != -1) { // 读取到缓冲区
            System.out.println("read " + n + " bytes.");
        }
    }
}
```

**阻塞：**

当`inputStream`的`read()`方法读取数据时发送阻塞，必须等待返回结构后才能继续执行。

**InputStream实现类**：`ByteArrayInputStream`可以在内存中模拟一个`InputStream`

```java
public class Main {
    public static void main(String[] args) throws IOException {
        byte[] data = { 72, 101, 108, 108, 111, 33 };
        try (InputStream input = new ByteArrayInputStream(data)) {
            String s = readAsString(input);
            System.out.println(s);
        }
    }

    public static String readAsString(InputStream input) throws IOException {
        int n;
        StringBuilder sb = new StringBuilder();
        while ((n = input.read()) != -1) {
            sb.append((char) n);
        }
        return sb.toString();
    }
}
```

### OutputStream

Java标准库的`java.io.OutputStream`定义了所有输出流的超类：

- `FileOutputStream`实现了文件流输出；
- `ByteArrayOutputStream`在内存中模拟一个字节流输出。

某些情况下需要手动调用`OutputStream`的`flush()`方法来强制输出缓冲区。总是使用`try(resource)`来保证`OutputStream`正确关闭。

```java
public void writeFile() throws IOException {
    try (OutputStream output = new FileOutputStream("out/readme.txt")) {
        output.write("Hello".getBytes("UTF-8")); // Hello
    } // 编译器在此自动为我们写入finally并调用close()
}
```

**阻塞**

和`InputStream`一样，`OutputStream`的`write()`方法也是阻塞的。

**OutputStream实现类**

用`FileOutputStream`可以从文件获取输出流，这是`OutputStream`常用的一个实现类。此外，`ByteArrayOutputStream`可以在内存中模拟一个`OutputStream`：

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        byte[] data;
        try (ByteArrayOutputStream output = new ByteArrayOutputStream()) {
            output.write("Hello ".getBytes("UTF-8"));
            output.write("world!".getBytes("UTF-8"));
            data = output.toByteArray();
        }
        System.out.println(new String(data, "UTF-8"));
    }
}
```

### Fitter模式

Java的IO标准库使用Filter模式为`InputStream`和`OutputStream`增加功能：

- 可以把一个`InputStream`和任意个`FilterInputStream`组合；
- 可以把一个`OutputStream`和任意个`FilterOutputStream`组合。

Filter模式可以在运行期动态增加功能（又称Decorator模式）。

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        byte[] data = "hello, world!".getBytes("UTF-8");
        try (CountInputStream input = new CountInputStream(new ByteArrayInputStream(data))) {
            int n;
            while ((n = input.read()) != -1) {
                System.out.println((char)n);
            }
            System.out.println("Total read " + input.getBytesRead() + " bytes");
        }
    }
}

class CountInputStream extends FilterInputStream {
    private int count = 0;

    CountInputStream(InputStream in) {
        super(in);
    }

    public int getBytesRead() {
        return this.count;
    }

    public int read() throws IOException {
        int n = in.read();
        if (n != -1) {
            this.count ++;
        }
        return n;
    }

    public int read(byte[] b, int off, int len) throws IOException {
        int n = in.read(b, off, len);
        this.count += n;
        return n;
    }
}
```

### 操作Zip

`ZipInputStream`可以读取zip格式的流，`ZipOutputStream`可以把多份数据写入zip包；

配合`FileInputStream`和`FileOutputStream`就可以读写zip文件。

**读取Zip包**

```java
try (ZipInputStream zip = new ZipInputStream(new FileInputStream(...))) {
    ZipEntry entry = null;
    while ((entry = zip.getNextEntry()) != null) {
        String name = entry.getName();
        if (!entry.isDirectory()) {
            int n;
            while ((n = zip.read()) != -1) {
                ...
            }
        }
    }
}
```

**写入Zip包**

```java
try (ZipOutputStream zip = new ZipOutputStream(new FileOutputStream(...))) {
    File[] files = ...
    for (File file : files) {
        zip.putNextEntry(new ZipEntry(file.getName()));
        zip.write(getFileDataAsBytes(file));
        zip.closeEntry();
    }
}
```

### 读取classpath资源

把资源存储在classpath中可以避免文件路径依赖；

`Class`对象的`getResourceAsStream()`可以从classpath中读取指定资源；

根据classpath读取资源时，需要检查返回的`InputStream`是否为`null`。

```java
try (InputStream input = getClass().getResourceAsStream("/default.properties")) {
	if (input != null) {
	Properties props = new Properties();	props.load(inputStreamFromClassPath("/default.properties"));
props.load(inputStreamFromFile("./conf.properties"));
	}
}
```

### 序列化

可序列化的Java对象必须实现`java.io.Serializable`接口，类似`Serializable`这样的空接口被称为“标记接口”（Marker Interface）；

反序列化时不调用构造方法，可设置`serialVersionUID`作为版本号（非必需）；

Java的序列化机制仅适用于Java，如果需要与其它语言交换数据，必须使用通用的序列化方法，例如JSON。

### Reader

`Reader`定义了所有字符输入流的超类：

- `FileReader`实现了文件字符流输入，使用时需要指定编码；
- `CharArrayReader`和`StringReader`可以在内存中模拟一个字符流输入。

```java
public void readFile() throws IOException {
    try (Reader reader = new FileReader("src/readme.txt", StandardCharsets.UTF_8)) {
        char[] buffer = new char[1000];
        int n;
        while ((n = reader.read(buffer)) != -1) {
            System.out.println("read " + n + " chars.");
        }
    }
}

//CharArrayReader
try (Reader reader = new CharArrayReader("Hello".toCharArray())) {
}
//StringReader
try (Reader reader = new StringReader("Hello")) {
}
```

`Reader`是基于`InputStream`构造的：可以通过`InputStreamReader`在指定编码的同时将任何`InputStream`转换为`Reader`。

总是使用`try (resource)`保证`Reader`正确关闭。

```java
try (Reader reader = new InputStreamReader(new FileInputStream("src/readme.txt"), "UTF-8")) {
  
}
```

### Writer

`Writer`定义了所有字符输出流的超类：

- `FileWriter`实现了文件字符流输出；
- `CharArrayWriter`和`StringWriter`在内存中模拟一个字符流输出。

```java
try (Writer writer = new FileWriter("readme.txt", StandardCharsets.UTF_8)) {
    writer.write('a');
}
```

使用`try (resource)`保证`Writer`正确关闭。

`Writer`是基于`OutputStream`构造的，可以通过`OutputStreamWriter`将`OutputStream`转换为`Writer`，转换时需要指定编码。

```java
try (Writer writer = new OutputStreamWriter(new FileOutputStream("readme.txt"), "UTF-8")) {
}
```

### PrintStream和PrintWriter

`PrintStream`是一种能接收各种数据类型的输出，打印数据时比较方便：

- `System.out`是标准输出；
- `System.err`是标准错误输出。

`PrintWriter`是基于`Writer`的输出。

## 日期与实践

### Locale

在编写日期和时间的程序前，我们要准确理解日期、时间和时刻的概念。

由于存在本地时间，我们需要理解时区的概念，并且必须牢记由于夏令时的存在，同一地区用`GMT/UTC`和城市表示的时区可能导致时间不同。

计算机通过`Locale`来针对当地用户习惯格式化日期、时间、数字、货币等。

### Date和Calendar

计算机表示的时间是以整数表示的时间戳存储的，即Epoch Time，Java使用`long`型来表示以毫秒为单位的时间戳，通过`System.currentTimeMillis()`获取当前时间戳。

Java有两套日期和时间的API：

- 旧的Date、Calendar和TimeZone；
- 新的LocalDateTime、ZonedDateTime、ZoneId等。

分别位于`java.util`和`java.time`包中。

```java
import java.text.*
import java.util.*
public class Main {
    public static void main(String[] args) {
        Date date = new Date();
        var sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println(sdf.format(date));
        var sdf1 = new SimpleDateFormat("EEEE MMMM dddd, yyyy");
        System.out.println(sdf1.format(date));
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        // 当前时间:
        Calendar c = Calendar.getInstance();
        // 清除所有:
        c.clear();
        // 设置年月日时分秒:
        c.set(2019, 10 /* 11月 */, 20, 8, 15, 0);
        // 加5天并减去2小时:
        c.add(Calendar.DAY_OF_MONTH, 5);
        c.add(Calendar.HOUR_OF_DAY, -2);
        // 显示时间:
        var sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        sdf.setTimeZone(TimeZone.getTimeZone("America/New_York"));
        Date d = c.getTime();
        System.out.println(sdf.format(d));
        // 2019-11-25 6:15:00
    }
}
```

### LocalDateTime

Java 8引入了新的日期和时间API，它们是不变类，默认按ISO 8601标准格式化和解析；

使用`LocalDateTime`可以非常方便地对日期和时间进行加减，或者调整日期和时间，它总是返回新对象；

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        LocalDateTime dt = LocalDateTime.now(); // 当前日期和时间
        LocalDate d = dt.toLocalDate(); // 转换到当前日期
        LocalTime t = dt.toLocalTime(); // 转换到当前时间
        System.out.println(d); // 严格按照ISO 8601格式打印
        System.out.println(t); // 严格按照ISO 8601格式打印
        System.out.println(dt); // 严格按照ISO 8601格式打印
    }
}

// 指定日期和时间:
LocalDate d2 = LocalDate.of(2019, 11, 30); // 2019-11-30, 注意11=11月
LocalTime t2 = LocalTime.of(15, 16, 17); // 15:16:17
LocalDateTime dt2 = LocalDateTime.of(2019, 11, 30, 15, 16, 17);
LocalDateTime dt3 = LocalDateTime.of(d2, t2);
//将字符串转换为LocalDateTime就可以传入标准格式
LocalDateTime dt = LocalDateTime.parse("2019-11-19T15:16:17");
LocalDate d = LocalDate.parse("2019-11-19");
LocalTime t = LocalTime.parse("15:16:17");
```

使用`isBefore()`和`isAfter()`可以判断日期和时间的先后；

使用`Duration`和`Period`可以表示两个日期和时间的“区间间隔”。

### ZonedDateTime

`ZonedDateTime`是带时区的日期和时间，可用于时区转换；

```java
import java.time.*
public class Main {
    public static void main(String[] args) {
        // 以中国时区获取当前时间:
        ZonedDateTime zbj = ZonedDateTime.now(ZoneId.of("Asia/Shanghai"));
        // 转换为纽约时间:
        ZonedDateTime zny = zbj.withZoneSameInstant(ZoneId.of("America/New_York"));
        System.out.println(zbj);
        System.out.println(zny);
    }
}
```

`ZonedDateTime`和`LocalDateTime`可以相互转换。

```java
import java.time.*;
public class Main {
    public static void main(String[] args) {
        LocalDateTime departureAtBeijing = LocalDateTime.of(2019, 9, 15, 13, 0, 0);
        int hours = 13;
        int minutes = 20;
        LocalDateTime arrivalAtNewYork = calculateArrivalAtNY(departureAtBeijing, hours, minutes);
        System.out.println(departureAtBeijing + " -> " + arrivalAtNewYork);
        // test:
        if (!LocalDateTime.of(2019, 10, 15, 14, 20, 0)
                .equals(calculateArrivalAtNY(LocalDateTime.of(2019, 10, 15, 13, 0, 0), 13, 20))) {
            System.err.println("测试失败!");
        } else if (!LocalDateTime.of(2019, 11, 15, 13, 20, 0)
                .equals(calculateArrivalAtNY(LocalDateTime.of(2019, 11, 15, 13, 0, 0), 13, 20))) {
            System.err.println("测试失败!");
        }
    }

    static LocalDateTime calculateArrivalAtNY(LocalDateTime bj, int h, int m) {
        LocalDateTime arrivalBJLocal = bj.plusHours(h).plusMinutes(m);
        ZonedDateTime arrivalBJZone = arrivalBJLocal.atZone(ZoneId.of("Asia/Shanghai"));
        ZonedDateTime arrivalNYZone = arrivalBJZone.withZoneSameInstant(ZoneId.of("America/New_York"));
        return bj=arrivalNYZone.toLocalDateTime();  
    }
}
```

### DateTimeFormatter

对`ZonedDateTime`或`LocalDateTime`进行格式化，需要使用`DateTimeFormatter`类；

`DateTimeFormatter`可以通过格式化字符串和`Locale`对日期和时间进行定制输出

```java
import java.time.*;
import java.time.format.*;
import java.util.Locale;
public class Main {
    public static void main(String[] args) {
        ZonedDateTime zdt = ZonedDateTime.now();
        var formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd'T'HH:mm ZZZZ");
        System.out.println(formatter.format(zdt));
        var zhFormatter = DateTimeFormatter.ofPattern("yyyy MMM dd EE HH:mm", Locale.CHINA);
        System.out.println(zhFormatter.format(zdt));
        var usFormatter = DateTimeFormatter.ofPattern("E, MMMM/dd/yyyy HH:mm", Locale.US);
        System.out.println(usFormatter.format(zdt));
    }
}
```

### Instant

`Instant`表示高精度时间戳，它可以和`ZonedDateTime`以及`long`互相转换。

### 实践

处理日期和时间时，尽量使用新的`java.time`包；

在数据库中存储时间戳时，尽量使用`long`型时间戳，它具有省空间，效率高，不依赖数据库的优点。

**旧API转新API**

```java
// Date -> Instant:
Instant ins1 = new Date().toInstant();

// Calendar -> Instant -> ZonedDateTime:
Calendar calendar = Calendar.getInstance();
Instant ins2 = Calendar.getInstance().toInstant();
ZonedDateTime zdt = ins2.atZone(calendar.getTimeZone().toZoneId());
```

**新API转旧API**

```java
// ZonedDateTime -> long:
ZonedDateTime zdt = ZonedDateTime.now();
long ts = zdt.toEpochSecond() * 1000;

// long -> Date:
Date date = new Date(ts);

// long -> Calendar:
Calendar calendar = Calendar.getInstance();
calendar.clear();
calendar.setTimeZone(TimeZone.getTimeZone(zdt.getZone().getId()));
calendar.setTimeInMillis(zdt.toEpochSecond() * 1000);
```

**数据库存储日期和时间**


| 数据库    | 对应Java类（旧）   | 对应Java类（新） |
| :-------- | :----------------- | :--------------- |
| DATETIME  | java.util.Date     | LocalDateTime    |
| DATE      | java.sql.Date      | LocalDate        |
| TIME      | java.sql.Time      | LocalTime        |
| TIMESTAMP | java.sql.Timestamp | LocalDateTime    |

用长整数`long`表示，在数据库中存储为`BIGINT`类型，编写`timestampToString()`的方法为不同用户不同偏好显示不同本地时间：

```java
import java.time.*;
import java.time.format.*;
import java.util.Locale;
public class Main {
    public static void main(String[] args) {
        long ts = 1574208900000L;
        System.out.println(timestampToString(ts, Locale.CHINA, "Asia/Shanghai"));
        System.out.println(timestampToString(ts, Locale.US, "America/New_York"));
    }

    static String timestampToString(long epochMilli, Locale lo, String zoneId) {
        Instant ins = Instant.ofEpochMilli(epochMilli);
        DateTimeFormatter f = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM, FormatStyle.SHORT);
        return f.withLocale(lo).format(ZonedDateTime.ofInstant(ins, ZoneId.of(zoneId)));
    }
}
```

## 关键字


|  **关键字**  | **含义**                                                     |
| :----------: | :----------------------------------------------------------- |
|   abstract   | 表明类或者成员方法具有抽象属性                               |
|    assert    | 断言，用来进行程序调试                                       |
|   boolean    | 基本数据类型之一，声明布尔类型的关键字                       |
|    break     | 提前跳出一个块                                               |
|     byte     | 基本数据类型之一，字节类型                                   |
|     case     | 用在switch语句之中，表示其中的一个分支                       |
|    catch     | 用在异常处理中，用来捕捉异常                                 |
|     char     | 基本数据类型之一，字符类型                                   |
|    class     | 声明一个类                                                   |
|    const     | 保留关键字，没有具体含义                                     |
|   continue   | 回到一个块的开始处                                           |
|   default    | 默认，例如，用在switch语句中，表明一个默认的分支。Java8 中也作用于声明接口函数的默认实现 |
|      do      | 用在do-while循环结构中                                       |
|    double    | 基本数据类型之一，双精度浮点数类型                           |
|     else     | 用在条件语句中，表明当条件不成立时的分支                     |
|     enum     | 枚举                                                         |
|   extends    | 表明一个类型是另一个类型的子类型。对于类，可以是另一个类或者抽象类；对于接口，可以是另一个接口 |
|    final     | 用来说明最终属性，表明一个类不能派生出子类，或者成员方法不能被覆盖，或者成员域的值不能被改变，用来定义常量 |
|   finally    | 用于处理异常情况，用来声明一个基本肯定会被执行到的语句块     |
|    float     | 基本数据类型之一，单精度浮点数类型                           |
|     for      | 一种循环结构的引导词                                         |
|     goto     | 保留关键字，没有具体含义                                     |
|      if      | 条件语句的引导词                                             |
|  implements  | 表明一个类实现了给定的接口                                   |
|    import    | 表明要访问指定的类或包                                       |
|  instanceof  | 用来测试一个对象是否是指定类型的实例对象                     |
|     int      | 基本数据类型之一，整数类型                                   |
|  interface   | 接口                                                         |
|     long     | 基本数据类型之一，长整数类型                                 |
|    native    | 用来声明一个方法是由与计算机相关的语言（如C/C++/FORTRAN语言）实现的 |
|     new      | 用来创建新实例对象                                           |
|   package    | 包                                                           |
|   private    | 一种访问控制方式：私用模式                                   |
|  protected   | 一种访问控制方式：保护模式                                   |
|    public    | 一种访问控制方式：共用模式                                   |
|    return    | 从成员方法中返回数据                                         |
|    short     | 基本数据类型之一,短整数类型                                  |
|    static    | 表明具有静态属性                                             |
|   strictfp   | 用来声明FP_strict（单精度或双精度浮点数）表达式遵循[IEEE 754](https://baike.baidu.com/item/IEEE 754)算术规范 |
|    super     | 表明当前对象的父类型的引用或者父类型的构造方法               |
|    switch    | 分支语句结构的引导词                                         |
| synchronized | 表明一段代码需要同步执行                                     |
|     this     | 指向当前实例对象的引用                                       |
|    throw     | 抛出一个异常                                                 |
|    throws    | 声明在当前定义的成员方法中所有需要抛出的异常                 |
|  transient   | 声明不用序列化的成员域                                       |
|     try      | 尝试一个可能抛出异常的程序块                                 |
|     void     | 声明当前成员方法没有返回值                                   |
|   volatile   | 表明两个或者多个变量必须同步地发生变化                       |
|    while     | 用在循环结构中                                               |

## 加密与安全

### 编码算法

- URL编码和Base64编码都是编码算法，它们不是加密算法；
- URL编码的目的是把任意文本数据编码为%前缀表示的文本，便于浏览器和服务器处理；

```java
import java.net.URLEncoder;
import java.nio.charset.StandardCharsets;
public class Main {
    public static void main(String[] args) {
    //字符串转URL编码
        String encoded = URLEncoder.encode("中文!", StandardCharsets.UTF_8);
        System.out.println(encoded);
    }
}

```

```java
import java.net.URLDecoder;
import java.nio.charset.StandardCharsets;
public class Main {
    public static void main(String[] args) {
    //URL解码成字符串
        String decoded = URLDecoder.decode("%E4%B8%AD%E6%96%87%21", StandardCharsets.UTF_8);
        System.out.println(decoded);
    }
}

```

Base64编码的目的是把任意二进制数据编码为文本，但编码后数据量会增加1/3。

### 哈希算法（摘要算法Digest）

对任意一组输入数据进行计算，得到一个固定长度的输出摘要。

- 哈希算法可用于验证数据完整性，具有防篡改检测的功能；
- 常用的哈希算法有MD5、SHA-1等；
- 用哈希存储口令时要考虑彩虹表攻击。

#### 哈希碰撞

哈希碰撞是指两个不同的输入得到了相同的输出。


| 算法       | 输出长度（位） | 输出长度（字节） |
| :--------- | :------------- | :--------------- |
| MD5        | 128 bits       | 16 bytes         |
| SHA-1      | 160 bits       | 20 bytes         |
| RipeMD-160 | 160 bits       | 20 bytes         |
| SHA-256    | 256 bits       | 32 bytes         |
| SHA-512    | 512 bits       | 64 bytes         |

```java
import java.math.BigInteger;
import java.security.MessageDigest;
public class Main {
    public static void main(String[] args) throws Exception {
        // 创建一个MessageDigest实例:
        MessageDigest md = MessageDigest.getInstance("MD5");
        // 反复调用update输入数据:
        md.update("Hello".getBytes("UTF-8"));
        md.update("World".getBytes("UTF-8"));
        //对输入计算哈希值
        byte[] result = md.digest(); // 16 bytes: 68e109f0f40ca72a15e05cc22786f8e6
        //转成十六进制字符串
        System.out.println(new BigInteger(1, result).toString(16));
    }
}
```

#### 用途

由于相同的输入永远得到相同的输出，因此可用来对比数据是否被篡改。加密数据库的用户口令将明文转成MD5，黑客必须通过暴力枚举输入不同口令得到相同MD5，然而暴力枚举会消耗大量时间，因此通过彩虹表（预先计算好常用口令的MD5对照表）快速查找原始口令。当然也有抵御方法，通过口令添加随机数转MD5又叫做加盐处理，这样就无法反推出原始口令。

```
digest = md5(salt+inputPassword)
```

#### SHA-1

输出16bits即20字节的哈希算法，还包括SHA-256，SHA-512等算法，在JAVA标准库中支持所有哈希算法。

```java
import java.math.BigInteger;
import java.security.MessageDigest;
public class Main {
    public static void main(String[] args) throws Exception {
        // 创建一个MessageDigest实例:
        MessageDigest md = MessageDigest.getInstance("SHA-1");
        // 反复调用update输入数据:
        md.update("Hello".getBytes("UTF-8"));
        md.update("World".getBytes("UTF-8"));
        byte[] result = md.digest(); // 20 bytes: db8ac1c259eb89d4a131b253bacfca5f319d54f2
        System.out.println(new BigInteger(1, result).toString(16));
    }
}
```

### BouncyCastle

- BouncyCastle是一个开源的第三方算法提供商；
- BouncyCastle提供了很多Java标准库没有提供的哈希算法和加密算法；
- 使用第三方算法前需要通过`Security.addProvider()`注册。

[下载Jar包](https://www.bouncycastle.org/latest_releases.html)

Java标准库的`java.security`包提供了一种标准机制，允许第三方提供商无缝接入，使用BouncyCastle提供的RipeMD160算法需要先注册：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        // 注册BouncyCastle:
        Security.addProvider(new BouncyCastleProvider());
        // 按名称正常调用:
        MessageDigest md = MessageDigest.getInstance("RipeMD160");
        md.update("HelloWorld".getBytes("UTF-8"));
        byte[] result = md.digest();
        System.out.println(new BigInteger(1, result).toString(16));
    }
}
```

### Hmac算法

Hmac算法是一种标准的基于密钥的哈希算法，可以配合MD5、SHA-1等哈希算法，计算的摘要长度和原摘要算法长度相同。

例如，我们使用MD5算法，对应的就是HmacMD5算法，它相当于“加盐”的MD5：

```
HmacMD5 ≈ md5(secure_random_key, input)
```

对比加盐算法HmacMD5：

- HmacMD5使用的key长度是64字节，更安全；
- Hmac是标准算法，同样适用于SHA-1等其他哈希算法；
- Hmac输出和原有的哈希算法长度一致。

为了保证安全，我们不会自己指定key，而是通过Java标准库的KeyGenerator生成一个安全的随机的key。下面是使用HmacMD5的代码：

```java
import java.math.BigInteger;
import javax.crypto.*;
public class Main {
    public static void main(String[] args) throws Exception {
        KeyGenerator keyGen = KeyGenerator.getInstance("HmacMD5");
        SecretKey key = keyGen.generateKey();
        // 打印随机生成的key:
        byte[] skey = key.getEncoded();
        System.out.println(new BigInteger(1, skey).toString(16));
        Mac mac = Mac.getInstance("HmacMD5");
        mac.init(key);
        mac.update("HelloWorld".getBytes("UTF-8"));
        byte[] result = mac.doFinal();
        System.out.println(new BigInteger(1, result).toString(16));
    }
}

```

#### 步骤：

1. 通过名称`HmacMD5`获取`KeyGenerator`实例；
2. 通过`KeyGenerator`创建一个`SecretKey`实例；
3. 通过名称`HmacMD5`获取`Mac`实例；
4. 用`SecretKey`初始化`Mac`实例；
5. 对`Mac`实例反复调用`update(byte[])`输入数据；
6. 调用`Mac`实例的`doFinal()`获取最终的哈希值。

通过以下代码通过从byte[] 数组恢复验证计算哈希值和SecretKey：

```java
import java.util.Arrays;
import javax.crypto.*;
import javax.crypto.spec.*;
public class Main {
    public static void main(String[] args) throws Exception {
        byte[] hkey = new byte[] { 106, 70, -110, 125, 39, -20, 52, 56, 85, 9, -19, -72, 52, -53, 52, -45, -6, 119, -63,
                30, 20, -83, -28, 77, 98, 109, -32, -76, 121, -106, 0, -74, -107, -114, -45, 104, -104, -8, 2, 121, 6,
                97, -18, -13, -63, -30, -125, -103, -80, -46, 113, -14, 68, 32, -46, 101, -116, -104, -81, -108, 122,
                89, -106, -109 };

        SecretKey key = new SecretKeySpec(hkey, "HmacMD5"); //恢复SecretKey
        Mac mac = Mac.getInstance("HmacMD5");
        mac.init(key);
        mac.update("HelloWorld".getBytes("UTF-8"));
        byte[] result = mac.doFinal();
        System.out.println(Arrays.toString(result));
        // [126, 59, 37, 63, 73, 90, 111, -96, -77, 15, 82, -74, 122, -55, -67, 54]
    }
}
```

### 对称加密算法

- 对称加密算法使用同一个密钥进行加密和解密，常用算法有DES、AES和IDEA等；
- 密钥长度由算法设计决定，AES的密钥长度是128/192/256位；
- 使用对称加密算法需要指定算法名称、工作模式和填充模式。

对称算法用一个密码进行加密解密，加密接收密码明文输出密文：

```
secret = encrypt(key, message);
```

解密接收密码密文，输出明文：

```
plain = decrypt(key, secret);
```

DES由于秘钥过短，可在短时间暴力破解，不推荐。

#### 使用AES加密

目前应用最广泛的加密算法，先用ECB模式加密解密，这种模式只需要一个固定长度的密钥，固定的明文会生成固定的密文：

```java
import java.security.*;
import java.util.Base64;

import javax.crypto.*;
import javax.crypto.spec.*;

public class Main {
    public static void main(String[] args) throws Exception {
        // 原文:
        String message = "Hello, world!";
        System.out.println("Message: " + message);
        // 128位密钥 = 16 bytes Key:
        byte[] key = "1234567890abcdef".getBytes("UTF-8");
        // 加密:
        byte[] data = message.getBytes("UTF-8");
        byte[] encrypted = encrypt(key, data);
        System.out.println("Encrypted: " + Base64.getEncoder().encodeToString(encrypted));
        // 解密:
        byte[] decrypted = decrypt(key, encrypted);
        System.out.println("Decrypted: " + new String(decrypted, "UTF-8"));
    }
    // 加密:
    public static byte[] encrypt(byte[] key, byte[] input) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
        SecretKey keySpec = new SecretKeySpec(key, "AES");
        cipher.init(Cipher.ENCRYPT_MODE, keySpec);
        return cipher.doFinal(input);
    }
    // 解密:
    public static byte[] decrypt(byte[] key, byte[] input) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");
        SecretKey keySpec = new SecretKeySpec(key, "AES");
        cipher.init(Cipher.DECRYPT_MODE, keySpec);
        return cipher.doFinal(input);
    }
}
```

**步骤：**

1. 根据算法名称/工作模式/填充模式获取Cipher实例；
2. 根据算法名称初始化一个SecretKey实例，密钥必须是指定长度；
3. 使用SerectKey初始化Cipher实例，并设置加密或解密模式；
4. 传入明文或密文，获得密文或明文。

像ECB这种一对一的加密方式会导致安全性降低，更好的方式是通过CBC模式，它需要一个随机数作为IV参数，这样对于同一份明文，每次生成的密文都不同：

```java
import java.security.*;
import java.util.Base64;
import javax.crypto.*;
import javax.crypto.spec.*;
public class Main {
    public static void main(String[] args) throws Exception {
        // 原文:
        String message = "Hello, world!";
        System.out.println("Message: " + message);
        // 256位密钥 = 32 bytes Key:
        byte[] key = "1234567890abcdef1234567890abcdef".getBytes("UTF-8");
        // 加密:
        byte[] data = message.getBytes("UTF-8");
        byte[] encrypted = encrypt(key, data);
        System.out.println("Encrypted: " + Base64.getEncoder().encodeToString(encrypted));
        // 解密:
        byte[] decrypted = decrypt(key, encrypted);
        System.out.println("Decrypted: " + new String(decrypted, "UTF-8"));
    }

    // 加密:
    public static byte[] encrypt(byte[] key, byte[] input) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        SecretKeySpec keySpec = new SecretKeySpec(key, "AES");
        // CBC模式需要生成一个16 bytes的initialization vector:
        SecureRandom sr = SecureRandom.getInstanceStrong();
        byte[] iv = sr.generateSeed(16);
        IvParameterSpec ivps = new IvParameterSpec(iv);
        cipher.init(Cipher.ENCRYPT_MODE, keySpec, ivps);
        byte[] data = cipher.doFinal(input);
        // IV不需要保密，把IV和密文一起返回:
        return join(iv, data);
    }
    // 解密:
    public static byte[] decrypt(byte[] key, byte[] input) throws GeneralSecurityException {
        // 把input分割成IV和密文:
        byte[] iv = new byte[16];
        byte[] data = new byte[input.length - 16];
        System.arraycopy(input, 0, iv, 0, 16);
        System.arraycopy(input, 16, data, 0, data.length);
        // 解密:
        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        SecretKeySpec keySpec = new SecretKeySpec(key, "AES");
        IvParameterSpec ivps = new IvParameterSpec(iv);
        cipher.init(Cipher.DECRYPT_MODE, keySpec, ivps);
        return cipher.doFinal(data);
    }
    public static byte[] join(byte[] bs1, byte[] bs2) {
        byte[] r = new byte[bs1.length + bs2.length];
        System.arraycopy(bs1, 0, r, 0, bs1.length);
        System.arraycopy(bs2, 0, r, bs1.length, bs2.length);
        return r;
    }
}
```

在CBC模式下，需要一个随机生成的16字节IV参数，必须使用`SecureRandom`生成。因为多了一个`IvParameterSpec`实例，因此，初始化方法需要调用`Cipher`的一个重载方法并传入`IvParameterSpec`。每次生成的IV不同，密文不同。

### 口令加密算法

- PBE算法通过用户口令和安全的随机salt计算出Key，然后再进行加密；
- Key通过口令和安全的随机salt计算得出，大大提高了安全性；
- PBE算法内部使用的仍然是标准对称加密算法（例如AES）。

```
key = generate(userPassword, secureRandomPassword);
```

PBE的作用就是把用户输入的口令和一个安全随机的口令采用杂凑后计算出真正的密钥：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        // 把BouncyCastle作为Provider添加到java.security:
        Security.addProvider(new BouncyCastleProvider());
        // 原文:
        String message = "Hello, world!";
        // 加密口令:
        String password = "hello12345";
        // 16 bytes随机Salt:
        byte[] salt = SecureRandom.getInstanceStrong().generateSeed(16);
        System.out.printf("salt: %032x\n", new BigInteger(1, salt));
        // 加密:
        byte[] data = message.getBytes("UTF-8");
        byte[] encrypted = encrypt(password, salt, data);
        System.out.println("encrypted: " + Base64.getEncoder().encodeToString(encrypted));
        // 解密:
        byte[] decrypted = decrypt(password, salt, encrypted);
        System.out.println("decrypted: " + new String(decrypted, "UTF-8"));
    }

    // 加密:
    public static byte[] encrypt(String password, byte[] salt, byte[] input) throws GeneralSecurityException {
        PBEKeySpec keySpec = new PBEKeySpec(password.toCharArray());
        SecretKeyFactory skeyFactory = SecretKeyFactory.getInstance("PBEwithSHA1and128bitAES-CBC-BC");
        SecretKey skey = skeyFactory.generateSecret(keySpec);
        PBEParameterSpec pbeps = new PBEParameterSpec(salt, 1000);
        Cipher cipher = Cipher.getInstance("PBEwithSHA1and128bitAES-CBC-BC");
        cipher.init(Cipher.ENCRYPT_MODE, skey, pbeps);
        return cipher.doFinal(input);
    }

    // 解密:
    public static byte[] decrypt(String password, byte[] salt, byte[] input) throws GeneralSecurityException {
        PBEKeySpec keySpec = new PBEKeySpec(password.toCharArray());
        SecretKeyFactory skeyFactory = SecretKeyFactory.getInstance("PBEwithSHA1and128bitAES-CBC-BC");
        SecretKey skey = skeyFactory.generateSecret(keySpec);
        PBEParameterSpec pbeps = new PBEParameterSpec(salt, 1000);
        Cipher cipher = Cipher.getInstance("PBEwithSHA1and128bitAES-CBC-BC");
        cipher.init(Cipher.DECRYPT_MODE, skey, pbeps);
        return cipher.doFinal(input);
    }
}
```

### 密钥交换算法

- DH算法是一种密钥交换协议，通信双方通过不安全的信道协商密钥，然后进行对称加密传输。
- DH算法没有解决中间人攻击，不能确保与自己通信的是否真的是对方。

```java
import java.math.BigInteger;
import java.security.*;
import java.security.spec.*;
import javax.crypto.KeyAgreement;

public class Main {
    public static void main(String[] args) {
        // Bob和Alice:
        Person bob = new Person("Bob");
        Person alice = new Person("Alice");

        // 各自生成KeyPair:
        bob.generateKeyPair();
        alice.generateKeyPair();

        // 双方交换各自的PublicKey:
        // Bob根据Alice的PublicKey生成自己的本地密钥:
        bob.generateSecretKey(alice.publicKey.getEncoded());
        // Alice根据Bob的PublicKey生成自己的本地密钥:
        alice.generateSecretKey(bob.publicKey.getEncoded());

        // 检查双方的本地密钥是否相同:
        bob.printKeys();
        alice.printKeys();
        // 双方的SecretKey相同，后续通信将使用SecretKey作为密钥进行AES加解密...
    }
}

class Person {
    public final String name;

    public PublicKey publicKey;
    private PrivateKey privateKey;
    private byte[] secretKey;

    public Person(String name) {
        this.name = name;
    }

    // 生成本地KeyPair:
    public void generateKeyPair() {
        try {
            KeyPairGenerator kpGen = KeyPairGenerator.getInstance("DH");
            kpGen.initialize(512);
            KeyPair kp = kpGen.generateKeyPair();
            this.privateKey = kp.getPrivate();
            this.publicKey = kp.getPublic();
        } catch (GeneralSecurityException e) {
            throw new RuntimeException(e);
        }
    }

    public void generateSecretKey(byte[] receivedPubKeyBytes) {
        try {
            // 从byte[]恢复PublicKey:
            X509EncodedKeySpec keySpec = new X509EncodedKeySpec(receivedPubKeyBytes);
            KeyFactory kf = KeyFactory.getInstance("DH");
            PublicKey receivedPublicKey = kf.generatePublic(keySpec);
            // 生成本地密钥:
            KeyAgreement keyAgreement = KeyAgreement.getInstance("DH");
            keyAgreement.init(this.privateKey); // 自己的PrivateKey
            keyAgreement.doPhase(receivedPublicKey, true); // 对方的PublicKey
            // 生成SecretKey密钥:
            this.secretKey = keyAgreement.generateSecret();
        } catch (GeneralSecurityException e) {
            throw new RuntimeException(e);
        }
    }

    public void printKeys() {
        System.out.printf("Name: %s\n", this.name);
        System.out.printf("Private key: %x\n", new BigInteger(1, this.privateKey.getEncoded()));
        System.out.printf("Public key: %x\n", new BigInteger(1, this.publicKey.getEncoded()));
        System.out.printf("Secret key: %x\n", new BigInteger(1, this.secretKey));
    }
}
```

### 非对称加密算法

- 非对称加密就是加密和解密使用的不是相同的密钥，只有同一个公钥-私钥对才能正常加解密；
- 只使用非对称加密算法不能防止中间人攻击。

```java
import java.math.BigInteger;
import java.security.*;
import javax.crypto.Cipher;

public class Main {
    public static void main(String[] args) throws Exception {
        // 明文:
        byte[] plain = "Hello, encrypt use RSA".getBytes("UTF-8");
        // 创建公钥／私钥对:
        Person alice = new Person("Alice");
        // 用Alice的公钥加密:
        byte[] pk = alice.getPublicKey();
        System.out.println(String.format("public key: %x", new BigInteger(1, pk)));
        byte[] encrypted = alice.encrypt(plain);
        System.out.println(String.format("encrypted: %x", new BigInteger(1, encrypted)));
        // 用Alice的私钥解密:
        byte[] sk = alice.getPrivateKey();
        System.out.println(String.format("private key: %x", new BigInteger(1, sk)));
        byte[] decrypted = alice.decrypt(encrypted);
        System.out.println(new String(decrypted, "UTF-8"));
    }
}

class Person {
    String name;
    // 私钥:
    PrivateKey sk;
    // 公钥:
    PublicKey pk;

    public Person(String name) throws GeneralSecurityException {
        this.name = name;
        // 生成公钥／私钥对:
        KeyPairGenerator kpGen = KeyPairGenerator.getInstance("RSA");
        kpGen.initialize(1024);
        KeyPair kp = kpGen.generateKeyPair();
        this.sk = kp.getPrivate();
        this.pk = kp.getPublic();
    }

    // 把私钥导出为字节
    public byte[] getPrivateKey() {
        return this.sk.getEncoded();
    }

    // 把公钥导出为字节
    public byte[] getPublicKey() {
        return this.pk.getEncoded();
    }

    // 用公钥加密:
    public byte[] encrypt(byte[] message) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.ENCRYPT_MODE, this.pk);
        return cipher.doFinal(message);
    }

    // 用私钥解密:
    public byte[] decrypt(byte[] input) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.DECRYPT_MODE, this.sk);
        return cipher.doFinal(input);
    }
}
```

RSA的公钥和私钥都可以通过`getEncoded()`方法获得以`byte[]`表示的二进制数据，并根据需要保存到文件中。要从`byte[]`数组恢复公钥或私钥。

```java
byte[] pkData = ...
byte[] skData = ...
KeyFactory kf = KeyFactory.getInstance("RSA");
// 恢复公钥:
X509EncodedKeySpec pkSpec = new X509EncodedKeySpec(pkData);
PublicKey pk = kf.generatePublic(pkSpec);
// 恢复私钥:
PKCS8EncodedKeySpec skSpec = new PKCS8EncodedKeySpec(skData);
PrivateKey sk = kf.generatePrivate(skSpec);
```

### 签名算法

数字签名就是用发送方的私钥对原始数据进行签名，只有用发送方公钥才能通过签名验证。

数字签名用于：

- 防止伪造；
- 防止抵赖；
- 检测篡改。

常用的数字签名算法包括：MD5withRSA／SHA1withRSA／SHA256withRSA／SHA1withDSA／SHA256withDSA／SHA512withDSA／ECDSA等

哈希算法+RSA签名

```java
import java.math.BigInteger;
import java.nio.charset.StandardCharsets;
import java.security.*;

public class Main {
    public static void main(String[] args) throws GeneralSecurityException {
        // 生成RSA公钥/私钥:
        KeyPairGenerator kpGen = KeyPairGenerator.getInstance("RSA");
        kpGen.initialize(1024);
        KeyPair kp = kpGen.generateKeyPair();
        PrivateKey sk = kp.getPrivate();
        PublicKey pk = kp.getPublic();

        // 待签名的消息:
        byte[] message = "Hello, I am Bob!".getBytes(StandardCharsets.UTF_8);

        // 用私钥签名:
        Signature s = Signature.getInstance("SHA1withRSA");
        s.initSign(sk);
        s.update(message);
        byte[] signed = s.sign();
        System.out.println(String.format("signature: %x", new BigInteger(1, signed)));

        // 用公钥验证:
        Signature v = Signature.getInstance("SHA1withRSA");
        v.initVerify(pk);
        v.update(message);
        boolean valid = v.verify(signed);
        System.out.println("valid? " + valid);
    }
}
```

DSA算法签名比RSA更快，只能配合SHA使用：

- SHA1withDSA
- SHA256withDSA
- SHA512withDSA

椭圆曲线签名算法ECDSA：

从私钥推出公钥

### 数字证书

- 数字证书就是集合了多种密码学算法，用于实现数据加解密、身份认证、签名等多种功能的一种安全标准。
- 数字证书采用链式签名管理，顶级的Root CA证书已内置在操作系统中。
- 数字证书存储的是公钥，可以安全公开，而私钥必须严格保密。

在Java程序中，数字证书存储在一种Java专用的key store文件中，JDK提供了一系列命令来创建和管理key store。我们用下面的命令创建一个key store，并设定口令123456：

```java
keytool -storepass 123456 -genkeypair -keyalg RSA -keysize 1024 -sigalg SHA1withRSA -validity 3650 -alias mycert -keystore my.keystore -dname "CN=www.sample.com, OU=sample, O=sample, L=BJ, ST=BJ, C=CN"
```

- keyalg：指定RSA加密算法；
- sigalg：指定SHA1withRSA签名算法；
- validity：指定证书有效期3650天；
- alias：指定证书在程序中引用的名称；
- dname：最重要的`CN=www.sample.com`指定了`Common Name`，如果证书用在HTTPS中，这个名称必须与域名完全一致。

执行上述命令，JDK会在当前目录创建一个`my.keystore`文件，并存储创建成功的一个私钥和一个证书，它的别名是`mycert`。

有了key store存储的证书，我们就可以通过数字证书进行加解密和签名：

```java
import java.io.InputStream;
import java.math.BigInteger;
import java.security.*;
import java.security.cert.*;
import javax.crypto.Cipher;

public class Main {
    public static void main(String[] args) throws Exception {
        byte[] message = "Hello, use X.509 cert!".getBytes("UTF-8");
        // 读取KeyStore:
        KeyStore ks = loadKeyStore("/my.keystore", "123456");
        // 读取私钥:
        PrivateKey privateKey = (PrivateKey) ks.getKey("mycert", "123456".toCharArray());
        // 读取证书:
        X509Certificate certificate = (X509Certificate) ks.getCertificate("mycert");
        // 加密:
        byte[] encrypted = encrypt(certificate, message);
        System.out.println(String.format("encrypted: %x", new BigInteger(1, encrypted)));
        // 解密:
        byte[] decrypted = decrypt(privateKey, encrypted);
        System.out.println("decrypted: " + new String(decrypted, "UTF-8"));
        // 签名:
        byte[] sign = sign(privateKey, certificate, message);
        System.out.println(String.format("signature: %x", new BigInteger(1, sign)));
        // 验证签名:
        boolean verified = verify(certificate, message, sign);
        System.out.println("verify: " + verified);
    }

    static KeyStore loadKeyStore(String keyStoreFile, String password) {
        try (InputStream input = Main.class.getResourceAsStream(keyStoreFile)) {
            if (input == null) {
                throw new RuntimeException("file not found in classpath: " + keyStoreFile);
            }
            KeyStore ks = KeyStore.getInstance(KeyStore.getDefaultType());
            ks.load(input, password.toCharArray());
            return ks;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    static byte[] encrypt(X509Certificate certificate, byte[] message) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance(certificate.getPublicKey().getAlgorithm());
        cipher.init(Cipher.ENCRYPT_MODE, certificate.getPublicKey());
        return cipher.doFinal(message);
    }

    static byte[] decrypt(PrivateKey privateKey, byte[] data) throws GeneralSecurityException {
        Cipher cipher = Cipher.getInstance(privateKey.getAlgorithm());
        cipher.init(Cipher.DECRYPT_MODE, privateKey);
        return cipher.doFinal(data);
    }

    static byte[] sign(PrivateKey privateKey, X509Certificate certificate, byte[] message)
            throws GeneralSecurityException {
        Signature signature = Signature.getInstance(certificate.getSigAlgName());
        signature.initSign(privateKey);
        signature.update(message);
        return signature.sign();
    }

    static boolean verify(X509Certificate certificate, byte[] message, byte[] sig) throws GeneralSecurityException {
        Signature signature = Signature.getInstance(certificate.getSigAlgName());
        signature.initVerify(certificate);
        signature.update(message);
        return signature.verify(sig);
    }
}
```

以HTTPS协议为例，浏览器和服务器建立安全连接的步骤如下：

1. 浏览器向服务器发起请求，服务器向浏览器发送自己的数字证书；
2. 浏览器用操作系统内置的Root CA来验证服务器的证书是否有效，如果有效，就使用该证书加密一个随机的AES口令并发送给服务器；
3. 服务器用自己的私钥解密获得AES口令，并在后续通讯中使用AES加密。

## 多线程基础

### 进程和线程

在计算机中把一个任务称为一个进程，进程内部同时执行一个或多个子任务称为线程。一个应用程序可以实现：

- 多进程（每个进程只有一个线程）
- 单进程多线程模式（一个进程有多个线程）
- 多进程+多线程模式（多个进程有多个线程复杂度最高）

多进程的缺点：

- 创建进程比创建线程开销大
- 进程间的通信比线程通信慢

多进程的优点：

- 多进程的稳定性比多线程高，一个进程崩溃不会影响到其它进程，而多线程中一个线程崩溃直接导致整个进程崩溃。

### 多线程

JAVA内置多线程支持，在JVM进程中用一个主线程执行`main()`方法，在方法内部又可启用多个线程。在JAVA程序中多任务由多线程实现，经常需要读写共享数据，并且同步。如播放视频和音频的两个线程需要协调运行，否则画面和声音不同步，因此编程复杂度高，调试困难。

- 多线程模型是JAVA程序最基本的并发模型；
- 读写网络、数据库、web开发等都依赖JAVA多线程模型。

### 创建新线程

- Java用`Thread`对象表示一个线程，通过调用`start()`启动一个新线程；
- 一个线程对象只能调用一次`start()`方法；
- 线程的执行代码写在`run()`方法中；
- 线程调度由操作系统决定，程序本身无法决定调度顺序；
- `Thread.sleep()`可以把当前线程暂停一段时间。

创建一个新线程需要实例化`Thread`实例然后调用`start()`方法：

```java
public class Main {
	public static void main(String[] args){
		Thread t = new Thread();
		t.start();
	}
}
```

当线程启动后希望新线程执行指定的代码有以下几种方法：

- 方法一：从`Thread`派生一个自定义类，然后覆写`run()`方法：

```java
public class Main {
	public static void main(String[] args){
		Thread t = new MyThread();
		t.start;
	}
}

class MyThread extends Thread {
	@Override
	public void run(){
		System.out.println("Start new thread!");
	}
}
```

- 方法二：创建`Thread`实例时传入一个`Runnable`实例：

```java
public class Main {
	public static void main(String[] args){
		Thread t = new Thread(new MyRunnable());
		t.start();
	}
}

class MyRunnable implements Runnable {
	@Override
	public void run(){
		System.out.println("Start new thread!");
	}
}
```

用JAVA8引入lambda语法进一步简写启动主线程：

```java
public class Main {
	public static void main(String[] args) {
		Thread t = new Thread(() -> {
			System.out.println("Start new thread!");
		});
		t.start();
	}
}
```

主线程和新线程执行顺序：

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("main start...");
        Thread t = new Thread() {
            public void run() {
                System.out.println("thread run...");
                System.out.println("thread end.");
            }
        };
        t.start();
        System.out.println("main end...");
    }
}
```

首先打印`main start`，然后创建`Thread`对象，调用`start()`方法启动新线程，`main`线程继续执行打印`main end`，新线程`t`在`main`执行的同时并发执行，打印`thread run`和`thread end`，当`run()`方法结束时，新线程`t`结束，而`main()`方法结束时，主线程也结束了。

在线程中调用`Thread.sleep()`可强制当前线程暂停一段时间改变线程顺序：

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("main start...");
        Thread t = new Thread() {
            public void run() {
                System.out.println("thread run...");
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {}
                System.out.println("thread end.");
            }
        };
        t.start();
        try {
            Thread.sleep(20);
        } catch (InterruptedException e) {}
        System.out.println("main end...");
    }
}
```

此时先打印`main start`然后创建`Thread`对象调用`start()`方法启动新线程，继续执行时主线程休眠20毫秒，继续执行新线程`t`打印`thread run`，此时新线程休眠10毫秒继续执行打印`thread end`，最后休眠10毫秒执行主线程语句`main end`。

只有调用`Thread`实例的`start()`方法才能启动新线程，方法内部调用`private native void start0()`由JVM虚拟机内部的C代码实现。

### 线程的优先级

```java
Thread.setPriority(int n); // 1-10 默认是5
```

优先级高的线程被操作系统调度优先级较高，操作系统对优先级高的线程调度更频繁，但不能通过设置优先级保证高优先级的线程先执行。

### 线程的状态

- Java线程对象`Thread`的状态包括：`New`（新创建的线程尚未执行）、`Runnable`（运行中的线程正在执行`run()`方法的代码）、`Blocked`（运行中的线程，因为某些操作被阻塞而挂起）、`Waiting`（运行中的线程，因为某些操作在等待中）、`Timed Waiting`（运行中的线程，因为执行`sleep()`方法正在计时等待）和`Terminated`（`run()`方法执行完毕，线程终止）；
- 通过对另一个线程对象调用`join()`方法可以等待其执行结束；
- 可以指定等待时间，超过等待时间线程仍然没有结束就不再等待；
- 对已经运行结束的线程调用`join()`方法会立刻返回。

### 线程终止的原因

- 线程正常终止：`run()`方法执行完毕或`return`语句返回；
- 线程意外终止：`run()`方法因为未捕获的异常导致线程终止；
- 对某个线程的`Thread`实例调用`stop()`方法强制终止（不推荐）

`main`主线程在启动t线程后可以通过`t.join()`等待t线程结束再继续运行：主线程先打印Start，开始新线程打印Hello等待新线程结束打印end。

```java
public class Main {
	public static void main(String[] args) throws InterruptedException {
		Thread t = new Thread(() -> {
			Syste.out.println("Hello");
		});
		System.out.println("Start");
		t.start();
		t.join();
		System.out.println("end");
	}
}
```

### 中断线程

- 对目标线程调用`interrupt()`方法可以请求中断一个线程，目标线程通过检测`isInterrupted()`标志获取自身是否已中断。如果目标线程处于等待状态，该线程会捕获到`InterruptedException`；
- 目标线程检测到`isInterrupted()`为`true`或者捕获了`InterruptedException`都应该立刻结束自身线程；
- 通过标志位判断需要正确使用`volatile`关键字；
- `volatile`关键字解决了共享变量在线程间的可见性问题。

主线程运行run方法执行while循环，开始新线程等待1毫秒后中断新线程，新线程检测是否中断，中断结束新线程打印end，主线程结束。

```java
public class Main {
	public static void main(String[] args) throws InterruptedException {
		Thread t = new MyThread();
		t.start();
		Thread.sleep(1);
		t.interrupt();
		t.join();
		System.out.println("end");
	}
}

class MyThread extend Thread {
	public void run() {
		int n = 0;
		while (! isInterrupted()){
			n++;
			System.out.println(n + "hello!");
		}
	}
}
```

主线程中断`t`线程，此时`t`线程处于`hello`等待中，此等待立刻结束抛出`InterruptedException`，由于在`t`线程捕获了异常，因此可以准备结束该线程，在`t`线程结束前`hello`线程进行中断。

```java
public class Main {
	public static void main(String[] args) Throws InterruptedException{
		Thread t = new MyThread();
		t.start();
		Thread.sleep(1000);
		t.interrupt();
		t.join();
		System.out.println("end");
	}
}
class MyThread extends Thread {
	public void run(){
		Thread hello = new HelloThread();
		hello.start();
		try {
			hello.join();
		} catch (InterruptedException e) {
			System.out.println("interrupted!");
		}
		hello.interrupt();
	}
}
class HelloThread extends Thread {
	public void run() {
		int n = 0;
		while (! isInterrupted()) {
			n++;
			System.out.println(n + "hello!");
			try {
				Thread.sleep(100);
			} catch (InterruptedException e){
				break;
			}
		}
	}
}
```

通常用一个`running`标志位来标识线程是否应该继续运行，在外部线程中通过把`HelloThread.running`设置为`false`就可让线程结束。`HelloThread`的标志位`boolean running`是一个线程间共享的变量。线程间共享变量需要使用`volatile`关键字标记，确保每个线程都能读取到更新后的变量值。

`volatile`关键字的目的是告诉虚拟机：

- 每次访问变量时，总是获取主内存的最新值；
- 每次修改变量后，立刻回写到主内存。

```java
public class Main {
	public static void main(String[] args) {
		HelloThread t = new HelloThread();
		t.start();
		Thread.sleep(1);
		t.running = false;	//标志位置为false中断线程
	}
}
class  HelloThread extends Thread {
	public volatile boolean running = true;
	public void run(){
		int n = 0;
		while (running) {
			n ++;
			System.out.println(n + "hello!");
		}
		System.out.println("end");
	}
}
```

### 守护线程

- 守护线程是为其他线程服务的线程；
- 所有非守护线程都执行完毕后，虚拟机退出；
- 守护线程不能持有需要关闭的资源（如打开文件等）。

所有线程运行结束后，JVM退出，进程结束。当有一种线程的目的是无限循环，如定时触发任务线程：

```java
class TimerThread extend Thread {
	@Override
	public void run() {
		while (true) {
			System.out.println(LocalTime.now());
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e){
				break;
			}
		}
 	}
}
```

这类线程需要由守护线程执行，创建守护线程：

```java
Thread t = new MyThread();
t.setDaemon(true);
t.start;
```

在守护线程中不能包含任何需要关闭的资源，如打开文件，因为虚拟机退出时没有任何机会来关闭守护线程的文件，导致数据丢失。

### 线程同步

- 多线程同时读写共享变量时，会造成逻辑错误，因此需要通过`synchronized`同步；
- 同步的本质就是给指定对象加锁，加锁后才能继续执行后续代码；
- 注意加锁对象必须是同一个实例；
- 对JVM定义的单个原子操作不需要同步。

当多个线程同时运行时，线程的调度由操作系统决定，程序本身无法决定。因此任何一个线程都有可能在任何指令处被操作系统暂停，然后在某个时间段后继续执行。假设`n=100`，如果两个线程同时执行`n=n+1`得到的结果很可能不是102，当ILOAD共享变量后该线程中断，调度另一个线程执行ILOAD仍是100，执行IADD和ISTORE后结果是101。

对共享变量读写时要保证逻辑正确必须保证一组指令以原子方式执行：即某一个线程执行时，其他线程必须等待。通过加锁和解锁的操作保证在一个线程执行期间被中断后也不会执行其他线程的指令，只有执行的线程将锁释放后，其他线程才有机会获得锁并执行。加锁解锁之间的代码称为临界区，任何时候临界区最多只有一个线程能执行。

```java
public class Main {
    public static void main(String[] args) throws Exception {
        var add = new AddThread();
        var dec = new DecThread();
        add.start();
        dec.start();
        add.join();
        dec.join();
        System.out.println(Counter.count);
    }
}

class Counter {
    public static final Object lock = new Object();
    public static int count = 0;
}

class AddThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {	//对加法线程加锁
                Counter.count += 1;
            }	//无论有无异常，自动释放加法锁
        }
    }
}

class DecThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {	//对减法线程加锁
                Counter.count -= 1;
            }	//无论有无异常，自动释放减法锁
        }
    }
}
```

```java
public static final Object lock1 = new Object();
public static final Object lock2 = new Object();
synchronized(Counter.lock1) {
	Counter.count += 1;
}
synchronized(Counter.lock2) {
	Counter.count -= 1;
}
//当锁住的不是同一个对象进行加减时结果不为0，这两个线程都可以同时获得不同的锁被两个线程分别获取，试用于多线程并发执行。而JVM只保证同一个锁在任意时刻只能被一个线程获取。
```

### 不需synchronized

JVM规范定义了几种原子操作不需同步：

- 基本类型（`long`和`double`除外）赋值，例如：`int n = m`；
- 引用类型赋值，例如：`List<String> list = anotherList`。
- this.单条原子操作引用不需同步

  ```java
  public void set(String s) {
  	this.value = s;
  }
  ```


  多行赋值语句，this引用必须同步

  ```java
  class Pair {
  	int first;
  	int last;
  	public void set(int first, int last){
  		synchronized(this) {
  			this.first = first;
  			this.last = last;
  		}
  	}
  }
  ```


  上面的代码通过转换把非原子变为原子操作不需同步：

  ```java
  class Pair {
  	int[] pair;
  	public void set(int first, int last){
  		int[] ps = new int[] {first, last}; //方法内部定义的局部变量，互不影响
  		this.pair = ps; //引用赋值的原子操作
  	}
  }
  ```

### 同步方法

- 用`synchronized`修饰方法可以把整个方法变为同步代码块，`synchronized`方法加锁对象是`this`；
- 通过合理的设计和数据封装可以让一个类变为“线程安全”；
- 一个类没有特殊说明，默认不是thread-safe；
- 多线程能否安全访问某个非线程安全的实例，需要具体问题具体分析。

```java
public class Counter {
	private int count = 0;
	public void add(int n) {
		synchronized(this) {
			count += n;
		}
	}
	public void dec(int n) {
		synchronized(this) {
			count -= n;
		}
	}
	public int get(){
		return count;
	}
}
```

`synchronized`锁住的对象是`this`，当创建多个Counter实例的时候，它们之间互不影响，线程可以并发执行。

```java
var c1 = Counter();
var c2 = Counter();
// 对c1进行操作的线程：
new Thread(() -> {
	c1.add();
}).start();
new Thread(() -> {
	c1.dec();
}).start();

// 对c2进行操作的线程：
new Thread(() -> {
	c2.add();
}).start();
new Thread(() -> {
	c2.dec();
}).start();
```

### 线程安全

- 如`Counter`类允许多线程正确访问的类是线程安全的。
- JAVA标准库的`java.lang.StringBuffer`也是线程安全的。
- 还有一些不变类如`String`，`Integer`，`LocalDate` 所有的成员变量都是`final`，多线程只能读不能写也是线程安全的。
- 类似`Math`只提供静态方法，没有成员变量的类也是线程安全的。
- 除以上几种少数情况大部分类如`ArrayList`都是非线程安全的类，如果只读取不写入可以在线程间共享。

同步方法锁住`this`实例等效于`synchronized`修饰方法：

```java
public void add(int n) {
	synchronized(this) {
		count += n;
	}
}
public synchronized void add(int n) {
	count += n;
}
```

对于`static`方法没有`This`实例，针对类而不是实例，因此锁住的是该类的`Class`实例。

```java
public class Counter {
	public static void test(int n) {
		synchronized(Counter.class){
			...
		}
	}
}
```

对于`Counter`类的`get()`方法只读取一个`int`变量不需要同步，当包含多个变量时必须同步。

### 死锁

- Java的`synchronized`锁是可重入锁；
- 死锁产生的条件是多线程各自持有不同的锁，并互相试图获取对方已持有的锁，导致无限等待；
- 避免死锁的方法是多线程获取锁的顺序要一致。

### 重入锁

JVM允许对同一个线程获取同一个锁，反复获取的锁称为重入锁。由于可以重入锁，所以在获取锁的时候要判断是否第一次，第几次获取，每获取一次+1，每释放一次-1，直到0才会完全释放锁。

```java
public class Counter {
	private int count = 0;
	public synchronized void add(int n) {	//获取this锁
		if (n < 0) {
			dec(-n);	//调用dec方法获取this锁
		}else {
			count += n;
		}
	}
	public synchronized void dec(int n) {
		count += n;
	}
}
```

获取一个锁后再获取另一个锁可能导致死锁，如不同线程获取不同对象的锁：

```java
public void add(int m) {
	synchronized(lockA){	//获取lockA锁
		this.value += m;
		synchronized(lockB){	//获取lockB锁
			this.another += m;
		}	//释放lockB锁
	}	//释放lockA锁
}
public void dec(int m) {
	synchronized(lockB){	//获取lockB锁
		this.anotuer -= m;
		synchronized(lockA){	//获取lockA锁
			this.value -= m;
		}	//释放lockA锁
	}	//释放lockB锁
}
//改写后的线程2
public void dec(int m) {
	synchronized(lockA){
		this.value -= m;
		synchronized(lockB){
			this.another -= m;
		}
	}
}
```

- 线程1：进入`add()`获得`lockA`，准备获取`lockB`由于已被线程2获得导致失败无限等待。
- 线程2：进入`dec()`获得`lockB`，准备获取`lockA`由于已被线程1获得导致失败无限等待。

JVM没有任何机制解除死锁，只能强制结束进程。为了避免死锁，不同线程获取锁的顺序要一致，即严格按照先获得`lockA`再获得`lockB`的顺序。

### 使用wait和notify

`wait`和`notify`用于多线程协调运行：

- 在`synchronized`内部可以调用`wait()`使线程进入等待状态；
- 必须在已获得的锁对象上调用`wait()`方法；
- 在`synchronized`内部可以调用`notify()`或`notifyAll()`唤醒其他等待线程；
- 必须在已获得的锁对象上调用`notify()`或`notifyAll()`方法；
- 已唤醒的线程还需要重新获得锁后才能继续执行。

### 多线程协调

`synchronized`解决了多线程竞争的问题，当同时向队列添加任务需要解决多线程协调问题。原则是：当条件不满足时，线程进入等待状态；当条件满足时，线程被唤醒继续执行任务。`getTask`线程`while`循环条件不满足进入等待状态，`addTask`线程添加了任务后对`this`锁调用`notify()`随机唤醒一个正在等待的线程。

```java
class TaskQueue {
	Queue<String> queue = new LinkList<>();
	public synchronized void addTask(String s) {
		this.queue.add(s);
		//this.notify();	//唤醒this锁等待的线程
	}
	public synchronized getTask() {	//获取this锁
		while(queue.isEmpty()){	//while死循环，线程无法调用addTask已加this锁，要在当前获取锁的对象上调用wait使线程进入等待状态，释放线程获得的锁，当其它线程被唤醒后重新获得锁返回继续执行。
			//this.wait();
		}
		return queue.remove();
	}
}
```

使用`notifyAll()`将唤醒所有当前正在`this`锁等待的线程，通常`notifyall()`更安全，当多个线程都在`getTask()`方法内部的`wait()`等待可以全部唤醒。唤醒后执行`addTask()`释放`this`锁，多线程中的一个线程重新获取`this`锁，剩余线程继续等待。

### 使用ReentrantLock

- `ReentrantLock`可以替代`synchronized`进行同步；
- `ReentrantLock`获取锁更安全；
- 必须先获取到锁，再进入`try {...}`代码块，最后使用`finally`保证释放锁；
- 可以使用`tryLock()`尝试获取锁。

```java
// 传统synchronized加锁
public class Counter {
	private int count;
	public void add(int n) {
		synchronized(this) {
			count += n;
		}
	}
}
// 用java.util.concurrent.locks包下的ReentrantLock替代
public class Counter {
	private final Lock lock = new ReentrantLock();
	private int count;
	public void add(int n) {
		lock.lock();
		try{
			count += n;
		}finally {
			lock.unlock();
		}
	}
}
```

`RentrantLock`可重入锁，与`synchronized`不同的是`RentrantLock`可尝试获取锁，等待1秒返回结果，而不是死锁无限等待。

```java
if (lock.tryLock(1, TimeUnit.SECONDS)) {
	try {
		...
	}finally {
		lock.unlock();
	}
}
```

### 使用Condition

- `Condition`可以替代`wait`和`notify`；
- `Condition`对象必须从`Lock`对象获取。

`synchronized`可以配合`wait`和`notify`实现在线程不满足时等待，条件满足唤醒，用`RentrantLock`时使用`Condition`对象来实现。

```java
class TaskQueue {
	private final Lock lock = new RentrantLock();
	// 引入的Condition对象要从lock实例的newCondition()返回，获得绑定实例。
	private final Condition condition = lock.newCondition();
	private Queue<String> queue = new LinkedList<>();

	public void addTask(String s){
		lock.lock();
		try{
			queue.add(s);
			condition.signalAll();
		} finally {
			lock.unlock();
		}
	}

	public String getTask() {
		lock.lock();
		try {
			while (queue.isEmpty()) {
				condition.await();
			}
			return queue.remove();
		} finally {
			lock.unlock();
		}
	}
}
```

- `await()`会释放当前锁，进入等待状态；
- `signal()`会唤醒某个等待线程；
- `signalAll()`会唤醒所有等待线程；
- 唤醒线程从`await()`返回后需要重新获得锁。

和`tryLock()`类似，`await()`可以在等待时间后，如果没有被其他线程通过`signal()`或`signalall()`唤醒，可以自己醒来。

```java
if (condition.await(1, TimeUnit.SECONDS)) {
	// 被其他线程唤醒
} else {
	// 指定时间没有被其他线程唤醒
}
```

### 使用ReadWriteLock

使用`ReadWriteLock`可以提高读取效率：

- `ReadWriteLock`只允许一个线程写入；
- `ReadWriteLock`允许多个线程在没有写入时同时读取；
- `ReadWriteLock`适合读多写少的场景。

`ReentrantLock`保证只有一个线程执行临界区代码：

```java
public class Counter {
	private final Lock lock = new ReentrantLock();
	private int[] counts = new int[10];
	public void inc(int index){
		lock.lock();
		try{
			counts[index] += 1;
		} finally {
			lock.unlock();
		}
	}
	public int[] get() {
		lock.lock();
		try {
			return Arrays.copyOf(counts, counts.length);
		} finally {
			lock.unlock();
		}
	}
}
```

当需要调用`inc()`方法必须获取锁，但是当调用`get()`方法只读不修改数据实际允许多个线程同时调用读，只要有一个线程在写其他线程必须等待。

- 使用`ReadWriteLock`只允许一个线程写入，其他线程既不能写入也不能读取；
- 没有写入时多个线程允许同时读取。

把读写操作分别用读锁和写锁加锁，读取时多个线程可以同时获得锁，提高并发读的执行效率。

```java
public class Counter {
	private final ReadWriteLock rwlock = new ReentrantReadWriteLock();
	private final Lock rlock = rwlock.readLock();
	private final Lock wlock = rwlock.writeLock();
	private int[] counts = new int[10];
	public void inc(int index) {
        wlock.lock(); // 加写锁
        try {
            counts[index] += 1;
        } finally {
            wlock.unlock(); // 释放写锁
        }
    }

    public int[] get() {
        rlock.lock(); // 加读锁
        try {
            return Arrays.copyOf(counts, counts.length);
        } finally {
            rlock.unlock(); // 释放读锁
        }
    }
}
```

### 使用StampedLock

- `StampedLock`提供了乐观读锁，可取代`ReadWriteLock`以进一步提升并发性能；
- `StampedLock`是不可重入锁，不能在一个线程中反复获取同一个锁。
- `StampedLock`还可将悲观读锁升级为写锁的功能，即先读，如果读的数据满足条件就返回，如果数据不满足再尝试写。

`ReadWriteLock`可以解决多线程同时读，但只有一个线程能写。如果线程正在读，写的线程需要等待读的线程释放后才能获取写锁，即读的过程中不允许写，这是一种悲观的读锁。

Java8引入了新的乐观读写锁`StampedLock`进一步提升了并发执行效率，读的过程也允许获取写锁后写入。乐观地估计读的过程中大概率不会写入，但小概率写入会导致数据不一致，需要检测出来再读一遍。

写入的加锁和`ReadWriteLock`完全一样，不同的是读取，需要先通过`tryOptimisticRead()`获取一个乐观读锁并返回版本号，读取完成后通过`alidate()`验证版本号，如果在读的过程没有写入版本号不变，验证成功。如果在读的过程中写入，版本号发生变化，验证失败，在失败的时候通过获取悲观锁再次读取。

```java
public class Point {
    private final StampedLock stampedLock = new StampedLock();

    private double x;
    private double y;

    public void move(double deltaX, double deltaY) {
        long stamp = stampedLock.writeLock(); // 获取写锁
        try {
            x += deltaX;
            y += deltaY;
        } finally {
            stampedLock.unlockWrite(stamp); // 释放写锁
        }
    }

    public double distanceFromOrigin() {
        long stamp = stampedLock.tryOptimisticRead(); // 获得一个乐观读锁
        // 注意下面两行代码不是原子操作
        // 假设x,y = (100,200)
        double currentX = x;
        // 此处已读取到x=100，但x,y可能被写线程修改为(300,400)
        double currentY = y;
        // 此处已读取到y，如果没有写入，读取是正确的(100,200)
        // 如果有写入，读取是错误的(100,400)
        if (!stampedLock.validate(stamp)) { // 检查乐观读锁后是否有其他写锁发生
            stamp = stampedLock.readLock(); // 获取一个悲观读锁
            try {
                currentX = x;
                currentY = y;
            } finally {
                stampedLock.unlockRead(stamp); // 释放悲观读锁
            }
        }
        return Math.sqrt(currentX * currentX + currentY * currentY);
    }
}
```

**乐观锁原理**

使用数据版本（Version）记录机制实现，这是乐观锁最常用的一种实现方式。何谓数据版本？即为数据增加一个版本标识，一般是通过为数据库表增加一个数字类型的 “version” 字段来实现。当读取数据时，将version字段的值一同读出，数据每更新一次，对此version值加一。当我们提交更新的时候，判断数据库表对应记录的当前版本信息与第一次取出来的version值进行比对，如果数据库表当前版本号与第一次取出来的version值相等，则予以更新，否则认为是过期数据。

**悲观锁原理**

需要使用数据库的锁机制，比如MySQL， 

1. 先关闭auto commit属性，改为手动提交事务

2. 在事务内通过select...for update语句锁定数据，如：select * from t_goods where id=1 for update

3. 此时在t_goods表中，id为1的 那条数据就被我们锁定了，其它的事务必须等本次事务提交之后才能执行。这样我们可以保证当前的数据不会被其它事务修改

### 使用Concurrent集合

- 使用`java.util.concurrent`包提供的线程安全的并发集合可以大大简化多线程编程：
- 多线程同时读写并发集合是安全的；
- 尽量使用Java标准库提供的并发集合，避免自己编写同步代码。

通过`ReentrantLock`和`Condition`实现`BlockingQueue`，当一个线程调用TaskQueue的`getTask()`方法时，方法内部可能会让线程变成等待状态，直到队列满足条件不为空线程被唤醒后，`getTask()`方法才会返回。

```java
public class TaskQueue {
	private final Lock lock = new ReentrantLock();
	private final Condition condition = lock newCondition();
	private Queue<String> queue = new LinkedList<>();
	public void addTask(String s) {
    	lock.lock();
        try {
            queue.add(s);
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public String getTask() {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                condition.await();
            }
            return queue.remove();
        } finally {
            lock.unlock();
        }
    }
}
```

在Java标准库的`java.util.concurrent`包提供线程安全的集合：


| interface | non-thread-safe         | thread-safe                              |
| :-------- | :---------------------- | :--------------------------------------- |
| List      | ArrayList               | CopyOnWriteArrayList                     |
| Map       | HashMap                 | ConcurrentHashMap                        |
| Set       | HashSet / TreeSet       | CopyOnWriteArraySet                      |
| Queue     | ArrayDeque / LinkedList | ArrayBlockingQueue / LinkedBlockingQueue |
| Deque     | ArrayDeque / LinkedList | LinkedBlockingDeque                      |

因为所有同步和加锁的逻辑都在集合内部实现，使用线程安全和非线程安全的并发集合相同，调用只需要按接口引用。以`ConcurrentHashMap`为例，当需要多线程访问直接替换`HashMap<>()`就可以了：

```java
Map<String, String> map = new ConcurrentHashMap<>();
// 在不听线程读写：
map.put("A","1");
map.put("B","2");
map.get("A","1");
```

`java.util.Collections`工具类还提供旧的线程安全集合转换器，但通过`synchronized`加锁性能比`java.util.concurrent`集合要低很多。

```java
Map unsafeMap = new HashMap();
Map threadSafeMap = Collections.synchronizedMap(unsafeMap);
```

### 使用Atomic

使用`java.util.concurrent.atomic`提供的原子操作可以简化多线程编程：

- 原子操作实现了无锁的线程安全；
- 适用于计数器，累加器等。

以`AtomicInteger`为例，提供原子操作的封装类：

- 增加值并返回新值：`int addAndGet(int delta)`
- 加1后返回新值：`int incrementAndGet()`
- 获取当前值：`int get()`
- 用CAS方式设置：`int compareAndSet(int expect, int update)`

`Atomic`类是通过无锁的方式实现的线程安全访问，原理是Compare and Set。在这个操作中如果`AtomicInteger`的当前值是`prev`就更新`next`，返回`true`。如果当前值不是`prev`直接返回`false`，配合`do...while`循环，即使其他线程修改了`AtomicInteger`结果也是正确的。

```java
public int incrementAndGet (AtomicInteger var) {
	int prev, next;
	do {
		prev = var.get();
		next = prev + 1;
	} while (! var.compareAndSet(prev, next));
	return next;
}
```

利用`AtomicLong`编写一个多线程安全全局唯一ID生成器，通常不需要直接用`do...while`循环调用`compareAndSet`实现复杂并发操作，而是用`incrementAndGet()`这样的封装好的方法简化。

```java
class IdGenerator {
	AtomicLong var = new AtomicLong(0);
	public long getNextId(){
		return var.incrementAndGet();
	}
}
```

### 使用线程池

JDK提供了`ExecutorService`实现了线程池功能：

- 线程池内部维护一组线程，可以高效执行大量小任务；
- `Executors`提供了静态方法创建不同类型的`ExecutorService`；
- 必须调用`shutdown()`关闭`ExecutorService`；
- `ScheduledThreadPool`可以定期调度多个任务。

创建销毁线程需要资源，空间，时间。因此可以把多个小任务分给一组线程来执行，接收大量小任务并进行分发处理的就是线程池。线程池内包含若干个线程，没有任务所有线程等待状态，有新任务就分配线程池中的一个线程执行，如果所有线程都忙碌，新的任务要么放入队列等待，要么增加一个新的线程池处理。

Java标准库提供`ExecutorService`接口表示线程池，典型用法：

```java
// 创建固定大小的线程池
ExecutroService executor = Executors.newFixedThreadPool(3);
// 提交任务
executor.submit(task1);
executor.submit(task2);
executor.submit(task3);
executor.submit(task4);
```

`ExecutorSerice`是接口，Java标准库提供几个常用实现类被封装在`Executors`中：

- FixedThreadPool：线程数固定的线程池；
- CachedThreadPool：线程数根据任务动态调整线程池；
- SingleThreadExecutor：仅单线程执行的线程池。

以`FixedThreadPool`为例，一次放入6个任务，由于线程池固定了4个线程，因此4个任务会同时执行，等到有空闲线程后才会执行后面两个任务。`shutdown()`方法会等待正在执行的任务完成后关闭，`shutdownNow()`会立刻停止正在执行的任务，`awaitTermination()`则会等待指定时间让线程关闭。

```java
import java.util.concurrent.*;
public class Main {
	public static void main(String[] args) {
		// 创建一个固定大小的线程池
		ExecutorService es = Executor.newFixedThreadPool(4);
		for(int i = 0; i < 6; i++) {
			es.submit(new Task("" + i));
		}
		// 关闭线程池
		es.shutdown();
	}
}
class Task implements Runnable {
    private final String name;

    public Task(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println("start task " + name);
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
        System.out.println("end task " + name);
    }
}
```

如果线程池改为`CachedThreadPool`则会动态调整线程池大小，6个任务一次性全部同时执行，如果需要限制线程池大小在4-10之间查看`Executors.newCachedThreadPool()`方法的源码后可以这么写：

```java
int min = 4;
int max = 10;
ExecutorSerice es = new ThreadPoolExecutor(min, max, 60L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());
```

#### ScheduledThreadPool

还有一种需要定期反复执行的任务，可使用`ScheduledThreadPool`。

创建一个`ScheduledThreadPool`是通过`Executors`类：

```java
ScheduledExecutorService ses = Executors.newScheduledThreadPool(4);
```

提交一次性任务，在指定延迟后只执行一次：

```java
// 1秒后执行一次性任务
ses.schedule(new Task("one-time"), 1, TimeUnit.SECONDS);
```

任务固定每3秒执行：

```java
// 2秒后开始执行定时任务，每3秒执行:
ses.scheduleAtFixedRate(new Task("fixed-rate"), 2, 3, TimeUnit.SECONDS);
```

任务固定3秒为间隔时间执行：

```java
// 2秒后开始执行定时任务，以3秒为间隔执行:
ses.scheduleWithFixedDelay(new Task("fixed-delay"), 2, 3, TimeUnit.SECONDS);
```

Q1:在FixedRate模式下，假设每秒触发，如果某次任务执行时间超过1秒，后续任务会不会并发执行？

- 如果此任务的任何执行时间超过其周期，则**后续执行可能会延迟开始，但不会并发执行。**

Q2:如果任务抛出了异常，后续任务是否继续执行？

- 如果任务的任何执行遇到异常，则将**禁止后续任务的执行。**

Java标准库还提供了一个`java.util.Timer`类，这个类也可以定期执行任务，但是，一个`Timer`会对应一个`Thread`，所以，一个`Timer`只能定期执行一个任务，多个定时任务必须启动多个`Timer`，而一个`ScheduledThreadPool`就可以调度多个定时任务，所以，我们完全可以用`ScheduledThreadPool`取代旧的`Timer`。

### 使用Future

- 对线程池提交一个`Callable`任务，可以获得一个`Future`对象；
- 可以用`Future`在将来某个时刻获取结果。

实现`Callable`接口可以让线程池执行任务，并返回值，泛型接口可指定返回类型。当提交一个`callable`任务后同时得到一个`Future`对象，调用`get()`方法当任务完成获得异步执行结果，如果任务没有完成发生阻塞，直到任务完成后才返回结果。

```java
ExecutorService executor = Executors.newFixedThreadPool(4); 
// 定义任务:
Callable<String> task = new Task();
// 提交任务并获得Future:
Future<String> future = executor.submit(task);
// 从Future获取异步执行返回的结果:
String result = future.get(); // 可能阻塞
```

一个`Future<V>`接口表示一个未来可能会返回的结果，它定义的方法有：

- `get()`：获取结果（可能会等待）
- `get(long timeout, TimeUnit unit)`：获取结果，但只等待指定的时间；
- `cancel(boolean mayInterruptIfRunning)`：取消当前任务；
- `isDone()`：判断任务是否已完成。

### 使用CompletableFuture

`CompletableFuture`可以指定异步处理流程：

- `thenAccept()`处理正常结果；
- `exceptional()`处理异常结果；
- `thenApplyAsync()`用于串行化另一个`CompletableFuture`；
- `anyOf()`和`allOf()`用于并行化多个`CompletableFuture`。

使用`Future`获得异步执行结果时，要么调用阻塞方法`get()`，要么轮询看`isDone()`是否为`true`，这样主线程也会被迫等待。通过Java 8开始引入的`CompletableFuture`，传入回调对象，当异步任务完成或者发生异常时，自动调用回调对象的回调方法。

以获取股票价格为例，`CompletableFuture`示例：

```java
import java.util.concurrent.CompletableFuture;
public class Main {
    public static void main(String[] args) throws Exception {
        // 创建异步执行任务，实现Supplier接口的对象
        CompletableFuture<Double> cf = CompletableFuture.supplyAsync(Main::fetchPrice);
        // 如果执行成功，用lambda语法简化
        cf.thenAccept((result) -> {
            System.out.println("price: " + result);
        });
        // 如果执行异常:
        cf.exceptionally((e) -> {
            e.printStackTrace();
            return null;
        });
        // 主线程不要立刻结束，否则CompletableFuture默认使用的线程池会立刻关闭:
        Thread.sleep(200);
    }

    static Double fetchPrice() {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
        }
        if (Math.random() < 0.3) {
            throw new RuntimeException("fetch price failed!");
        }
        return 5 + Math.random() * 20;
    }
}
```

`CompletableFuture`的优点是：

- 异步任务结束时，会自动回调某个对象的方法；
- 异步任务出错时，会自动回调某个对象的方法；
- 主线程设置好回调后，不再关心异步任务的执行。

多个`CompletableFuture`可以串行执行，如第一个`CompletableFuture`根据证券名称查询证券代码，第二个`CompletableFuture`根据证券代码查询证券价格，串行操作如下：

```java
import java.util.concurrent.CompletableFuture;
public class Main {
    public static void main(String[] args) throws Exception {
        // 第一个任务:
        CompletableFuture<String> cfQuery = CompletableFuture.supplyAsync(() -> {
            return queryCode("中国石油");
        });
        // cfQuery成功后继续执行下一个任务:
        CompletableFuture<Double> cfFetch = cfQuery.thenApplyAsync((code) -> {
            return fetchPrice(code);
        });
        // cfFetch成功后打印结果:
        cfFetch.thenAccept((result) -> {
            System.out.println("price: " + result);
        });
        // 主线程不要立刻结束，否则CompletableFuture默认使用的线程池会立刻关闭:
        Thread.sleep(2000);
    }

    static String queryCode(String name) {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
        }
        return "601857";
    }

    static Double fetchPrice(String code) {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
        }
        return 5 + Math.random() * 20;
    }
}
```

多个`CompletableFuture`还可以并行执行，如同时从新浪和网易查询证券代码，只要任意一个返回结果，就进行下一步查询价格，查询价格也同时从新浪和网易查询，只要任意一个返回结果。

```java
import java.util.concurrent.CompletableFuture;
public class Main {
    public static void main(String[] args) throws Exception {
        // 两个CompletableFuture执行异步查询:
        CompletableFuture<String> cfQueryFromSina = CompletableFuture.supplyAsync(() -> {
            return queryCode("中国石油", "https://finance.sina.com.cn/code/");
        });
        CompletableFuture<String> cfQueryFrom163 = CompletableFuture.supplyAsync(() -> {
            return queryCode("中国石油", "https://money.163.com/code/");
        });

        // 用anyOf合并为一个新的CompletableFuture:
        CompletableFuture<Object> cfQuery = CompletableFuture.anyOf(cfQueryFromSina, cfQueryFrom163);

        // 两个CompletableFuture执行异步查询:
        CompletableFuture<Double> cfFetchFromSina = cfQuery.thenApplyAsync((code) -> {
            return fetchPrice((String) code, "https://finance.sina.com.cn/price/");
        });
        CompletableFuture<Double> cfFetchFrom163 = cfQuery.thenApplyAsync((code) -> {
            return fetchPrice((String) code, "https://money.163.com/price/");
        });

        // 用anyOf合并为一个新的CompletableFuture:
        CompletableFuture<Object> cfFetch = CompletableFuture.anyOf(cfFetchFromSina, cfFetchFrom163);

        // 最终结果:
        cfFetch.thenAccept((result) -> {
            System.out.println("price: " + result);
        });
        // 主线程不要立刻结束，否则CompletableFuture默认使用的线程池会立刻关闭:
        Thread.sleep(200);
    }

    static String queryCode(String name, String url) {
        System.out.println("query code from " + url + "...");
        try {
            Thread.sleep((long) (Math.random() * 100));
        } catch (InterruptedException e) {
        }
        return "601857";
    }

    static Double fetchPrice(String code, String url) {
        System.out.println("query price from " + url + "...");
        try {
            Thread.sleep((long) (Math.random() * 100));
        } catch (InterruptedException e) {
        }
        return 5 + Math.random() * 20;
    }
}
```

除了`anyOf()`可以实现“任意个`CompletableFuture`只要一个成功”，`allOf()`可以实现“所有`CompletableFuture`都必须成功”，这些组合操作可以实现非常复杂的异步流程控制。

`CompletableFuture`的命名规则：

- `xxx()`：表示该方法将继续在已有的线程中执行；
- `xxxAsync()`：表示将异步在线程池中执行。

### 使用ForkJoin

Fork/Join是一种基于“分治”的算法：通过分解任务，并行执行，最后合并结果得到最终结果。

`ForkJoinPool`线程池可以把一个大任务分拆成小任务并行执行，任务类必须继承自`RecursiveTask`或`RecursiveAction`。

使用Fork/Join模式可以进行并行计算以提高效率。

原理：判断一个任务是否足够小，如果是，直接计算，否则就拆成几个小任务分别计算。这个过程可以反复裂变成一系列小任务。

对大数据并行求和示例：

```java
import java.util.Random;
import java.util.concurrent.*;
public class Main {
	public static void main(String[] args) throws Exception {
		// 创建2000个随机数组成的数组
		long[] array = new long[2000];
		long expectedSum = 0;
		for (int i = 0; i < array.length; i++) {
			array[i] = random();
			expectedSum += array[i];
		}
		System.out.println("Expected sum:" + expectedSum);
		// fork/join
		ForkJoinTask<Long> task = new SumTask(array, 0, array.length);
		long startTime = System.currentTimeMillis();
		Long result = ForkJoinPool.commonPool().invoke(task);
		long endTime = System.currentTimeMillis();
		System.out.println("Fork/join sum :" + result + " in " + (endTime - startTime) + " ms.");
	}
	static Random random = new Random(0);
	static long random() {
		return random.nextInt(10000);
	}
}

class SumTask extends RecursieTask<long> {
	static final int THRESHOLD = 500;
    long[] array;
    int start;
    int end;
	SumTask(long[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

	@Override
    protected Long compute() {
        if (end - start <= THRESHOLD) {
            // 如果任务足够小,直接计算:
            long sum = 0;
            for (int i = start; i < end; i++) {
                sum += this.array[i];
                // 故意放慢计算速度:
                try {
                    Thread.sleep(1);
                } catch (InterruptedException e) {
                }
            }
            return sum;
        }
        // 任务太大,一分为二:
        int middle = (end + start) / 2;
        System.out.println(String.format("split %d~%d ==> %d~%d, %d~%d", start, end, start, middle, middle, end));
        SumTask subtask1 = new SumTask(this.array, start, middle);
        SumTask subtask2 = new SumTask(this.array, middle, end);
        // 并行运行两个子任务
        invokeAll(subtask1, subtask2);
        // 获取子任务的结果
        Long subresult1 = subtask1.join();
        Long subresult2 = subtask2.join();
        Long result = subresult1 + subresult2;
        System.out.println("result = " + subresult1 + " + " + subresult2 + " ==> " + result);
        return result;
	}
}
```

### 使用ThreadLocal

- `ThreadLocal`表示线程的“局部变量”，它确保每个线程的`ThreadLocal`变量都是各自独立的；
- `ThreadLocal`适合在一个线程的处理流程中保持上下文（避免了同一参数在所有方法中传递）；
- 使用`ThreadLocal`要用`try ... finally`结构，并在`finally`中清除。

`Thread`对象代表一个线程，可以在代码中调用`Thread.currentThread()`获取当前线程。

```java
public class Main {
	public static void main(String[] args) throws Exception {
		log("start main...");
		new Thread(() -> {
			log("run task...");
		}).start();
		new Thread(() -> {
			log("print...");
		}).start();
		log("end main");
	}
	static void log(String s) {
		System.out.println(Thread.currentThread().getName() + ":" + s);
	}
}
```

在一个线程中横跨若干方法调用传递对象称为上下文（Context）。给每个方法都增加一个context参数非常麻烦，Java标准库提供了一个特殊的`ThreadLocal`，它可以在线程中传递同一个对象。

```java
// 静态实例初始化
static ThreadLocal<User> threadLocalUser = new ThreadLocal<>();
// 使用方式
void processUser(user){
	try{
		threadLocalUser.set(user);
		step1();
		step2();
		// 如果不清除，该线程执行其他代码时会将上一次的状态带进去
	} finally {
		threadLocalUser.remove();
	}
}
// 通过设置一个user实例关联到ThreadLocal中，在移除之前所有方法都可以随时获取user实例，普通方法调用一定是同一个线程执行，所以threadLocalUser.get()获取的user是同一个实例。
void step1() {
    User u = threadLocalUser.get();
    log();
    printUser();
}

void log() {
    User u = threadLocalUser.get();
    println(u.name);
}

void step2() {
    User u = threadLocalUser.get();
    checkUser(u.id);
}
```

为了保证释放`ThreadLocal`关联的实例，通过`AutoCloseable`接口配合`try (resource) {...}`结构，让编译器自动为我们关闭。

```java
public class UserContext implements AutoCloseable {

    static final ThreadLocal<String> ctx = new ThreadLocal<>();

    public UserContext(String user) {
        ctx.set(user);
    }

    public static String currentUser() {
        return ctx.get();
    }

    @Override
    public void close() {
        ctx.remove();
    }
}
```

使用时借助`try (resource) {...}`结构：

```java
try (var ctx = new UserContext("Bob")) {
    // 可任意调用UserContext.currentUser():
    String currentUser = UserContext.currentUser();
} // 在此自动调用UserContext.close()方法释放ThreadLocal关联对象
```

这样就在`UserContext`中完全封装了`ThreadLocal`，外部代码在`try (resource) {...}`内部可以随时调用`UserContext.currentUser()`获取当前线程绑定的用户名。

## 网络编程

### TCP编程

- 服务器端用`ServerSocket`监听指定端口；
- 客户端使用`Socket(InetAddress, port)`连接服务器；
- 服务器端用`accept()`接收连接并返回`Socket`；
- 双方通过`Socket`打开`InputStream`/`OutputStream`读写数据；
- 服务器端通常使用多线程同时处理多个客户端连接，利用线程池可大幅提升效率；
- `flush()`用于强制输出缓冲区到网络。

当Socket连接成功地在服务器端和客户端之间建立后：

- 对服务器端来说，它的Socket是指定的IP地址和指定的端口号；
- 对客户端来说，它的Socket是它所在计算机的IP地址和一个由操作系统分配的随机端口号。

#### 服务器端

Java标准库提供了`ServerSocket`来实现对指定IP和指定端口的监听。

```java
public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocket ss = new ServerSocket(6666); // 监听指定端口
        System.out.println("server is running...");
        for (;;) {
            Socket sock = ss.accept(); // ss.accept()表示每当有新的客户端连接进来后，就返回一个Socket实例，这个Socket实例就是用来和刚连接的客户端进行通信的。由于客户端很多，要实现并发处理，我们就必须为每个新的Socket创建一个新线程来处理，这样，主线程的作用就是接收新的连接，每当收到新连接后，就创建一个新线程进行处理。这里也完全可以利用线程池来处理客户端连接，能大大提高运行效率。
            System.out.println("connected from " + sock.getRemoteSocketAddress());
            Thread t = new Handler(sock);
            t.start();
        }
    }
}

class Handler extends Thread {
    Socket sock;

    public Handler(Socket sock) {
        this.sock = sock;
    }

    @Override
    public void run() {
        try (InputStream input = this.sock.getInputStream()) {
            try (OutputStream output = this.sock.getOutputStream()) {
                handle(input, output);
            }
        } catch (Exception e) {
            try {
                this.sock.close();
            } catch (IOException ioe) {
            }
            System.out.println("client disconnected.");
        }
    }

    private void handle(InputStream input, OutputStream output) throws IOException {
        var writer = new BufferedWriter(new OutputStreamWriter(output, StandardCharsets.UTF_8));
        var reader = new BufferedReader(new InputStreamReader(input, StandardCharsets.UTF_8));
        writer.write("hello\n");
        writer.flush();
        for (;;) {
            String s = reader.readLine();
            if (s.equals("bye")) {
                writer.write("bye\n");
                writer.flush();
                break;
            }
            writer.write("ok: " + s + "\n");
            writer.flush();
        }
    }
}
```



#### 客户端

```java
public class Client {
    public static void main(String[] args) throws IOException {
        Socket sock = new Socket("localhost", 6666); // 连接指定服务器和端口
        try (InputStream input = sock.getInputStream()) {
            try (OutputStream output = sock.getOutputStream()) {
                handle(input, output);
            }
        }
        sock.close();
        System.out.println("disconnected.");
    }

    private static void handle(InputStream input, OutputStream output) throws IOException {
        var writer = new BufferedWriter(new OutputStreamWriter(output, StandardCharsets.UTF_8));
        var reader = new BufferedReader(new InputStreamReader(input, StandardCharsets.UTF_8));
        Scanner scanner = new Scanner(System.in);
        System.out.println("[server] " + reader.readLine());
        for (;;) {
            System.out.print(">>> "); // 打印提示
            String s = scanner.nextLine(); // 读取一行输入
            writer.write(s);
            writer.newLine();
            writer.flush();
            String resp = reader.readLine();
            System.out.println("<<< " + resp);
            if (resp.equals("bye")) {
                break;
            }
        }
    }
}
```

#### Socket流

因为TCP是一种基于流的协议，因此，Java标准库使用`InputStream`和`OutputStream`来封装Socket的数据流，这样我们使用Socket的流，和普通IO流类似：

```java
// 用于读取网络数据:
InputStream in = sock.getInputStream();
// 用于写入网络数据:
OutputStream out = sock.getOutputStream();
```

### UDP编程

使用UDP协议通信时，服务器和客户端双方无需建立连接，UDP端口和TCP端口虽然都使用0~65535，但他们是两套独立的端口：

- 服务器端用`DatagramSocket(port)`监听端口；
- 客户端使用`DatagramSocket.connect()`指定远程地址和端口；
- 双方通过`receive()`和`send()`读写数据；
- `DatagramSocket`没有IO流接口，数据被直接写入`byte[]`缓冲区。

#### 服务器端

在服务器端，使用UDP也需要监听指定的端口。Java提供了`DatagramSocket`来实现这个功能。

```java
DatagramSocket ds = new DatagramSocket(6666); // 监听指定端口
for (;;) { // 无限循环
    // 数据缓冲区:
    byte[] buffer = new byte[1024];
    DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
    ds.receive(packet); // 收取一个UDP数据包
    // 收取到的数据存储在buffer中，由packet.getOffset(), packet.getLength()指定起始位置和长度
    // 将其按UTF-8编码转换为String:
    String s = new String(packet.getData(), packet.getOffset(), packet.getLength(), StandardCharsets.UTF_8);
    // 发送数据:
    byte[] data = "ACK".getBytes(StandardCharsets.UTF_8);
    packet.setData(data);
    ds.send(packet);
}
```

#### 客户端

```java
DatagramSocket ds = new DatagramSocket();
ds.setSoTimeout(1000);
ds.connect(InetAddress.getByName("localhost"), 6666); // 连接指定服务器和端口
// 发送:
byte[] data = "Hello".getBytes();
DatagramPacket packet = new DatagramPacket(data, data.length);
ds.send(packet);
// 接收:
byte[] buffer = new byte[1024];
packet = new DatagramPacket(buffer, buffer.length);
ds.receive(packet);
String resp = new String(packet.getData(), packet.getOffset(), packet.getLength());
ds.disconnect();
```



### 发送Email

使用JavaMail API发送邮件本质上是一个MUA软件通过SMTP协议发送邮件至MTA服务器；

打开调试模式可以看到详细的SMTP交互信息；

某些邮件服务商需要开启SMTP，并需要独立的SMTP登录密码。

#### 准备SMTP登陆信息

MUA：Mail User Agent

MTA：Mail Transfer Agent

MDA：Mail Delivery Agent

SMTP协议：Simple Mail Transport Protocol，使用标准端口25，也可以使用加密端口465或587。

- QQ邮箱：SMTP服务器是smtp.qq.com，端口是465/587；
- 163邮箱：SMTP服务器是smtp.163.com，端口是465；
- Gmail邮箱：SMTP服务器是smtp.gmail.com，端口是465/587。

有了服务器和端口还需要用户名和口令。

**创建Mave工程，添加JavaMail依赖**

```xml
<dependencies>
    <dependency>
        <groupId>javax.mail</groupId>
        <artifactId>javax.mail-api</artifactId>
        <version>1.6.2</version>
    </dependency>
    <dependency>
        <groupId>com.sun.mail</groupId>
        <artifactId>javax.mail</artifactId>
        <version>1.6.2</version>
    </dependency>
    ...
```

**通过JavaMail的API连接SMTP服务器**

以587端口为例，连接SMTP服务器时，需要准备一个Properties对象，填入相关信息。最后获取Session实例时，如果服务器需要认证，还需要传入一个Authenticator对象，并返回指定的用户名和口令。当我们获取到Session实例后，打开调试模式可以看到SMTP通信的详细内容。

```java
// 服务器地址:
String smtp = "smtp.office365.com";
// 登录用户名:
String username = "jxsmtp101@outlook.com";
// 登录口令:
String password = "********";
// 连接到SMTP服务器587端口:
Properties props = new Properties();
props.put("mail.smtp.host", smtp); // SMTP主机名
props.put("mail.smtp.port", "587"); // 主机端口号
props.put("mail.smtp.auth", "true"); // 是否需要用户认证
props.put("mail.smtp.starttls.enable", "true"); // 启用TLS加密
// 获取Session实例:
Session session = Session.getInstance(props, new Authenticator() {
    protected PasswordAuthentication getPasswordAuthentication() {
        return new PasswordAuthentication(username, password);
    }
});
// 设置debug模式便于调试:
session.setDebug(true);
```

#### 发送邮件

构造一个`Message`对象，然后调用`Transport.send(Message)`即可完成发送，绝大多数邮件服务器要求发送方地址和登录用户名必须一致，否则发送将失败。

```java
MimeMessage message = new MimeMessage(session);
// 设置发送方地址:
message.setFrom(new InternetAddress("me@example.com"));
// 设置接收方地址:
message.setRecipient(Message.RecipientType.TO, new InternetAddress("xiaoming@somewhere.com"));
// 设置邮件主题:
message.setSubject("Hello", "UTF-8");
// 设置邮件正文:
message.setText("Hi Xiaoming...", "UTF-8");
// 发送:
Transport.send(message);
```

```java
这是JavaMail打印的调试信息:
DEBUG: setDebug: JavaMail version 1.6.2
DEBUG: getProvider() returning javax.mail.Provider[TRANSPORT,smtp,com.sun.mail.smtp.SMTPTransport,Oracle]
DEBUG SMTP: need username and password for authentication
DEBUG SMTP: protocolConnect returning false, host=smtp.office365.com, ...
DEBUG SMTP: useEhlo true, useAuth true
开始尝试连接smtp.office365.com:
DEBUG SMTP: trying to connect to host "smtp.office365.com", port 587, ...
DEBUG SMTP: connected to host "smtp.office365.com", port: 587
发送命令EHLO:
EHLO localhost
SMTP服务器响应250:
250-SG3P274CA0024.outlook.office365.com Hello
250-SIZE 157286400
...
DEBUG SMTP: Found extension "SIZE", arg "157286400"
发送命令STARTTLS:
STARTTLS
SMTP服务器响应220:
220 2.0.0 SMTP server ready
EHLO localhost
250-SG3P274CA0024.outlook.office365.com Hello [111.196.164.63]
250-SIZE 157286400
250-PIPELINING
250-...
DEBUG SMTP: Found extension "SIZE", arg "157286400"
...
尝试登录:
DEBUG SMTP: protocolConnect login, host=smtp.office365.com, user=********, password=********
DEBUG SMTP: Attempt to authenticate using mechanisms: LOGIN PLAIN DIGEST-MD5 NTLM XOAUTH2 
DEBUG SMTP: Using mechanism LOGIN
DEBUG SMTP: AUTH LOGIN command trace suppressed
登录成功:
DEBUG SMTP: AUTH LOGIN succeeded
DEBUG SMTP: use8bit false
开发发送邮件，设置FROM:
MAIL FROM:<********@outlook.com>
250 2.1.0 Sender OK
设置TO:
RCPT TO:<********@sina.com>
250 2.1.5 Recipient OK
发送邮件数据:
DATA
服务器响应354:
354 Start mail input; end with <CRLF>.<CRLF>
真正的邮件数据:
Date: Mon, 2 Dec 2019 09:37:52 +0800 (CST)
From: ********@outlook.com
To: ********001@sina.com
Message-ID: <1617791695.0.1575250672483@localhost>
邮件主题是编码后的文本:
Subject: =?UTF-8?Q?JavaMail=E9=82=AE=E4=BB=B6?=
MIME-Version: 1.0
Content-Type: text/plain; charset=UTF-8
Content-Transfer-Encoding: base64

邮件正文是Base64编码的文本:
SGVsbG8sIOi/meaYr+S4gOWwgeadpeiHqmphdmFtYWls55qE6YKu5Lu277yB
.
邮件数据发送完成后，以\r\n.\r\n结束，服务器响应250表示发送成功:
250 2.0.0 OK <HK0PR03MB4961.apcprd03.prod.outlook.com> [Hostname=HK0PR03MB4961.apcprd03.prod.outlook.com]
DEBUG SMTP: message successfully delivered to mail server
发送QUIT命令:
QUIT
服务器响应221结束TCP连接:
221 2.0.0 Service closing transmission channel
```

[SMTP协议响应码](https://www.iana.org/assignments/smtp-enhanced-status-codes/smtp-enhanced-status-codes.txt)

#### 发送HTML邮件

```java
message.setText(body, "UTF-8");
改为：
message.setText(body, "UTF-8", "html");
```

#### 发送附件

发送附件不能直接调用`message.setText()`方法，而是要构造一个`Multipart`对象， 一个`Multipart`对象可以添加若干个`BodyPart`，其中第一个`BodyPart`是文本，即邮件正文，后面的BodyPart是附件。`BodyPart`依靠`setContent()`决定添加的内容，如果添加文本，用`setContent("...", "text/plain;charset=utf-8")`添加纯文本，或者用`setContent("...", "text/html;charset=utf-8")`添加HTML文本。如果添加附件，需要设置文件名（不一定和真实文件名一致），并且添加一个`DataHandler()`，传入文件的MIME类型。二进制文件可以用`application/octet-stream`，Word文档则是`application/msword`。最后，通过`setContent()`把`Multipart`添加到`Message`中即可发送。

```java
Multipart multipart = new MimeMultipart();
// 添加text:
BodyPart textpart = new MimeBodyPart();
textpart.setContent(body, "text/html;charset=utf-8");
multipart.addBodyPart(textpart);
// 添加image:
BodyPart imagepart = new MimeBodyPart();
imagepart.setFileName(fileName);
imagepart.setDataHandler(new DataHandler(new ByteArrayDataSource(input, "application/octet-stream")));
multipart.addBodyPart(imagepart);
// 设置邮件内容为multipart:
message.setContent(multipart);
```

#### 发送内嵌图片的HTML邮件

如果给一个`<img src="http://example.com/test.jpg">`，这样的外部图片链接通常会被邮件客户端过滤，并提示用户显示图片并不安全。只有内嵌的图片才能正常在邮件中显示。内嵌图片实际上也是一个附件即邮件本身也是`Multipart`，但需要做额外的处理：在HTML邮件中引用图片时，需要设定一个ID，用类似`<img src=\"cid:img01\">`引用，然后，在添加图片作为BodyPart时，除了要正确设置MIME类型（根据图片类型使用`image/jpeg`或`image/png`），还需要设置一个Header：`imagepart.setHeader("Content-ID", "<img01>");` 这个ID和HTML中引用的ID对应起来，邮件客户端就可以正常显示内嵌图片。

```java
Multipart multipart = new MimeMultipart();
// 添加text:
BodyPart textpart = new MimeBodyPart();
textpart.setContent("<h1>Hello</h1><p><img src=\"cid:img01\"></p>", "text/html;charset=utf-8");
multipart.addBodyPart(textpart);
// 添加image:
BodyPart imagepart = new MimeBodyPart();
imagepart.setFileName(fileName);
imagepart.setDataHandler(new DataHandler(new ByteArrayDataSource(input, "image/jpeg")));
// 与HTML的<img src="cid:img01">关联:
imagepart.setHeader("Content-ID", "<img01>");
multipart.addBodyPart(imagepart);
```

### 接收Email

- 使用Java接收Email时，可以用POP3协议或IMAP协议。
- 使用POP3协议时，需要用Maven引入JavaMail依赖，并确定POP3服务器的域名／端口／是否使用SSL等，然后，调用相关API接收Email。
- 设置debug模式可以查看通信详细内容，便于排查错误。

发送邮件客户端总是通过SMTP协议把邮件发送给MTA。接收Email则想反，收件人用自己的客户端把邮件从MDA服务器上抓取到本地的过程。

POP3：标准端口110，加密端口995

IMAP：标准端口143，加密端口993

IMAP和POP3的主要区别是，IMAP协议在本地的所有操作都会自动同步到服务器上，并且IMAP可以允许用户在邮件服务器的收件箱中创建文件夹。

POP3收取Email：

```java
// 准备登录信息:
String host = "pop3.example.com";
int port = 995;
String username = "bob@example.com";
String password = "password";

Properties props = new Properties();
props.setProperty("mail.store.protocol", "pop3"); // 协议名称
props.setProperty("mail.pop3.host", host);// POP3主机名
props.setProperty("mail.pop3.port", String.valueOf(port)); // 端口号
// 启动SSL:
props.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
props.put("mail.smtp.socketFactory.port", String.valueOf(port));

// 连接到Store:
URLName url = new URLName("pop3", host, post, "", username, password);
Session session = Session.getInstance(props, null);
session.setDebug(true); // 显示调试信息
Store store = new POP3SSLStore(session, url);
store.connect();
```

一个`Store`对象表示整个邮箱的存储，要收取邮件，我们需要通过`Store`访问指定的`Folder`（文件夹），通常是`INBOX`表示收件箱：

```java
// 获取收件箱:
Folder folder = store.getFolder("INBOX");
// 以读写方式打开:
folder.open(Folder.READ_WRITE);
// 打印邮件总数/新邮件数量/未读数量/已删除数量:
System.out.println("Total messages: " + folder.getMessageCount());
System.out.println("New messages: " + folder.getNewMessageCount());
System.out.println("Unread messages: " + folder.getUnreadMessageCount());
System.out.println("Deleted messages: " + folder.getDeletedMessageCount());
// 获取每一封邮件:
Message[] messages = folder.getMessages();
for (Message message : messages) {
    // 打印每一封邮件:
    printMessage((MimeMessage) message);
}
```

当我们获取到一个`Message`对象时，可以强制转型为MimeMessage，然后打印出邮件主题、发件人、收件人等信息：

```java
void printMessage(MimeMessage msg) throws IOException, MessagingException {
    // 邮件主题:
    System.out.println("Subject: " + MimeUtility.decodeText(msg.getSubject()));
    // 发件人:
    Address[] froms = msg.getFrom();
    InternetAddress address = (InternetAddress) froms[0];
    String personal = address.getPersonal();
    String from = personal == null ? address.getAddress() : (MimeUtility.decodeText(personal) + " <" + address.getAddress() + ">");
    System.out.println("From: " + from);
    // 继续打印收件人:
    ...
}
```

比较麻烦的是获取邮件的正文。一个`MimeMessage`对象也是一个`Part`对象，它可能只包含一个文本，也可能是一个`Multipart`对象，即由几个`Part`构成，因此，需要递归地解析出完整的正文：

```java
String getBody(Part part) throws MessagingException, IOException {
    if (part.isMimeType("text/*")) {
        // Part是文本:
        return part.getContent().toString();
    }
    if (part.isMimeType("multipart/*")) {
        // Part是一个Multipart对象:
        Multipart multipart = (Multipart) part.getContent();
        // 循环解析每个子Part:
        for (int i = 0; i < multipart.getCount(); i++) {
            BodyPart bodyPart = multipart.getBodyPart(i);
            String body = getBody(bodyPart);
            if (!body.isEmpty()) {
                return body;
            }
        }
    }
    return "";
}
```

最后关闭`Folder`和`Store`：

```java
folder.close(true); // 传入true表示删除操作会同步到服务器上（即删除服务器收件箱的邮件）
store.close();
```

### HTTP编程

- Java提供了`HttpClient`作为新的HTTP客户端编程接口用于取代老的`HttpURLConnection`接口；
- `HttpClient`使用链式调用并通过内置的`BodyPublishers`和`BodyHandlers`来更方便地处理数据。

**HTTP请求**

HTTP请求由HTTP Header和HTTP Body两部分组成，Header的第一行总是`请求方法 路径 HTTP版本`，后续的每一行都是固定的`Header: Value`格式：

- Host：表示请求的域名，因为一台服务器上可能有多个网站，因此有必要依靠Host来识别用于请求；
- User-Agent：表示客户端自身标识信息，不同的浏览器有不同的标识，服务器依靠User-Agent判断客户端类型；
- Accept：表示客户端能处理的HTTP响应格式，`*/*`表示任意格式，`text/*`表示任意文本，`image/png`表示PNG格式的图片；
- Accept-Language：表示客户端接收的语言，多种语言按优先级排序，服务器依靠该字段给用户返回特定语言的网页版本。

如果是`GET`请求那么该HTTP请求只有Header没有Body，参数必须附加在URL上，并以URLEncode方式编码，URL长度限制参数不能太多。如果是`POST`请求则带有Body，以一个空行分隔，通常要设置`Content-Type`表示Body的类型，`Content-Length`表示Body的长度。

```http
POST /login HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 30

username=hello&password=123456
```

**HTTP响应**

响应也是由Header和Body组成，第一行总是HTTP版本 响应代码 响应说明。HTTP/1.1协议允许在一个TCP连接中反复发送-响应，但发送一个请求后必须等待服务器响应才能发送下一个请求，HTTP/2.0允许客户端在没有收到响应的时候，发送多个HTTP请求，服务器返回响应的时候，不一定按顺序返回，只要双方能识别出哪个响应对应哪个请求，就可以做到并行发送和接收。

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 133251

<!DOCTYPE html>
<html><body>
<h1>Hello</h1>
...
```

HTTP有固定的响应代码：

- 1xx：表示一个提示性响应，例如101表示将切换协议，常见于WebSocket连接；
- 2xx：表示一个成功的响应，例如200表示成功，206表示只发送了部分内容；
- 3xx：表示一个重定向的响应，例如301表示永久重定向，303表示客户端应该按指定路径重新发送请求；
- 4xx：表示一个因为客户端问题导致的错误响应，例如400表示因为Content-Type等各种原因导致的无效请求，404表示指定的路径不存在；
- 5xx：表示一个因为服务器问题导致的错误响应，例如500表示服务器内部故障，503表示服务器暂时无法响应。

**HTTP服务端**

 

**HTTP客户端**

发送请求后获取响应内容，Java标准库提供基于HTTP的包，早期通过`HttpURLConnection`访问HTTP：

```java
URL url = new URL("http://www.example.com/path/to/target?a=1&b=2");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("GET");
conn.setUseCaches(false);
conn.setConnectTimeout(5000); // 请求超时5秒
// 设置HTTP头:
conn.setRequestProperty("Accept", "*/*");
conn.setRequestProperty("User-Agent", "Mozilla/5.0 (compatible; MSIE 11; Windows NT 5.1)");
// 连接并发送HTTP请求:
conn.connect();
// 判断HTTP响应是否200:
if (conn.getResponseCode() != 200) {
    throw new RuntimeException("bad response");
}		
// 获取所有响应Header:
Map<String, List<String>> map = conn.getHeaderFields();
for (String key : map.keySet()) {
    System.out.println(key + ": " + map.get(key));
}
// 获取响应内容:
InputStream input = conn.getInputStream();
...
```

上面的代码比较繁琐并且需要手动处理`InputStream`，从Java 11开始引入了新的`HttpClient`使用链式调用API简化HTTP处理，因为`HttpClient`内部使用线程池优化多个HTTP连接，可以复用。

**使用`GET`请求获取文本内容：**

```java
import java.net.URI;
import java.net.http.*;
import java.net.http.HttpClient.Version;
import java.time.Duration;
import java.util.*;

public class Main {
    // 全局HttpClient:
    static HttpClient httpClient = HttpClient.newBuilder().build();

    public static void main(String[] args) throws Exception {
        String url = "https://www.sina.com.cn/";
        HttpRequest request = HttpRequest.newBuilder(new URI(url))
            // 设置Header:
            .header("User-Agent", "Java HttpClient").header("Accept", "*/*")
            // 设置超时:
            .timeout(Duration.ofSeconds(5))
            // 设置版本:
            .version(Version.HTTP_2).build();
        HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());
        // HTTP允许重复的Header，因此一个Header可对应多个Value:
        Map<String, List<String>> headers = response.headers().map();
        for (String header : headers.keySet()) {
            System.out.println(header + ": " + headers.get(header).get(0));
        }
        System.out.println(response.body().substring(0, 1024) + "...");
    }
}
```

如果我们要获取图片这样的二进制内容，只需要把`HttpResponse.BodyHandlers.ofString()`换成`HttpResponse.BodyHandlers.ofByteArray()`，就可以获得一个`HttpResponse<byte[]>`对象。如果响应的内容很大，不希望一次性全部加载到内存，可以使用`HttpResponse.BodyHandlers.ofInputStream()`获取一个`InputStream`流。

**使用`POST`请求要准备好发送的Body数据并正确设置`Content-Type`：**

```java
String url = "http://www.example.com/login";
String body = "username=bob&password=123456";
HttpRequest request = HttpRequest.newBuilder(new URI(url))
    // 设置Header:
    .header("Accept", "*/*")
    .header("Content-Type", "application/x-www-form-urlencoded")
    // 设置超时:
    .timeout(Duration.ofSeconds(5))
    // 设置版本:
    .version(Version.HTTP_2)
    // 使用POST并设置Body:
    .POST(BodyPublishers.ofString(body, StandardCharsets.UTF_8)).build();
HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());
String s = response.body();
```

**使用okhttp3的RequestBody**

[又拍云内容识别](https://help.upyun.com/knowledge-base/audit_nostorage/#e59bbee78987e8af86e588abe59bbee78987e58685e5aeb9)

```java
package file;

import java.io.File;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.security.InvalidKeyException;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.security.SignatureException;
import java.text.SimpleDateFormat;
import java.util.Base64;
import java.util.Calendar;
import java.util.Locale;
import java.util.TimeZone;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;

import okhttp3.MediaType;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.RequestBody;
import okhttp3.Response;
/**
 * 又拍云图片识别
 * 文档：http://docs.upyun.com/ai/audit_nostorage/#_2
 * @author mark
 *
 */
public class Identify {

	private static final String HMAC_SHA1_ALGORITHM = "HmacSHA1";

	public static void main(String[] args) throws Exception {
		//图片路径
		File pic = new File("20190604.jpg");
		//秘钥，控制台获取
		String clientKey = "";
		String clientSecret = "";
		//鉴权参数
		String method = "POST";
		String uri = "/image/bin/check?act=porn&act=political&act=terror";
		String date = getRFC1123Time();
		
		 String sign = sign(clientKey, clientSecret, method, uri, date, "", "");
         System.out.println(sign);

		OkHttpClient client = new OkHttpClient().newBuilder().build();
		MediaType mediaType = MediaType.parse("image/jpeg");
		RequestBody body = RequestBody.create(mediaType, pic);
		Request request = new Request.Builder()
				.url("http://banma.api.upyun.com" + uri)
				.method("POST", body)
				.addHeader("Authorization", sign)
				.addHeader("Date", date)
				.addHeader("Content-Type", "image/jpeg")
				.build();
		Response response = client.newCall(request).execute();
		System.out.println(response.body().string());
	}

	/**
	 * 获取 RFC1123 格式的时间字符串 Sun, 06 Nov 1994 08:49:37 GMT
	 * 
	 * @see https://www.w3.org/Protocols/rfc2616/rfc2616.txt
	 * @return RFC1123 格式的时间字符串
	 */
	public static String getRFC1123Time() {
		Calendar cal = Calendar.getInstance();
		// 按照文档要求格式化时间格式，并指定日期格式符号的区域设置
		SimpleDateFormat sdf = new SimpleDateFormat("EEE, dd MMM yyyy HH:mm:ss z", Locale.US);
		// 设置时区
		sdf.setTimeZone(TimeZone.getTimeZone("GMT"));
		return sdf.format(cal.getTime());
	}

	public static String md5(String string) {
		byte[] hash;
		try {
			hash = MessageDigest.getInstance("MD5").digest(string.getBytes("UTF-8"));
		} catch (UnsupportedEncodingException e) {
			throw new RuntimeException("UTF-8 is unsupported", e);
		} catch (NoSuchAlgorithmException e) {
			throw new RuntimeException("MessageDigest不支持MD5Util", e);
		}
		StringBuilder hex = new StringBuilder(hash.length * 2);
		for (byte b : hash) {
			if ((b & 0xFF) < 0x10)
				hex.append("0");
			hex.append(Integer.toHexString(b & 0xFF));
		}
		return hex.toString();
	}

	public static byte[] hashHmac(String data, String key)
			throws SignatureException, NoSuchAlgorithmException, InvalidKeyException {
		SecretKeySpec signingKey = new SecretKeySpec(key.getBytes(), HMAC_SHA1_ALGORITHM);
		Mac mac = Mac.getInstance(HMAC_SHA1_ALGORITHM);
		mac.init(signingKey);
		return mac.doFinal(data.getBytes());
	}

	public static String sign(String key, String secret, String method, String uri, String date, String policy,
			String md5) throws Exception {
		String value = method + "&" + uri + "&" + date;
		if (policy != "") {
			value = value + "&" + policy;
		}
		if (md5 != "") {
			value = value + "&" + md5;
		}
		byte[] hmac = hashHmac(value, secret);
		String sign = Base64.getEncoder().encodeToString(hmac);
		return "UPYUN " + key + ":" + sign;
	}

}
```



### RMI远程调用

- Java提供了RMI实现远程方法调用：
- RMI通过自动生成stub和skeleton实现网络调用，客户端只需要查找服务并获得接口实例，服务器端只需要编写实现类并注册为服务；
- RMI的序列化和反序列化可能会造成安全漏洞，因此调用双方必须是内网互相信任的机器，不要把1099端口暴露在公网上作为对外服务。

RMI是Remote Method Invocation的缩写，是指一个JVM中的代码可以通过网络实现远程调用另一个JVM的某个方法。要实现RMI服务端和客户端必须共享同一接口，此接口必须派生自`java.rmi.Remote`，并在每个方法声明抛出`RemoteException`。

```java
public interface WorldClock extends Remote {
    LocalDateTime getLocalDateTime(String zoneId) throws RemoteException;
}
```

服务器实现类：

```java
public class WorldClockService implements WorldClock {
    @Override
    public LocalDateTime getLocalDateTime(String zoneId) throws RemoteException {
        return LocalDateTime.now(ZoneId.of(zoneId)).withNano(0);
    }
}
```

通过Java RMI提供的一系列底层接口把编写的服务以RMI形式暴露在网络上给客户端调用：

```java
public class Server {
    public static void main(String[] args) throws RemoteException {
        System.out.println("create World clock remote service...");
        // 实例化一个WorldClock:
        WorldClock worldClock = new WorldClockService();
        // 将此服务转换为远程服务接口:
        WorldClock skeleton = (WorldClock) UnicastRemoteObject.exportObject(worldClock, 0);
        // 将RMI服务注册到1099端口:
        Registry registry = LocateRegistry.createRegistry(1099);
        // 注册此服务，服务名为"WorldClock":
        registry.rebind("WorldClock", skeleton);
    }
}
```

将`WorldClock.java`这个接口文件复制到客户端实现RMI调用：

```java
public class Client {
    public static void main(String[] args) throws RemoteException, NotBoundException {
        // 连接到服务器localhost，端口1099:
        Registry registry = LocateRegistry.getRegistry("localhost", 1099);
        // 查找名称为"WorldClock"的服务并强制转型为WorldClock接口:
        WorldClock worldClock = (WorldClock) registry.lookup("WorldClock");
        // 正常调用接口方法:
        LocalDateTime now = worldClock.getLocalDateTime("Asia/Shanghai");
        // 打印调用结果:
        System.out.println(now);
    }
}
```

把客户端的“实现类”称为`stub`，而服务器端的网络服务类称为`skeleton`，是由`Registry`内部动态生成的，整个过程由RMI底层负责实现序列化和反序列化，可能会造成严重的安全漏洞，调用双方必须是内网互相信任的机器，不要把默认的1099端口暴露在公网对外服务，RMI双方必须是JAVA程序，不同语言进行RPC调用可选择通用协议[gRPC](https://grpc.io/)

```ascii
┌ ─ ─ ─ ─ ─ ─ ─ ─ ┐         ┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
  ┌─────────────┐                                 ┌─────────────┐
│ │   Service   │ │         │                     │   Service   │ │
  └─────────────┘                                 └─────────────┘
│        ▲        │         │                            ▲        │
         │                                               │
│        │        │         │                            │        │
  ┌─────────────┐   Network   ┌───────────────┐   ┌─────────────┐
│ │ Client Stub ├─┼─────────┼>│Server Skeleton│──>│Service Impl │ │
  └─────────────┘             └───────────────┘   └─────────────┘
└ ─ ─ ─ ─ ─ ─ ─ ─ ┘         └ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘
```

## XML与JSON

- XML使用嵌套结构的数据表示方式，支持格式验证；
- XML常用于配置文件、网络消息传输等。

**XML结构**

首行必定是声明版本、编码，接着声明文档定义类型，然后是XML文档内容，有且只有一个根元素，根元素可以包含任意子元素，子元素可以包含属性且必须正确嵌套，空元素用`<tag/>`表示。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE note SYSTEM "book.dtd">
<book id="1">
    <name>Java核心技术</name>
    <author>Cay S. Horstmann</author>
    <isbn lang="CN">1234567</isbn>
    <tags>
        <tag>Java</tag>
        <tag>Network</tag>
    </tags>
    <pubDate/>
</book>
```

特殊符号需要转义：

| 字符 | 表示    |
| :--- | :------ |
| <    | & lt;   |
| >    | & gt;   |
| &    | & amp;  |
| "    | & quot; |
| '    | & apos; |

DTD文档可以指定一系列规则，例如：

- 根元素必须是`book`
- `book`元素必须包含`name`，`author`等指定元素
- `isbn`元素必须包含属性`lang`

XML是一个技术体系，除了我们经常用到的XML文档本身外，XML还支持：

- DTD和XSD：验证XML结构和数据是否有效；
- Namespace：XML节点和属性的名字空间；
- XSLT：把XML转化为另一种文本；
- XPath：一种XML节点查询语言；

### 使用DOM

- Java提供的DOM API可以将XML解析为DOM结构，以Document对象表示；
- DOM可在内存中完整表示XML数据结构；
- DOM解析速度慢，内存占用大。

XML是一种树形结构的文档，标准解析API：

- DOM：从根节点开始一次性读取XML，并在内存中表示为树形结构；
- SAX：以流的形式读取XML，使用事件回调。

上面的XML结构解析为DOM结构：

```ascii
                      ┌─────────┐
                      │document │
                      └─────────┘
                           │
                           ▼
                      ┌─────────┐
                      │  book   │
                      └─────────┘
                           │
     ┌──────────┬──────────┼──────────┬──────────┐
     ▼          ▼          ▼          ▼          ▼
┌─────────┐┌─────────┐┌─────────┐┌─────────┐┌─────────┐
│  name   ││ author  ││  isbn   ││  tags   ││ pubDate │
└─────────┘└─────────┘└─────────┘└─────────┘└─────────┘
                                      │
                                 ┌────┴────┐
                                 ▼         ▼
                             ┌───────┐ ┌───────┐
                             │  tag  │ │  tag  │
                             └───────┘ └───────┘
```

Java提供DOM API解析XML：

- Document：代表整个XML文档；
- Element：代表一个XML元素；
- Attribute：代表一个元素的某个属性。

`DocumentBuilder.parse()`用于解析一个XML，它可以接收InputStream，File或者URL，如果解析无误将获得一个Document对象。

```java
InputStream input = Main.class.getResourceAsStream("/book.xml");
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
DocumentBuilder db = dbf.newDocumentBuilder();
Document doc = db.parse(input);
```

这个对象代表了整个XML文档的树形结构，需要遍历以便读取指定元素的值：

```java
void printNode(Node n, int indent) {
    for (int i = 0; i < indent; i++) {
        System.out.print(' ');
    }
    switch (n.getNodeType()) {
    case Node.DOCUMENT_NODE: // Document节点
        System.out.println("Document: " + n.getNodeName());
        break;
    case Node.ELEMENT_NODE: // 元素节点
        System.out.println("Element: " + n.getNodeName());
        break;
    case Node.TEXT_NODE: // 文本
        System.out.println("Text: " + n.getNodeName() + " = " + n.getNodeValue());
        break;
    case Node.ATTRIBUTE_NODE: // 属性
        System.out.println("Attr: " + n.getNodeName() + " = " + n.getNodeValue());
        break;
    default: // 其他
        System.out.println("NodeType: " + n.getNodeType() + ", NodeName: " + n.getNodeName());
    }
    for (Node child = n.getFirstChild(); child != null; child = child.getNextSibling()) {
        printNode(child, indent + 1);
    }
}
```



### 使用SAX

- SAX是一种流式解析XML的API；
- SAX通过事件触发，读取速度快，消耗内存少；
- 调用方必须通过回调方法获得解析过程中的数据。

使用DOM解析XML优点是用起来省事，主要缺点是内存占用太大。另一种解析XML的方式SAX是Simple API for XML的缩写，基于流的解析方式边读取XML边解析并以事件回调的方式让调用者获取数据。

SAX解析会触发一系列事件：

- startDocument：开始读取XML文档；
- startElement：读取到了一个元素，例如`<book>`；
- characters：读取到了字符；
- endElement：读取到了一个结束的元素，例如`</book>`；
- endDocument：读取XML文档结束。

用SAX API解析XML：

```java
InputStream input = Main.class.getResourceAsStream("/book.xml");
SAXParserFactory spf = SAXParserFactory.newInstance();
SAXParser saxParser = spf.newSAXParser();
saxParser.parse(input, new MyHandler());
```

传入回调对象`MyHandler`继承自`DefaultHandler`：

```java
class MyHandler extends DefaultHandler {
    public void startDocument() throws SAXException {
        print("start document");
    }

    public void endDocument() throws SAXException {
        print("end document");
    }

    public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
        print("start element:", localName, qName);
    }

    public void endElement(String uri, String localName, String qName) throws SAXException {
        print("end element:", localName, qName);
    }

    public void characters(char[] ch, int start, int length) throws SAXException {
        print("characters:", new String(ch, start, length));
    }

    public void error(SAXParseException e) throws SAXException {
        print("error:", e);
    }

    void print(Object... objs) {
        for (Object obj : objs) {
            System.out.print(obj);
            System.out.print(" ");
        }
        System.out.println();
    }
}
```



### 使用Jackson

使用Jackson解析XML，可以直接把XML解析为JavaBean。

**添加Maven依赖**

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-xml -->
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
    <version>2.11.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.codehaus.woodstox/woodstox-core-asl -->
<dependency>
    <groupId>org.codehaus.woodstox</groupId>
    <artifactId>woodstox-core-asl</artifactId>
    <version>4.4.1</version>
</dependency>

```

定义JavaBean通过Jackson创建`XmlMapper`调用`readValue(InputStream, Class)`解析XML返回一个JavaBean：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<book id="1">
    <name>Java核心技术</name>
    <author>Cay S. Horstmann</author>
    <isbn lang="CN">1234567</isbn>
    <tags>
        <tag>Java</tag>
        <tag>Network</tag>
    </tags>
    <pubDate/>
</book>
```

```java
public class Book {
    public long id;
    public String name;
    public String author;
    public String isbn;
    public List<String> tags;
    public String pubDate;
}

InputStream input = Main.class.getResourceAsStream("/book.xml");
JacksonXmlModule module = new JacksonXmlModule();
XmlMapper mapper = new XmlMapper(module);
Book book = mapper.readValue(input, Book.class);
System.out.println(book.id);
System.out.println(book.name);
System.out.println(book.author);
System.out.println(book.isbn);
System.out.println(book.tags);
System.out.println(book.pubDate);
```



### 使用JSON

- JSON是轻量级的数据表示方式，常用于Web应用；
- Jackson可以实现JavaBean和JSON之间的转换；
- 可以通过Module扩展Jackson能处理的数据类型；
- 可以自定义`JsonSerializer`和`JsonDeserializer`来定制序列化和反序列化。

JSON是JavaScript Object Notation的缩写，去除了JavaScript执行代码保留对象格式。逐渐代替了标签繁琐，格式复杂的XML。JSON的显著优点：

- JSON只允许使用UTF-8编码，不存在编码问题；
- JSON只允许使用双引号作为key，特殊字符用`\`转义，格式简单；
- 浏览器内置JSON支持，如果把数据用JSON发送给浏览器，可以用JavaScript直接处理。

仅支持以下几种数据类型：

- 键值对：`{"key": value}`
- 数组：`[1, 2, 3]`
- 字符串：`"abc"`
- 数值（整数和浮点数）：`12.34`
- 布尔值：`true`或`false`
- 空值：`null`

浏览器支持使用JavaScript对JSON进行读写：

```java
// JSON string to JavaScript object:
jsObj = JSON.parse(jsonStr);
// JavaScript object to JSON string:
jsonStr = JSON.stringify(jsObj);
```

在Java中对JSON也有标准的JSR 353 API，XML可以通过Jackson和JavaBean相互转换，JSON和JavaBean相互转换也可以通过第三方库Jackson实现，或其他第三方库Gson、Fastjson。

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.11.2</version>
</dependency>
```

创建一个`ObjectMapper`对象，关闭`DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES`功能使得解析时如果JavaBean不存在该属性时解析不会报错。

```java
InputStream input = Main.class.getResourceAsStream("/book.json");
ObjectMapper mapper = new ObjectMapper();
// 反序列化时忽略不存在的JavaBean属性:
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
Book book = mapper.readValue(input, Book.clas);
```

把JavaBean解析为JSON为序列化：

```java
String json = mapper.writeValueAsString(book);
```

要把JSON的某些值解析为特定的JAVA对象，如`LocalDate`：

```json
{
    "name": "Java核心技术",
    "pubDate": "2016-09-01"
}
```

解析为：

```java
public class Book {
    public String name;
    public LocalDate pubDate;
}
```

需要进入标准库的JSR 310关于JavaTime的数据格式定义至Maven，然后在创建`ObjectMapper`时注册一个新的`JavaTimeModule`：

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.datatype/jackson-datatype-jsr310 -->
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
    <version>2.11.2</version>
</dependency>

```

```java
ObjectMapper mapper = new ObjectMapper().registerModule(new JavaTimeModule());
```

如果内置的解析规则和扩展的解析规则都不满足需求可自定义解析：

如`Book`类的`isbn`是一个`BigInteger`：

```java
public class Book {
	public String name;
	public BigInteger isbn;
}
```

但JSON数据并不是标准的整型格式：

```json
{
    "name": "Java核心技术",
    "isbn": "978-7-111-54742-6"
}
```

直接解析报错，需定义一个`IsbnDeserializer`，用于解析含有非数字的字符串：

```java
public class IsbnDeserializer extends JsonDeserializer<BigInteger> {
    public BigInteger deserialize(JsonParser p, DeserializationContext ctxt) throws IOException, JsonProcessingException {
        // 读取原始的JSON字符串内容:
        String s = p.getValueAsString();
        if (s != null) {
            try {
                return new BigInteger(s.replace("-", ""));
            } catch (NumberFormatException e) {
                throw new JsonParseException(p, s, e);
            }
        }
        return null;
    }
}
```

然后在`Book`类中使用注解标注：

```java
public class Book {
    public String name;
    // 表示反序列化isbn时使用自定义的IsbnDeserializer:
    @JsonDeserialize(using = IsbnDeserializer.class)
    public BigInteger isbn;
}
```

## JDBC编程

### JDBC简介

JDBC是Java DataBase Connectivity的缩写，它是Java程序访问数据库的标准接口。

- 各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发；
- Java程序编译期仅依赖java.sql包，不依赖具体数据库的jar包；
- 可随时替换底层数据库，访问数据库的Java代码基本不变。

**关系数据库**

#### 安装Mysql≥5.5.3

在Mac或Linux上，需要编辑MySQL的配置文件，把数据库默认的编码全部改为UTF-8。MySQL的配置文件默认存放在`/etc/my.cnf`或者`/etc/mysql/my.cnf`：

```java
[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci

# 重启后通过命令查看编码
mysql> show variables like '%char%';
```

**NoSQL**

**JDBC**

Java标准库自带的JDBC接口其实就是定义了一组接口，而某个具体的JDBC驱动其实就是实现了这些接口的类：

```ascii
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐

│  ┌───────────────┐  │
   │   Java App    │
│  └───────────────┘  │
           │
│          ▼          │
   ┌───────────────┐
│  │JDBC Interface │<─┼─── JDK
   └───────────────┘
│          │          │
           ▼
│  ┌───────────────┐  │
   │ MySQL Driver  │<───── Oracle
│  └───────────────┘  │
           │
└ ─ ─ ─ ─ ─│─ ─ ─ ─ ─ ┘
           ▼
   ┌───────────────┐
   │     MySQL     │
   └───────────────┘
```

一个MySQL的JDBC的驱动就是一个jar包，它本身也是纯Java编写的。我们自己编写的代码只需要引用Java标准库提供的java.sql包下面的相关接口，由此再间接地通过MySQL驱动的jar包通过网络访问MySQL服务器，所有复杂的网络通讯都被封装到JDBC驱动中，因此，Java程序本身只需要引入一个MySQL驱动的jar包就可以正常访问MySQL服务器：

```ascii
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
   ┌───────────────┐
│  │   App.class   │  │
   └───────────────┘
│          │          │
           ▼
│  ┌───────────────┐  │
   │  java.sql.*   │
│  └───────────────┘  │
           │
│          ▼          │
   ┌───────────────┐     TCP    ┌───────────────┐
│  │ mysql-xxx.jar │──┼────────>│     MySQL     │
   └───────────────┘            └───────────────┘
└ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘
          JVM
```

### JDBC查询

- JDBC接口的`Connection`代表一个JDBC连接；
- 使用JDBC查询时，总是使用`PreparedStatement`进行查询而不是`Statement`；
- 查询结果总是`ResultSet`，即使使用聚合查询也不例外。

Java的标准库`java.sql`大部分都是接口，接口不能直接实例化而是必须实例化对应的实现类，通过JDBC驱动实现JDBC接口。当选择MySQL 5.x.x数据库要找到JDBC的驱动其实就是一个JAR包，通过Maven添加`scope`是`runtime`的依赖，只有在运行期间才使用。

```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
    <scope>runtime</scope>
</dependency>
```

通过脚本插入数据：

```java
-- 创建数据库learjdbc:
DROP DATABASE IF EXISTS learnjdbc;
CREATE DATABASE learnjdbc;

-- 创建登录用户learn/口令learnpassword
CREATE USER IF NOT EXISTS learn@'%' IDENTIFIED BY 'learnpassword';
GRANT ALL PRIVILEGES ON learnjdbc.* TO learn@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;

-- 创建表students:
USE learnjdbc;
CREATE TABLE students (
  id BIGINT AUTO_INCREMENT NOT NULL,
  name VARCHAR(50) NOT NULL,
  gender TINYINT(1) NOT NULL,
  grade INT NOT NULL,
  score INT NOT NULL,
  PRIMARY KEY(id)
) Engine=INNODB DEFAULT CHARSET=UTF8;

-- 插入初始数据:
INSERT INTO students (name, gender, grade, score) VALUES ('小明', 1, 1, 88);
INSERT INTO students (name, gender, grade, score) VALUES ('小红', 1, 1, 95);
INSERT INTO students (name, gender, grade, score) VALUES ('小军', 0, 1, 93);
INSERT INTO students (name, gender, grade, score) VALUES ('小白', 0, 1, 100);
INSERT INTO students (name, gender, grade, score) VALUES ('小牛', 1, 2, 96);
INSERT INTO students (name, gender, grade, score) VALUES ('小兵', 1, 2, 99);
INSERT INTO students (name, gender, grade, score) VALUES ('小强', 0, 2, 86);
INSERT INTO students (name, gender, grade, score) VALUES ('小乔', 0, 2, 79);
INSERT INTO students (name, gender, grade, score) VALUES ('小青', 1, 3, 85);
INSERT INTO students (name, gender, grade, score) VALUES ('小王', 1, 3, 90);
INSERT INTO students (name, gender, grade, score) VALUES ('小林', 0, 3, 91);
INSERT INTO students (name, gender, grade, score) VALUES ('小贝', 0, 3, 77);
```

#### JDBC连接

Connection代表一个JDBC连接，它相当于Java程序到数据库的连接（通常是TCP连接）。打开一个Connection时，需要准备URL、用户名和口令才能成功连接到数据库。

URL是由数据库厂商指定的格式，例如，MySQL的URL是：

```java
jdbc:mysql://<hostname>:<port>/<db>?key1=value1&key2=value2
jdbc:mysql://localhost:3306/learnjdbc?useSSL=false&characterEncoding=utf8
```

```java
// JDBC连接的URL, 不同数据库有不同的格式:
String JDBC_URL = "jdbc:mysql://localhost:3306/test";
String JDBC_USER = "root";
String JDBC_PASSWORD = "password";
// 获取连接:
Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD);
// TODO: 访问数据库...
// 关闭连接:
conn.close();
```

#### JDBC查询

1. 通过`Connection`提供的`createStatement()`方法创建一个`Statement`对象，用于执行一个查询；
2. 执行`Statement`对象提供的`executeQuery("SELECT * FROM students")`并传入SQL语句，执行查询并获得返回的结果集，使用`ResultSet`来引用这个结果集；
3. 反复调用`ResultSet`的`next()`方法并读取每一行结果。

`Statment`和`ResultSet`都是需要关闭的资源，因此嵌套使用`try (resource)`确保及时关闭；`rs.next()`用于判断是否有下一行记录，如果有，将自动把当前行移动到下一行；`ResultSet`获取列时，索引从`1`开始而不是`0`；必须根据`SELECT`的列的对应位置的数据类型来调用`getLong(1)`，`getString(2)`这些方法。

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {
    try (Statement stmt = conn.createStatement()) {
        try (ResultSet rs = stmt.executeQuery("SELECT id, grade, name, gender FROM students WHERE gender=1")) {
            while (rs.next()) {	//
                long id = rs.getLong(1); // 注意：索引从1开始
                long grade = rs.getLong(2);
                String name = rs.getString(3);
                int gender = rs.getInt(4);
            }
        }
    }
}
```

#### SQL注入

使用`Statement`拼字符串非常容易引发SQL注入，因为SQL参数是从方法传入的。

```java
User login(String name, String pass) {
	    ...
    stmt.executeQuery("SELECT * FROM user WHERE login='" + name + "' AND pass='" + pass + "'");
    ...
}
```

`name`和`pass`通常是Web页面输入后由程序接收的，如果用户输入是程序期待的值就可以拼出正确的SQL。但是如果用户输入是精心构造的字符串就可以拼出意想不到的SQL。

```java
SELECT * FROM user WHERE login='bob' OR pass=' AND pass=' OR pass=''
```

这样就不用判断口令是否正确，登录形同虚设。要避免SQL注入攻击，可以针对所有字符串参数进行转义，但很麻烦，而且需要在任何使用SQL的地方增加转义代码。还有一种办法就是使用`PreparedStatement`可以完全避免SQL注入的问题，因为`PreparedStatement`始终使用`?`作为占位符，并且把数据连同SQL本身传给数据库，这样可以保证每次传给数据库的SQL是相同的，只是占位符的数据不同，还能高效利用数据库本身对查询的缓存，所以，`PreparedStatement`比`Statement`更安全，而且更快。改写如下：

```java
User login(String name, String pass) {
    ...
    String sql = "SELECT * FROM user WHERE login=? AND pass=?";
    PreparedStatement ps = conn.prepareStatement(sql);
    ps.setObject(1, name);
    ps.setObject(2, pass);
    ...
}
```

使用`PreparedStatement`和`Statement`稍有不同，必须首先调用`setObject()`设置每个占位符`?`的值，最后获取的仍然是`ResultSet`对象。从结果集读取列时，使用`String`类型的列名比索引要易读，而且不易出错。

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {
    try (PreparedStatement ps = conn.prepareStatement("SELECT id, grade, name, gender FROM students WHERE gender=? AND grade=?")) {
        ps.setObject(1, "M"); // 注意：索引从1开始
        ps.setObject(2, 3);
        try (ResultSet rs = ps.executeQuery()) {
            while (rs.next()) {
                long id = rs.getLong("id");
                long grade = rs.getLong("grade");
                String name = rs.getString("name");
                String gender = rs.getString("gender");
            }
        }
    }
}
```

#### 数据类型

JDBC在`java.sql.Types`定义了一组常量来表示如何映射SQL数据类型：

| SQL数据类型   | Java数据类型             |
| :------------ | :----------------------- |
| BIT, BOOL     | boolean                  |
| INTEGER       | int                      |
| BIGINT        | long                     |
| REAL          | float                    |
| FLOAT, DOUBLE | double                   |
| CHAR, VARCHAR | String                   |
| DECIMAL       | BigDecimal               |
| DATE          | java.sql.Date, LocalDate |
| TIME          | java.sql.Time, LocalTime |

### JDBC更新

- 使用JDBC执行`INSERT`、`UPDATE`和`DELETE`都可视为更新操作；
- 更新操作使用`PreparedStatement`的`executeUpdate()`进行，返回受影响的行数。

增删改查，行话叫CRUD：Create，Retrieve，Update和Delete。查询可以使用`PreparedStatement`进行各种`SELECT`然后处理结果集。

#### 插入

设置参数和查询一样，有几个`?`占位符就必须要设置对应的参数，当成功执行`executeUpdate()`后，返回值是`int`表示插入的记录数量。此出因为只插入了一条记录返回`1`。

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {
    try (PreparedStatement ps = conn.prepareStatement(
            "INSERT INTO students (id, grade, name, gender) VALUES (?,?,?,?)")) {
        ps.setObject(1, 999); // 注意：索引从1开始
        ps.setObject(2, 1); // grade
        ps.setObject(3, "Bob"); // name
        ps.setObject(4, "M"); // gender
        int n = ps.executeUpdate(); // 1
    }
}
```



#### 插入并获取主键

对于使用自增主键的程序需要获取插入后自增主键的值，要获取自增主键不能先插入再查询，因为两条SQL执行期间可能有别的程序也插入了同一个表，获取自增主键的正确写法是在创建`PreparedStatement`的时候，指定一个`RETURN_GENERATED_KEYS`标志位，表示JDBC驱动必须返回插入的自增主键。

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {
    try (PreparedStatement ps = conn.prepareStatement(
            "INSERT INTO students (grade, name, gender) VALUES (?,?,?)",
            Statement.RETURN_GENERATED_KEYS)) {
        ps.setObject(1, 1); // grade
        ps.setObject(2, "Bob"); // name
        ps.setObject(3, "M"); // gender
        int n = ps.executeUpdate(); // 1
        try (ResultSet rs = ps.getGeneratedKeys()) {
            if (rs.next()) {
                long id = rs.getLong(1); // 注意：索引从1开始
            }
        }
    }
}
```



#### 更新

更新操作是`UPDATE`语句，它可以一次更新若干列的记录。`executeUpdate()`返回数据库实际更新的行数，返回结果可能是正数，也可能是0表示没有更新。

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {
    try (PreparedStatement ps = conn.prepareStatement("UPDATE students SET name=? WHERE id=?")) {
        ps.setObject(1, "Bob"); // 注意：索引从1开始
        ps.setObject(2, 999);
        int n = ps.executeUpdate(); // 返回更新的行数
    }
}
```



#### 删除

删除操作是`DELETE`语句，它可以一次删除若干列。

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {
    try (PreparedStatement ps = conn.prepareStatement("DELETE FROM students WHERE id=?")) {
        ps.setObject(1, 999); // 注意：索引从1开始
        int n = ps.executeUpdate(); // 删除的行数
    }
}
```



### JDBC事物

数据库事务（Transaction）是由若干个SQL语句构成的一个操作序列，有点类似于Java的`synchronized`同步。数据库系统保证在一个事务中的所有SQL要么全部执行成功，要么全部不执行。

数据库事务（Transaction）具有ACID特性：

- Atomicity：原子性
- Consistency：一致性
- Isolation：隔离性
- Durability：持久性

JDBC提供了事务的支持，使用Connection可以开启、提交或回滚事务。

对应用程序来说，数据库事务非常重要，很多运行着关键任务的应用程序，都必须依赖数据库事务保证程序的结果正常。假设小明准备给小红支付100，两人在数据库中的记录主键分别是`123`和`456`，那么用两条SQL语句操作如下：

```sql
UPDATE accounts SET balance = balance - 100 WHERE id=123 AND balance >= 100;
UPDATE accounts SET balance = balance + 100 WHERE id=456;
```

这两条语句必须以事物的方式执行才能保证业务正确，因为一旦第一条SQL执行成功而第二条SQL失败的话，系统的钱就会凭空减少100，而有了事务要么这笔转账成功，要么转账失败双方账户的钱都不变。

在JDBC执行事物就是把多条SQL包裹在一个数据库事物中心执行。

```java
Connection conn = openConnection();
try {
    // 关闭自动提交:
    conn.setAutoCommit(false);
    // 执行多条SQL语句:
    insert(); update(); delete();
    // 提交事务可能失败抛出异常:
    conn.commit();
} catch (SQLException e) {
    // 回滚事务，把Connection对象状态恢复到初始值:
    conn.rollback();
} finally {
    conn.setAutoCommit(true);
    conn.close();
}
```

默认有种“隐式事务”自动提交。只要关闭了`Connection`的`autoCommit`，那么就可以在一个事务中执行多条语句，事务以`commit()`方法结束。

**设定事务的隔离级别：**

```java
// 设定隔离级别为READ COMMITTED:
conn.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
```

### JDBC Batch

使用JDBC的batch操作会大大提高执行效率，对内容相同，参数不同的SQL，要优先考虑batch操作。

使用JDBC操作数据库经常会执行一些批量操作，只有占位符参数不同，SQL实际是一样的，可以通过循环执行每个`PreparedStatement`：

```java
INSERT INTO coupons (user_id, type, expires) VALUES (123, 'DISCOUNT', '2030-12-31');
INSERT INTO coupons (user_id, type, expires) VALUES (234, 'DISCOUNT', '2030-12-31');
INSERT INTO coupons (user_id, type, expires) VALUES (345, 'DISCOUNT', '2030-12-31');
INSERT INTO coupons (user_id, type, expires) VALUES (456, 'DISCOUNT', '2030-12-31');
...
for (var params : paramsList) {
    PreparedStatement ps = conn.preparedStatement("INSERT INTO coupons (user_id, type, expires) VALUES (?,?,?)");
    ps.setLong(params.get(0));
    ps.setString(params.get(1));
    ps.setString(params.get(2));
    ps.executeUpdate();
}
// 类似还有UPDATE employees SET salary = salary * ? WHERE id = ?
```

虽然可行但性能很低，通过batch执行速度远远快于循环执行每个SQL：

```java
try (PreparedStatement ps = conn.prepareStatement("INSERT INTO students (name, gender, grade, score) VALUES (?, ?, ?, ?)")) {
    // 对同一个PreparedStatement反复设置参数并调用addBatch():
    for (Student s : students) {
        ps.setString(1, s.name);
        ps.setBoolean(2, s.gender);
        ps.setInt(3, s.grade);
        ps.setInt(4, s.score);
        ps.addBatch(); // 添加到batch
    }
    // 执行batch:
    int[] ns = ps.executeBatch();
    for (int n : ns) {
        System.out.println(n + " inserted."); // batch中每个SQL执行的结果数量
    }
}
```



### JDBC连接池

- 数据库连接池是一种复用`Connection`的组件，它可以避免反复创建新连接，提高JDBC代码的运行效率；
- 可以配置连接池的详细参数并监控连接池。

在执行JDBC的增删改查操作时，如果每次操作都打开，关闭连接会消耗大量的系统资源，可以通过连接池复用已经创建好的连接。JDBC连接池有一个标准的接口`javax.sql.DataSource`，要使用JDBC连接池必须选择一个JDBC连接池的实现。常用的JDBC连接池有：

- HikariCP
- C3P0
- BoneCP（使用最广泛）
- Druid

添加依赖：

```xml
<!-- https://mvnrepository.com/artifact/com.zaxxer/HikariCP -->
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>3.4.5</version>
</dependency>
```

创建一个`DataSource`实例表示连接池：

```java
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:mysql://localhost:3306/test");
config.setUsername("root");
config.setPassword("password");
config.addDataSourceProperty("connectionTimeout", "1000"); // 连接超时：1秒
config.addDataSourceProperty("idleTimeout", "60000"); // 空闲超时：60秒
config.addDataSourceProperty("maximumPoolSize", "10"); // 最大连接数：10
// 占用资源大，一般作为全局变量，贯穿整个生命周期
DataSource ds = new HikariDataSource(config);
```

使用连接池：

```java
try (Connection conn = ds.getConnection()) { // 在此获取连接
    ...
} // 在此“关闭”连接
```

通过连接池获取连接时，并不需要指定JDBC的相关URL、用户名、口令等信息，因为这些信息已经存储在连接池内部了（创建`HikariDataSource`时传入的`HikariConfig`持有这些信息）。一开始，连接池内部并没有连接，所以，第一次调用`ds.getConnection()`，会迫使连接池内部先创建一个`Connection`，再返回给客户端使用。当我们调用`conn.close()`方法时（`在try(resource){...}`结束处），不是真正“关闭”连接，而是释放到连接池中，以便下次获取连接时能直接返回。

因此，连接池内部维护了若干个`Connection`实例，如果调用`ds.getConnection()`，就选择一个空闲连接，并标记它为“正在使用”然后返回，如果对`Connection`调用`close()`，那么就把连接再次标记为“空闲”从而等待下次调用。这样一来，我们就通过连接池维护了少量连接，但可以频繁地执行大量的SQL语句。

通常连接池提供了大量的参数可以配置，例如，维护的最小、最大活动连接数，指定一个连接在空闲一段时间后自动关闭等，需要根据应用程序的负载合理地配置这些参数。此外，大多数连接池都提供了详细的实时状态以便进行监控。

## 函数式编程

函数时一种最基本的任务，大型程序就是一个顶层函数调用若干底层函数，被调用的函数又可以调用其他函数。是面向过程程序设计的基本单元。

### Lambda基础

- 单方法接口被称为`FunctionalInterface`。
- 接收`FunctionalInterface`作为参数的时候，可以把实例化的匿名类改写为Lambda表达式，能大大简化代码。
- Lambda表达式的参数和返回值均可由编译器自动推断。

函数式编程（Functional Programming）是把函数作为基本运算单元，函数可以作为变量，可以接收函数，还可以返回函数。历史上研究函数式编程的理论是Lambda演算，所以把支持函数式编程的编码风格称为Lambda表达式。

#### Lambda表达式

在Java程序中经常会遇到一些单方法接口：

- Comparator
- Runnable
- Callable

以`Comparator`为例，调用`Arrays.sort()`时，可以传入一个`Comparator`实例，以匿名类方式编写如下：

```java
String[] array = ...
Arrays.sort(array, new Comparator<String>() {
    public int compare(String s1, String s2) {
        return s1.compareTo(s2);
    }
});
```

上面的写法非常繁琐，Java8可以用Lambda表达式替换单方法接口。改写如下：

```java
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
        Arrays.sort(array, (s1, s2) -> {
            return s1.compareTo(s2);
        });
        System.out.println(String.join(", ", array));
    }
}
```

其中参数是`(s1, s2)`，参数类型可以省略，因为编译器可以自动推断出`String`类型。`-> { ... }`表示方法体，所有代码写在内部即可。Lambda表达式没有`class`定义，因此写法非常简洁。

如果只有一行`return xxx`的代码，完全可以用更简单的写法：

```java
Arrays.sort(array, (s1, s2) -> s1.compareTo(s2));
```

返回值的类型也是由编译器自动推断的，这里推断返回`int`，因此只要返回`int`就不会报错。

#### FunctionalInterface

把只定义了单方法的接口称之为`FunctionalInterface`，用注解`@FunctionalInterface`标记。例如，`Callable`接口：

```java
@FunctionalInterface
public interface Callable<V> {
    V call() throws Exception;
}
```

而`Comparator`接口有很多方法，但只有一个抽象方法`int compare(T o1, T o2)`，其他的方法都是`default`方法或`static`方法。另外注意到`boolean equals(Object obj)`是`Object`定义的方法，不算在接口方法内。因此，`Comparator`也是一个`FunctionalInterface`。

```java
@FunctionalInterface
public interface Comparator<T> {

    int compare(T o1, T o2);

    boolean equals(Object obj);

    default Comparator<T> reversed() {
        return Collections.reverseOrder(this);
    }

    default Comparator<T> thenComparing(Comparator<? super T> other) {
        ...
    }
    ...
}
```



### 方法引用

`FunctionalInterface`允许传入：

- 接口的实现类（传统写法，代码较繁琐）；
- Lambda表达式（只需列出参数名，由编译器推断类型）；
- 符合方法签名的静态方法；
- 符合方法签名的实例方法（实例类型被看做第一个参数类型）；
- 符合方法签名的构造方法（实例类型被看做返回类型）。

`FunctionalInterface`不强制继承关系，不需要方法名称相同，只要求方法参数（类型和数量）与方法返回类型相同，即认为方法签名相同。

#### 静态方法引用

使用Lambda表达式就可以不必编写`FunctionalInterface`接口的实现类，从而简化代码：

```java
Arrays.sort(array, (s1, s2) -> {
    return s1.compareTo(s2);
});
```

因为`Comparator<String>`接口定义的方法是`int compare(String, String)`，和静态方法`int cmp(String, String)`相比，除了方法名外，方法参数一致，返回类型相同。因此如果某个方法签名和接口恰好一致还可以直接传入方法引用：

```java
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
        Arrays.sort(array, Main::cmp);	// 直接把方法名作为Lambda表达式传入
        System.out.println(String.join(", ", array));
    }

    static int cmp(String s1, String s2) {
        return s1.compareTo(s2);
    }
}
```

#### 实例方法引用

```java
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
        Arrays.sort(array, String::compareTo);
        System.out.println(String.join(", ", array));
    }
}
```

`String.compareTo()`的方法定义，这个方法的签名只有一个参数，为什么和`int Comparator<String>.compare(String, String)`能匹配呢？

```java
public final class String {
    public int compareTo(String o) {
        ...
    }
}
```

因为实例方法有一个隐含的`this`参数，`String`类的`compareTo()`方法在实际调用的时候，第一个隐含参数总是传入`this`，相当于静态方法：

```java
public static int compareTo(this, String o);
```

#### 构造方法引用

如果要把一个`List<String>`转换为`List<Person>`，应该怎么办？

```java
class Person {
    String name;
    public Person(String name) {
        this.name = name;
    }
}

List<String> names = List.of("Bob", "Alice", "Tim");
List<Person> persons = ???
```

传统做法是先定义一个`ArrayList<Person>`然后for循环填充这个`List`：

```java
List<String> names = List.of("Bob", "Alice", "Tim");
List<Person> persons = new ArrayList<>();
for (String name : names) {
    persons.add(new Person(name));
}
```

更简单地实现`String`到`Person`转换，可以引用`Person`的构造方法：

```java
// 引用构造方法
import java.util.*;
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        List<String> names = List.of("Bob", "Alice", "Tim");
        List<Person> persons = names.stream().map(Person::new).collect(Collectors.toList());
        System.out.println(persons);
    }
}

class Person {
    String name;
    public Person(String name) {
        this.name = name;
    }
    public String toString() {
        return "Person:" + this.name;
    }
}

```

这里`Stream`的`map()`方法要传入`FunctionalInterface`：

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

把泛型对应上就是方法签名`Person apply(String)`，即传入参数`String`，返回类型`Person`。而`Person`类的构造方法恰好满足这个条件，因为构造方法的参数是`String`，而构造方法虽然没有`return`语句，但它会隐式地返回`this`实例，类型就是`Person`，因此，此处可以引用构造方法。构造方法的引用写法是`类名::new`，因此，此处传入`Person::new`。

### 使用Stream

Stream API的特点是：

- Stream API提供了一套新的流式处理的抽象序列；
- Stream API支持函数式编程和链式操作；
- Stream可以表示无限序列，并且大多数情况下是惰性求值的。

从Java8开始还引入了全新的流式API：Stream API。它位于`java.util.stream`包中。不同于`java.io`的`InputStream`和`OutputStream`，它代表的是任意Java对象的序列。

| java.io | java.util.stream         |                            |
| :------ | :----------------------- | -------------------------- |
| 存储    | 顺序读写的`byte`或`char` | 顺序输出的任意Java对象实例 |
| 用途    | 序列化至文件或网络       | 内存计算／业务逻辑         |

这个`Stream`和`List`也不一样，`List`存储的每个元素都是已经存储在内存中的某个Java对象，而`Stream`输出的元素可能并没有预先存储在内存中，而是实时计算出来的。换句话说，`List`的用途是操作一组已存在的Java对象，而`Stream`实现的是惰性计算。

| java.util.List | java.util.stream         |                      |
| :------------- | :----------------------- | -------------------- |
| 元素           | 已分配并存储在内存       | 可能未分配，实时计算 |
| 用途           | 操作一组已存在的Java对象 | 惰性计算             |

 例如要表示一个全部自然数的集合，用`List`是不可能写出来的，因为自然数是无限的，内存再大也没法放到`List`中。但是用`Stream`可以做到。

```java
Stream<BigInteger> naturals = createNaturalStream();
```

先不考虑这个方法是如何实现的，对每个自然数做一个平方：

```java
Stream<BigInteger> streamNxN = naturals.map(n -> n.multiply(n));
```

`streamNxN`也有无限多个元素，要打印它，必须首先把无限多个元素变成有限个元素，可以用`limit()`方法截取前100个元素，最后用`forEach()`处理每个元素，这样，我们就打印出了前100个自然数的平方：

```java
Stream<BigInteger> naturals = createNaturalStream();
naturals.map(n -> n.multiply(n)) // 1, 4, 9, 16, 25...
        .limit(100)
        .forEach(System.out::println);
```

Stream API的基本用法就是：创建一个`Stream`，然后做若干次转换，最后调用一个求值方法获取真正计算的结果：

```java
int result = createNaturalStream() // 创建Stream
             .filter(n -> n % 2 == 0) // 任意个转换
             .map(n -> n * n) // 任意个转换
             .limit(100) // 任意个转换
             .sum(); // 最终计算结果
```

#### 创建Stream

创建`Stream`的方法有 ：

- 通过指定元素、指定数组、指定`Collection`创建`Stream`；
- 通过`Supplier`创建`Stream`，可以是无限序列；
- 通过其他类的相关方法创建。

基本类型的`Stream`有`IntStream`、`LongStream`和`DoubleStream`。

**Stream.of()**

创建`Stream`最简单的方式是直接用`Stream.of()`静态方法，传入可变参数即创建了一个能输出确定元素的`Stream`，没啥实质性用途，但测试的时候很方便。

```java
import java.util.stream.Stream;
public class Main {
    public static void main(String[] args) {
        Stream<String> stream = Stream.of("A", "B", "C", "D");
        // forEach()方法相当于内部循环调用，
        // 可传入符合Consumer接口的void accept(T t)的方法引用：
        stream.forEach(System.out::println);
    }
}
```

**基于数组或Collection**

第二种创建`Stream`的方法是基于一个数组或者`Collection`，这样该`Stream`输出的元素就是数组或者`Collection`持有的元素。把数组变成`Stream`使用`Arrays.stream()`方法。对于`Collection`（`List`、`Set`、`Queue`等），直接调用`stream()`方法就可以获得`Stream`。

```java
import java.util.*;
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        Stream<String> stream1 = Arrays.stream(new String[] { "A", "B", "C" });
        Stream<String> stream2 = List.of("X", "Y", "Z").stream();
        stream1.forEach(System.out::println);
        stream2.forEach(System.out::println);
    }
}
```

**基于Supplier**

创建`Stream`还可以通过`Stream.generate()`方法，它需要传入一个`Supplier`对象。

```java
Stream<String> s = Stream.generate(Supplier<String> sp);
```

不断调用`Supplier.get()`方法来不断产生下一个元素，这种`Stream`保存的不是元素而是算法，它可以用来表示无限序列。

```java
import java.util.function.*;
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        Stream<Integer> natual = Stream.generate(new NatualSupplier());
        // 注意：无限序列必须先变成有限序列再打印:
        natual.limit(20).forEach(System.out::println);
    }
}

class NatualSupplier implements Supplier<Integer> {
    int n = 0;
    public Integer get() {
        n++;
        return n;
    }
}

```

受`int`范围限制不是真的无限大，如果用`List`表示，即便在`int`范围内，也会占用巨大的内存，而`Stream`几乎不占用空间，因为每个元素都是实时计算出来的。对于无限序列直接调用求值操作会进入死循环，`limit()`方法可以截取前面若干个元素将无限序列变成有限序列，再调用`forEach()`或者`count()`操作。

**其他方法**

创建`Stream`的第三种方法是通过一些API提供的接口，直接获得`Stream`。

例如`Files`类的`lines()`方法可以把一个文件变成一个`Stream`，每个元素代表文件的一行内容：

```java
try (Stream<String> lines = Files.lines(Paths.get("/path/to/file.txt"))) {
    ...
}
```

另外正则表达式的`Pattern`对象有一个`splitAsStream()`方法，可以直接把一个长字符串分割成`Stream`序列而不是数组：

```java
Pattern p = Pattern.compile("\\s+");
Stream<String> s = p.splitAsStream("The quick brown fox jumps over the lazy dog");
s.forEach(System.out::println);
```

**基本类型**

Java泛型不支持基本类型，所以无法用`Stream<int>`，使用`Stream<Integer>`会频繁的装箱、拆箱操作。为了提高运行效率，Java标准库提供了`IntStream`、`LongStream`和`DoubleStream`这三种使用基本类型的`Stream`，它们的使用方法和范型`Stream`没有大的区别。

```java
// 将int[]数组变为IntStream:
IntStream is = Arrays.stream(new int[] { 1, 2, 3 });
// 将Stream<String>转换为LongStream:
LongStream ls = List.of("1", "2", "3").stream().mapToLong(Long::parseLong);
```

#### 使用map

`map()`方法用于将一个`Stream`的每个元素映射成另一个元素并转换成一个新的`Stream`；所谓`map`操作，就是把一种操作运算，映射到一个序列的每一个元素上。例如，对`x`计算它的平方，可以使用函数`f(x) = x * x`。我们把这个函数映射到一个序列1，2，3，4，5上，就得到了另一个序列1，4，9，16，25：

```java
Stream<Integer> s = Stream.of(1, 2, 3, 4, 5);
Stream<Integer> s2 = s.map(n -> n * n);
```

利用`map()`，不但能完成数学计算，对于字符串操作，以及任何Java对象都是非常有用的。

```java
import java.util.*;
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        List.of("  Apple ", " pear ", " ORANGE", " BaNaNa ")
                .stream()
                .map(String::trim) // 去空格
                .map(String::toLowerCase) // 变小写
                .forEach(System.out::println); // 打印
    }
}

```

#### 使用filter

使用`filter()`方法可以对一个`Stream`的每个元素进行测试，通过测试的元素被过滤后生成一个新的`Stream`。所谓`filter()`操作，就是对一个`Stream`的所有元素一一进行测试，不满足条件的就被“滤掉”了，剩下的满足条件的元素就构成了一个新的`Stream`。例如，我们对1，2，3，4，5这个`Stream`调用`filter()`，传入的测试函数`f(x) = x % 2 != 0`用来判断元素是否是奇数，这样就过滤掉偶数，只剩下奇数，因此我们得到了另一个序列1，3，5：

用`IntStream`写出上面的逻辑如下：

```java
import java.util.stream.IntStream;
public class Main {
    public static void main(String[] args) {
        IntStream.of(1, 2, 3, 4, 5, 6, 7, 8, 9)
                .filter(n -> n % 2 != 0)
                .forEach(System.out::println);
    }
}

```

`filter()`除了常用于数值外，也可应用于任何Java对象。如从一组给定的`LocalDate`中过滤掉工作日，以便得到休息日：

```java
import java.time.*;
import java.util.function.*;
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        Stream.generate(new LocalDateSupplier())
                .limit(31)
                .filter(ldt -> ldt.getDayOfWeek() == DayOfWeek.SATURDAY || ldt.getDayOfWeek() == DayOfWeek.SUNDAY)
                .forEach(System.out::println);
    }
}

class LocalDateSupplier implements Supplier<LocalDate> {
    LocalDate start = LocalDate.of(2020, 1, 1);
    int n = -1;
    public LocalDate get() {
        n++;
        return start.plusDays(n);
    }
}

```



#### 使用reduce

`reduce()`方法将一个`Stream`的每个元素依次作用于`BinaryOperator`，并将结果合并。`reduce()`是聚合方法，聚合方法会立刻对`Stream`进行计算。

```java
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        int sum = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9).reduce(0, (acc, n) -> acc + n);
        System.out.println(sum); // 45
    }
}

```

`reduce()`方法传入的对象是`BinaryOperator`接口，它定义了一个`apply()`方法，负责把上次的结果和本次的元素进行运算并返回结果：

```java
@FunctionalInterface
public interface BinaryOperator<T> {
    // Bi操作：两个输入，一个输出
    T apply(T t, T u);
}
```

`reduce()`操作首先初始化结果为指定值（这里是0），紧接着，`reduce()`对每个元素依次调用`(acc, n) -> acc + n`，`acc`是上次计算的结果，因此这个`reduce()`操作是求和。如果去掉初始值会得到一个`Optional<Integer>`：

```java
Optional<Integer> opt = stream.reduce((acc, n) -> acc + n);
if (opt.isPresent()) {
    System.out.println(opt.get());
}
```

因为`Stream`的元素有可能是0个，这样就没法调用`reduce()`的聚合函数了，因此返回`Optional`对象，需要进一步判断结果是否存在。

利用`reduce()`可以把求和改成求积：

```java
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        int s = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9).reduce(1, (acc, n) -> acc * n);
        System.out.println(s); // 362880
    }
}

```

除了可以对数值进行累积计算外，灵活运用`reduce()`也可以对Java对象进行操作。下面的代码演示了如何将配置文件的每一行配置通过`map()`和`reduce()`操作聚合成一个`Map<String, String>`：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        // 按行读取配置文件:
        List<String> props = List.of("profile=native", "debug=true", "logging=warn", "interval=500");
        Map<String, String> map = props.stream()
                // 把k=v转换为Map[k]=v:
                .map(kv -> {
                    String[] ss = kv.split("\\=", 2);
                    return Map.of(ss[0], ss[1]);
                })
                // 把所有Map聚合到一个Map:
                .reduce(new HashMap<String, String>(), (m, kv) -> {
                    m.putAll(kv);
                    return m;
                });
        // 打印结果:
        map.forEach((k, v) -> {
            System.out.println(k + " = " + v);
        });
    }
}

```



#### 输出集合

`Stream`通过`collect()`方法可以方便地输出为`List`、`Set`、`Map`，还可以分组输出。

对于`Stream`转换操作如`map()`、`filter()`不会触发任何计算，而聚合操作`reduce()`会立刻促使`Stream`输出它的每一个元素，并依次纳入计算以获得最终结果。

```java
// 因为s1是一个Long类型的序列，它的元素高达922亿个，但执行上述代码，既不会有任何内存增长，也不会有任何计算，因为转换操作只是保存了转换规则，无论我们对一个Stream转换多少次，都不会有任何实际计算发生。
import java.util.function.Supplier; 
import java.util.stream.Stream;
public class Main {
    public static void main(String[] args)     {
        Stream<Long> s1 = Stream.generate(new NatualSupplier());
        Stream<Long> s2 = s1.map(n -> n * n);
        Stream<Long> s3 = s2.map(n -> n - 1);
        System.out.println(s3); // java.util.stream.ReferencePipeline$3@49476842
    }
}

class NatualSupplier implements Supplier<Long> {
    long n = 0;
    public Long get() {
        n++;
        return n;
    }
}

```

```java
// 对s4进行reduce()聚合计算，会不断请求s4输出它的每一个元素。因为s4的上游是s3，它又会向s3请求元素，导致s3向s2请求元素，s2向s1请求元素，最终，s1从Supplier实例中请求到真正的元素，并经过一系列转换，最终被reduce()聚合出结果。可见，聚合操作是真正需要从Stream请求数据的，对一个Stream做聚合计算后，结果就不是一个Stream，而是一个其他的Java对象。
Stream<Long> s1 = Stream.generate(new NatualSupplier());
Stream<Long> s2 = s1.map(n -> n * n);
Stream<Long> s3 = s2.map(n -> n - 1);
Stream<Long> s4 = s3.limit(10);
s4.reduce(0, (acc, n) -> acc + n);
```

**输出为List**

因为`List`的元素是确定的Java对象，把`Stream`保存到集合是聚合操作，强制`Stream`输出每个元素。

```java
import java.util.*;
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        Stream<String> stream = Stream.of("Apple", "", null, "Pear", "  ", "Orange");
        List<String> list = stream.filter(s -> s != null && !s.isBlank()).collect(Collectors.toList());
        System.out.println(list);
    }
}
```

调用`collect()`并传入`Collectors.toList()`对象，它实际上是一个`Collector`实例，通过类似`reduce()`的操作，把每个元素添加到一个收集器中（实际上是`ArrayList`），类似的`collect(Collectors.toSet())`可以把`Stream`的每个元素收集到`Set`中。

**输出为数组**

把Stream的元素输出为数组和输出为List类似，我们只需要调用`toArray()`方法，并传入数组的“构造方法”：

```java
List<String> list = List.of("Apple", "Banana", "Orange");
String[] array = list.stream().toArray(String[]::new);
```

**输出为Map**

如果我们要把Stream的元素收集到Map中，就稍微麻烦一点。因为对于每个元素，添加到Map时需要key和value，因此，我们要指定两个映射函数，分别把元素映射为key和value：

```java
import java.util.*;
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        Stream<String> stream = Stream.of("APPL:Apple", "MSFT:Microsoft");
        Map<String, String> map = stream
                .collect(Collectors.toMap(
                        // 把元素s映射为key:
                        s -> s.substring(0, s.indexOf(':')),
                        // 把元素s映射为value:
                        s -> s.substring(s.indexOf(':') + 1)));
        System.out.println(map);
    }
}

```

**分组输出**

`Stream`可以按组输出，使用`Collectors.groupingBy()`，它需要提供两个函数：一个是分组的key，这里使用`s -> s.substring(0, 1)`，表示只要首字母相同的`String`分到一组，第二个是分组的value，这里直接使用`Collectors.toList()`，表示输出为`List`：

```java
import java.util.*;
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("Apple", "Banana", "Blackberry", "Coconut", "Avocado", "Cherry", "Apricots");
        Map<String, List<String>> groups = list.stream()
                .collect(Collectors.groupingBy(s -> s.substring(0, 1), Collectors.toList()));
        System.out.println(groups);
    }
}
```

```java
{
    A=[Apple, Avocado, Apricots],
    B=[Banana, Blackberry],
    C=[Coconut, Cherry]
}
```

#### 其他操作

`Stream`的常用操作：

转换操作：`map()`，`filter()`，`sorted()`，`distinct()`；

合并操作：`concat()`，`flatMap()`；

并行处理：`parallel()`；

聚合操作：`reduce()`，`collect()`，`count()`，`max()`，`min()`，`sum()`，`average()`；

其他操作：`allMatch()`, `anyMatch()`, `forEach()`。

**排序**

`Stream`排序只需调用`sorted()`方法：

```java
import java.util.*;
import java.util.stream.*;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("Orange", "apple", "Banana")
            .stream()
            .sorted()
            .collect(Collectors.toList());
        System.out.println(list);
    }
}
```

此方法要求`Stream`的每个元素必须实现`Comparable`接口。如果要自定义排序，传入指定的`Comparator`即可：

```java
List<String> list = List.of("Orange", "apple", "Banana")
    .stream()
    .sorted(String::compareToIgnoreCase)
    .collect(Collectors.toList());
```

**去重**

可以直接用`distinct()`：

```java
List.of("A", "B", "A", "C", "B", "D")
    .stream()
    .distinct()
    .collect(Collectors.toList()); // [A, B, C, D]
```

**截取**

`skip()`用于跳过当前`Stream`的前N个元素，`limit()`用于截取当前`Stream`最多前N个元素：

```java
List.of("A", "B", "C", "D", "E", "F")
    .stream()
    .skip(2) // 跳过A, B
    .limit(3) // 截取C, D, E
    .collect(Collectors.toList()); // [C, D, E]
```

**合并**

使用静态方法`concat()`：

```java
Stream<String> s1 = List.of("A", "B", "C").stream();
Stream<String> s2 = List.of("D", "E").stream();
// 合并:
Stream<String> s = Stream.concat(s1, s2);
System.out.println(s.collect(Collectors.toList())); // [A, B, C, D, E]
```

**flatMap**

如果`Stream`元素是集合：

```java
Stream<List<Integer>> s = Stream.of(
        Arrays.asList(1, 2, 3),
        Arrays.asList(4, 5, 6),
        Arrays.asList(7, 8, 9));
```

转换为`Stream<Integer>`，就可以使用`flatMap()`：

```java
Stream<Integer> i = s.flatMap(list -> list.stream());
```

**并行**

对`Stream`的元素进行处理是单线程的，在元素数量非常大的情况，多线程并行处理可以大大加快处理速度。只需要用`parallel()`进行转换：

```java
Stream<String> s = ...
String[] result = s.parallel() // 变成一个可以并行处理的Stream
                   .sorted() // 可以进行并行排序
                   .toArray(String[]::new);
```

**其他聚合方法**

除了`reduce()`和`collect()`外，`Stream`还有一些常用的聚合方法：

- `count()`：用于返回元素个数；
- `max(Comparator<? super T> cp)`：找出最大元素；
- `min(Comparator<? super T> cp)`：找出最小元素。

针对`IntStream`、`LongStream`和`DoubleStream`，还额外提供了以下聚合方法：

- `sum()`：对所有元素求和；
- `average()`：对所有元素求平均数。

还有一些方法，用来测试`Stream`的元素是否满足以下条件：

- `boolean allMatch(Predicate<? super T>)`：测试是否所有元素均满足测试条件；
- `boolean anyMatch(Predicate<? super T>)`：测试是否至少有一个元素满足测试条件。

最后一个常用的方法是`forEach()`，它可以循环处理`Stream`的每个元素，我们经常传入`System.out::println`来打印`Stream`的元素：

```java
Stream<String> s = ...
s.forEach(str -> {
    System.out.println("Hello, " + str);
});
```

## 设计模式

设计模式，即Design Patterns，是指在软件设计中，被反复使用的一种代码设计经验。使用设计模式的目的是为了可重用代码，提高代码的可扩展性和可维护性。主要是基于OOP编程提炼的，它基于以下几个原则：

**开闭原则Open Closed Principle**

开闭原则是指软件应该对扩展开放，而对修改关闭。意思是在增加新功能的时候，能不改代码就尽量不要改，最好只增加代码就完成了新功能。

**里氏替换原则**

里氏替换是一种面向对象的设计原则，即如果我们调用一个父类的方法可以成功，那么替换成子类调用也应该完全可以运行。

### 创建型模式

#### 工厂方法

定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method使一个类的实例化延迟到其子类。

定义一个解析字符串到`Number`的`Factory`：

```java
public interface Factory {
    Number parse(String s);
}
```

有了工厂接口再编写一个工厂的实现类：

```java
public class NumberFactoryImpl implements NumberFactory {
    public Number parse(String s) {
        return new BigDecimal(s);
    }
}
```

客户端在`NumberFactory`接口中定义一个静态方法`getFactory()`来返回真正的子类：

```java
public interface NumberFactory {
    // 创建方法:
    Number parse(String s);

    // 获取工厂实例:
    static NumberFactory getFactory() {
        return impl;
    }

    static NumberFactory impl = new NumberFactoryImpl();
}
```

在客户端中只需要和工厂接口`NumberFactory`以及抽象产品`Number`打交道：

```java
NumberFactory factory = NumberFactory.getFactory();
Number result = factory.parse("123.456");
```

可以完全忽略真正的工厂`NumberFactoryImpl`和实际的产品`BigDecimal`，这样做的好处是允许创建产品的代码独立地变换，而不会影响到调用方。实际上大多数情况下我们并不需要抽象工厂，而是通过静态方法直接返回产品。

```java
public class NumberFactory {
    public static Number parse(String s) {
        return new BigDecimal(s);
    }
}
```

这种简化的使用静态方法创建产品的方式称为静态工厂方法（Static Factory Method）。静态工厂方法广泛地应用在Java标准库中，例如：

```java
Integer n = Integer.valueOf(100);
```

`Integer`既是产品又是静态工厂。它提供了静态方法`valueOf()`来创建`Integer`。那么这种方式和直接写`new Integer(100)`有何区别呢？我们观察`valueOf()`方法：

```java
public final class Integer {
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
    ...
}
```

好处在于`valueOf()`内部可能会使用`new`创建一个新的`Integer`实例，但也可能直接返回一个缓存的`Integer`实例。对于调用方来说，没必要知道`Integer`创建的细节。如果调用方直接使用`Integer n = new Integer(100)`，那么就失去了使用缓存优化的可能性。**工厂方法可以隐藏创建产品的细节，且不一定每次都会真正创建产品，完全可以返回缓存的产品，从而提升速度并减少内存消耗。**

另一个经常使用的静态工厂方法是`List.of()`：

```java
List<String> list = List.of("A", "B", "C");
```

这个静态工厂方法接收可变参数，然后返回`List`接口。需要注意的是，调用方获取的产品总是`List`接口，而且并不关心它的实际类型。即使调用方知道`List`产品的实际类型是`java.util.ImmutableCollections$ListN`，也不要去强制转型为子类，因为静态工厂方法`List.of()`保证返回`List`，但也完全可以修改为返回`java.util.ArrayList`。这就是里氏替换原则：返回实现接口的任意子类都可以满足该方法的要求，且不影响调用方。

**总是引用接口而非实现类，能允许变换子类而不影响调用方，即尽可能面向抽象编程。**

和`List.of()`类似，我们使用`MessageDigest`时，为了创建某个摘要算法，总是使用静态工厂方法`getInstance(String)`：

```java
MessageDigest md5 = MessageDigest.getInstance("MD5");
MessageDigest sha1 = MessageDigest.getInstance("SHA-1");
```

调用方通过产品名称获得产品实例，不但调用简单，而且获得的引用仍然是`MessageDigest`这个抽象类。

#### 抽象方法

抽象工厂模式是为了让创建工厂和一组产品与使用相分离，并可以随时切换到另一个工厂以及另一组产品；

抽象工厂模式实现的关键点是定义工厂接口和产品接口，但如何实现工厂与产品本身需要留给具体的子类实现，客户端只和抽象工厂与抽象产品打交道。

> 提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。

抽象工厂模式（Abstract Factory）和工厂方法不太一样，它要解决的问题比较复杂，不但工厂是抽象的，产品是抽象的，而且有多个产品需要创建。因此，这个抽象工厂会对应到多个实际工厂，每个实际工厂负责创建多个实际产品：

定义一个Markdown转HTML和Word的接口：

```java
public interface AbstractFactory {
    // 创建Html文档:
    HtmlDocument createHtml(String md);
    // 创建Word文档:
    WordDocument createWord(String md);
}
```

`HtmlDocument`和`WordDocument`接口：

```java
// Html文档接口:
public interface HtmlDocument {
    String toHtml();
    void save(Path path) throws IOException;
}

// Word文档接口:
public interface WordDocument {
    void save(Path path) throws IOException;
}
```

这样就定义好了抽象工厂以及两个抽象产品，因为实现比较困难，让供应商来完成。FastDoc Soft的产品便宜，并且转换速度快，而GoodDoc Soft的产品贵，但转换效果好。

FastDoc Soft实现必须要实际的产品即`FastHtmlDocument`和`FastWordDocument`：

```java
public class FastHtmlDocument implements HtmlDocument {
    public String toHtml() {
        ...
    }
    public void save(Path path) throws IOException {
        ...
    }
}

public class FastWordDocument implements WordDocument {
    public void save(Path path) throws IOException {
        ...
    }
}
```

FastDoc Soft必须提供一个实际的工厂来生产这两种产品，即`FastFactory`：

```java
public class FastFactory implements AbstractFactory {
    public HtmlDocument createHtml(String md) {
        return new FastHtmlDocument(md);
    }
    public WordDocument createWord(String md) {
        return new FastWordDocument(md);
    }
}
```

这样就可以使用FastDoc Soft的服务了，客户端：

```java
// 创建AbstractFactory，实际类型是FastFactory:
AbstractFactory factory = new FastFactory();
// 生成Html文档:
HtmlDocument html = factory.createHtml("#Hello\nHello, world!");
html.save(Paths.get(".", "fast.html"));
// 生成Word文档:
WordDocument word = factory.createWord("#Hello\nHello, world!");
word.save(Paths.get(".", "fast.doc"));
```

如果要同时使用GoodDoc Soft的服务，因为用了抽象工厂模式，只需要根据定义的抽象工厂和抽象产品接口，实现自己的实际工厂和实际产品即可：

```java
// 实际工厂:
public class GoodFactory implements AbstractFactory {
    public HtmlDocument createHtml(String md) {
        return new GoodHtmlDocument(md);
    }
    public WordDocument createWord(String md) {
        return new GoodWordDocument(md);
    }
}

// 实际产品:
public class GoodHtmlDocument implements HtmlDocument {
    ...
}

public class GoodWordDocument implements HtmlDocument {
    ...
}
```

客户端要使用GoodDoc Soft的服务，只需要把原来的`new FastFactory()`切换为`new GoodFactory()`即可。

客户端代码除了通过`new`创建了`FastFactory`或`GoodFactory`外，其余代码只引用了产品接口，并未引用任何实际产品（例如，`FastHtmlDocument`），如果把创建工厂的代码放到`AbstractFactory`中，就可以连实际工厂也屏蔽了：

```java
public interface AbstractFactory {
    public static AbstractFactory createFactory(String name) {
        if (name.equalsIgnoreCase("fast")) {
            return new FastFactory();
        } else if (name.equalsIgnoreCase("good")) {
            return new GoodFactory();
        } else {
            throw new IllegalArgumentException("Invalid factory name");
        }
    }
}
```



#### 建造者

Builder模式是为了创建一个复杂的对象，需要多个步骤完成创建，或者需要多个零件组装的场景，且创建过程中可以灵活调用不同的步骤或组件。

> 将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

生成器使用多个小型工厂来最终创建出一个完整的对象。以Markdown转HTML为例，因为直接编写一个完整的转换器比较困难，但如果针对类似下面的一行文本：

```
# this is a heading
```

转换为HTML：

```
<h1>this is a heading</h1>
```

把Markdown转HTML看作一行一行的转换，每一行根据语法使用不同转换器，最后把结果组合起来：

- 如果以`#`开头，使用`HeadingBuilder`转换；
- 如果以`>`开头，使用`QuoteBuilder`转换；
- 如果以`---`开头，使用`HrBuilder`转换；
- 其余使用`ParagraphBuilder`转换。

```java
public class HtmlBuilder {
    private HeadingBuilder headingBuilder = new HeadingBuilder();
    private HrBuilder hrBuilder = new HrBuilder();
    private ParagraphBuilder paragraphBuilder = new ParagraphBuilder();
    private QuoteBuilder quoteBuilder = new QuoteBuilder();

    public String toHtml(String markdown) {
        StringBuilder buffer = new StringBuilder();
        markdown.lines().forEach(line -> {
            if (line.startsWith("#")) {
                buffer.append(headingBuilder.buildHeading(line)).append('\n');
            } else if (line.startsWith(">")) {
                buffer.append(quoteBuilder.buildQuote(line)).append('\n');
            } else if (line.startsWith("---")) {
                buffer.append(hrBuilder.buildHr(line)).append('\n');
            } else {
                buffer.append(paragraphBuilder.buildParagraph(line)).append('\n');
            }
        });
        return buffer.toString();
    }
}
```

这样一来，只需要针对每一种类型编写不同的Builder，如针对以`#`开头的行，需要`HeadingBuilder`：

```java
public class HeadingBuilder {
    public String buildHeading(String line) {
        int n = 0;
        while (line.charAt(0) == '#') {
            n++;
            line = line.substring(1);
        }
        return String.format("<h%d>%s</h%d>", n, line.strip(), n);
    }
}
```

> 实际解析Markdown是带有状态的，即下一行的语义可能与上一行相关。这里我们简化了语法，把每一行视为可以独立转换。

JavaMail的`MimeMessage`就可以看作是一个Builder模式，只不过Builder和最终产品合二为一，都是`MimeMessage`：

```java
Multipart multipart = new MimeMultipart();
// 添加text:
BodyPart textpart = new MimeBodyPart();
textpart.setContent(body, "text/html;charset=utf-8");
multipart.addBodyPart(textpart);
// 添加image:
BodyPart imagepart = new MimeBodyPart();
imagepart.setFileName(fileName);
imagepart.setDataHandler(new DataHandler(new ByteArrayDataSource(input, "application/octet-stream")));
multipart.addBodyPart(imagepart);

MimeMessage message = new MimeMessage(session);
// 设置发送方地址:
message.setFrom(new InternetAddress("me@example.com"));
// 设置接收方地址:
message.setRecipient(Message.RecipientType.TO, new InternetAddress("xiaoming@somewhere.com"));
// 设置邮件主题:
message.setSubject("Hello", "UTF-8");
// 设置邮件内容为multipart:
message.setContent(multipart);
```

很多时候可以简化Builder模式，以链式调用的方式来创建对象。

```java
StringBuilder builder = new StringBuilder();
builder.append(secure ? "https://" : "http://")
       .append("www.lsaiah.cn")
       .append("/")
       .append("?t=0");
String url = builder.toString();
```

由于经常需要构造URL字符串，使用Builder编写一个URLBuilder：

```java
String url = URLBuilder.builder() // 创建Builder
        .setDomain("www.lsaiah.cn") // 设置domain
        .setScheme("https") // 设置scheme
        .setPath("/") // 设置路径
        .setQuery(Map.of("a", "123", "q", "K&R")) // 设置query
        .build(); // 完成build
```

#### 原型

原型模式即Prototype是根据一个现有对象实例复制出一个新的实例，复制出的类型和属性与原实例相同。

> 用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

已经有了一个`String[]`数组，想再创建一个一模一样的`String[]`数组。就是把现有数组的元素复制到新数组。如果我们把这个创建过程封装一下，就成了原型模式。

```java
// 原型:
String[] original = { "Apple", "Pear", "Banana" };
// 新对象:
String[] copy = Arrays.copyOf(original, original.length);
```

对于普通类Java的`Object`提供了一个`clone()`方法，它的意图就是复制一个新的对象出来，需要实现一个`Cloneable`接口来标识一个对象是可复制的：

```java
public class Student implements Cloneable {
    private int id;
    private String name;
    private int score;

    // 复制新对象并返回:
    public Object clone() {
        Student std = new Student();
        std.id = this.id;
        std.name = this.name;
        std.score = this.score;
        return std;
    }
}
```

使用的时候，因为`clone()`的方法签名是定义在`Object`中，返回类型也是`Object`，所以要强制转型，比较麻烦：

```java
Student std1 = new Student();
std1.setId(123);
std1.setName("Bob");
std1.setScore(88);
// 复制新对象:
Student std2 = (Student) std1.clone();
System.out.println(std1);
System.out.println(std2);
System.out.println(std1 == std2); // false
```

实际上，使用原型模式更好的方式是定义一个`copy()`方法，返回明确的类型：

```java
public class Student {
    private int id;
    private String name;
    private int score;

    public Student copy() {
        Student std = new Student();
        std.id = this.id;
        std.name = this.name;
        std.score = this.score;
        return std;
    }
}
```

原型模式应用不是很广泛，因为很多实例会持有类似文件、Socket这样的资源，而这些资源是无法复制给另一个对象共享的，只有存储简单类型的“值”对象可以复制。

#### 单例

Singleton模式是为了保证一个程序的运行期间，一个进程中某个类有且只有一个全局唯一实例；

Singleton模式既可以严格实现，也可以以约定的方式把普通类视作单例。

> 保证一个类仅有一个实例，并提供一个访问它的全局访问点。

因为只有一个实例，因此调用方不能使用`new Xyz()`来创建实例，所以单例的构造方法必须是`private`，防止调用方自己创建实例，但在类的内部可以用一个静态字段来引用唯一创建的实例：

1. 只有`private`构造方法，确保外部无法实例化；

```java
public class Singleton {
    // 静态字段引用唯一实例:
    private static final Singleton INSTANCE = new Singleton();

    // private构造方法保证外部无法实例化:
    private Singleton() {
    }
}
```

外部调用方获得唯一实例需要提供一个静态方法：

2. 通过`private static`变量持有唯一实例，保证全局唯一性；

```java
public class Singleton {
    // 静态字段引用唯一实例:
    private static final Singleton INSTANCE = new Singleton();

    // 通过静态方法返回实例:
    public static Singleton getInstance() {
        return INSTANCE;
    }

    // private构造方法保证外部无法实例化:
    private Singleton() {
    }
}
```

或者把`static`变量暴露给外部：

3. 通过`public static`方法返回此唯一实例，使外部调用方能获取到实例。

```java
public class Singleton {
    // 静态字段引用唯一实例:
    public static final Singleton INSTANCE = new Singleton();

    // private构造方法保证外部无法实例化:
    private Singleton() {
    }
}
```

延迟加载，即在调用方第一次调用`getInstance()`时才初始化全局唯一实例：

```java
public class Singleton {
    private static Singleton INSTANCE = null;

    public static Singleton getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }

    private Singleton() {
    }
}
```

在多线程中会创建多个实例，必须对整个方法加锁：

```java
public synchronized static Singleton getInstance() {
    if (INSTANCE == null) {
        INSTANCE = new Singleton();
    }
    return INSTANCE;
}
```

但加锁会影响性能，双重检查类似：

```java
public static Singleton getInstance() {
    if (INSTANCE == null) {
        synchronized (Singleton.class) {
            if (INSTANCE == null) {
                INSTANCE = new Singleton();
            }
        }
    }
    return INSTANCE;
}
```

由于java内存模型，双重检查不成立，要真正实现延迟加载，只能通过Java的ClassLoader机制完成。如果没有特殊需求，使用Singleton模式的时候最好不要延迟加载，这样会使代码更简单。

另一种实现Singleton的方式是利用java的`enum`，编写一个只有一个枚举的类：

```java
public enum World {
    // 唯一枚举:
	INSTANCE;

	private String name = "world";

	public String getName() {
		return this.name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

枚举也完全可以像其他类那样定义自己的字段，方法。上面的`World`类在调用方可以这样用：

```java
String name = World.INSTANCE.getName();
```

使用枚举实现Singleton还避免了第一种实现方式的潜在问题：即序列化和反序列化会绕过普通类的`private`构造方法从而创建出多个实例。

实际上很多程序，尤其是Web程序，大部分服务类都应该被视作Singleton，如果全部按Singleton的写法写，会非常麻烦，所以通常是通过约定让框架（例如Spring）来实例化这些类，保证只有一个实例，调用方自觉通过框架获取实例而不是`new`操作符：

```java
@Component // 表示一个单例组件
public class MyService {
    ...
}
```

### 结构型模式

虽然面向对象的继承机制提供了最基本的子类扩展父类的功能，但结构型模式不仅仅简单地使用继承，而更多地通过组合与运行期的动态组合来实现更灵活的功能。

#### 适配器

> 将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

适配器模式是Adapter也称Wrapper，当一个接口需要B接口，但是待传入的是A接口。在程序设计中已有一个`Task`类，实现了`Callable`接口：

```java
public class Task implements Callable<Long> {
    private long num;
    public Task(long num) {
        this.num = num;
    }

    public Long call() throws Exception {
        long r = 0;
        for (long n = 1; n <= this.num; n++) {
            r = r + n;
        }
        System.out.println("Result: " + r);
        return r;
    }
}
```

现在想通过一个线程去执行它：

```java
Callable<Long> callable = new Task(123450000L);
Thread thread = new Thread(callable); // compile error!
thread.start();
```

编译报错，因为`Thread`接收`Runnable`接口，但不接收`Callable`接口。

一是改写`Task`类，把实现的`Callable`改为`Runnable`，但很可能`Task`在其他地方作为`Callable`被引用，改写`Task`接口会导致其他正常工作的代码无法编译。

另一个办法不用改写`Task`类，而是用一个Adapter，把这个`Callable`接口“变成”`Runnable`接口，这样，就可以正常编译：

```java
Callable<Long> callable = new Task(123450000L);
Thread thread = new Thread(new RunnableAdapter(callable));
thread.start();
```

这个`RunnableAdapter`类就是Adapter，它接收一个`Callable`输出一个`Runnable`。实现`RunnableAdapter`类：

```java
public class RunnableAdapter implements Runnable {
    // 引用待转换接口:
    private Callable<?> callable;

    public RunnableAdapter(Callable<?> callable) {
        this.callable = callable;
    }

    // 实现指定接口:
    public void run() {
        // 将指定接口调用委托给转换接口调用:
        try {
            callable.call();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

**编写一个Adapter步骤如下：**

1. 实现目标接口，这里是`Runnable`；
2. 内部持有一个待转换接口的引用，这里是通过字段持有`Callable`接口；
3. 在目标接口的实现方法内部，调用待转换接口的方法。

这样一来，Thread就可以接收这个`RunnableAdapter`，因为它实现了`Runnable`接口。`Thread`作为调用方，它会调用`RunnableAdapter`的`run()`方法，在这个`run()`方法内部，又调用了`Callable`的`call()`方法，相当于`Thread`通过一层转换，间接调用了`Callable`的`call()`方法。

适配器模式在Java标准库中有广泛应用。比如我们持有数据类型是`String[]`，但是需要`List`接口时，可以用一个Adapter：

```java
String[] exist = new String[] {"Good", "morning", "Bob", "and", "Alice"};
Set<String> set = new HashSet<>(Arrays.asList(exist));
```

假设持有一个`InputStream`，希望调用`readText(Reader)`方法，但参数是`Reader`而不是`InputStream`，使用适配器把`InputStream`变成`Reader`：

```java
InputStream input = Files.newInputStream(Paths.get("/path/to/file"));
Reader reader = new InputStreamReader(input, "UTF-8");
readText(reader);
```

`InputStreamReader`就是Java标准库提供的`Adapter`，它负责把一个`InputStream`适配为`Reader`。类似的还有`OutputStreamWriter`。

如果我们把`readText(Reader)`方法参数从`Reader`改为`FileReader`，因为我们需要一个`FileReader`类型，就必须把`InputStream`适配为`FileReader`：

```java
FileReader reader = new InputStream(input, "utf-8");
```

编译报错，因为`InputStreamReader`只能转换出`Reader`接口，要把`InputStream`转换为`FileReader`非常困难。而面向抽象编程这一原则就体现出了威力：持有高层接口不但代码更灵活，而且把各种接口组合起来也更容易。一旦持有某个具体的子类类型，要想做一些改动就非常困难。

Adapter模式可以将一个A接口转换为B接口，使得新的对象符合B接口规范。编写Adapter实际上就是编写一个实现了B接口，并且内部持有A接口的类：

```java
public BAdapter implements B {
    private A a;
    public BAdapter(A a) {
        this.a = a;
    }
    public void b() {
        a.a();
    }
}
```

在Adapter内部将B接口的调用“转换”为对A接口的调用。

只有A、B接口均为抽象接口时，才能非常简单地实现Adapter模式。

#### 桥接

> 将抽象部分与它的实现部分分离，使它们都可以独立变化

桥接模式通过分离一个抽象接口和它的实现部分，使得设计可以按两个维度独立扩展。

假设某个汽车厂商生产三种品牌的汽车：Big、Tiny和Boss，每种品牌又可以选择燃油、纯电和混合动力。如果用传统的继承来表示各个最终车型，一共有3个抽象类加9个最终子类：

```ascii
                   ┌───────┐
                   │  Car  │
                   └───────┘
                       ▲
    ┌──────────────────┼───────────────────┐
    │                  │                   │
┌───────┐          ┌───────┐          ┌───────┐
│BigCar │          │TinyCar│          │BossCar│
└───────┘          └───────┘          └───────┘
    ▲                  ▲                  ▲
    │                  │                  │
    │ ┌───────────────┐│ ┌───────────────┐│ ┌───────────────┐
    ├─│  BigFuelCar   │├─│  TinyFuelCar  │├─│  BossFuelCar  │
    │ └───────────────┘│ └───────────────┘│ └───────────────┘
    │ ┌───────────────┐│ ┌───────────────┐│ ┌───────────────┐
    ├─│BigElectricCar │├─│TinyElectricCar│├─│BossElectricCar│
    │ └───────────────┘│ └───────────────┘│ └───────────────┘
    │ ┌───────────────┐│ ┌───────────────┐│ ┌───────────────┐
    └─│ BigHybridCar  │└─│ TinyHybridCar │└─│ BossHybridCar │
      └───────────────┘  └───────────────┘  └───────────────┘
```

如果要新增一个品牌，或者加一个新的引擎（比如核动力），那么子类的数量增长更快。所以桥接模式就是为了避免直接继承带来的子类爆炸。

在桥接模式中，首先把`Car`按品牌进行子类化，每个品牌选择什么发动机，不再使用子类扩充，而是通过一个抽象的修正类以组合的形式引入。

首先定义抽象类`Car`，它引用一个`Engine`：

```java
public abstract class Car {
    // 引用Engine:
    protected Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }

    public abstract void drive();
}
```

`Engine`的定义如下：

```java
public interface Engine {
    void start();
}
```

接着在一个修正的抽象类`RefindCar`中定义一些额外操作：

```java
public abstract class RefinedCar extends Car {
    public RefinedCar(Engine engine) {
        super(engine);
    }

    public void drive() {
        this.engine.start();
        System.out.println("Drive " + getBrand() + " car...");
    }

    public abstract String getBrand();
}
```

这样最终的不同品牌继承自`RefindCar`，例如`BossCar`：

```java
public class BossCar extends RefinedCar {
    public BossCar(Engine engine) {
        super(engine);
    }

    public String getBrand() {
        return "Boss";
    }
}
```

而每一种引擎继承自`Engine`，例如`HybridEngine`：

```java
public class HybridEngine implements Engine {
    public void start() {
        System.out.println("Start Hybrid Engine...");
    }
}
```

客户端通过自己选择一个品牌再配合一个引擎，得到最终的Car：

```java
RefinedCar car = new BossCar(new HybridEngine());
car.drive();
```

好处在于，如果要增加一种引擎，只需要针对`Engine`派生一个新的子类，如果要增加一个品牌，只需要针对`RefinedCar`派生一个子类，任何`RefinedCar`的子类都可以和任何一种`Engine`自由组合，即一辆汽车的两个维度：品牌和引擎都可以独立地变化。

```ascii
       ┌───────────┐
       │    Car    │
       └───────────┘
             ▲
             │
       ┌───────────┐       ┌─────────┐
       │RefinedCar │ ─ ─ ─>│ Engine  │
       └───────────┘       └─────────┘
             ▲                  ▲
    ┌────────┼────────┐         │ ┌──────────────┐
    │        │        │         ├─│  FuelEngine  │
┌───────┐┌───────┐┌───────┐     │ └──────────────┘
│BigCar ││TinyCar││BossCar│     │ ┌──────────────┐
└───────┘└───────┘└───────┘     ├─│ElectricEngine│
                                │ └──────────────┘
                                │ ┌──────────────┐
                                └─│ HybridEngine │
                                  └──────────────┘
```



#### 组合

> 将对象组合成树形结构以表示部分-整体的层次结构，使得用户对单个对象和组合对象具有一致性。

组合模式经常用于树形结构，使用Composite可以把一个叶子节点与一个父节点统一处理。举个例子在XML或HTML中，从根节点开始，每个节点都可能包含任意个其他节点，这些层层嵌套的节点就构成了一颗树。要以树的结构表示XML，可以先抽象出节点类型`Node`：

```java
public interface Node {
    // 添加一个节点为子节点:
    Node add(Node node);
    // 获取子节点:
    List<Node> children();
    // 输出为XML:
    String toXml();
}
```

对于一个`<abc>`这样的节点，我们可以称之为`ElementNode`，它可以作为容器包含多个子节点：

```java
public class ElementNode implements Node {
    private String name;
    private List<Node> list = new ArrayList<>();

    public ElementNode(String name) {
        this.name = name;
    }

    public Node add(Node node) {
        list.add(node);
        return this;
    }

    public List<Node> children() {
        return list;
    }

    public String toXml() {
        String start = "<" + name + ">\n";
        String end = "</" + name + ">\n";
        StringJoiner sj = new StringJoiner("", start, end);
        list.forEach(node -> {
            sj.add(node.toXml() + "\n");
        });
        return sj.toString();
    }
}
```

对于普通文本，把它看作`TextNode`，没有子节点：

```java
public class TextNode implements Node {
	private String text;

	public TextNode(String text) {
		this.text = text;
	}

	public Node add(Node node) {
		throw new UnsupportedOperationException();
	}

	public List<Node> children() {
		return List.of();
	}

	public String toXml() {
		return text;
	}
}
```

还可以有注释节点：

```java
public class CommentNode implements Node {
	private String text;

	public CommentNode(String text) {
		this.text = text;
	}

	public Node add(Node node) {
		throw new UnsupportedOperationException();
	}

	public List<Node> children() {
		return List.of();
	}

	public String toXml() {
		return "<!-- " + text + " -->";
	}
}
```

通过`ElementNode`、`TextNode`和`CommentNode`，我们就可以构造出一颗树：

```java
Node root = new ElementNode("school");
root.add(new ElementNode("classA")
        .add(new TextNode("Tom"))
        .add(new TextNode("Alice")));
root.add(new ElementNode("classB")
        .add(new TextNode("Bob"))
        .add(new TextNode("Grace"))
        .add(new CommentNode("comment...")));
System.out.println(root.toXml());
```

通过`root`节点输出XML如下：

```xml
<school>
<classA>
Tom
Alice
</classA>
<classB>
Bob
Grace
<!-- comment... -->
</classB>
</school>
```

可见，使用组合Composite模式时需要先统一单个节点以及容器节点的接口，作为容器节点的`ElementNode`又可以添加任意个`Node`，这样就可以构成层级结构。

```ascii
             ┌───────────┐
             │   Node    │
             └───────────┘
                   ▲
      ┌────────────┼────────────┐
      │            │            │
┌───────────┐┌───────────┐┌───────────┐
│ElementNode││ TextNode  ││CommentNode│
└───────────┘└───────────┘└───────────┘
```



#### 装饰器

使用Decorator模式，可以独立增加核心功能，也可以独立增加附加功能，二者互不影响；在运行期动态地给核心功能增加任意个附加功能。

> 动态地给一个对象添加一些额外的职责，就增加功能来说，相比生成子类更为灵活。

如果要给不同的最终数据源增加缓冲功能、计算签名功能、加密解密功能，那么3个最终数据源，3个功能一共需要9个子类，如果继续增加子类会爆炸式增长，显然不可取。

装饰器Decorator模式的目的就是把一个个的附加功能用装饰器的方式给一层一层地累加到原始数据源上，最终，通过组合获得我们想要的功能。

如：给`FileInputStream`增加缓冲和解压缩功能，用Decorator模式写出来如下。

```java
// 创建原始的数据源:
InputStream fis = new FileInputStream("test.gz");
// 增加缓冲功能:
InputStream bis = new BufferedInputStream(fis);
// 增加解压缩功能:
InputStream gis = new GZIPInputStream(bis);
```

或者一次性写成：

```java
InputStream input = new GZIPInputStream( // 第二层装饰
                        new BufferedInputStream( // 第一层装饰
                            new FileInputStream("test.gz") // 核心功能
                        ));
```

`BufferedInputStream`和`GZIPInputStream`，它们实际上都是从`FilterInputStream`继承的，这个`FilterInputStream`就是一个抽象的Decorator。我们用图把Decorator模式画出来如下：

```ascii
             ┌───────────┐
             │ Component │
             └───────────┘
                   ▲
      ┌────────────┼─────────────────┐
      │            │                 │
┌───────────┐┌───────────┐     ┌───────────┐
│ComponentA ││ComponentB │...  │ Decorator │
└───────────┘└───────────┘     └───────────┘
                                     ▲
                              ┌──────┴──────┐
                              │             │
                        ┌───────────┐ ┌───────────┐
                        │DecoratorA │ │DecoratorB │...
                        └───────────┘ └───────────┘
```

最顶层的Component是接口，对应到IO的就是`InputStream`这个抽象类。ComponentA、ComponentB是实际的子类，对应到IO的就是`FileInputStream`、`ServletInputStream`这些数据源。Decorator是用于实现各个附加功能的抽象装饰器，对应到IO的就是`FilterInputStream`。而从Decorator派生的就是一个一个的装饰器，它们每个都有独立的功能，对应到IO的就是`BufferedInputStream`、`GZIPInputStream`等。

Decorator模式它实际上把核心功能和附加功能给分开了。核心功能指`FileInputStream`这些真正读数据的源头，附加功能指加缓冲、压缩、解密这些功能。如果我们要新增核心功能，就增加Component的子类，例如`ByteInputStream`。如果我们要增加附加功能，就增加Decorator的子类，例如`CipherInputStream`。两部分都可以独立地扩展，而具体如何附加功能，由调用方自由组合，从而极大地增强了灵活性。

假设需要渲染一个HTML文本，但是文本还可以附加一些效果，比如加粗、斜体、下划线等。为了实现动态附加效果，可以采用Decorator模式。

 首先定义顶层接口`TextNode`：

```java
public interface TextNode {
    // 设置text:
    void setText(String text);
    // 获取text:
    String getText();
}
```

对于核心节点，例如`<span>`，需要从`TextNode`直接继承：

```java
public class SpanNode implements TextNode {
    private String text;

    public void setText(String text) {
        this.text = text;
    }

    public String getText() {
        return "<span>" + text + "</span>";
    }
}
```

为了实现Decorator模式需要有一个抽象的Decorator类：

```java
public abstract class NodeDecorator implements TextNode {
    protected final TextNode target;

    protected NodeDecorator(TextNode target) {
        this.target = target;
    }

    public void setText(String text) {
        this.target.setText(text);
    }
}
```

这个`NodeDecorator`类的核心是持有一个`TextNode`，即将要把功能附加到的`TextNode`实例。接下来就可以写一个加粗功能：

```java
public class BoldDecorator extends NodeDecorator {
    public BoldDecorator(TextNode target) {
        super(target);
    }

    public String getText() {
        return "<b>" + target.getText() + "</b>";
    }
}
```

类似的可以继续加`ItalicDecorator`、`UnderlineDecorator`等。客户端可以自由组合这些Decorator：

```java
TextNode n1 = new SpanNode();
TextNode n2 = new BoldDecorator(new UnderlineDecorator(new SpanNode()));
TextNode n3 = new ItalicDecorator(new BoldDecorator(new SpanNode()));
n1.setText("Hello");
n2.setText("Decorated");
n3.setText("World");
System.out.println(n1.getText());
// 输出<span>Hello</span>

System.out.println(n2.getText());
// 输出<b><u><span>Decorated</span></u></b>

System.out.println(n3.getText());
// 输出<i><b><span>World</span></b></i>
```



#### 外观

Facade模式是为了给客户端提供一个统一入口，并对外屏蔽内部子系统的调用细节。

> 为子系统中的一组接口提供一个一致的界面，Facade模式定义了一个高层接口，这个接口使得这一子系统更容易使用。

如果客户端要跟许多子系统打交道，那么客户端需要了解各个子系统的接口，比较麻烦。如果有一个统一的“中介”，让客户端只跟中介打交道，中介再去跟各个子系统打交道，对客户端来说就比较简单。所以Facade就相当于搞了一个中介。

以注册公司为例，假设注册公司需要三步：

1. 向工商局申请公司营业执照；
2. 在银行开设账户；
3. 在税务局开设纳税号。

以下是三个系统的接口：

```java
// 工商注册:
public class AdminOfIndustry {
    public Company register(String name) {
        ...
    }
}

// 银行开户:
public class Bank {
    public String openAccount(String companyId) {
        ...
    }
}

// 纳税登记:
public class Taxation {
    public String applyTaxCode(String companyId) {
        ...
    }
}
```

如果子系统比较复杂，并且客户对流程不熟悉，那就把这些流程全部委托给中介：

```java
public class Facade {
    public Company openCompany(String name) {
        Company c = this.admin.register(name);
        String bankAccount = this.bank.openAccount(c.getId());
        c.setBankAccount(bankAccount);
        String taxCode = this.taxation.applyTaxCode(c.getId());
        c.setTaxCode(taxCode);
        return c;
    }
}
```

这样客户端只跟Facade打交道，一次完成公司注册的所有繁琐流程：

```java
Company c = facade.openCompany("Facade Software Ltd.");
```

很多Web程序，内部有多个子系统提供服务，经常使用一个统一的Facade入口，例如一个`RestApiController`，使得外部用户调用的时候，只关心Facade提供的接口，不用管内部到底是哪个子系统处理的。

更复杂的Web程序，会有多个Web服务，这个时候，经常会使用一个统一的网关入口来自动转发到不同的Web服务，这种提供统一入口的网关就是Gateway，它本质上也是一个Facade，但可以附加一些用户认证、限流限速的额外服务。

#### 享元

> 运用共享技术有效地支持大量细粒度的对象

享元模式的设计思想是尽量复用已创建的对象，常用于工厂方法内部的优化。享元（Flyweight）的核心思想很简单：如果一个对象实例一经创建就不可变，那么反复创建相同的实例就没有必要，直接向调用方返回一个共享的实例就行，这样即节省内存，又可以减少创建对象的过程，提高运行速度。

享元模式在Java标准库中有很多应用，包装类型如`Byte`、`Integer`都是不变类，因此，反复创建同一个值相同的包装类型是没有必要的。以`Integer`为例，如果我们通过`Integer.valueOf()`这个静态工厂方法创建`Integer`实例，当传入的`int`范围在`-128`~`+127`之间时，会直接返回缓存的`Integer`实例：

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Integer n1 = Integer.valueOf(100);
        Integer n2 = Integer.valueOf(100);
        System.out.println(n1 == n2); // true
    }
}
```

对于`Byte`来说，因为它一共只有256个状态，所以，通过`Byte.valueOf()`创建的`Byte`实例，全部都是缓存对象。

因此，享元模式就是通过工厂方法创建对象，在工厂方法内部，很可能返回缓存的实例，而不是新创建实例，从而实现不可变实例的复用。

> 总是使用工厂方法而不是new操作符创建实例，可获得享元模式的好处。

在实际应用中，享元模式主要应用于缓存，即客户端如果重复请求某些对象，不必每次查询数据库或者读取文件，而是直接返回内存中缓存的数据。

以`Student`为例，设计一个静态工厂方法，它在内部可以返回缓存的对象：

```java
public class Student {
    // 持有缓存:
    private static final Map<String, Student> cache = new HashMap<>();

    // 静态工厂方法:
    public static Student create(int id, String name) {
        String key = id + "\n" + name;
        // 先查找缓存:
        Student std = cache.get(key);
        if (std == null) {
            // 未找到,创建新对象:
            System.out.println(String.format("create new Student(%s, %s)", id, name));
            std = new Student(id, name);
            // 放入缓存:
            cache.put(key, std);
        } else {
            // 缓存中存在:
            System.out.println(String.format("return cached Student(%s, %s)", std.id, std.name));
        }
        return std;
    }

    private final int id;
    private final String name;

    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

在实际应用中，我们经常使用成熟的缓存库，例如[Guava](https://github.com/google/guava)的[Cache](https://github.com/google/guava/blob/master/guava/src/com/google/common/cache/Cache.java)，因为它提供了最大缓存数量限制、定时过期等实用功能。

#### 代理

> 为其他对象提供一种代理以控制对这个对象的访问

代理模式通过封装一个已有接口，并向调用方返回相同的接口类型，能让调用方在不改变任何代码的前提下增强某些功能（例如，鉴权、延迟加载、连接池复用等）。

使用Proxy模式要求调用方持有接口，作为Proxy的类也必须实现相同的接口类型。

它和Adapter模式类似，Adapter模式用于把A接口转换为B接口：

```java
public BAdapter implements B {
    private A a;
    public BAdapter(A a) {
        this.a = a;
    }
    public void b() {
        a.a();
    }
}
```

而Proxy模式不是把A接口转换为B接口，它还是转换成A接口：

```java
public AProxy implements A {
    private A a;
    public AProxy(A a) {
        this.a = a;
    }
    public void a() {
        this.a.a();
    }
}
```

Proxy就是给A接口再包一层，在调用`a.a()`的前后，加一些额外的代码：

```java
public void a() {
    if (getCurrentUser().isRoot()) {
        this.a.a();
    } else {
        throw new SecurityException("Forbidden");
    }
}
```

这样一来，我们就实现了权限检查，只有符合要求的用户，才会真正调用目标方法，否则，会直接抛出异常。

用Proxy实现这个权限检查，我们可以获得更清晰、更简洁的代码：

- A接口：只定义接口；
- ABusiness类：只实现A接口的业务逻辑；
- APermissionProxy类：只实现A接口的权限检查代理。

如果我们希望编写其他类型的代理，可以继续增加类似ALogProxy，而不必对现有的A接口、ABusiness类进行修改。

Proxy还广泛应用在：

**远程代理**

远程代理即Remote Proxy，本地的调用者持有的接口实际上是一个代理，这个代理负责把对接口的方法访问转换成远程调用，然后返回结果。Java内置的RMI机制就是一个完整的远程代理模式。

**虚代理**

虚代理即Virtual Proxy，它让调用者先持有一个代理对象，但真正的对象尚未创建。如果没有必要，这个真正的对象是不会被创建的，直到客户端需要真的必须调用时，才创建真正的对象。JDBC的连接池返回的JDBC连接（Connection对象）就可以是一个虚代理，即获取连接时根本没有任何实际的数据库连接，直到第一次执行JDBC查询或更新操作时，才真正创建实际的JDBC连接。

**保护代理**

保护代理即Protection Proxy，它用代理对象控制对原始对象的访问，常用于鉴权。

**智能引用**

智能引用即Smart Reference，它也是一种代理对象，如果有很多客户端对它进行访问，通过内部的计数器可以在外部调用者都不使用后自动释放它。

我们来看一下如何应用代理模式编写一个JDBC连接池（`DataSource`）。我们首先来编写一个虚代理，即如果调用者获取到`Connection`后，并没有执行任何SQL操作，那么这个Connection Proxy实际上并不会真正打开JDBC连接。调用者代码如下：

```java
DataSource lazyDataSource = new LazyDataSource(jdbcUrl, jdbcUsername, jdbcPassword);
System.out.println("get lazy connection...");
try (Connection conn1 = lazyDataSource.getConnection()) {
    // 并没有实际打开真正的Connection
}
System.out.println("get lazy connection...");
try (Connection conn2 = lazyDataSource.getConnection()) {
    try (PreparedStatement ps = conn2.prepareStatement("SELECT * FROM students")) { // 打开了真正的Connection
        try (ResultSet rs = ps.executeQuery()) {
            while (rs.next()) {
                System.out.println(rs.getString("name"));
            }
        }
    }
}
```

现在我们来思考如何实现这个`LazyConnectionProxy`。为了简化代码，我们首先针对`Connection`接口做一个抽象的代理类：

```java
public abstract class AbstractConnectionProxy implements Connection {

    // 抽象方法获取实际的Connection:
    protected abstract Connection getRealConnection();

    // 实现Connection接口的每一个方法:
    public Statement createStatement() throws SQLException {
        return getRealConnection().createStatement();
    }

    public PreparedStatement prepareStatement(String sql) throws SQLException {
        return getRealConnection().prepareStatement(sql);
    }

    ...其他代理方法...
}
```

这个`AbstractConnectionProxy`代理类的作用是把`Connection`接口定义的方法全部实现一遍，因为`Connection`接口定义的方法太多了，后面我们要编写的`LazyConnectionProxy`只需要继承`AbstractConnectionProxy`，就不必再把`Connection`接口方法挨个实现一遍。

`LazyConnectionProxy`实现如下：

```java
public class LazyConnectionProxy extends AbstractConnectionProxy {
    private Supplier<Connection> supplier;
    private Connection target = null;

    public LazyConnectionProxy(Supplier<Connection> supplier) {
        this.supplier = supplier;
    }

    // 覆写close方法：只有target不为null时才需要关闭:
    public void close() throws SQLException {
        if (target != null) {
            System.out.println("Close connection: " + target);
            super.close();
        }
    }

    @Override
    protected Connection getRealConnection() {
        if (target == null) {
            target = supplier.get();
        }
        return target;
    }
}
```

如果调用者没有执行任何SQL语句，那么`target`字段始终为`null`。只有第一次执行SQL语句时（即调用任何类似`prepareStatement()`方法时，触发`getRealConnection()`调用），才会真正打开实际的JDBC Connection。

最后，我们还需要编写一个`LazyDataSource`来支持这个`LazyConnecitonProxy`：

```java
public class LazyDataSource implements DataSource {
    private String url;
    private String username;
    private String password;

    public LazyDataSource(String url, String username, String password) {
        this.url = url;
        this.username = username;
        this.password = password;
    }

    public Connection getConnection(String username, String password) throws SQLException {
        return new LazyConnectionProxy(() -> {
            try {
                Connection conn = DriverManager.getConnection(url, username, password);
                System.out.println("Open connection: " + conn);
                return conn;
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        });
    }
    ...
}
```

输出如下：

```java
get lazy connection...
get lazy connection...
Open connection: com.mysql.jdbc.JDBC4Connection@7a36aefa
小明
小红
小军
小白
...
Close connection: com.mysql.jdbc.JDBC4Connection@7a36aefa
```

可见第一个`getConnection()`调用获取到的`LazyConnectionProxy`并没有实际打开真正的JDBC Connection。

使用连接池的时候，我们更希望能重复使用连接。如果调用方编写这样的代码：

```java
DataSource pooledDataSource = new PooledDataSource(jdbcUrl, jdbcUsername, jdbcPassword);
try (Connection conn = pooledDataSource.getConnection()) {
}
try (Connection conn = pooledDataSource.getConnection()) {
    // 获取到的是同一个Connection
}
try (Connection conn = pooledDataSource.getConnection()) {
    // 获取到的是同一个Connection
}
```

调用方并不关心是否复用了`Connection`，但从`PooledDataSource`获取的`Connection`确实自带这个优化功能。如何实现可复用`Connection`的连接池？答案仍然是使用代理模式。

```java
public class PooledConnectionProxy extends AbstractConnectionProxy {
    // 实际的Connection:
    Connection target;
    // 空闲队列:
    Queue<PooledConnectionProxy> idleQueue;

    public PooledConnectionProxy(Queue<PooledConnectionProxy> idleQueue, Connection target) {
        this.idleQueue = idleQueue;
        this.target = target;
    }

    public void close() throws SQLException {
        System.out.println("Fake close and released to idle queue for future reuse: " + target);
        // 并没有调用实际Connection的close()方法,
        // 而是把自己放入空闲队列:
        idleQueue.offer(this);
    }

    protected Connection getRealConnection() {
        return target;
    }
}
```

复用连接的关键在于覆写`close()`方法，它并没有真正关闭底层JDBC连接，而是把自己放回一个空闲队列，以便下次使用。

空闲队列由`PooledDataSource`负责维护：

```java
public class PooledDataSource implements DataSource {
    private String url;
    private String username;
    private String password;

    // 维护一个空闲队列:
    private Queue<PooledConnectionProxy> idleQueue = new ArrayBlockingQueue<>(100);

    public PooledDataSource(String url, String username, String password) {
        this.url = url;
        this.username = username;
        this.password = password;
    }

    public Connection getConnection(String username, String password) throws SQLException {
        // 首先试图获取一个空闲连接:
        PooledConnectionProxy conn = idleQueue.poll();
        if (conn == null) {
            // 没有空闲连接时，打开一个新连接:
            conn = openNewConnection();
        } else {
            System.out.println("Return pooled connection: " + conn.target);
        }
        return conn;
    }

    private PooledConnectionProxy openNewConnection() throws SQLException {
        Connection conn = DriverManager.getConnection(url, username, password);
        System.out.println("Open new connection: " + conn);
        return new PooledConnectionProxy(idleQueue, conn);
    }
    ...
}
```

我们执行调用方代码，输出如下：

```java
Open new connection: com.mysql.jdbc.JDBC4Connection@61ca2dfa
Fake close and released to idle queue for future reuse: com.mysql.jdbc.JDBC4Connection@61ca2dfa
Return pooled connection: com.mysql.jdbc.JDBC4Connection@61ca2dfa
Fake close and released to idle queue for future reuse: com.mysql.jdbc.JDBC4Connection@61ca2dfa
Return pooled connection: com.mysql.jdbc.JDBC4Connection@61ca2dfa
Fake close and released to idle queue for future reuse: com.mysql.jdbc.JDBC4Connection@61ca2dfa
```

除了第一次打开了一个真正的JDBC Connection，后续获取的`Connection`实际上是同一个JDBC Connection。但是，对于调用方来说，完全不需要知道底层做了哪些优化。

我们实际使用的DataSource，例如HikariCP，都是基于代理模式实现的，原理同上，但增加了更多的如动态伸缩的功能（一个连接空闲一段时间后自动关闭）。

Proxy模式和Decorator模式有些类似。确实，这两者看起来很像，但区别在于：Decorator模式让调用者自己创建核心类，然后组合各种功能，而Proxy模式决不能让调用者自己创建再组合，否则就失去了代理的功能。Proxy模式让调用者认为获取到的是核心类接口，但实际上是代理类。

### 行为型模式

#### 责任链

> 使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。

责任链模式是一种把多个处理器组合在一起，依次处理请求的模式；

责任链模式的好处是添加新的处理器或者重新排列处理器非常容易；

责任链模式经常用在拦截、预处理请求等。

责任链模式（Chain of Responsibility）是一种处理请求的模式，它让多个处理器都有机会处理该请求，直到其中某个处理成功为止。责任链模式把多个处理器串成链，然后让请求在链上传递：

```ascii
     ┌─────────┐
     │ Request │
     └─────────┘
          │
┌ ─ ─ ─ ─ ┼ ─ ─ ─ ─ ┐
          ▼
│  ┌─────────────┐  │
   │ ProcessorA  │
│  └─────────────┘  │
          │
│         ▼         │
   ┌─────────────┐
│  │ ProcessorB  │  │
   └─────────────┘
│         │         │
          ▼
│  ┌─────────────┐  │
   │ ProcessorC  │
│  └─────────────┘  │
          │
└ ─ ─ ─ ─ ┼ ─ ─ ─ ─ ┘
          │
          ▼
```

在实际场景中，财务审批就是一个责任链模式。假设某个员工需要报销一笔费用，审核者可以分为：

- Manager：只能审核1000元以下的报销；
- Director：只能审核10000元以下的报销；
- CEO：可以审核任意额度。

用责任链模式设计此报销流程时，每个审核者只关心自己责任范围内的请求，并且处理它。对于超出自己责任范围的，扔给下一个审核者处理，这样，将来继续添加审核者的时候，不用改动现有逻辑。

我们来看看如何实现责任链模式。

首先，我们要抽象出请求对象，它将在责任链上传递：

```java
public class Request {
    private String name;
    private BigDecimal amount;

    public Request(String name, BigDecimal amount) {
        this.name = name;
        this.amount = amount;
    }

    public String getName() {
        return name;
    }

    public BigDecimal getAmount() {
        return amount;
    }
}
```

其次要抽象出处理器：

```java
public interface Handler {
    // 返回Boolean.TRUE = 成功
    // 返回Boolean.FALSE = 拒绝
    // 返回null = 交下一个处理
	Boolean process(Request request);
}
```

并且做好约定：如果返回`Boolean.TRUE`，表示处理成功，如果返回`Boolean.FALSE`，表示处理失败（请求被拒绝），如果返回`null`，则交由下一个`Handler`处理。

然后，依次编写ManagerHandler、DirectorHandler和CEOHandler。以ManagerHandler为例：

```java
public class ManagerHandler implements Handler {
    public Boolean process(Request request) {
        // 如果超过1000元，处理不了，交下一个处理:
        if (request.getAmount().compareTo(BigDecimal.valueOf(1000)) > 0) {
            return null;
        }
        // 对Bob有偏见:
        return !request.getName().equalsIgnoreCase("bob");
    }
}
```

有了不同的`Handler`后，我们还要把这些`Handler`组合起来，变成一个链，并通过一个统一入口处理：

```java
public class HandlerChain {
    // 持有所有Handler:
    private List<Handler> handlers = new ArrayList<>();

    public void addHandler(Handler handler) {
        this.handlers.add(handler);
    }

    public boolean process(Request request) {
        // 依次调用每个Handler:
        for (Handler handler : handlers) {
            Boolean r = handler.process(request);
            if (r != null) {
                // 如果返回TRUE或FALSE，处理结束:
                System.out.println(request + " " + (r ? "Approved by " : "Denied by ") + handler.getClass().getSimpleName());
                return r;
            }
        }
        throw new RuntimeException("Could not handle request: " + request);
    }
}
```

现在，我们就可以在客户端组装出责任链，然后用责任链来处理请求：

```java
// 构造责任链:
HandlerChain chain = new HandlerChain();
chain.addHandler(new ManagerHandler());
chain.addHandler(new DirectorHandler());
chain.addHandler(new CEOHandler());
// 处理请求:
chain.process(new Request("Bob", new BigDecimal("123.45")));
chain.process(new Request("Alice", new BigDecimal("1234.56")));
chain.process(new Request("Bill", new BigDecimal("12345.67")));
chain.process(new Request("John", new BigDecimal("123456.78")));
```

责任链模式本身很容易理解，需要注意的是，`Handler`添加的顺序很重要，如果顺序不对，处理的结果可能就不是符合要求的。

此外，责任链模式有很多变种。有些责任链的实现方式是通过某个`Handler`手动调用下一个`Handler`来传递`Request`，例如：

```java
public class AHandler implements Handler {
    private Handler next;
    public void process(Request request) {
        if (!canProcess(request)) {
            // 手动交给下一个Handler处理:
            next.process(request);
        } else {
            ...
        }
    }
}
```

还有一些责任链模式，每个`Handler`都有机会处理`Request`，通常这种责任链被称为拦截器（Interceptor）或者过滤器（Filter），它的目的不是找到某个`Handler`处理掉`Request`，而是每个`Handler`都做一些工作，比如：

- 记录日志；
- 检查权限；
- 准备相关资源；
- ...

例如，JavaEE的Servlet规范定义的`Filter`就是一种责任链模式，它不但允许每个`Filter`都有机会处理请求，还允许每个`Filter`决定是否将请求“放行”给下一个`Filter`：

```java
public class AuditFilter implements Filter {
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
        log(req);
        if (check(req)) {
            // 放行:
            chain.doFilter(req, resp);
        } else {
            // 拒绝:
            sendError(resp);
        }
    }
}
```

这种模式不但允许一个`Filter`自行决定处理`ServletRequest`和`ServletResponse`，还可以“伪造”`ServletRequest`和`ServletResponse`以便让下一个`Filter`处理，能实现非常复杂的功能。

#### 命令

> 将一个请求封装成一个对象，从而使你可用不同的请求对客户进行参数化，对请求排队或记录请求日志，以及支持可撤销的操作。

命令模式的设计思想是把命令的创建和执行分离，使得调用者无需关心具体的执行过程。

通过封装`Command`对象，命令模式可以保存已执行的命令，从而支持撤销、重做等操作。

命令模式（Command）是指，把请求封装成一个命令，然后执行该命令。

在使用命令模式前，我们先以一个编辑器为例子，看看如何实现简单的编辑操作：

```java
public class TextEditor {
    private StringBuilder buffer = new StringBuilder();

    public void copy() {
        ...
    }

    public void paste() {
        String text = getFromClipBoard();
        add(text);
    }

    public void add(String s) {
        buffer.append(s);
    }

    public void delete() {
        if (buffer.length() > 0) {
            buffer.deleteCharAt(buffer.length() - 1);
        }
    }

    public String getState() {
        return buffer.toString();
    }
}
```

我们用一个`StringBuilder`模拟一个文本编辑器，它支持`copy()`、`paste()`、`add()`、`delete()`等方法。

正常情况，我们像这样调用`TextEditor`：

```java
TextEditor editor = new TextEditor();
editor.add("Command pattern in text editor.\n");
editor.copy();
editor.paste();
System.out.println(editor.getState());
```

这是直接调用方法，调用方需要了解`TextEditor`的所有接口信息。

如果改用命令模式，我们就要把调用方发送命令和执行方执行命令分开。怎么分？

解决方案是引入一个`Command`接口：

```java
public interface Command {
    void execute();
}
```

调用方创建一个对应的`Command`，然后执行，并不关心内部是如何具体执行的。

为了支持`CopyCommand`和`PasteCommand`这两个命令，我们从`Command`接口派生：

```java
public class CopyCommand implements Command {
    // 持有执行者对象:
    private TextEditor receiver;

    public CopyCommand(TextEditor receiver) {
        this.receiver = receiver;
    }

    public void execute() {
        receiver.copy();
    }
}

public class PasteCommand implements Command {
    private TextEditor receiver;

    public PasteCommand(TextEditor receiver) {
        this.receiver = receiver;
    }

    public void execute() {
        receiver.paste();
    }
}
```

最后我们把`Command`和`TextEditor`组装一下，客户端这么写：

```java
TextEditor editor = new TextEditor();
editor.add("Command pattern in text editor.\n");
// 执行一个CopyCommand:
Command copy = new CopyCommand(editor);
copy.execute();
editor.add("----\n");
// 执行一个PasteCommand:
Command paste = new PasteCommand(editor);
paste.execute();
System.out.println(editor.getState());
```

这就是命令模式的结构：

```ascii
┌──────┐      ┌───────┐
│Client│─ ─ ─>│Command│
└──────┘      └───────┘
                  │  ┌──────────────┐
                  ├─>│ CopyCommand  │
                  │  ├──────────────┤
                  │  │editor.copy() │─ ┐
                  │  └──────────────┘
                  │                    │  ┌────────────┐
                  │  ┌──────────────┐   ─>│ TextEditor │
                  └─>│ PasteCommand │  │  └────────────┘
                     ├──────────────┤
                     │editor.paste()│─ ┘
                     └──────────────┘
```

为什么搞了一大堆`Command`，多了好几个类，还不如直接这么写简单：

```java
TextEditor editor = new TextEditor();
editor.add("Command pattern in text editor.\n");
editor.copy();
editor.paste();
```

实际上，使用命令模式，确实增加了系统的复杂度。如果需求很简单，那么直接调用显然更直观而且更简单。

那么我们还需要命令模式吗？

答案是视需求而定。如果`TextEditor`复杂到一定程度，并且需要支持Undo、Redo的功能时，就需要使用命令模式，因为我们可以给每个命令增加`undo()`：

```java
public interface Command {
    void execute();
    void undo();
}
```

然后把执行的一系列命令用`List`保存起来，就既能支持Undo，又能支持Redo。这个时候，我们又需要一个`Invoker`对象，负责执行命令并保存历史命令：

```ascii
┌─────────────┐
│   Client    │
└─────────────┘
       │

       │
       ▼
┌─────────────┐
│   Invoker   │
├─────────────┤    ┌───────┐
│List commands│─ ─>│Command│
│invoke(c)    │    └───────┘
│undo()       │        │  ┌──────────────┐
└─────────────┘        ├─>│ CopyCommand  │
                       │  ├──────────────┤
                       │  │editor.copy() │─ ┐
                       │  └──────────────┘
                       │                    │  ┌────────────┐
                       │  ┌──────────────┐   ─>│ TextEditor │
                       └─>│ PasteCommand │  │  └────────────┘
                          ├──────────────┤
                          │editor.paste()│─ ┘
                          └──────────────┘
```

可见，模式带来的设计复杂度的增加是随着需求而增加的，它减少的是系统各组件的耦合度。

#### 解释器

> 给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。

解释器模式通过抽象语法树实现对用户输入的解释执行。

解释器模式的实现通常非常复杂，且一般只能解决一类特定问题。

解释器模式（Interpreter）是一种针对特定问题设计的一种解决方案。例如，匹配字符串的时候，由于匹配条件非常灵活，使得通过代码来实现非常不灵活。举个例子，针对以下的匹配条件：

- 以`+`开头的数字表示的区号和电话号码，如`+861012345678`；
- 以英文开头，后接英文和数字，并以.分隔的域名，如`www.lsaiah.cn`；
- 以`/`开头的文件路径，如`/path/to/file.txt`；
- ...

因此，需要一种通用的表示方法——正则表达式来进行匹配。正则表达式就是一个字符串，但要把正则表达式解析为语法树，然后再匹配指定的字符串，就需要一个解释器。

实现一个完整的正则表达式的解释器非常复杂，但是使用解释器模式却很简单：

```java
String s = "+861012345678";
System.out.println(s.matches("^\\+\\d+$"));
```

类似的，当我们使用JDBC时，执行的SQL语句虽然是字符串，但最终需要数据库服务器的SQL解释器来把SQL“翻译”成数据库服务器能执行的代码，这个执行引擎也非常复杂，但对于使用者来说，仅仅需要写出SQL字符串即可。

#### 迭代器

> 提供一种方法顺序访问一个聚合对象中的各个元素，而又不需要暴露该对象的内部表示。

Iterator模式常用于遍历集合，它允许集合提供一个统一的`Iterator`接口来遍历元素，同时保证调用者对集合内部的数据结构一无所知，从而使得调用者总是以相同的接口遍历各种不同类型的集合。

迭代器模式（Iterator）实际上在Java的集合类中已经广泛使用了。我们以`List`为例，要遍历`ArrayList`，即使我们知道它的内部存储了一个`Object[]`数组，也不应该直接使用数组索引去遍历，因为这样需要了解集合内部的存储结构。如果使用`Iterator`遍历，那么，`ArrayList`和`LinkedList`都可以以一种统一的接口来遍历：

```java
List<String> list = ...
for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
    String s = it.next();
}
```

实际上，因为Iterator模式十分有用，因此，Java允许我们直接把任何支持`Iterator`的集合对象用`foreach`循环写出来：

```java
List<String> list = ...
for (String s : list) {

}
```

然后由Java编译器完成Iterator模式的所有循环代码。

虽然我们对如何使用Iterator有了一定了解，但如何实现一个Iterator模式呢？我们以一个自定义的集合为例，通过Iterator模式实现倒序遍历：

```java
public class ReverseArrayCollection<T> implements Iterable<T> {
    // 以数组形式持有集合:
    private T[] array;

    public ReverseArrayCollection(T... objs) {
        this.array = Arrays.copyOfRange(objs, 0, objs.length);
    }

    public Iterator<T> iterator() {
        return ???;
    }
}
```

实现Iterator模式的关键是返回一个`Iterator`对象，该对象知道集合的内部结构，因为它可以实现倒序遍历。我们使用Java的内部类实现这个`Iterator`：

```java
public class ReverseArrayCollection<T> implements Iterable<T> {
    private T[] array;

    public ReverseArrayCollection(T... objs) {
        this.array = Arrays.copyOfRange(objs, 0, objs.length);
    }

    public Iterator<T> iterator() {
        return new ReverseIterator();
    }

    class ReverseIterator implements Iterator<T> {
        // 索引位置:
        int index;

        public ReverseIterator() {
            // 创建Iterator时,索引在数组末尾:
            this.index = ReverseArrayCollection.this.array.length;
        }

        public boolean hasNext() {
            // 如果索引大于0,那么可以移动到下一个元素(倒序往前移动):
            return index > 0;
        }

        public T next() {
            // 将索引移动到下一个元素并返回(倒序往前移动):
            index--;
            return array[index];
        }
    }
}
```

使用内部类的好处是内部类隐含地持有一个它所在对象的`this`引用，可以通过`ReverseArrayCollection.this`引用到它所在的集合。上述代码实现的逻辑非常简单，但是实际应用时，如果考虑到多线程访问，当一个线程正在迭代某个集合，而另一个线程修改了集合的内容时，是否能继续安全地迭代，还是抛出`ConcurrentModificationException`，就需要更仔细地设计。

#### 中介

> 用一个中介对象来封装一系列的对象交互。中介者使各个对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

中介模式是通过引入一个中介对象，把多边关系变成多个双边关系，从而简化系统组件的交互耦合度。

中介模式（Mediator）又称调停者模式，它的目的是把多方会谈变成双方会谈，从而实现多方的松耦合。

有些童鞋听到中介立刻想到房产中介，立刻气不打一处来。这个中介模式与房产中介还真有点像，所以消消气，先看例子。

考虑一个简单的点餐输入：

 汉堡

 鸡块

 薯条

 咖啡

选择全部 取消所有 反选

这个小系统有4个参与对象：

- 多选框；
- “选择全部”按钮；
- “取消所有”按钮；
- “反选”按钮。

它的复杂性在于，当多选框变化时，它会影响“选择全部”和“取消所有”按钮的状态（是否可点击），当用户点击某个按钮时，例如“反选”，除了会影响多选框的状态，它又可能影响“选择全部”和“取消所有”按钮的状态。

所以这是一个多方会谈，逻辑写起来很复杂：

```ascii
┌─────────────────┐     ┌─────────────────┐
│  CheckBox List  │<───>│SelectAll Button │
└─────────────────┘     └─────────────────┘
         ▲ ▲                     ▲
         │ └─────────────────────┤
         ▼                       │
┌─────────────────┐     ┌────────┴────────┐
│SelectNone Button│<────│ Inverse Button  │
└─────────────────┘     └─────────────────┘
```

如果我们引入一个中介，把多方会谈变成多个双方会谈，虽然多了一个对象，但对象之间的关系就变简单了：

```ascii
            ┌─────────────────┐
     ┌─────>│  CheckBox List  │
     │      └─────────────────┘
     │      ┌─────────────────┐
     │ ┌───>│SelectAll Button │
     ▼ ▼    └─────────────────┘
┌─────────┐
│Mediator │
└─────────┘
     ▲ ▲    ┌─────────────────┐
     │ └───>│SelectNone Button│
     │      └─────────────────┘
     │      ┌─────────────────┐
     └─────>│ Inverse Button  │
            └─────────────────┘
```

下面我们用中介模式来实现各个UI组件的交互。首先把UI组件给画出来：

```java
public class Main {
    public static void main(String[] args) {
        new OrderFrame("Hanburger", "Nugget", "Chip", "Coffee");
    }
}

class OrderFrame extends JFrame {
    public OrderFrame(String... names) {
        setTitle("Order");
        setSize(460, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        Container c = getContentPane();
        c.setLayout(new FlowLayout(FlowLayout.LEADING, 20, 20));
        c.add(new JLabel("Use Mediator Pattern"));
        List<JCheckBox> checkboxList = addCheckBox(names);
        JButton selectAll = addButton("Select All");
        JButton selectNone = addButton("Select None");
        selectNone.setEnabled(false);
        JButton selectInverse = addButton("Inverse Select");
        new Mediator(checkBoxList, selectAll, selectNone, selectInverse);
        setVisible(true);
    }

    private List<JCheckBox> addCheckBox(String... names) {
        JPanel panel = new JPanel();
        panel.add(new JLabel("Menu:"));
        List<JCheckBox> list = new ArrayList<>();
        for (String name : names) {
            JCheckBox checkbox = new JCheckBox(name);
            list.add(checkbox);
            panel.add(checkbox);
        }
        getContentPane().add(panel);
        return list;
    }

    private JButton addButton(String label) {
        JButton button = new JButton(label);
        getContentPane().add(button);
        return button;
    }
}
```

然后，我们设计一个Mediator类，它引用4个UI组件，并负责跟它们交互：

```java
public class Mediator {
    // 引用UI组件:
    private List<JCheckBox> checkBoxList;
    private JButton selectAll;
    private JButton selectNone;
    private JButton selectInverse;

    public Mediator(List<JCheckBox> checkBoxList, JButton selectAll, JButton selectNone, JButton selectInverse) {
        this.checkBoxList = checkBoxList;
        this.selectAll = selectAll;
        this.selectNone = selectNone;
        this.selectInverse = selectInverse;
        // 绑定事件:
        this.checkBoxList.forEach(checkBox -> {
            checkBox.addChangeListener(this::onCheckBoxChanged);
        });
        this.selectAll.addActionListener(this::onSelectAllClicked);
        this.selectNone.addActionListener(this::onSelectNoneClicked);
        this.selectInverse.addActionListener(this::onSelectInverseClicked);
    }

    // 当checkbox有变化时:
    public void onCheckBoxChanged(ChangeEvent event) {
        boolean allChecked = true;
        boolean allUnchecked = true;
        for (var checkBox : checkBoxList) {
            if (checkBox.isSelected()) {
                allUnchecked = false;
            } else {
                allChecked = false;
            }
        }
        selectAll.setEnabled(!allChecked);
        selectNone.setEnabled(!allUnchecked);
    }

    // 当点击select all:
    public void onSelectAllClicked(ActionEvent event) {
        checkBoxList.forEach(checkBox -> checkBox.setSelected(true));
        selectAll.setEnabled(false);
        selectNone.setEnabled(true);
    }

    // 当点击select none:
    public void onSelectNoneClicked(ActionEvent event) {
        checkBoxList.forEach(checkBox -> checkBox.setSelected(false));
        selectAll.setEnabled(true);
        selectNone.setEnabled(false);
    }

    // 当点击select inverse:
    public void onSelectInverseClicked(ActionEvent event) {
        checkBoxList.forEach(checkBox -> checkBox.setSelected(!checkBox.isSelected()));
        onCheckBoxChanged(null);
    }
}
```

使用Mediator模式后，我们得到了以下好处：

- 各个UI组件互不引用，这样就减少了组件之间的耦合关系；
- Mediator用于当一个组件发生状态变化时，根据当前所有组件的状态决定更新某些组件；
- 如果新增一个UI组件，我们只需要修改Mediator更新状态的逻辑，现有的其他UI组件代码不变。

Mediator模式经常用在有众多交互组件的UI上。为了简化UI程序，MVC模式以及MVVM模式都可以看作是Mediator模式的扩展。

#### 备忘录

> 在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。

备忘录Memento模式是为了保存对象的内部状态，并在将来恢复，大多数软件提供的保存、打开，以及编辑过程中的Undo、Redo都是备忘录模式的应用。

其实我们使用的几乎所有软件都用到了备忘录模式。最简单的备忘录模式就是保存到文件，打开文件。对于文本编辑器来说，保存就是把`TextEditor`类的字符串存储到文件，打开就是恢复`TextEditor`类的状态。对于图像编辑器来说，原理是一样的，只是保存和恢复的数据格式比较复杂而已。Java的序列化也可以看作是备忘录模式。

在使用文本编辑器的时候，我们还经常使用Undo、Redo这些功能。这些其实也可以用备忘录模式实现，即不定期地把`TextEditor`类的字符串复制一份存起来，这样就可以Undo或Redo。

标准的备忘录模式有这么几种角色：

- Memonto：存储的内部状态；
- Originator：创建一个备忘录并设置其状态；
- Caretaker：负责保存备忘录。

实际上我们在使用备忘录模式的时候，不必设计得这么复杂，只需要对类似`TextEditor`的类，增加`getState()`和`setState()`就可以了。

我们以一个文本编辑器`TextEditor`为例，它内部使用`StringBuilder`允许用户增删字符：

```java
public class TextEditor {
    private StringBuilder buffer = new StringBuilder();

    public void add(char ch) {
        buffer.append(ch);
    }

    public void add(String s) {
        buffer.append(s);
    }

    public void delete() {
        if (buffer.length() > 0) {
            buffer.deleteCharAt(buffer.length() - 1);
        }
    }
}
```

为了支持这个`TextEditor`能保存和恢复状态，我们增加`getState()`和`setState()`两个方法：

```java
public class TextEditor {
    ...

    // 获取状态:
    public String getState() {
        return buffer.toString();
    }

    // 恢复状态:
    public void setState(String state) {
        this.buffer.delete(0, this.buffer.length());
        this.buffer.append(state);
    }
}
```

对这个简单的文本编辑器，用一个`String`就可以表示其状态，对于复杂的对象模型，通常我们会使用JSON、XML等复杂格式。

#### 观察者

> 定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

观察者Observer模式，又称发布-订阅模式，是一种一对多的通知机制，使得双方无需关心对方，只关心通知本身。

假设一个电商网站，有多种`Product`（商品），同时，`Customer`（消费者）和`Admin`（管理员）对商品上架、价格改变都感兴趣，希望能第一时间获得通知。于是，`Store`（商场）可以这么写：

```java
public class Store {
    Customer customer;
    Admin admin;

    private Map<String, Product> products = new HashMap<>();

    public void addNewProduct(String name, double price) {
        Product p = new Product(name, price);
        products.put(p.getName(), p);
        // 通知用户:
        customer.onPublished(p);
        // 通知管理员:
        admin.onPublished(p);
    }

    public void setProductPrice(String name, double price) {
        Product p = products.get(name);
        p.setPrice(price);
        // 通知用户:
        customer.onPriceChanged(p);
        // 通知管理员:
        admin.onPriceChanged(p);
    }
}
```

我们观察上述`Store`类的问题：它直接引用了`Customer`和`Admin`。先不考虑多个`Customer`或多个`Admin`的问题，上述`Store`类最大的问题是，如果要加一个新的观察者类型，例如工商局管理员，`Store`类就必须继续改动。

因此，上述问题的本质是`Store`希望发送通知给那些关心`Product`的对象，但`Store`并不想知道这些人是谁。观察者模式就是要分离被观察者和观察者之间的耦合关系。

要实现这一目标也很简单，`Store`不能直接引用`Customer`和`Admin`，相反，它引用一个`ProductObserver`接口，任何人想要观察`Store`，只要实现该接口，并且把自己注册到`Store`即可：

```java
public class Store {
    private List<ProductObserver> observers = new ArrayList<>();
    private Map<String, Product> products = new HashMap<>();

    // 注册观察者:
    public void addObserver(ProductObserver observer) {
        this.observers.add(observer);
    }

    // 取消注册:
    public void removeObserver(ProductObserver observer) {
        this.observers.remove(observer);
    }

    public void addNewProduct(String name, double price) {
        Product p = new Product(name, price);
        products.put(p.getName(), p);
        // 通知观察者:
        observers.forEach(o -> o.onPublished(p));
    }

    public void setProductPrice(String name, double price) {
        Product p = products.get(name);
        p.setPrice(price);
        // 通知观察者:
        observers.forEach(o -> o.onPriceChanged(p));
    }
}
```

就是这么一个小小的改动，使得观察者类型就可以无限扩充，而且，观察者的定义可以放到客户端：

```java
// observer:
Admin a = new Admin();
Customer c = new Customer();
// store:
Store store = new Store();
// 注册观察者:
store.addObserver(a);
store.addObserver(c);
```

甚至可以注册匿名观察者：

```java
store.addObserver(new ProductObserver() {
    public void onPublished(Product product) {
        System.out.println("[Log] on product published: " + product);
    }

    public void onPriceChanged(Product product) {
        System.out.println("[Log] on product price changed: " + product);
    }
});
```

用一张图画出观察者模式：

```ascii
┌─────────┐      ┌───────────────┐
│  Store  │─ ─ ─>│ProductObserver│
└─────────┘      └───────────────┘
     │                   ▲
                         │
     │             ┌─────┴─────┐
     ▼             │           │
┌─────────┐   ┌─────────┐ ┌─────────┐
│ Product │   │  Admin  │ │Customer │ ...
└─────────┘   └─────────┘ └─────────┘
```

观察者模式也有很多变体形式。有的观察者模式把被观察者也抽象出接口：

```java
public interface ProductObservable { // 注意此处拼写是Observable不是Observer!
    void addObserver(ProductObserver observer);
    void removeObserver(ProductObserver observer);
}
```

对应的实体被观察者就要实现该接口：

```java
public class Store implements ProductObservable {
    ...
}
```

有些观察者模式把通知变成一个Event对象，从而不再有多种方法通知，而是统一成一种：

```java
public interface ProductObserver {
    void onEvent(ProductEvent event);
}
```

让观察者自己从Event对象中读取通知类型和通知数据。

广义的观察者模式包括所有消息系统。所谓消息系统，就是把观察者和被观察者完全分离，通过消息系统本身来通知：

```ascii
                 ┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
                   Messaging System
                 │                       │
                   ┌──────────────────┐
              ┌──┼>│Topic:newProduct  │──┼─┐    ┌─────────┐
              │    └──────────────────┘    ├───>│ConsumerA│
┌─────────┐   │  │ ┌──────────────────┐  │ │    └─────────┘
│Producer │───┼───>│Topic:priceChanged│────┘
└─────────┘   │  │ └──────────────────┘  │
              │    ┌──────────────────┐         ┌─────────┐
              └──┼>│Topic:soldOut     │──┼─────>│ConsumerB│
                   └──────────────────┘         └─────────┘
                 └ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘
```

消息发送方称为Producer，消息接收方称为Consumer，Producer发送消息的时候，必须选择发送到哪个Topic。Consumer可以订阅自己感兴趣的Topic，从而只获得特定类型的消息。

使用消息系统实现观察者模式时，Producer和Consumer甚至经常不在同一台机器上，并且双方对对方完全一无所知，因为注册观察者这个动作本身都在消息系统中完成，而不是在Producer内部完成。

此外，注意到我们在编写观察者模式的时候，通知Observer是依靠语句：

```java
observers.forEach(o -> o.onPublished(p));
```

这说明各个观察者是依次获得的同步通知，如果上一个观察者处理太慢，会导致下一个观察者不能及时获得通知。此外，如果观察者在处理通知的时候，发生了异常，还需要被观察者处理异常，才能保证继续通知下一个观察者。

思考：如何改成异步通知，使得所有观察者可以并发同时处理？

Java标准库有个`java.util.Observable`类和一个`Observer`接口，用来帮助我们实现观察者模式。但是，这个类非常不好用，实现观察者模式的时候，也不推荐借助他们。

#### 状态

> 允许一个对象在其内部状态改变时改变它的行为。对象看起来似乎修改了她的类。

状态State模式的设计思想是把不同状态的逻辑分离到不同的状态类中，从而使得增加新状态更容易；

状态模式的实现关键在于状态转换。简单的状态转换可以直接由调用方指定，复杂的状态转换可以在内部根据条件触发完成。

什么是状态？我们以QQ聊天为例，一个用户的QQ有几种状态：

- 离线状态（尚未登录）；
- 正在登录状态；
- 在线状态；
- 忙状态（暂时离开）。

如何表示状态？我们定义一个`enum`就可以表示不同的状态。但不同的状态需要对应不同的行为，比如收到消息时：

```java
if (state == ONLINE) {
    // 闪烁图标
} else if (state == BUSY) {
    reply("现在忙，稍后回复");
} else if ...
```

状态模式的目的是为了把上述一大串`if...else...`的逻辑给分拆到不同的状态类中，使得将来增加状态比较容易。

例如，我们设计一个聊天机器人，它有两个状态：

- 未连线；
- 已连线。

对于未连线状态，我们收到消息也不回复：

```java
public class DisconnectedState implements State {
    public String init() {
        return "Bye!";
    }

    public String reply(String input) {
        return "";
    }
}
```

对于已连线状态，我们回应收到的消息：

```java
public class ConnectedState implements State {
    public String init() {
        return "Hello, I'm Bob.";
    }

    public String reply(String input) {
        if (input.endsWith("?")) {
            return "Yes. " + input.substring(0, input.length() - 1) + "!";
        }
        if (input.endsWith(".")) {
            return input.substring(0, input.length() - 1) + "!";
        }
        return input.substring(0, input.length() - 1) + "?";
    }
}
```

状态模式的关键设计思想在于状态切换，我们引入一个`BotContext`完成状态切换：

```java
public class BotContext {
	private State state = new DisconnectedState();

	public String chat(String input) {
		if ("hello".equalsIgnoreCase(input)) {
            // 收到hello切换到在线状态:
			state = new ConnectedState();
			return state.init();
		} else if ("bye".equalsIgnoreCase(input)) {
            /  收到bye切换到离线状态:
			state = new DisconnectedState();
			return state.init();
		}
		return state.reply(input);
	}
}
```

这样，一个价值千万的AI聊天机器人就诞生了：

```java
Scanner scanner = new Scanner(System.in);
BotContext bot = new BotContext();
for (;;) {
    System.out.print("> ");
    String input = scanner.nextLine();
    String output = bot.chat(input);
    System.out.println(output.isEmpty() ? "(no reply)" : "< " + output);
}
```

试试效果：

```java
> hello
< Hello, I'm Bob.
> Nice to meet you.
< Nice to meet you!
> Today is cold?
< Yes. Today is cold!
> bye
< Bye!
```

#### 策略

> 定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换，本模式使得算法可独立于使用它的客户而变化。

策略Strategy模式是为了允许调用方选择一个算法，从而通过不同策略实现不同的计算结果。通过扩展策略，不必修改主逻辑，即可获得新策略的结果。

策略模式在Java标准库中应用非常广泛，我们以排序为例，看看如何通过`Arrays.sort()`实现忽略大小写排序：

```java
import java.util.Arrays;
public class Main {
    public static void main(String[] args) throws InterruptedException {
        String[] array = { "apple", "Pear", "Banana", "orange" };
        Arrays.sort(array, String::compareToIgnoreCase);
        System.out.println(Arrays.toString(array));
    }
}
```

如果我们想忽略大小写排序，就传入`String::compareToIgnoreCase`，如果我们想倒序排序，就传入`(s1, s2) -> -s1.compareTo(s2)`，这个比较两个元素大小的算法就是策略。

我们观察`Arrays.sort(T[] a, Comparator<? super T> c)`这个排序方法，它在内部实现了TimSort排序，但是，排序算法在比较两个元素大小的时候，需要借助我们传入的`Comparator`对象，才能完成比较。因此，这里的策略是指比较两个元素大小的策略，可以是忽略大小写比较，可以是倒序比较，也可以根据字符串长度比较。

因此，上述排序使用到了策略模式，它实际上指，在一个方法中，流程是确定的，但是，某些关键步骤的算法依赖调用方传入的策略，这样，传入不同的策略，即可获得不同的结果，大大增强了系统的灵活性。

如果我们自己实现策略模式的排序，用冒泡法编写如下：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) throws InterruptedException {
        String[] array = { "apple", "Pear", "Banana", "orange" };
        sort(array, String::compareToIgnoreCase);
        System.out.println(Arrays.toString(array));
    }

    static <T> void sort(T[] a, Comparator<? super T> c) {
        for (int i = 0; i < a.length - 1; i++) {
            for (int j = 0; j < a.length - 1 - i; j++) {
                if (c.compare(a[j], a[j + 1]) > 0) { // 注意这里比较两个元素的大小依赖传入的策略
                    T temp = a[j];
                    a[j] = a[j + 1];
                    a[j + 1] = temp;
                }
            }
        }
    }
}

```

一个完整的策略模式要定义策略以及使用策略的上下文。我们以购物车结算为例，假设网站针对普通会员、Prime会员有不同的折扣，同时活动期间还有一个满100减20的活动，这些就可以作为策略实现。先定义打折策略接口：

```java
public interface DiscountStrategy {
    // 计算折扣额度:
    BigDecimal getDiscount(BigDecimal total);
}
```

接下来，就是实现各种策略。普通用户策略如下：

```java
public class UserDiscountStrategy implements DiscountStrategy {
    public BigDecimal getDiscount(BigDecimal total) {
        // 普通会员打九折:
        return total.multiply(new BigDecimal("0.1")).setScale(2, RoundingMode.DOWN);
    }
}
```

满减策略如下：

```java
public class OverDiscountStrategy implements DiscountStrategy {
    public BigDecimal getDiscount(BigDecimal total) {
        // 满100减20优惠:
        return total.compareTo(BigDecimal.valueOf(100)) >= 0 ? BigDecimal.valueOf(20) : BigDecimal.ZERO;
    }
}
```

最后，要应用策略，我们需要一个`DiscountContext`：

```java
public class DiscountContext {
    // 持有某个策略:
    private DiscountStrategy strategy = new UserDiscountStrategy();

    // 允许客户端设置新策略:
    public void setStrategy(DiscountStrategy strategy) {
        this.strategy = strategy;
    }

    public BigDecimal calculatePrice(BigDecimal total) {
        return total.subtract(this.strategy.getDiscount(total)).setScale(2);
    }
}
```

调用方必须首先创建一个DiscountContext，并指定一个策略（或者使用默认策略），即可获得折扣后的价格：

```java
DiscountContext ctx = new DiscountContext();

// 默认使用普通会员折扣:
BigDecimal pay1 = ctx.calculatePrice(BigDecimal.valueOf(105));
System.out.println(pay1);

// 使用满减折扣:
ctx.setStrategy(new OverDiscountStrategy());
BigDecimal pay2 = ctx.calculatePrice(BigDecimal.valueOf(105));
System.out.println(pay2);

// 使用Prime会员折扣:
ctx.setStrategy(new PrimeDiscountStrategy());
BigDecimal pay3 = ctx.calculatePrice(BigDecimal.valueOf(105));
System.out.println(pay3);
```

上述完整的策略模式如下图所示：

```ascii
┌───────────────┐      ┌─────────────────┐
│DiscountContext│─ ─ ─>│DiscountStrategy │
└───────────────┘      └─────────────────┘
                                ▲
                                │ ┌─────────────────────┐
                                ├─│UserDiscountStrategy │
                                │ └─────────────────────┘
                                │ ┌─────────────────────┐
                                ├─│PrimeDiscountStrategy│
                                │ └─────────────────────┘
                                │ ┌─────────────────────┐
                                └─│OverDiscountStrategy │
                                  └─────────────────────┘
```

策略模式的核心思想是在一个计算方法中把容易变化的算法抽出来作为“策略”参数传进去，从而使得新增策略不必修改原有逻辑。

#### 模板方法

> 定义一个操作中的算法的骨架，而将一些步骤延迟到子类中，使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

模板方法Template Method是一种高层定义骨架，底层实现细节的设计模式，适用于流程固定，但某些步骤不确定或可替换的情况。

它的主要思想是，定义一个操作的一系列步骤，对于某些暂时确定不下来的步骤，就留给子类去实现好了，这样不同的子类就可以定义出不同的步骤。

因此，模板方法的核心在于定义一个“骨架”。我们还是举例说明。

假设我们开发了一个从数据库读取设置的类：

```java
public class Setting {
    public final String getSetting(String key) {
        String value = readFromDatabase(key);
        return value;
    }

	private String readFromDatabase(String key) {
        // TODO: 从数据库读取
    }
}
```

由于从数据库读取数据较慢，我们可以考虑把读取的设置缓存起来，这样下一次读取同样的key就不必再访问数据库了。但是怎么实现缓存，暂时没想好，但不妨碍我们先写出使用缓存的代码：

```java
public class Setting {
    public final String getSetting(String key) {
        // 先从缓存读取:
        String value = lookupCache(key);
        if (value == null) {
            // 在缓存中未找到,从数据库读取:
            value = readFromDatabase(key);
            System.out.println("[DEBUG] load from db: " + key + " = " + value);
            // 放入缓存:
            putIntoCache(key, value);
        } else {
            System.out.println("[DEBUG] load from cache: " + key + " = " + value);
        }
        return value;
    }
}
```

整个流程没有问题，但是，`lookupCache(key)`和`putIntoCache(key, value)`这两个方法还根本没实现，怎么编译通过？这个不要紧，我们声明抽象方法就可以：

```java
public abstract class AbstractSetting {
    public final String getSetting(String key) {
        String value = lookupCache(key);
        if (value == null) {
            value = readFromDatabase(key);
            putIntoCache(key, value);
        }
        return value;
    }

    protected abstract String lookupCache(String key);

    protected abstract void putIntoCache(String key, String value);
}
```

因为声明了抽象方法，自然整个类也必须是抽象类。如何实现`lookupCache(key)`和`putIntoCache(key, value)`这两个方法就交给子类了。子类其实并不关心核心代码`getSetting(key)`的逻辑，它只需要关心如何完成两个小小的子任务就可以了。

假设我们希望用一个`Map`做缓存，那么可以写一个`LocalSetting`：

```java
public class LocalSetting extends AbstractSetting {
    private Map<String, String> cache = new HashMap<>();

    protected String lookupCache(String key) {
        return cache.get(key);
    }

    protected void putIntoCache(String key, String value) {
        cache.put(key, value);
    }
}
```

如果我们要使用Redis做缓存，那么可以再写一个`RedisSetting`：

```java
public class RedisSetting extends AbstractSetting {
    private RedisClient client = RedisClient.create("redis://localhost:6379");

    protected String lookupCache(String key) {
        try (StatefulRedisConnection<String, String> connection = client.connect()) {
            RedisCommands<String, String> commands = connection.sync();
            return commands.get(key);
        }
    }

    protected void putIntoCache(String key, String value) {
        try (StatefulRedisConnection<String, String> connection = client.connect()) {
            RedisCommands<String, String> commands = connection.sync();
            commands.set(key, value);
        }
    }
}
```

客户端代码使用本地缓存的代码这么写：

```java
AbstractSetting setting1 = new LocalSetting();
System.out.println("test = " + setting1.getSetting("test"));
System.out.println("test = " + setting1.getSetting("test"));
```

要改成Redis缓存，只需要把`LocalSetting`替换为`RedisSetting`：

```java
AbstractSetting setting2 = new RedisSetting();
System.out.println("autosave = " + setting2.getSetting("autosave"));
System.out.println("autosave = " + setting2.getSetting("autosave"));
```

可见，模板方法的核心思想是：父类定义骨架，子类实现某些细节。

为了防止子类重写父类的骨架方法，可以在父类中对骨架方法使用`final`。对于需要子类实现的抽象方法，一般声明为`protected`，使得这些方法对外部客户端不可见。

Java标准库也有很多模板方法的应用。在集合类中，`AbstractList`和`AbstractQueuedSynchronizer`都定义了很多通用操作，子类只需要实现某些必要方法。

#### 访问者

> 表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

访问者模式是为了抽象出作用于一组复杂对象的操作，并且后续可以新增操作而不必对现有的对象结构做任何改动。

访问者模式（Visitor）是一种操作一组对象的操作，它的目的是不改变对象的定义，但允许新增不同的访问者，来定义新的操作。

访问者模式的设计比较复杂，如果我们查看GoF原始的访问者模式，它是这么设计的：

```ascii
   ┌─────────┐       ┌───────────────────────┐
   │ Client  │─ ─ ─ >│        Visitor        │
   └─────────┘       ├───────────────────────┤
        │            │visitElementA(ElementA)│
                     │visitElementB(ElementB)│
        │            └───────────────────────┘
                                 ▲
        │                ┌───────┴───────┐
                         │               │
        │         ┌─────────────┐ ┌─────────────┐
                  │  VisitorA   │ │  VisitorB   │
        │         └─────────────┘ └─────────────┘
        ▼
┌───────────────┐        ┌───────────────┐
│ObjectStructure│─ ─ ─ ─>│    Element    │
├───────────────┤        ├───────────────┤
│handle(Visitor)│        │accept(Visitor)│
└───────────────┘        └───────────────┘
                                 ▲
                        ┌────────┴────────┐
                        │                 │
                ┌───────────────┐ ┌───────────────┐
                │   ElementA    │ │   ElementB    │
                ├───────────────┤ ├───────────────┤
                │accept(Visitor)│ │accept(Visitor)│
                │doA()          │ │doB()          │
                └───────────────┘ └───────────────┘
```

上述模式的复杂之处在于上述访问者模式为了实现所谓的“双重分派”，设计了一个回调再回调的机制。因为Java只支持基于多态的单分派模式，这里强行模拟出“双重分派”反而加大了代码的复杂性。

这里我们只介绍简化的访问者模式。假设我们要递归遍历某个文件夹的所有子文件夹和文件，然后找出`.java`文件，正常的做法是写个递归：

```java
void scan(File dir, List<File> collector) {
    for (File file : dir.listFiles()) {
        if (file.isFile() && file.getName().endsWith(".java")) {
            collector.add(file);
        } else if (file.isDir()) {
            // 递归调用:
            scan(file, collector);
        }
    }
}
```

上述代码的问题在于，扫描目录的逻辑和处理.java文件的逻辑混在了一起。如果下次需要增加一个清理`.class`文件的功能，就必须再重复写扫描逻辑。

因此，访问者模式先把数据结构（这里是文件夹和文件构成的树型结构）和对其的操作（查找文件）分离开，以后如果要新增操作（例如清理`.class`文件），只需要新增访问者，不需要改变现有逻辑。

用访问者模式改写上述代码步骤如下：

首先，我们需要定义访问者接口，即该访问者能够干的事情：

```java
public interface Visitor {
    // 访问文件夹:
    void visitDir(File dir);
    // 访问文件:
    void visitFile(File file);
}
```

紧接着，我们要定义能持有文件夹和文件的数据结构`FileStructure`：

```java
public class FileStructure {
    // 根目录:
    private File path;
    public FileStructure(File path) {
        this.path = path;
    }
}
```

然后，我们给`FileStructure`增加一个`handle()`方法，传入一个访问者：

```java
public class FileStructure {
    ...

    public void handle(Visitor visitor) {
		scan(this.path, visitor);
	}

	private void scan(File file, Visitor visitor) {
		if (file.isDirectory()) {
            // 让访问者处理文件夹:
			visitor.visitDir(file);
			for (File sub : file.listFiles()) {
                // 递归处理子文件夹:
				scan(sub, visitor);
			}
		} else if (file.isFile()) {
            // 让访问者处理文件:
			visitor.visitFile(file);
		}
	}
}
```

这样，我们就把访问者的行为抽象出来了。如果我们要实现一种操作，例如，查找`.java`文件，就传入`JavaFileVisitor`：

```java
FileStructure fs = new FileStructure(new File("."));
fs.handle(new JavaFileVisitor());
```

这个`JavaFileVisitor`实现如下：

```java
public class JavaFileVisitor implements Visitor {
    public void visitDir(File dir) {
        System.out.println("Visit dir: " + dir);
    }

    public void visitFile(File file) {
        if (file.getName().endsWith(".java")) {
            System.out.println("Found java file: " + file);
        }
    }
}
```

类似的，如果要清理`.class`文件，可以再写一个`ClassFileClearnerVisitor`：

```java
public class ClassFileCleanerVisitor implements Visitor {
	public void visitDir(File dir) {
	}

	public void visitFile(File file) {
		if (file.getName().endsWith(".class")) {
			System.out.println("Will clean class file: " + file);
		}
	}
}
```

可见，访问者模式的核心思想是为了访问比较复杂的数据结构，不去改变数据结构，而是把对数据的操作抽象出来，在“访问”的过程中以回调形式在访问者中处理操作逻辑。如果要新增一组操作，那么只需要增加一个新的访问者。

实际上，Java标准库提供的`Files.walkFileTree()`已经实现了一个访问者模式：

```java
import java.io.*;
import java.nio.file.*;
import java.nio.file.attribute.*;
public class Main {
    public static void main(String[] args) throws IOException {
        Files.walkFileTree(Paths.get("."), new MyFileVisitor());
    }
}

// 实现一个FileVisitor:
class MyFileVisitor extends SimpleFileVisitor<Path> {
    // 处理Directory:
    public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs) throws IOException {
        System.out.println("pre visit dir: " + dir);
        // 返回CONTINUE表示继续访问:
        return FileVisitResult.CONTINUE;
    }

    // 处理File:
    public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {
        System.out.println("visit file: " + file);
        // 返回CONTINUE表示继续访问:
        return FileVisitResult.CONTINUE;
    }
}

```

`Files.walkFileTree()`允许访问者返回`FileVisitResult.CONTINUE`以便继续访问，或者返回`FileVisitResult.TERMINATE`停止访问。

类似的，对XML的SAX处理也是一个访问者模式，我们需要提供一个SAX Handler作为访问者处理XML的各个节点。
