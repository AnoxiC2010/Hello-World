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