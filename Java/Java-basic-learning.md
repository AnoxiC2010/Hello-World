# 文件输入与输出 Scanner&PrintWriter

```java
package com.test;
import java.io.File;
import java.io.IOException;
import java.io.PrintWriter;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("C:\\Users\\AnoxiC2010\\Desktop\\Test.txt");
        Path path = Paths.get("C:\\Users\\AnoxiC2010\\Desktop\\Test.txt");
        //文件输入的两种方式
        Scanner in = new Scanner(file, "UTF-8");
//      Scanner in = new Scanner(path, "UTF-8");
        String s = "";
        while (in.hasNextLine()) {
            s = in.nextLine();
            System.out.println(s);
        }
        //文件输出
        PrintWriter out = new PrintWriter("C:\\Users\\AnoxiC2010\\Desktop\\Test.txt", "UTF-8");
        out.println("第一行");
        out.println("第二行");
        out.close();//不close的话不会写入
        //查看是否写入
        in = new Scanner(path, "UTF-8");//不新建对象的话不读取新内容
        while (in.hasNextLine()) {
            s = in.nextLine();
            System.out.println(s);
        }
        in.close();//关不关到都能输出
    }
}
```

> java.util.Scanner 5.0
>
> * Scanner(File f)
>
>   构造一个从给定文件读取数据的Scanner
>
> * Scanner(String data)
>
>   构造一个从给定字符串读取数据的Scanner
>
> java.io.PrintWriter 1.1
>
> * PrintWriter(String fileName)
>
>   构造一个将数据写入文件的PrintWriter。文件名由参数指定
>
> java.nio.file.Paths 7
>
> * static Path get(String pathname)
>
>   根据给定的路径名构造一个Path

# 循环相关break lable

除了循环也可以将标签应用到任何语句中，甚至可以应用到if语句或者块语句中，像goto，但只能跳出语句块，不能跳入语句块，不提倡。

循环中continue也可以配合lable使用

```java
lable:{
    ...
    if(condition) break lable; //exit block
    ...
}
//jumps here when the break statement executes
```

# 大数值 BigInteger & Decimal

```java
//将普通数值转换为大数值，使用静态valueOf方法
BigInteger a = BigInteger.valueOf(100);
```

> java.math.BigInteger 1.1
>
> * BigInteger add(BigInter other)
>
> * BigInteger subtract(BigInteger other)
>
> * BigInteger multiply(BigInteger other)
>
> * BigInteger divide(BigInteger other)
>
> * BigInteger mod(BigInteger other)
>
>   返回这个大整数和另一个大整数other的和、差、积、商、余数。
>
> * int compareTo(BigInteger other)
>
>   如果此大整数与另一个大整数other相等，返回0；此大整数小于另一个大整数other，返回负数；否则返回正数。
>
> * static BigInteger valueOf(long x)
>
>   返回值等于x的大整数。
>
> java.math.BigInteger 1.1
>
> * BigDecimal add(BigDecimal other)
>
> * BigDecimal subtract(BigDecimal other)
>
> * BigDecimal multiply(BigDecimal other)
>
> * BigDecimal divide(BigDecimal other RoundingMode mode) 5.0
>
>   返回这个大实数与另一个大实数other的和、差、积、商。要想计算商，必须给出舍入方式（rounding mode）。RoundingMode.HALF_UP是在学校中学习的四舍五入方式。它适用于常规的计算。有关于其他的舍入方式可查看API文档。
>
> * int compareTo(BigDecimal other)
>
>   如果这个大实数与另一个大实数相等，返回0；小于另一个大实数，返回负数；否则，返回正数。
>
> * static BigDecimal valueOf(long x)
>
> * static BigDecimal valueOf(long x, int scale)
>
>   返回值为x或x/10(scale 上标)的一个大实数。

# 数组

```java
//无论哪种创建方式，都不会跳过默认初始化的过程
//for each增强循环
//定义一个变量variable用于暂存集合collection中的每一个元素
//这个变量相当于是拷贝的元素，fori循环中拿到的直接就是集合的元素
//collection必须是数组或者实现了Iterable接口的类对象
for(variable : collection) statement

//利用Arrays类的toString方法简单打印数组中的所有值
Arrays.tostring(collection)返回形如[xx,xx,xx]的字符串
 
//利用匿名数组再不创建新变量的情况下重新初始化一个数组
int smallPrimes = {2, 3, 5, 7};
smallPrimes = new int[]{17, 19, 23, 29};

//数组拷贝
//用Arrays类的copyOf方法把一个数组中的所有值拷贝到新的数组中去
int[] numbers = {1, 2, 3, 4, 5};
int[] copiedNumbers = Arrays.copyOf(numbers, numbers.length());
//第二个参数是新数组长度，通常可利用它来增加数组长度
//多余元素被赋初始值，相反长度小于原来，则只拷贝最前面的元素
int[] copiedNumbers = Arrays.copyOf(numbers, 2 * numbers.length());

//命令行参数
//main方法的参数args可以通过在命令行执行命令语句后加入参数以String数组形式传入
//如命令行执行Hello.class：java Hello xxx xxx xxx 后面几个为args数组的元素

//数组排序


```

