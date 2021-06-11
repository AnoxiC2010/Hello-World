# Java Bugs

## java.util.NoSuchElementException: No line found

有多个Scanner对象的情况下，关闭一个后导致其他Scanner对象无法读取。即使是调用了两个不同的方法，它们各自都创建了Scanner对象，先执行的方法关闭了Scanner资源后，后调用的方法中的Scanner也无法读取。

网上提示：第一个scanner.close会把System.in也关闭掉，System.in是InputStream的对象,并且关掉之后不能再打开。

这么写不好，一定要这么写的话，多个方法中的Scanner对象，在main中统一关闭，或者使用一个公用的Scanner对象。

实际工程中应该有优秀的框架阻止我这种愚蠢的写法。

System.in 源码提示：

```java
The "standard" input stream. This stream is already open and ready to supply input data. Typically this stream corresponds to keyboard input or another input source specified by the host environment or user.
```

说明System.in是早主机环境早就打开给用户的标准键盘等输入设备响应的输入流。不会是关了就没了吧...虽然源码中有重新设置输入流的静态方法，但这么麻烦的话肯定是用法不对。



## java.io.FileNotFoundException: C:\D\aaa (Access is denied)

我创建字节输入流的时候，文件路径传了文件夹，导致这个错误，传入正确的文件路径即可。(Access is denied)让我误以为是C盘没有权限，又是已管理员运行程序又是重启，对比之前的正常代码才发现是文件夹的问题。

## java.lang.IllegalThreadStateException

一个线程重复start，抛出异常但该线程会继续执行到结束。但重复start的错误代码所在主线程之后的代码不会执行。



## java.lang.StackOverflowError

```
public class Demo {
    Demo demo = new Demo();
    public static void main(String[] args) {
        Demo demo = new Demo();
    }
}
//目前分析是构造器调用导致的栈溢出，而不是引用导致的栈溢出。要确认一下，我认为新的引用应该出现在堆上而不是栈上
```

## ConcurrentModificationException

为了避免在多线程情况下, 一个线程在使用iterator遍历Collection集合类, 而另一个线程在对这个Collection集合类进行添加或者删除操作, 导致遍历结果出现问题, 产生了并发修改异常(如果一个线程在做添加和删除, 而另一个线程在做iterator遍历, 那么iterator遍历会立马抛出一个并发修改异常)

在单线程的情况下, 如果在iterator迭代的过程中, 我们使用了源集合类的添加,删除方法, 来修改源集合类, 那么在iterator迭代的过程中`modCount != expectedModCount`
会不相等, 也会产生并发修改异常



## SQL语句在Java程序中没有结果，在数据库中有结果

SQL语句过滤条件中含中文

编码问题，因为

因为数据库编码为utf-8，但jdbc的url地址没有指定编码格式

在url最后添加?useUnicode=true&characterEncoding=UTF-8

## ERROR 1213 (40001): Deadlock found when trying to get lock; try restarting transaction

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



## org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesJdbc The web application [ROOT] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. 

参考：此问题不影响使用
https://blog.csdn.net/fragrant_no1/article/details/88327180

[2021-05-14 10:57:35,623] Artifact mall:war exploded: Artifact is being deployed, please wait...
14-May-2021 22:57:35.756 WARNING [RMI TCP Connection(7)-127.0.0.1] org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesJdbc The web application [ROOT] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak, the JDBC Driver has been forcibly unregistered.
14-May-2021 22:57:35.757 WARNING [RMI TCP Connection(7)-127.0.0.1] org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesJdbc The web application [ROOT] registered the JDBC driver [org.apache.ibatis.datasource.unpooled.UnpooledDataSource.DriverProxy] but failed to unregister it when the web application was stopped. To prevent a memory leak, the JDBC Driver has been forcibly unregistered.
14-May-2021 22:57:35.765 WARNING [RMI TCP Connection(7)-127.0.0.1] org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesThreads The web application [ROOT] appears to have started a thread named [Abandoned connection cleanup thread] but has failed to stop it. This is very likely to create a memory leak. Stack trace of thread:
 java.lang.Object.wait(Native Method)
 java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:144)
 com.mysql.jdbc.AbandonedConnectionCleanupThread.run(AbandonedConnectionCleanupThread.java:64)
 java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
 java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
 java.lang.Thread.run(Thread.java:748)
14-May-2021 22:57:36.166 WARNING [RMI TCP Connection(7)-127.0.0.1] org.apache.tomcat.util.descriptor.web.WebXml.setVersion Unknown version string [4.0]. Default version will be used.
14-May-2021 22:57:38.699 INFO [RMI TCP Connection(7)-127.0.0.1] org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.
[2021-05-14 10:57:39,238] Artifact mall:war exploded: Artifact is deployed successfully
[2021-05-14 10:57:39,239] Artifact mall:war exploded: Deploy took 3,616 milliseconds
14-May-2021 22:57:40.617 INFO [Abandoned connection cleanup thread] org.apache.catalina.loader.WebappClassLoaderBase.checkStateForResourceLoading Illegal access: this web application instance has been stopped already. Could not load []. The following stack trace is thrown for debugging purposes as well as to attempt to terminate the thread which caused the illegal access.
 java.lang.IllegalStateException: Illegal access: this web application instance has been stopped already. Could not load []. The following stack trace is thrown for debugging purposes as well as to attempt to terminate the thread which caused the illegal access.
	at org.apache.catalina.loader.WebappClassLoaderBase.checkStateForResourceLoading(WebappClassLoaderBase.java:1364)
	at org.apache.catalina.loader.WebappClassLoaderBase.getResource(WebappClassLoaderBase.java:1021)
	at com.mysql.jdbc.AbandonedConnectionCleanupThread.checkContextClassLoaders(AbandonedConnectionCleanupThread.java:90)
	at com.mysql.jdbc.AbandonedConnectionCleanupThread.run(AbandonedConnectionCleanupThread.java:63)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)





## 路径空格警告

IDEA

使用类加载器.getResource("").getPath();获取到的路径会把空格替换为%。导致new File(path).exist()是false。

不要在路径中使用空格



##  java.lang.IllegalStateException: An Errors/BindingResult argument is expected to be declared immediately after the model attribute, the @RequestBody or the @RequestPart arguments to which they apply: public com.cskaoyan.bean.vo.BaseRespVo com.cskaoyan.controller.UserController.login(org.springframework.validation.BindingResult,com.cskaoyan.bean.LoginUserBO,javax.servlet.http.HttpSession)

使用spring-mvc框架配置参数校验时发生的错误

```java
@PostMapping("login")
public BaseRespVo login(@Valid LoginUserBO bo, BindingResult bindingResult, HttpSession session) {
    if (bindingResult.hasFieldErrors()) {//参数校验
        return ValidateUtils.paramsValidate(bindingResult);
    }
    ...
}
```

描述：

当把参数中的BindingResult放在HttpSession之后，参数校验不通过也不会进入到这个Handler方法中的断点，没有任何错误提示；当把BindingResult放在第一位即@Valid标记参数之前才会复现这个错误。

从这个错误得知BindingResult作为参数必须放在@Validated @Valid 标记需要校验的参数紧挨着的下一位，中间不能相隔其他参数，最坏的情况是连提示都没有。

 

## org.apache.ibatis.binding.BindingException: Invalid bound statement (not found): com.cskaoyan.mapper.UserMapper.getUsers

当把前一个项目的代码复制到新项目时，运行时报错。确认spring整合mybatis配置没有问题，且复制的dao层没有问题。tomcat点击热deploy也不行。

尝试删除target目录并再次build生成artifact产生新的target目录，再次运行tomcat目录异常解决。

总结应该是IDEA在编译时可能没有正确的编译mapper的xml文件或接口，再出现这种问题后确认代码没有问题就应该检查项目的target目录看看dao层的编译是否正常。或者直接删除旧的target或用maven的clean，之后再次编译生成新的target。



# Java的诡异运行结果

当代码运行结果和自我预测的结果不一致时，一定是我的认知浅薄。

但这些细微之处都太秃废精力，好想躺下...求包养...在这个宇宙online虚拟机中，迟早都会被垃圾回收掉的，这短暂的生命周期里，就没有个对象考虑收留下吗，我会翻跟头，会喷火，养的好的话还会升级，解锁新技能...



朋友介绍国企编制18k的驻外工作，很香，一个小人一直推着我去。想了很多，问了些人，这平平无奇的生活中我最想解决的bug是拥有随时脱离熟悉的环境在陌生的地方一个人生活下去的能力，实在不能就删号吧。就先做一个贫弱的码农吧，这总让我联想到不好的事情，但是

反复横跳还是拒绝了这个在编躺平的机会，我意识到折磨自己已经成为了快乐的来源

诅咒已经生效了呀

# <mark>IDEA和Tomcat环境中 代码运行矛盾</mark>

WIN10 - IDEA 2018.3.6 - tomcat 8.5.37 - Project SDK 1.8.0_281

## 字符串编码矛盾

IDEA下 run窗口输出

```java
//正常，
public class Demo {
    public static void main(String[] args) throws UnsupportedEncodingException {
        String s = "testServlet2 : 顺便中文乱码测试";
        System.out.println(s);//不乱
        String r = new String(s.getBytes(), "utf-8");
        System.out.println(r);//不乱，说明默认编码为utf-8
        r = new String(s.getBytes(), "gbk");
        System.out.println(r);//乱(正常)
        r = new String(r.getBytes("gbk"), "utf-8");
        System.out.println(r);//不乱(正常)
    }
}
/*
testServlet2 : 顺便中文乱码测试
testServlet2 : 顺便中文乱码测试
testServlet2 : 椤轰究涓枃涔辩爜娴嬭瘯
testServlet2 : 顺便中文乱码测试
*/
```

IDEA 启动tomcat后 run/debug窗口输出

```java
//使用UTF-8这个错误编码在中间环节编解码一次，输出反常
@WebServlet("/testServlet2")
public class TestServlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("TestServlet2.doGet");
        resp.setCharacterEncoding("gbk");
        String s = "testServlet2 : 顺便中文乱码测试";
        System.out.println(s);
         String r = new String(s.getBytes(), "gbk");
        System.out.println(r);//不乱，说明默认编码为gbk
        String r = new String(s.getBytes(), "utf-8");
        System.out.println(r);//乱(正常，其实从乱码的表现来看，和demo比已经不正常了，但是这里必须乱码是肯定的)
        r = new String(r.getBytes("utf-8"), "gbk");
        System.out.println(r);//乱(反常)！！！！
        resp.getWriter().println(r);
    }
}
/*Debug Output窗口输出：
TestServlet2.doGet
testServlet2 : 顺便中文乱码测试
testServlet2 : 顺便中文乱码测试
testServlet2 : ??????????????
testServlet2 : 顺锟斤拷锟斤拷锟斤拷锟斤拷锟斤拷锟斤拷锟?
*/
```

```java
//使用iso-8859-1这个错误编码在中间环节编解码一次，输出正常，和在IDEA普通demo中的结果一样
@WebServlet("/testServlet2")
public class TestServlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("TestServlet2.doGet");
        resp.setCharacterEncoding("gbk");
        String s = "testServlet2 : 顺便中文乱码测试";
        System.out.println(s);
         String r = new String(s.getBytes(), "gbk");
        System.out.println(r);//不乱，说明默认编码为gbk
        String r = new String(s.getBytes(), "iso-8859-1");
        System.out.println(r);//乱(正常，其实从乱码的表现来看，和demo比已经不正常了，但是这里必须乱码是肯定的)
        r = new String(r.getBytes("iso-8859-1"), "gbk");
        System.out.println(r);//乱(反常)！！！！
        resp.getWriter().println(r);
    }
}
/*Debug Output窗口输出：
TestServlet2.doGet
testServlet2 : 顺便中文乱码测试
testServlet2 : 顺便中文乱码测试
testServlet2 : ??±?????????????
testServlet2 : 顺便中文乱码测试
*/
```

以上结果看来问题出新在new String(byte[] bytes, String charsetName)方法，查看源码

```
/**
     * Constructs a new {@code String} by decoding the specified array of bytes
     * using the specified {@linkplain java.nio.charset.Charset charset}.  The
     * length of the new {@code String} is a function of the charset, and hence
     * may not be equal to the length of the byte array.
     * 说给这个构造的bytes[]和charsetName不一致时，行为就暧昧不明了，结果就不能按常理预料了，所以没事别瞎搞就没这些个幺蛾子了，再深入的还得往底层看...先躺下了
     * <p> The behavior of this constructor when the given bytes are not valid
     * in the given charset is unspecified.  The {@link
     * java.nio.charset.CharsetDecoder} class should be used when more control
     * over the decoding process is required.
     *
     * @param  bytes
     *         The bytes to be decoded into characters
     *
     * @param  charsetName
     *         The name of a supported {@linkplain java.nio.charset.Charset
     *         charset}
     *
     * @throws  UnsupportedEncodingException
     *          If the named charset is not supported
     *
     * @since  JDK1.1
     */
public String(byte bytes[], String charsetName)
            throws UnsupportedEncodingException {
        this(bytes, 0, bytes.length, charsetName);
    }
```



## 反射方法矛盾

IDEA中

```java
//User Bean的set方法，参数是数组
public void setHobby(String[] hobby) {
    this.hobby = hobby;
}

//在IDEA的测试demo中
public static void main(String[] args) throws IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
    Class<User> userClass = User.class;
    User user = userClass.newInstance();

    Method setHobby = userClass.getDeclaredMethod("setHobby", String[].class);
    String[] hobby = new String[]{"sleep", "play"};

    setHobby.invoke(user, hobby);//不可以，参数数量错误
    setHobby.invoke(user, new Object[]{hobby});//可以
    setHobby.invoke(user, (Object)hobby);//可以

    System.out.println(Arrays.toString(user.getHobby()));
}
```

IDEA启动tomcat之后在servlet中

```java
//@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    @Override//在IDEA的tomcat环境中
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException {
        Class<User> userClass = User.class;
        User user = userClass.newInstance();

        Method setHobby = userClass.getDeclaredMethod("setHobby", String[].class);
        String[] hobby = new String[]{"sleep", "play"};

        setHobby.invoke(user, hobby);//可以，这是什么鬼！！！！！
        setHobby.invoke(user, new Object[]{hobby});//不可以，这是什么鬼！！！！
        setHobby.invoke(user, (Object)hobby);//可以

        System.out.println(Arrays.toString(user.getHobby()));
    }
}
```



# IO流的矛盾

一个含有输入流的方法在另一个try...catch语句中调用读不到东西，在try...catch块之前调用能读取到文件的数据

总结：和try...catch无关，和FileOutputStream的机制有关，new一个FileOutputStream会清空文件的数据...

所以问题变成了在建立输出流之前读取得到，和在输出流之后读取不到的问题，所以是我不够了解的问题

```java
@Test
public void testForIO() {
    String path = this.getClass().getClassLoader().getResource("users.jason").getPath();

    try (InputStream in = new FileInputStream(path)) {
        byte[] bytes = new byte[1024];
        int length = in.read(bytes);
        System.out.println("读取到数据长读=" + length);
        String str = new String(bytes, 0, length);
        System.out.println(str);
    }
    try (OutputStream out = new FileOutputStream(path)) {
        ...
    }
}
```

```java
@Test
public void testForIO() {
    String path = this.getClass().getClassLoader().getResource("users.jason").getPath();
	//一切的开始只是想偷懒一起catch住异常
    try (OutputStream out = new FileOutputStream(path)) {
        try (InputStream in = new FileInputStream(path)) {
            byte[] bytes = new byte[1024];
            int length = in.read(bytes);
            System.out.println("读取到数据长读=" + length);
            String str = new String(bytes, 0, length);
            System.out.println(str);
        }
    }
}
```









 
