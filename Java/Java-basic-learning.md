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
int a = new int[1000];
...
Arrays.sort(a);//升序，优化的快速排序算法
//生成随机数
//Math.random()方法返回[0,1)的随机浮点数。
int r = (int)(Math.random() * n);//返回[0,n-1)之间的随机数

//快速打印二维数组的数据元素列表[[...],[...],...]
System.out.println(Arrays.deepToString(arrs));
```

> java.util.Arrays 1.2
>
> * static String toString(type[] a) 5.0
>
>   返回包含a中数据元素的字符串，这些数据元素被放在括号内，并用逗号分隔。
>
>   参数：a 类型为int、long、short、char、byte、boolean、float或double的数组。
>
> * static type copyOf(type[] a, int length) 6
>
> * static type copyOfRange(type[] a, int start, int end) 6
>
>   返回与a类型相同的一个数组，其长度为length或者end-start，数组元素为a的值。
>
>   参数：a 类型为int、long、short、char、byte、boolean、float或double的数组。
>
>   start 起始下标（包含这个值）
>
>   end 终止下标（不包含这个值）。这个值可能大于a.length。在这种情况下，结果为0或false。
>
>   length 拷贝的数据元素长度。如果length值大于a.length，结果为0或false；否则，数组中只有前面length个数据元素的拷贝值
>
> * static void sort(type[] a)
>
>   采用优化的快速排序算法对数组进行排序。
>
>   参数：a 类型为int、long、short、char、byte、boolean、float或double的数组。
>
> * static int binarySearch(type[] a, type v)
>
> * static int binarySearch(type[] a, int start, int end, type v) 6
>
>   采用二分搜索算法查找值v。如果查找成功，则返回相应的下标值；否则，返回一个负数值r。-r-1是为了保持a有序v应插入的位置。
>
>   参数：a 类型为int、long、short、char、byte、boolean、float或double的***有序***数组。
>
>   start 起始下标（包含这个值）。
>
>   end 终止下标（不包含这个值）。
>
>   v 同a的数据元素类型相同的值。
>
> * static void fill(type[] a, type v)
>
>   将数组的所有数据元素值设置为v。
>
>   参数：a 类型为int、long、short、char、byte、boolean、float或double的数组。
>
>   v 与a数据元素类型相同的一个值。
>
> * static boolean equals(type[] a, type[] b)
>
>   如果两个数组大小相同，并且下标相同的元素都对应相等，返回true。
>
>   参数：a、b 类型为int、long、short、char、byte、boolean、float或double的两个数组。

# 继承

## 子类对象的初始化（initialization）

> 子类继承了父类，子类中就包含了父类的成员
>
> 那么在创建初始化子类对象时，和之前相比也会有较大的不同



> 子类创建对象过程

- JVM仍然是创建谁的对象，就加载谁，先加载子类
- 但是很快，在子类还没有加载的时候，JVM发现子类有父类
  - 父类如果不存在，子类肯定也不可能存在
- 于是JVM调转枪头，开始加载父类
- 加载完父类，再加载子类
- 类加载都结束后，开始创建堆上的对象
- 堆上的对象中，会有一片空间，用来存放父类的成员
  - 子类对象就由两部分组成
  - 一部分是子类自己独特的成员
  - 另一部分是从父类继承过来的成员
- 现在堆上就真实存在了一个子类对象
  - 子类对象中存放父类成员的内存区域，可以近似的看成是一个父类对象
  - 所以子类的内存图，近似看成子类对象“装着”父类对象
  - 注意，**这个父类对象只是近似看作，不会真的创建了父类对象**
    - 创建子类对象，只会类加载父类，不会真的创建父类

> 仍然有一个棘手的问题

- 子类对象中的，父类成员变量和子类自身成员变量，谁先初始化默认初始值？
  - 答：先给父类成员变量默认初始化，再默认初始化子类
  - 为什么？答：先父后子，没毛病，首先要有父亲，才有儿子
  - 其次，子类成员变量的初始化，可以依赖父类成员变量
    - 这个时候如果子类先初始化，显然是要报错的



> 问题仍然没有结束

- 子父类的类加载顺序是由JVM去保证的，但是子父类的初始化先后顺序又是怎么保证的呢？
- 它的原理是什么呢？

> 我们已经知道了构造器是可以用来初始化成员变量，会由JVM自动调用
>
> 用构造器去保障这种先后顺序，怎么做呢？

- 答：只需要在子类的构造器的第一行，调用父类构造器就可以了

​	 

> 我们没有自己手动在子类构造器的第一行，调用父类构造器，但是仍然达成了先初始化父类
>
> 再初始化子类的效果，是什么原因呢？
>
> 显然这里存在了一个隐式调用，于是子类对象的初始化就有了两种方式

- 隐式的调用了父类构造方法（JVM保证）
- 程序员显式的调用父类构造方法（代码保证）

## 显隐式子类初始化

> 重点学习一下两种方式

### 子类对象的隐式初始化（implicit）

> 隐式初始化，JVM自动调用，无需我们手动操作
>
> 条件为

- **父类中有默认的构造方法**
  - 子类的构造器中没有显式调用父类的构造方法
- 达成上述两个条件，则JVM在初始化子类对象时进行隐式初始化

  - 永远先执行父类的构造方法，顺序为
    - 最上层的父类（Object）
    - 其他父类（继承链中越处于上流越先执行）
  - 所有父类的构造方法都执行完毕，开始执行子类构造方法

> 需要注意的是
>
> - 隐式初始化，JVM总是调用父类的无参构造，如果父类没有，就要报错
> - Object类也有默认无参
> - 隐式初始化总是不传参数，如果我们想要对参数进行赋值，就必须使用显式的子类初始化

### 子类对象的显式初始化（Explicit ）

> 显式初始化，需要程序员手动写代码，告诉JVM调用哪个父类构造器
>
> 如何使用？

- 必须在子类构造器的第一行，显式的调用父类构造方法，那么如何调用父类构造器？

  - 使用super关键字调用

  - 语法

    ```Java
    super(父类构造器参数);
    ```

> 什么是super关键字？

- super代表当前类的父类"对象"的引用
- this代表当前类的对象
- 两者的使用没有明显差别，只是
  - **this在当前类中不受访问权限控制，super访问父类成员，受访问权限控制**
  - 因为当前类中即便是private仍然可以访问，但是super就不在当前类中了

```
this VS super  

this关键字：表示当前对象的引用         super关键字：super代表父类对象的引用

this调用当前类中定义的构造方法：this(实参列表)          
super调用父类中定义的构造方法：super(实参列表)

this访问当前对象的成员变量值          super访问父类对象中，成员变量的值

this访问当前对象的成员方法            super访问父类对象，成员方法                            
```



**super与this关键字**

**this关键字概念：**

this代表所在类的对象引用。

**记住： 方法被哪个对象调用，this就代表哪个对象。**

**1.super可以在子类中 调用父类中名称相同的 成员方法和成员变量**

**2.this可以在方法中调用 类中的与方法内局部变量名称相同 的成员方法和成员变量**

**3.super和this的区别**

(a).this 代表**当前类的对象**

代表对象的内存空间标识（用来存储当前类定义的内容，成员变量、方法）

(b).super （代表父类对象） 可以这么理解，实际并不代表父类对象

代表对象的内存空间的标识（用来存储父类定义的内容，成员变量、方法）

**使用场景：**

当局部变量和成员变量名字相同时用this，子类变量和父类变量名字相同时用super

**super用法：（this和super均适用）**

**1.访问成员变量**

this.成员变量  super.成员变量    （局部变量直接调用不需要修饰符）

**2.访问构造方法**

 this(…)           super(…)        如果是有参方法，()里面写参数

**3.访问成员方法**

this.成员方法()  super.成员方法()



隐式子类对象创建：

​	条件： 

​		a. 当父类提供了默认的构造函数(无参构造方法)

​        b. 子类的构造方法中, 没有显式调用父类的其它构造方法

​	结果：

​		JVM自动在子类构造方法第一句加上  “ super() “

​		在执行子类的构造方法之前，JVM会自动执行父类

​	

显式子类对象创建：

​	程序员写代码告诉JVM在调用子类构造器之前调用父类构造方法

​	可以在子类构造器的第一行使用super关键字，调用父类的构造方法

​		

总结：

​	**1，无论是隐式还是显式，最终都是为了保证父类构造器先于子类执行**

​	2，若父类中不存在默认构造方法，则必须在子类构造方法中使用super关键字调用父类构造器

​	3，在子类构造方法中，super语句必须在第一行

​	4，在子类构造方法中，也可以用this调用自身构造，也必须在第一行

​	5，this和super不能共存

​	6，构造代码块和静态代码块也是“先父后子”



为什么this和super都必须在第一行？

​	因为子类构造器第一行永远都有一个super关键字调用，如果你自己的super和this不在第一行，会形成循环

## 子类的属性隐藏（field hidden）

> 子父类中能否拥有同名的属性呢？
>
> 如果可以，请尝试

- 创建子类对象，使用对象名点的形式访问同名变量，结果是什么？
- 创建子类对象，在子类中，编写方法，返回该属性
  - 用子类对象调用该方法，返回的结果是？
  - 方法的就近原则
- 创建子类对象，在父类中，编写方法，返回该属性
  - 用子类对象调用该方法，返回的结果是？
  - 方法的就近原则
- 最终我们发现，子类可以访问到父类的成员变量
  - 但是由于编译器检索机制的限制，好像父类的属性被隐藏了一样
  - 称之为子类的属性隐藏





> 如果我就想在子类方法中，访问父类的同名成员变量，怎么办？

- super关键字





> 对象名点成员变量名的，编译器检索机制

- 先从子类本身中去找---->子类中找不到再去父类中找----->再找不到就报错
- 但是一般来说，我们都是通过方法访问成员变量



> 注意事项

- 静态成员变量也可以被继承，但是静态成员变量如果是同名的，是一个全新的，会覆盖掉原先的静态成员
- 子类父类的同名静态成员各自独立属于自己的类，如果子类没有便完全继承父类的静态成员

## 子类的方法覆盖（override）

> 子父类中能否拥有同名的方法？
>

- 在父子类中声明两个个一模一样的方法，但是方法体输出不同
  - 创建子类对象，直接调用该方法，结果是什么？
- 再在父子类中定义两个方法，分别在方法体中调用自身方法名一样的方法
  - 创建子类对象，分别调用两个方法，结果是什么？
- 我们发现无论怎么操作，都只能访问子类中的同名方法，这就是方法的覆盖



> 如果想在子类的方法中，访问父类方法，应该怎么办？

- super关键字



> 对象名点方法访问的方式，编译器的检索机制

- 先从子类本身中去找---->子类中找不到再去父类中找----->再找不到就报错



> 什么时候使用方法的覆盖？

- 当我们需要在子类中，修改父类方法的实现的时候
- 使用方法的覆盖时，添加@Override注解来标记
- 例如：比如对于动物的叫，人类的吃



> 方法覆盖的注意事项

- 父类中私有方法不能被重写
- 子类重写父类方法时，访问权限不能更低
- 静态方法在使用现象上，很像是被重写了，但实际上静态方法不能被重写，而是直接是一个新的静态成员



- 重写 VS 重载

|              | 重载（overload） |                      重写（override）                       |
| :----------: | :--------------: | :---------------------------------------------------------: |
| 发生的类不同 |   发生在同类中   |               发生在子父类之间,肯定不是一个类               |
|    方法名    |     必须相同     |                          必须相同                           |
|   参数列表   |     必须不同     |                          必须相同                           |
|  权限修饰符  |      不影响      |            重写的方法访问权限必须大于等于原方法             |
|     异常     |      不影响      |                重写的方法不能抛出更多的异常                 |
|  返回值类型  |      不影响      | 重写的方法的返回值类型必须和原方法兼容,代表可以不是完全一致 |

- 被static、final、private修饰的父类方法无法被重写

## final和限制继承

> final是一个修饰符，可以用来修饰类、方法和变量（包括成员变量和局部变量）

### final修饰类

> 表示”最终的类“
>
> 当用final修饰一个类时，表明这个类不能被继承

- 除了这一点外，这个类照常使用，比如创建对象，比如访问方法，比如继承别的类
- 从设计角度来讲，一个final修饰的类应该是
  - 不需要复用成员
  - 功能已经特别强大，足够满足需求
  - 需要绝对的保证安全，以致于不让它被继承
- 常见的final类
  - String
  - System
  - Math
  - 所有基本数据类型的包装类和一个Void
    - Boolean，Character，Short，Integer，Long，Float，Double，Byte，Void
- 除非你十分确定这个类以后不会被用来继承 ，为了保证安全，可以设置一个类为final类
  - 不然一般情况下，尽量不要把一个类设置成final

### final修饰方法

> 表示”最终的方法“
>
> 当一个方法被final修饰后，可以被继承，但是无法被重写

- final修饰方法，就把方法锁住了，任何继承这个方法的类都无法覆盖该方法
- 如果一个方法已经能够满足需求，并且明确知道它不应该被修改，修改会产生问题
  - 可以将方法设置成final方法
  - 否则，正常情况下，不要随便使用final修饰方法

### final修饰变量

> 表示”最终的变量“，是一种自定义常量，普遍来说，命名应该采用全大写，下划线连接的方式
>
> 修饰变量是final用得最多的地方，也是可能混淆的地方
>
> 总体来说，final修饰变量后
>
> - 如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改
> - 如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象
>   - 因为final修饰的是引用，而引用中装的是地址
>   - 但是其引用的对象的状态是可以改变的

#### final修饰普通成员变量

- final修饰普通成员变量，表示常量，**在整个创建对象过程中它们的值，只能修改一次也必须显式赋值一次**
- 总结给普通成员变量赋值的方式
  - 显式初始化语句
  - 构造代码块
  - 构造器
  - 创建对象后，调set方法，或者直接赋值
  - 对于final修饰的普通成员变量，前三种必然要有一种
- 对于同一个类的同一个final修饰的变量，不同对象可能会有不同的值

#### final修饰静态成员变量（常用）

- final修饰静态成员变量，表示全局常量，**在整个静态成员赋值过程中，只能修改一次也必须显式赋值一次**
  - **访问一个类的基本数据类型和String类型的final静态全局常量不触发类加载**
  - 被所有对象共享，每个对象都必须有同一个值，且不可改变
- 给final静态成员变量赋值
  - 显式赋值语句
  - 静态代码块
- final static 还是static final？
  - 建议用static final
- 静态常量一般全部大写，用下划线隔开

#### final修饰局部变量

- 在方法体和方法参数列表中定义final常量，表示该常量不可更改
- 必须显式的赋值初始化，不然也没别的方式初始化了
- 在形参列表中声明final常量，表示该参数是一个常量，在接收后无法修改

#### 特别的final修饰引用数据类型

- 表示引用指向的对象不可变，但对象的状态可变
- 例如**final修饰Student类型的变量s，则s的地址值不能改变，**
- **也就是说s要始终指向同一个对象。  但是所指对象的属性值可以改变，只要对象的地址不变即可**



final修饰匿名对象 会咋样？

final new Student() 语法不允许



```
final修饰普通成员变量，表示常量，创建对象的时候，也只能初始化它的值一次，这个过程中也不能改了。创建对象以后，它们的值就不可改变了
```



- 对于一个final变量
  - 构建对象之后，它们的值就不可以被修改了
  - 如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改
  - 如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象
- 使用final修饰一个基本数据类型变量，该变量就变成了常量，只能被赋值初始的一次
  - 自定义常量，无法在程序执行时改变
  - 对于常量，我们最好static final一起使用
- 对于基本数据类型而言，final修饰后，值就不可变了
- 对于引用数据类型而言，final修饰后，引用的地址就不可变了，但引用对象的状态可以改变



> 注意：

- 常量必须显示初始化，即必须赋值一次，不用能默认值。
- 只能赋值一次，即使之后的赋值和原来的值一样也不行
- 在静态常量在常量池中，使用不需要类加载，静态常量需要创建对象赋初值的，使用需要类加载
