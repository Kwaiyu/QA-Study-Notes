Java是SUN公司的詹姆斯·高斯林在90年代初开发的一种编程语言后来被Oracle收购，随着互联网的高速发展，Java逐渐成为最重要的编程语言。对于软件测试来说，掌握一两门编程语言已是必要条件，具体学习哪门编程语言还需根据自己的工作需求以及兴趣爱好来考虑。下图是TIOBE统计的编程语言长期历史排名：

![img](https://qwq.lsaiah.cn/usr/uploads/Picture/tiobe.png)

## 基础语法

### 二维数组

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

```
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

### 构造方法、封装

- 实例在创建时通过`new`操作符会调用其对应的构造方法，构造方法用于初始化实例，方法名就是类名，没有返回值；
- 没有定义构造方法时，编译器会自动创建一个默认的无参数构造方法；
- 可以定义多个构造方法，编译器根据参数自动判断；
- 可以在一个构造方法内部调用另一个构造方法，便于代码复用。

```
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

```
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

```
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
package com.lsaiah.java;

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

### 多态

- 子类可以重写父类的方法（Override），重写在子类中改变了父类方法的行为；
- Java的方法调用总是作用于运行期对象的实际类型，这种行为称为多态；
- `final`修饰符有多种作用：
  - `final`修饰的方法可以阻止被重写；
  - `final`修饰的class可以阻止被继承；
  - `final`修饰的field必须在创建对象时初始化，随后不可修改。

```
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

```
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

```
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

- 静态字段属于所有实例“共享”的字段，实际上是属于`class`的字段；
- 调用静态方法不需要实例，无法访问`this`，但可以访问静态字段和其他静态方法；
- 静态方法常用于工具类和辅助方法，如Arrays.sort();。

```
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
- JDK的核心类使用`java.lang`包，编译器会自动导入；
- JDK的其它常用类定义在`java.util.*`，`java.math.*`，`java.text.*`，……；
- 包名推荐使用倒置的域名，例如`org.apache`。
- 使用完整类名或使用import语句导入
- JAVA 提供API：https://docs.oracle.com/javase/8/docs/api/

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


| 基础数据类型 | 引用类型            |
| :----------- | :------------------ |
| byte         | java.lang.Byte      |
| short        | java.lang.Short     |
| int          | java.lang.Integer   |
| long         | java.lang.Long      |
| float        | java.lang.Float     |
| double       | java.lang.Double    |
| boolean      | java.lang.Boolean   |
| char         | java.lang.Character |

- Java核心库提供的包装类型可以把基本类型包装为`class`；
- 自动装箱和自动拆箱都是在编译期完成的（JDK>=1.5）；
- 装箱和拆箱会影响执行效率，装箱是将基础数据类型变为引用类型的赋值，拆箱是将引用类型变为基础数据类型可能发生`NullPointerException`；
- 包装类型的比较必须使用`equals()`；
- 整数和浮点数的包装类型都继承自`Number`；
- 包装类型提供了大量实用方法。

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
**示例：**

```
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
        OuterClass.InnerClass myInner = myOuter.new InnerClass();
        System.out.println(myInner.y + myOuter.x);
        System.out.println(myInner.myInnerMethod());
    }
}
```

当Private修饰内部类时，外部对象将无法访问内部类。
当Static修饰内部类时，可以不创建外部类对象访问内部类，无法访问外部类的成员。

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

```
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

- `List`是按索引顺序访问的长度可变的有序表，允许`null`元素和重复元素。优先使用`ArrayList`而不是`LinkedList`；
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

Documents/
&nbsp&nbsp word/
&nbsp&nbsp&nbsp&nbsp 1.docx
&nbsp&nbsp&nbsp&nbsp 2.docx
&nbsp&nbsp&nbsp&nbsp work/
&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp abc.doc
&nbsp&nbsp ppt/
&nbsp&nbsp other/

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

## 错题集

- 多维数组求平均数

```java
package com.lsaiah.java;

public class Main {
    public static void main(String[] args) {
        // 用二维数组表示的学生成绩:
        int[][] scores = {
                {82, 90, 91},
                {68, 72, 64},
                {95, 91, 89},
                {67, 52, 60},
                {79, 81, 85},
        };
        //外循环控制一维数组
        for(int i=1;i<scores.length;i++){
            double sum = 0;//定义一个初始化sum
            //内循环控制元素里面的数组
            for(var j=0;j<scores[i].length;j++){
                sum+=scores[i][j];//每个元素里面的数组相加
            }
            double avg = sum /scores[i].length;//求每个元素的平均数
            System.out.println("第"+ i +"班的平均成绩是："+avg);//输出每个元素的平均数
        }
    }
}
```

## 加密与安全

### 编码算法

- URL编码和Base64编码都是编码算法，它们不是加密算法；
- URL编码的目的是把任意文本数据编码为%前缀表示的文本，便于浏览器和服务器处理；

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
public class Main {
	public static void main(String[] args){
		Thread t = new Thread();
		t.start();
	}
}
```

当线程启动后希望新线程执行指定的代码有以下几种方法：

- 方法一：从`Thread`派生一个自定义类，然后覆写`run()`方法：

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

  ```
  public void set(String s) {
  	this.value = s;
  }
  ```


  多行赋值语句，this引用必须同步

  ```
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

  ```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

### 使用Concurrent集合

- 使用`java.util.concurrent`包提供的线程安全的并发集合可以大大简化多线程编程：
- 多线程同时读写并发集合是安全的；
- 尽量使用Java标准库提供的并发集合，避免自己编写同步代码。

通过`ReentrantLock`和`Condition`实现`BlockingQueue`，当一个线程调用TaskQueue的`getTask()`方法时，方法内部可能会让线程变成等待状态，直到队列满足条件不为空线程被唤醒后，`getTask()`方法才会返回。

```
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

```
Map<String, String> map = new ConcurrentHashMap<>();
// 在不听线程读写：
map.put("A","1");
map.put("B","2");
map.get("A","1");
```

`java.util.Collections`工具类还提供旧的线程安全集合转换器，但通过`synchronized`加锁性能比`java.util.concurrent`集合要低很多。

```
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

```
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

```
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

```
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

```
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

```
int min = 4;
int max = 10;
ExecutorSerice es = new ThreadPoolExecutor(min, max, 60L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());
```

#### ScheduledThreadPool

还有一种需要定期反复执行的任务，可使用`ScheduledThreadPool`。

创建一个`ScheduledThreadPool`是通过`Executors`类：

```
ScheduledExecutorService ses = Executors.newScheduledThreadPool(4);
```

提交一次性任务，在指定延迟后只执行一次：

```
// 1秒后执行一次性任务
ses.schedule(new Task("one-time"), 1, TimeUnit.SECONDS);
```

任务固定每3秒执行：

```
// 2秒后开始执行定时任务，每3秒执行:
ses.scheduleAtFixedRate(new Task("fixed-rate"), 2, 3, TimeUnit.SECONDS);
```

任务固定3秒为间隔时间执行：

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
message.setText(body, "UTF-8");
改为：
message.setText(body, "UTF-8", "html");
```

#### 发送附件

发送附件不能直接调用`message.setText()`方法，而是要构造一个`Multipart`对象， 一个`Multipart`对象可以添加若干个`BodyPart`，其中第一个`BodyPart`是文本，即邮件正文，后面的BodyPart是附件。`BodyPart`依靠`setContent()`决定添加的内容，如果添加文本，用`setContent("...", "text/plain;charset=utf-8")`添加纯文本，或者用`setContent("...", "text/html;charset=utf-8")`添加HTML文本。如果添加附件，需要设置文件名（不一定和真实文件名一致），并且添加一个`DataHandler()`，传入文件的MIME类型。二进制文件可以用`application/octet-stream`，Word文档则是`application/msword`。最后，通过`setContent()`把`Multipart`添加到`Message`中即可发送。

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
POST /login HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 30

username=hello&password=123456
```

**HTTP响应**

响应也是由Header和Body组成，第一行总是HTTP版本 响应代码 响应说明。HTTP/1.1协议允许在一个TCP连接中反复发送-响应，但发送一个请求后必须等待服务器响应才能发送下一个请求，HTTP/2.0允许客户端在没有收到响应的时候，发送多个HTTP请求，服务器返回响应的时候，不一定按顺序返回，只要双方能识别出哪个响应对应哪个请求，就可以做到并行发送和接收。

```
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

```
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

```
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

```
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



### RMI远程调用

- Java提供了RMI实现远程方法调用：
- RMI通过自动生成stub和skeleton实现网络调用，客户端只需要查找服务并获得接口实例，服务器端只需要编写实现类并注册为服务；
- RMI的序列化和反序列化可能会造成安全漏洞，因此调用双方必须是内网互相信任的机器，不要把1099端口暴露在公网上作为对外服务。

RMI是Remote Method Invocation的缩写，是指一个JVM中的代码可以通过网络实现远程调用另一个JVM的某个方法。要实现RMI服务端和客户端必须共享同一接口，此接口必须派生自`java.rmi.Remote`，并在每个方法声明抛出`RemoteException`。

```
public interface WorldClock extends Remote {
    LocalDateTime getLocalDateTime(String zoneId) throws RemoteException;
}
```

服务器实现类：

```
public class WorldClockService implements WorldClock {
    @Override
    public LocalDateTime getLocalDateTime(String zoneId) throws RemoteException {
        return LocalDateTime.now(ZoneId.of(zoneId)).withNano(0);
    }
}
```

通过Java RMI提供的一系列底层接口把编写的服务以RMI形式暴露在网络上给客户端调用：

```
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

```
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

```
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

```
InputStream input = Main.class.getResourceAsStream("/book.xml");
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
DocumentBuilder db = dbf.newDocumentBuilder();
Document doc = db.parse(input);
```

这个对象代表了整个XML文档的树形结构，需要遍历以便读取指定元素的值：

```
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

```
InputStream input = Main.class.getResourceAsStream("/book.xml");
SAXParserFactory spf = SAXParserFactory.newInstance();
SAXParser saxParser = spf.newSAXParser();
saxParser.parse(input, new MyHandler());
```

传入回调对象`MyHandler`继承自`DefaultHandler`：

```
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

```
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

```
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

```
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

```
// JSON string to JavaScript object:
jsObj = JSON.parse(jsonStr);
// JavaScript object to JSON string:
jsonStr = JSON.stringify(jsObj);
```

在Java中对JSON也有标准的JSR 353 API，XML可以通过Jackson和JavaBean相互转换，JSON和JavaBean相互转换也可以通过第三方库Jackson实现，或其他第三方库Gson、Fastjson。

```
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.11.2</version>
</dependency>
```

创建一个`ObjectMapper`对象，关闭`DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES`功能使得解析时如果JavaBean不存在该属性时解析不会报错。

```
InputStream input = Main.class.getResourceAsStream("/book.json");
ObjectMapper mapper = new ObjectMapper();
// 反序列化时忽略不存在的JavaBean属性:
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
Book book = mapper.readValue(input, Book.clas);
```

把JavaBean解析为JSON为序列化：

```
String json = mapper.writeValueAsString(book);
```

要把JSON的某些值解析为特定的JAVA对象，如`LocalDate`：

```
{
    "name": "Java核心技术",
    "pubDate": "2016-09-01"
}
```

解析为：

```
public class Book {
    public String name;
    public LocalDate pubDate;
}
```

需要进入标准库的JSR 310关于JavaTime的数据格式定义至Maven，然后在创建`ObjectMapper`时注册一个新的`JavaTimeModule`：

```
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.datatype/jackson-datatype-jsr310 -->
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
    <version>2.11.2</version>
</dependency>

```

```
ObjectMapper mapper = new ObjectMapper().registerModule(new JavaTimeModule());
```

如果内置的解析规则和扩展的解析规则都不满足需求可自定义解析：

如`Book`类的`isbn`是一个`BigInteger`：

```
public class Book {
	public String name;
	public BigInteger isbn;
}
```

但JSON数据并不是标准的整型格式：

```
{
    "name": "Java核心技术",
    "isbn": "978-7-111-54742-6"
}
```

