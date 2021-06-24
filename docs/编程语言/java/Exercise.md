### 1.多维数组求平均数

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

### 2.随机生成一个由十进制数组成的100个字符的字符串，并统计出由123组成的子字符串的个数。

```java
import org.springframework.util.StringUtils;
 
import java.util.Arrays;
import java.util.Random;
public class MainTest {
 
    public static void main(String[] args) {
        String[] str = new String[100];
        StringBuffer buffer = new StringBuffer(100);
        for (int i = 0; i < str.length; i++) {
            str[i] = randomString(1);
            buffer.append(str[i]);
        }
        System.out.println(buffer);
        int count = method1(buffer.toString(), "123");
        System.out.println("123子字符串的个数是：" + count);
 
    }
 
    /**
     * 统计重复个数
     * @param buffer
     * @param a
     * @return
     */
    public  static int method1(String buffer,String a){
          int len =   buffer.length() - buffer.replace(a,"").length();
          return  len / a.length();
    }
 
    /**
     * 生成随机数
     * @param a
     * @return
     */
    public static String randomString(int a) {
        char[] cs = new char[a];
        String pool = "";
        for (short i = '0'; i <= '9'; i++) {
            pool = pool + (char) i;
        }
        for (int h = 0; h < cs.length; h++) {
            int index = (int) (Math.random() * pool.length());//产生一个pool范围内的随机数
            cs[h] = pool.charAt(index);//返回指定索引处的字符
        }
        String str1 = new String(cs);
        return str1;
    }
}
```

### 抽象类和接口的区别

**区别语法层面的区别**

1. 抽象类中可以包含具体的实现方法，而接口中只能有public abstract方法
2. 抽象类中的成员变量可以是各种类型的，而接口中只能有public static final类型
3. 抽象类中可以包含静态代码块和静态方法，而接口中不可以
4. 一个类只能继承一个抽象类，但是可以实现多个接口

**设计层面的区别**

1. 抽象层次不同，抽象类对整个类整体进行抽象，包括属性、行为；而接口只是对类的局部行为进行抽象。比如，门和警报的例子：门都有open和close的动作，可以定义一个抽象类Door，包含open和close方法。这样所有的门都可以继承这个抽象类，从而包含了这两个方法。另外报警功能alarm是一种延伸的功能，但是不可以加到抽象类Door中，因为并不是每个门都有报警的功能。所以需要定义一个接口，接口中包含alarm方法，这样需要有报警功能的门来实现这个接口，没有则不需要。
2. 跨域不同，抽象类和子类之间体现的是一种继承关系，父类和子类在概念本质上相同的。接口则不然，接口和其实现者不需要有任何关联，接口的实现者仅仅是实现了接口的规定的规范。
3. 设计层次不同，抽象类是自下而上的设计，先有多个子类，然后抽取共同特性，形成了抽象类。而接口则不同，接口的设计的时候根本不知道会有什么实现者以什么形式来实现。接口只是一种实现定义好的规范。

### JVM内存结构，每个区存放什么数据

**方法区：**各线程共享，主要存放类信息、常量、静态变量

**虚拟机栈：**线程私有，主要存放基本数据类型（int、char、float...）和对象的引用

**本地方法栈：**线程私有，为虚拟机使用到的Native方法服务，如Java使用c或者c++编写的接口服务时，代码在此区运行

**堆：**线程共享，主要存放对象的实例和数组

**程序计数器：**它的作用可以看做是当前线程所执行的字节码的行号指示器，记录线程上次执行到程序的哪个位置

### 设计模式-单例模式

```java
public class SingletonTest {
//     1、构造方法私有化
    private SingletonTest() {}
//     2、创建私有静态内部类
    private static class SingletonHolder {
//     3、创建静态私有final类型的实例对象
     private static final SingletonTest singleton2 = new SingletonTest(); }
//     4、创建公有静态获取实例的方法
    public static SingletonTest getInstance() {
        return SingletonHolder.singleton2;
    }
}
```

### 3.

```java
    class Parent(object):
    	x=1
    class Child1(Parent):
        pass
    class Child2(Parent):
        pass
    print(Parent.x,Child1.x,Child2.x)
    Child1.x=2
    print(Parent.x,Child1.x,Child2.x)
    Parent.x=3
    print(Parent.x,Child1.x,Child2.x)
            
1 1 1 #继承自父类的类属性 x，所以都一样，指向同一块内存地址。 
1 2 1 #更改 Child1，Child1 的 x 指向了新的内存地址。 
3 2 3 #更改 Parent，Parent 的 x 指向了新的内存地址。
```

### 4.

列表是可变数据类型，只要list=[]不变，那么内存地址就不会变

```python
def extendList(val,list=[]):
	list.append(val)
	return list
list1=extendList(10) # list1=[10] 坑点
list2=extendList(123,[]) #list2=[123]
list3=extendList('a') # list1和list3的内存地址是同一个 [10,'a']
print("list1=%s"%list1)
print("list2=%s"%list2)
print("list3=%s"%list3) 
    
list1=[10,'a'] 
list2=[123] 
list3=[10, 'a']
```

### 5.现有字典 d={"a":24，"g":52，"i":12，"k":33}请按字典
中的 value值进行排序？

```java
sorted(d.items(), key=lambda x: x[1]) 中 d.items() 为待排序的对象；key=lambda x: x[1] 为对前面的对象中的第二维数据（即value）的值进行排序
```

### 6.请用python代码写一个单例模式，并简述单例模式的应
用场景

```python
class Singleton(object): 
    def__new__(cls):# 为对象分配内存空间 
        if not hasattr(cls,'instance'): # instance做一个标记，如果instance存 在，那么就证明已经生成过对象 
            cls.instance=super(Singleton,cls).__new__(cls) # 分配内存地址 
            return cls.instance 
应用场景： 
1. 任务管理器 
2. 回收站 
3. 网站的计数器 
4. 日志应用 
5. Web应用的配置对象 
6. 数据库连接池
```

### 7.简述什么是装饰器及应用场景，并手写装饰器

```python
import time def timer(func): 
    def deco(*args,**kwargs): 
        start_time=time.time() 
        func(*args,**kwargs) 
        stop_time=time.time() 
        print("running time is %s "%(stop_time-start_time)) 
        return deco 
    @timer 
    def test1(): 
        time.sleep(3) 
        print("in the test 1") 
   	test1()
```

### 8.pthon中match和search的区别？

match()函数只检测是不是在 string 的开始位置匹配，

search()会扫描整个 string 查找匹配；

也就是说 match()只有在0位置匹配成功的话才有返回，

如果不是开始位置匹配成功的话，match()就返回 None。 

### 9. 对生成器和迭代器

**迭代器：**

迭代是Python最强大的功能之一，是访问集合元素的一种方式。 迭代器是一个可以记住遍历的位置的对象。 迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。 迭代器有两个基本的方法： iter() 和 next() 。 字符串，列表或元组对象都可用于创建迭代器：任何实现了 __iter__ 和 __next__() 方法的对象都是迭代器。 __iter__ 返回迭代器自身， __next__返回容器中的下一个值。

**生成器：**

生成器是一种特殊的迭代器，它的返回值不是通过 return 而是用 yield , 生成器算得上是Python语言中最吸引人的特性之一，生成器其实是一种特殊的迭代器，不过这种迭代器更加优雅。它不需要再像上面的类一样写 __iter__() 和 __next__() 方法了，只需要一个 yield 关键字。 生成器一定是迭代器，但迭代器不一定是生成器。生成器是边计算边遍历的。

### 10.现在有一个列表L = [1,2,3,4,5,6,6,5,4,3,2,1]，去重列表中的重复项，用多种方式处理。

```java
//第一种方法：借助字典键值的唯一性 
L = [1,2,3,4,5,6,6,5,4,3,2,1] 
    d = {} 
	new_d = d.fromkeys(L) 
    obj = new_d.keys() 
    new_l = list(obj) 
    new_l是去重之后的列表 
 //第二种方法：set()去重 
    new_l = list(set(L)) 
    new_l是去重之后的列表 
//第三种方法：遍历 
    new_l = [] 
    for x in L: 
		if x not in new_1: 
			new_1.append(x) 
	//new_l是去重之后的列表
```

### 11.用一条SQL语句 查询出每门课都大于80分的学生姓名。
表scores如下：

```
name course score 
张三 语文 81 
张三 数学 75 
李四 语文 76 
李四 数学 90 
王五 数学 100 
王五 英语 90
```

学生的最低分数大于80，那么就可以查询出每门课都大于80分的学生姓名

```mysql
select name,min(score) from scores group by name having min(score)>80;
```

### 12.测纸杯

功能度：用水杯装水看漏不漏；水能不能被喝到

安全性：杯子有没有毒或细菌

可靠性：杯子从不同高度落下的损坏程度

可移植性：杯子在不同的地方、温度等环境下是否都可以正常使用

兼容性：杯子是否能够容纳果汁、白水、酒精、汽油等

易用性：杯子是否烫手、是否有防滑措施、是否方便饮用

用户文档：使用手册是否对杯子的用法、限制、使用条件等有详细描述

疲劳测试：将杯子盛上水放24小时检查泄漏时间和情况；盛上汽油放24小时检查泄漏时间和情况等

压力测试：用根针并在针上面不断加重量，看压强多大时会穿透

### 13.如何保证软件质量

1. 产品，保证迭代过程中的产品逻辑，对于可能的兼容，升级做出预判，并给出方案

2. 设计，满足产品表达的同时，保证设计的延续性

3. 开发，产品细节的保证，技术方案选择要严谨，考虑兼容，性能，开发完成后要充分自测，严格遵循开发规范操作

4. 测试，验证产品逻辑，站在用户角度对交互设计进行系统验证，尽可能多的使用技术手段保证测试质量

### 14. 如何制定测试计划

测试计划包括测试目标、测试范围、测试环境的说明、测试类型的说明（功能，安全，性能，稳定性）、测试工具、模块的划分、测试负责人、测试执行轮次的时间安排、相关文档在文档管理库中的位置、测试的风险 。其中模块划分需要根据测试人员对于业务的熟悉程度及个人能力进行分配，工作量的估算需要根据以往测试时的经验，结合本次需求的修改，可以大致估算出测试量

### 15.安全测试

1. SQL 注入的原理，如何对没有回显的 SQL 注入进行测试？ 

   关键词：数据库、DNSLog 

2. SSRF 能干什么？ 

   关键词：内网探测、GETSHELL 

3. 如何测试命令执行漏洞？ 

   关键词：nc、HTTP 请求 

4. 如何找到 CDN 隐藏的服务器真实 ip？ 

   关键词：历史记录，全球节点 DNS 解析 

5. 简述印象最深的一次渗透测试。  

6. 如何进行 web 网站的指纹识别？ 

   关键词：静态资源、特征路径、特征头、特征页面内容、特征状态码和 404 

7. 如何进行子域名收集？ 

   关键词：搜索引擎、页面内容、暴力破解、DNS 域传送漏洞 

8. 说说常用扫描器的执行流程，如果让你写，你的思路？ 

   关键词：爬虫，特征识别，漏洞扫描 

9. 挖过什么逻辑漏洞，简单描述下。 

   关键词：越权、验证码、登录、付款 

10. 给你一个网站做渗透测试，你如何展开工作？ 

    关键词：信息收集、信息分析、漏洞利用 