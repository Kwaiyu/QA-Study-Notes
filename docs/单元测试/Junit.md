## 使用Junit测试

### 简介

官方文档：https://junit.org/junit5/docs/current/user-guide/

以往通过运行main()方法来测试代码，但只能有一个main()方法，不能将测试代码分离，并且不能打印出测试结果和期望结果，很难编写通用测试代码，因此需要一种测试框架来帮助编写测试。JUnit单元测试框架专门用于运行编写Java单元测试，经过不断迭代目前已经发展到了5.6.2版本，为JDK8以及更高的版本提供更好的支持（如支持Lambda）和更丰富的测试形式（如重复测试，参数化测试）。Junit5与以前的Junit版本不同，它由三个不同的子项目及不同模块组成。

> **JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage**

- Junit Platform 可作为一个在JVM上启动的基础测试框架，它还定义了TestEngine用于在平台上运行的测试框架API，该平台还提供了一个控制台启动和一个基于Junit4的运行器。流行的IDE和构建工具中也对Junit Platform提供支持。
- Junit Jupiter 是新的编程模型和扩展模型组合，用于在Junit5中编写测试和扩展，提供TestEngine在平台运行基于Jupiter的测试。
- Junit Vintage 提供TestEngine在平台上运行基于Junit3和Junit4的测试

### 运行测试

**Intellij IDEA**

为了使用不同版本的Junit，需要添加不同版本的`junit-platform-launcher`， `junit-jupiter-engine`以及`junit-vintage-engine`在类路径中的JAR文件依赖项。

*Gradle依赖项*：build.gradle

```
// Only needed to run tests in a version of IntelliJ IDEA that bundles older versions
testRuntimeOnly("org.junit.platform:junit-platform-launcher:1.6.2")
testRuntimeOnly("org.junit.jupiter:junit-jupiter-engine:5.6.2")
testRuntimeOnly("org.junit.vintage:junit-vintage-engine:5.6.2")
```

Maven依赖项：pom.xml

```xml
<!-- Only needed to run tests in a version of IntelliJ IDEA that bundles older versions -->
<dependency>
    <groupId>org.junit.platform</groupId>
    <artifactId>junit-platform-launcher</artifactId>
    <version>1.6.2</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.6.2</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
    <version>5.6.2</version>
    <scope>test</scope>
</dependency>
```

- 设置>async>关闭instrumenting agent
- 设置>Java 编译器>Project bytecode version 和 Target bytecode version 改成本地JDK版本
- 项目结构>Project>同步项目SDK和项目语言级别改成本地JDK版本和相同级别
- 项目结构>模块>源码>语言级别改成JDK版本相同级别
- 项目结构>模块>依赖>将Junit相关库范围改成Compile

### 编写测试

- 单元测试代码本身必须非常简单，能一下看明白，决不能再为测试代码编写测试；
- 每个单元测试应当互相独立，不依赖运行的顺序；
- 测试时不但要覆盖常用测试用例，还要特别注意测试边界条件，例如输入为`0`，`null`，空字符串`""`等情况。

```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class Factorial {
    public static long fact(long n) {
        long r = 1;
        for (long i = 1; i <= n; i++) {
            r = r * i;
        }
        return r;
    }
}

public class FactorialTest {
    @Test
    void testFact() {
        //assertEquals(expected, actual)
        assertEquals(1, Factorial.fact(1));
        assertEquals(2, Factorial.fact(2));
        assertEquals(6, Factorial.fact(3));
        assertEquals(3628800, Factorial.fact(10));
        assertEquals(2432902008176640000L, Factorial.fact(20));
    }
}
```

一个JUnit测试包含若干`@Test`方法，并使用`Assertions`进行断言，注意浮点数`assertEquals()`要指定`delta`。

## Fixture

编写Fixture是指针对每个`@Test`方法，编写`@BeforeEach`方法用于初始化测试资源，编写`@AfterEach`用于清理测试资源；

必要时，可以编写`@BeforeAll`和`@AfterAll`，使用静态变量来初始化耗时的资源，并且在所有`@Test`方法的运行前后仅执行一次。

`@Test`方法内部的成员变量相互独立。

```java
public class Calculator {
    private long n = 0;

    public long add(long x) {
        n = n + x;
        return n;
    }

    public long sub(long x) {
        n = n - x;
        return n;
    }
}
```

在测试时要先初始化对象，通过`@BeforeEach`初始化，相当于Junit4的`@Before`表示带注释的方法在当前类中的每个`@Test`，`@RepeatedTest`，`@ParameterizedTest`或`@TestFactory`方法之前执行，除非重写这些方法，否则它们将被继承。通过`@AfterEach`来清理资源，相当于Junit4的`@After`表示带注释的方法应在当前类中的每个`@Test`，`@RepeatedTest`，`@ParameterizedTest`或`@TestFactory`方法之后执行，除非重写这些方法，否则它们将被继承。

```java
public class CalculatorTest {

    Calculator calculator;

    @BeforeEach
    public void setUp() {
        this.calculator = new Calculator();
    }

    @AfterEach
    public void tearDown() {
        this.calculator = null;
    }

    @Test
    void testAdd() {
        assertEquals(100, this.calculator.add(100));
        assertEquals(150, this.calculator.add(50));
        assertEquals(130, this.calculator.add(-20));
    }

    @Test
    void testSub() {
        assertEquals(-100, this.calculator.sub(100));
        assertEquals(-150, this.calculator.sub(50));
        assertEquals(-130, this.calculator.sub(-20));
    }
}
```

`@BeforeAll`和`@AfterAll`相当于Junit4的`@BeforeClass`和`@AfterClass`，表示带注释的方法应该在当前类中的所有@Test，@RepeatedTest，@ParameterizedTest和@TestFactory方法之前执行，此类方法是继承的（除非它们被隐藏或覆盖），并且必须是静态的（除非使用“每类”测试实例生命周期），只能初始化静态变量和静态方法。

```java
//资源初始化和清理更加繁琐，耗费较长的时间，如初始化数据库
public class DatabaseTest {
    static Database db;

    @BeforeAll
    public static void initDatabase() {
        db = createDb(...);
    }
  
    @AfterAll
    public static void dropDatabase() {
        ...
    }
}
```

## 异常测试

测试异常可以使用`assertThrows()`，期待捕获到指定类型的异常；

对可能发生的每种类型的异常都必须进行测试。

```java
public class Factorial {
    public static long fact(long n) {
        if (n < 0) {
            throw new IllegalArgumentException();
        }
        long r = 1;
        for (long i = 1; i <= n; i++) {
            r = r * i;
        }
        return r;
    }
}

@Test
void testNegative() {
    //assertThrows(期望捕获异常，封装执行产生异常代码)，捕获到异常测试通过，未捕获到或类型不对测试失败
    assertThrows(IllegalArgumentException.class, new Executable() {
        @Override
        public void execute() throws Throwable {
            //必定抛出IllegalArgumentException
            Factorial.fact(-1);
        }
    });
}

//JAVA 8函数式编程，单方法接口简写：
@Test
void testNegative() {
    assertThrows(IllegalArgumentException.class, () -> {
        Factorial.fact(-1);
    });
}
```

## 条件测试

条件测试是根据某些注解在运行期让JUnit自动忽略某些测试。

- @Disabled：用于禁用测试类或测试方法；类似于JUnit 4的@Ignore。这样的注释不是继承的。
- @EnableOnOs(OS.WINDOWS)：指定在Windows操作系统运行测试，@DisabledOnOs(OS.WINDOWS) 指定不在Windows操作系统运行测试。
- @DisabledOnJre(JRE.JAVA_8)：指定在JAVA8以上的版本测试
- @EnabledIfSystemProperty(named = "os.arch", matches = ".*64.*")：指定64位操作系统
- @EnabledIfEnvironmentVariable(named = "DEBUG", matches = "true")：传入环境变量DEBUG=true时测试
- @EnabledIf("java.time.LocalDate.now().getDayOfWeek()==java.time.DayOfWeek.SUNDAY")：执行语句是否在星期日，返回true时执行测试。

## 参数化测试

使用参数化测试，可以提供一组测试数据，对一个测试方法反复测试。

参数既可以在测试代码中写死，也可以通过`@CsvFileSource`放到外部的CSV文件中。

```java
//使用一组正数测试Math.abs()
@ParameterizedTest
@ValueSource(ints = {0,1,5,100})
void testAbs(int x) {
    assertEquals(x,Math.abs(x));
}
//使用一组负数测试Math.abs()
@ParameterizedTest
@ValueSource(ints = {-1,-5,-100})
void testAbsNegative(int x){
    assertEquals(-x,Math.abs(x));
}
```

```java
//方法将字符串第一个字母变为大写，后续字母变为小写
public class StringUtils {
    public static String capitalize(String s) {
        if (s.length() == 0) {
            return s;
        }
        return Character.toUpperCase(s.charAt(0)) + s.substring(1).toLowerCase();
    }
}
//通过@MethodSource编写同名静态方法提供参数，或指定方法名
@ParameterizedTest
@MethodSource
void testCapitalize(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}

static List<Arguments> testCapitalize() {
    return List.of( // arguments:
            Arguments.arguments("abc", "Abc"), //
            Arguments.arguments("APPLE", "Apple"), //
            Arguments.arguments("gooD", "Good"));
}
//通过@CsvSource提供参数
@ParameterizedTest
@CsvSource({ "abc, Abc", "APPLE, Apple", "gooD, Good" })
void testCapitalize(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}
//通过@CsvFileSource指定的CSV文件提供参数，JUnit只在classpath中查找指定的CSV文件，将文件放在test目录下
@ParameterizedTest
@CsvFileSource(resources = { "/test-capitalize.csv" })
void testCapitalizeUsingCsvFile(String input, String result) {
    assertEquals(result, StringUtils.capitalize(input));
}
```