# Java Bugs

# java.util.NoSuchElementException: No line found

有多个Scanner对象的情况下，关闭一个后导致其他Scanner对象无法读取。即使是调用了两个不同的方法，它们各自都创建了Scanner对象，先执行的方法关闭了Scanner资源后，后调用的方法中的Scanner也无法读取。

网上提示：第一个scanner.close会把System.in也关闭掉，System.in是InputStream的对象,并且关掉之后不能再打开。

这么写不好，一定要这么写的话，多个方法中的Scanner对象，在main中统一关闭，或者使用一个公用的Scanner对象。

实际工程中应该有优秀的框架阻止我这种愚蠢的写法。

System.in 源码提示：

```java
The "standard" input stream. This stream is already open and ready to supply input data. Typically this stream corresponds to keyboard input or another input source specified by the host environment or user.
```

说明System.in是早主机环境早就打开给用户的标准键盘等输入设备响应的输入流。不会是关了就没了吧...虽然源码中有重新设置输入流的静态方法，但这么麻烦的话肯定是用法不对。



# java.io.FileNotFoundException: C:\D\aaa (Access is denied)

我创建字节输入流的时候，文件路径传了文件夹，导致这个错误，传入正确的文件路径即可。(Access is denied)让我误以为是C盘没有权限，又是已管理员运行程序又是重启，对比之前的正常代码才发现是文件夹的问题。

# java.lang.IllegalThreadStateException

一个线程重复start，抛出异常但该线程会继续执行到结束。但重复start的错误代码所在主线程之后的代码不会执行。



# java.lang.StackOverflowError

```
public class Demo {
    Demo demo = new Demo();
    public static void main(String[] args) {
        Demo demo = new Demo();
    }
}
//目前分析是构造器调用导致的栈溢出，而不是引用导致的栈溢出。要确认一下，我认为新的引用应该出现在堆上而不是栈上
```

# ConcurrentModificationException

为了避免在多线程情况下, 一个线程在使用iterator遍历Collection集合类, 而另一个线程在对这个Collection集合类进行添加或者删除操作, 导致遍历结果出现问题, 产生了并发修改异常(如果一个线程在做添加和删除, 而另一个线程在做iterator遍历, 那么iterator遍历会立马抛出一个并发修改异常)

在单线程的情况下, 如果在iterator迭代的过程中, 我们使用了源集合类的添加,删除方法, 来修改源集合类, 那么在iterator迭代的过程中`modCount != expectedModCount`
会不相等, 也会产生并发修改异常



# SQL语句在Java程序中没有结果，在数据库中有结果

SQL语句过滤条件中含中文

编码问题，因为

因为数据库编码为utf-8，但jdbc的url地址没有指定编码格式

在url最后添加?useUnicode=true&characterEncoding=UTF-8

# ERROR 1213 (40001): Deadlock found when trying to get lock; try restarting transaction

MySQL数据库死锁

版本MySQL57

条件：

串行化测试MySQL事务,在两个cmd窗口中

```
start transaction 1		start transaction 2
qurey1					query1  --both ok
						update2 --2 is blocked
update1 + enter	--error shows
```



# 路径空格警告

IDEA

使用类加载器.getResource("").getPath();获取到的路径会把空格替换为%。导致new File(path).exist()是false。

不要在路径中使用空格