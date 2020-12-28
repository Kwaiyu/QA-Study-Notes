1. 多维数组求平均数

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

2. 随机生成一个由十进制数组成的100个字符的字符串，并统计出由123组成的子字符串的个数。

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

   

