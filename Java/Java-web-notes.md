# EE

## 学习方式

debug

核心点：搞清楚程序的执行流程、通路。比如说一条河流被污染，需要找到污染源，大体分为三部分、上游、中游、下游

程序执行步骤比较多

IDEA工具debug

总结梳理编程过程中遇到的bug。

## EE概述

Enterprise Edition。企业版。为了企业开发复杂的网站而设计的。企业开发的应用都是比较庞大且复杂的，如果单纯使用SE阶段的相关的API，的确可以完成我们的场景需求，但是太过于复杂。可以将这些复杂的代码又做了进一步的封装处理，提供了一些非常简单的API来供企业开发者来使用，这个就称为EE。  之前SE阶段50行代码，EE将这50行代码封装到一个方法中，只需要调用该方法即可完成之前SE阶段的50行代码的功能。SE阶段的一个延续。涉及到网络。

EE其实就是网络编程的一个延续。

介绍EE阶段的几个概念

**客户端**：手机里面装的app、京东、淘宝等这些app；浏览器

**服务器**（服务端）：提供服务的。打开app、访问某个网站为什么可以看到数据呢？服务器会给我们提供了对应的数据，传输了过来

传输的过程，如何传输的呢？**协议**，其实就是一串具有特殊特定格式的消息

![image-20210505100232954](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210505100232954.png)

## 协议

协议：

双方再进行消息的通讯，需要传输学生的信息给另一方

发送方：需要去发送学生的信息

张三  23 湖北省武汉市   java

李四  25 湖北省天门市  python



接收方如何来接收：

每行是一个学生的信息，根据空格来分割，又可以分割出学生姓名、年龄、籍贯、专业



协议其实也就是如此。也就是指数据的传输的格式，只要双方都遵循着这一规范、都按照这个规则来，那么就可以顺利地解析出里面的内容来

Standard Edition。标准版、个人版



## 

# HTTP协议（整个EE阶段的重点）

全称。Hyper Text Transfer Protocol.超文本传输协议。

html

http和html之间非常紧密的联系。

实验室主要做科研。

写论文。html  h1  div span  img

如何与其他人进行共享？组会。

http用来传输html的。文本、音频、视频、图片等。

bernes lee

linus

Rod Johnson-Spring创作者。音乐学博士。

## HTTP工作流程

HTTP是在网络中进行数据传输的，就不避免的要利用到网络的底层基础设施。

网络的底层基础设施：

按照逻辑上的划分，一般我们划分为四个层：

应用层--------通讯双方真正要传输的数据内容  （我们需要邮寄的包裹、物品）

传输层--------传输的方式、TCP方式、UDP方式  （快递）

网络层--------IP协议（数据如何在网络中流转的）  （包裹的发送方、接收方信息）

链路层-------网卡驱动等硬件相关  					（快递的物流设施）

### 域名解析

以在浏览器输入http://www.baidu.com，首先需要进行域名解析

什么是域名？其实就是一串非常容易记住的有意义的词汇，对于计算机来说是无任何意义的，对于计算机来说，真正有意义的是ip地址，ip地址很复杂，人不容易记住的，所以创建了域名-------------ip地址做了一个映射关系   DNS



详细的过程：

1.浏览器去搜寻自身的DNS缓存信息（第一次访问某个网站，你会发现速度是要稍微慢一些的）

2.去到操作系统中搜寻DNS缓存信息

3.读取hosts文件

4.向本地配置的DNS服务器发起域名解析请求

### TCP三次握手

为什么进行三次握手呢？

一次不可以？两次不可以？

至少要三次。



### 发送HTTP请求

当我们的网络底层通路建立成功之后，接下来就可以真正发送HTTP请求了。

其实就是发送了一串具有特殊格式的消息过去

过程：

浏览器根据你输入的网址，然后生成对应的**HTTP请求消息**

消息很大，一般情况下，会经过TCP进行拆包，拆成各个片段

经过ip，会加上ip头部（就是网络地址）

经由链路层传输出去，再网络上进行中转传输

当到达目标机器以后

经由链路层进入到服务器主机内

首先将ip头部给去掉

将多个片段的消息重新组装，同时去掉tcp头部

HTTP请求消息是具有某种特殊格式的消息，服务器根据这个格式，那就可以解析出请求消息里面的所有内容来

就可以得知客户端究竟想访问哪个页面

### 服务器做出响应

服务器拿到该页面，然后将文件的流写入到输出流中，然后接下来生成一个**HTTP响应消息**

响应消息也会先经过tcp，进行一个拆包，同时加上tcp头部

再次经过ip，加上ip头部

再通过链路层，传输出去，网络上一边中转，一边传输，到达客户端主机



客户端再次去掉响应消息的ip头部、tcp头部，消息重新组装，形成完整的**HTTP响应消息**

### 浏览器解析标签

一般情况下，我们返回来的都是一个html页面，所以浏览器解析遇到的所有的html标签，如果遇到img标签，js标签、css标签等，

`<img src =>`

`<style ref= xxxxx.css>`


   都会去执行相同的操作，去发送HTTP请求，请求对应的资源文件 

### 渲染页面

当把所有需要的文件全部都拿到之后，接下来，根据css样式、js定义、图片等最终渲染出一个好看的页面。

## HTTP报文（非常重要）

HTTP请求消息和HTTP响应消息，我们一般情况下也称之为HTTP请求报文和HTTP响应报文

### HTTP请求报文

利用到一个抓包工具。fiddler（正常情况下来说，无法抓取到https，浏览器不要设置代理，否则也抓不到）

http://www.cskaoyan.com

![image-20210505112332879](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210505112332879.png)

#### 请求行

一共可以分为三个部分。 

**请求方法**：使用何种请求方式去访问当前的资源地址。**GET、POST**、还有一些其他不怎么常用的HEAD、OPTIONS、PUT、DELETE等

**GET和POST请求方法究竟有什么样的不同呢**？

先演示一下get请求和post请求

GET请求：

![image-20210505113046681](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210505113046681.png)

POST请求：

![image-20210505113132845](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210505113132845.png)

**特别注意一点**：

**get请求和post请求方式的区别不在于请求参数的位置，这个只是浏览器的默认行为**。（浏览器会默认情况下在发送get请求时，将参数放置在地址栏中；在发送post请求时将请求参数放置在请求体中，但是他们不是get和post的请求的差异）

我们完全可以发送一个post请求，并且将请求参数放置在地址栏中。



**真正的差异在于语义上的差异**。

**get主要是用来获取资源（查询某个商品）记住这一个点：默认情况下，浏览器基本发送的都是get请求**

**post主要是用来提交资源：登录、提交订单、付款（将本地的数据提交给服务器，那么应该使用post请求方式）**

**请求资源**

访问多个页面，我们观察HTTP请求报文

页面1：

![image-20210505113751553](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210505113751553.png)

页面2：

![image-20210505113829405](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210505113829405.png)

比对两者之间的差异：

其实可以通过请求资源可以发现客户端真实请求的页面。



**版本协议**

说明当前HTTP协议使用的版本。

目前来说HTTP协议是1.1版本，上一个版本是1.0版本

现阶段的版本相较于上一阶段的版本最大的提升在于可以在一个TCP连接内发送多个HTTP请求了。

目前在规划的版本是HTTP 2.0   3.0

#### 请求头

​	**Accept**:浏览器可接受的    MIME类型 */*   (大类型)/(小类型)

​	什么叫MIME？

​	其实就是将互联网上面的资源进行了一个分类，分成了几个大类；

​	文本、音频、视频、图片等

​	每个大类下面又可以分为很多个小类

​	文本：txt、html等

​	音频：mp3、flac等

​	视频：mp4、mkv、avi等

​	如何去定义一个资源呢？

​	其实就可以使用大类型/小类型的方式来表示互联网上面的任何一个资源，比如text/html;image/png;

​	**Accept-Charset**: 浏览器通过这个头告诉服务器，它支持哪种字符集
​	**Accept-Encoding**:浏览器能够进行解码的数据编码方式，比如gzip 
​	**Accept-Language**: 浏览器所希望的语言种类，当服务器能够提供一种以上的语言版本时要用到。 可以在浏览器中进行设置。

​	twitter、google支持多语言的

​	**Host**:初始URL中的主机和端口 
​	**Referer**:包含一个URL，用户从该URL代表的页面出发访问当前请求的页面 （防盗链）

​	比如我通过一个页面A跳转到页面B

​	还可以直接访问页面B

​	上述两种情形，通过请求报文能不能区分呢？



通过页面进行跳转：

POST http://www.cskaoyan.com/ HTTP/1.1
Host: www.cskaoyan.com
Connection: keep-alive
Content-Length: 9
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://localhost:63342
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
**Referer: http://localhost:63342/**
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9

username=



直接访问：

GET http://www.cskaoyan.com/ HTTP/1.1
Host: www.cskaoyan.com
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: cZBD_2132_saltkey=wWh8vvV8; cZBD_2132_lastvisit=1619588107; cZBD_2132_sendmail=1; Hm_lvt_5f3c4e32676aacc710ede84276010d9b=1620184889,1620185406,1620185479,1620196679; cZBD_2132_sid=WBQ1rC; cZBD_2132_lastact=1620196710%09home.php%09misc; Hm_lpvt_5f3c4e32676aacc710ede84276010d9b=1620196710

referer表示的是从哪个页面出发，来到当前页面的

应用场景：

防盗链（qq空间里面照片，有的照片在空间以外是无法显示出来的）

网站里面有广告（google ads）

你搭建了一个网站，网站里面接了广告。只要用户通过你的网站去访问这个广告，引流。



​	**Content-Type**:内容类型。更多的是用在响应中。

​	**If-Modified-Since**: Wed, 02 Feb 2011 12:04:56 GMT 服务器利用这个头与服务器的文件进行比对，如果一致，则告诉浏览器从缓存中直接读取文件。
​	**User-Agent**:浏览器类型.
​	**Content-Length**:表示请求消息正文的长度 。请求体的长度
​	Connection:表示是否需要持久连接。如果服务器看到这里的值为“Keep -Alive”，或者看到请求使用的是HTTP 1.1（HTTP 1.1默认进行持久连接 
​	Cookie:这是最重要的请求头信息之一 
​	Date：Date: Mon, 22 Aug 2011 01:55:39 GMT请求时间GMT

#### 空行

#### 请求体

### HTTP响应报文

给客户端看的。

#### 响应行

版本协议

**状态码**

200  OK  表示请求的访问过程没有任何问题

**301、302、307 重定向相关的状态码**。指示你向新的地址再去发送请求。**需要搭配着一个Location响应头一起使用。**

www.bing.com 微软

因为fiddler无法抓取到https的请求，所以我们可以使用chrome来抓包。

**304 Not Modified 未修改**。将要使用缓存。

**404 Not Found**.是一个有效的响应，表示访问的资源在当前服务器上面不存在。

**500  服务器报错**。出现这个状态码，就意味着服务器发生了异常。去查看服务器的输出日志，排查故障。

原因短语

#### 响应头

**Location**: http://www.cskaoyan.com/指示新的资源的位置 
**Server**: apache tomcat 指示服务器的类型
**Content-Encoding**: gzip 服务器发送的数据采用的编码类型
**Content-Length**: 80 告诉浏览器正文的长度
**Content-Language**: zh-cn服务发送的文本的语言
**Content-Type**: text/html;  服务器发送的内容的MIME类型
**Last-Modified**: Tue, 11 Jul 2000 18:23:51 GMT文件的最后修改时间
**Refresh**: 1;url=http://www.cskaoyan.com指示客户端刷新频率。单位是秒

**Content-Disposition**: attachment; filename=aaa.zip指示客户端保存文件
**Set-Cookie**: SS=Q0=5Lb_nQ; path=/search服务器端发送的Cookie
**Expires**: 0
**Cache-Control**: no-cache (1.1)  
**Connection**: close/Keep-Alive   
**Date**: Tue, 11 Jul 2000 18:23:51 GMT

#### 空行

#### 响应体

**里面的数据一般情况下回显示在浏览器的主窗口中**。

## HTTPS

https是一个全新的协议吗？不是。

https = http + secure

安全性暴涨。

http有哪些安全性问题？

1.通讯过程全程明文传输，没有任何加密

2.没有验证通讯另外一方的身份

3.报文没有完整性校验，被篡改也无法发现



https

1.加密

2.通过证书来验证身份

3.加上完整性校验



加密：加密算法。

对称加密（加密解密使用的是同一把秘钥，速度很快，但是安全性没有太大的保障）、非对称加密（公钥加密的只可以使用私钥来进行解密，安全性很高，但是速度很慢）。

https采用的是哪种呢？都采用了，混合加密。





# Javaweb 应用

WEB应用程序指供浏览器访问的程序，通常也简称为web应用。

一个web应用由多个静态web资源和动态web资源组成，如:
html、css、js文件
Jsp文件、java程序、支持jar包、
配置文件
……

Web应用开发好后，若想供外界访问，需要把web应用所在目录交给web服务器管理，这个过程称之为虚似目录的映射，应用在webapps目录下无需映射。



## JavaWEB应用的组成结构

开发web应用时，不同类型的文件有严格的存放规则，否则不仅可能会使web应用无法访问，还会导致web服务器启动报错。

```
mail----web应用所在目录
 |
 |----html jsp css js 文件等(根目录下的文件外界可以直接访问)
 |
 |----WEB-INF目录(该目录外界无法直接访问，由web服务器负责调用)
         |
         |----classes目录----(java类)
         |----lib目录----(java类运行时所需的jar包)
         |----web.xml文件----(web应用的配置文件)
         |
```

web应用中，web.xml文件是其中最重要的一个文件，它用于对web应用中的web资源进行配置。

在WEB-INF目录的classes及lib子目录下，都可以存放java类文件。在运行时，Servlet容器的类加载器先加载classes目录下的类，再加载lib目录下的JAR文件中的类。因此，如果两个目录下存在同名的类，classes目录下的类具有优先权。
我们注意到Tomcat的安装目录下也有一个lib目录，这个与Web应用中的lib目录的区别在于：
Tomcat的lib子目录：存放的JAR文件不仅能被Tomcat访问，还能被所有在Tomcat中发布的JavaWeb应用访问。
JavaWeb应用的lib子目录：存放的JAR文件只能被当前JavaWeb应用访问。
假如Tomcat类加载器要加载一个MyClass的类，它会按照以下先后顺序到各个目录中去查找MyClass的class文件，直到找到为止，如果所有目录中都不存在MyClass.class的文件，则会抛出异常：
1、在JavaWeb应用的WEB-INF/classes中查找MyClass.class文件。
2、在JavaWeb应用的 WEB-INF/lib目录下的JAR文件中查找MyClass.class文件。
3、在Tomcat的lib子目录下直接查找MyClass.class文件。
4、在Tomcat的lib子目录下JAR的文件中查找MyClass.class文件。
Note：
Tomcat6.x与Tomcat5.x的目录结构有所区别。在Tomcat5.x版本中，Tomcat允许在server/lib目录、common/lib和shared/lib目录下存放JAR文件，这3个目录的区别在于：
在server/lib目录下的JAR文件只可被Tomcat访问。
在shared/lib目录下的JAR文件可以被所有的JavaWeb应用访问，但不能被Tomcat访问。
在common/lib目录下的JAR文件可以被Tomcat和所有JavaWeb应用访问。



## 发布JavaWeb应用

开放式目录和war

```
Jar –cvf  ***.war .
```



## Web应用（组件）的URL

无论是开放式目录结构还是打包文件方式发布web应用，web应用的默认URL入口都是Web应用的根目录名。例如要访问MyApp应用，它的URL入口为/MyApp，如访问本地服务http://localhost:8080/MyApp

（http://127.0.0.1:8080/MyApp）





# 服务器

## 概念

互联网上面的资源：

**静态资源**：数据的内容是一成不变的，不会有任何改变，不会因为任何人、任何时间点发生改变

**动态资源**：数据是富有变化性的，不同人、不同时间点看到的内容都是不一样的。比如用户登录显示用户的用户名、个性化推荐

Servlet就是用来开发动态web资源的。



网站的架构：

B/S:浏览器/服务器。PC机上面，浏览器性能非常好，使用非常方便。

C/S:客户端/服务器。个人手机上面，倾向于使用app。只有网站只有手机端的app可以使用，拼xx。



为什么要有服务器？

服务器一般两层含义。一层含义，性能很好地计算机。

另外一层含义，软件层面，可以将本地上面的资源文件，然后共享给同网络的其他用户来访问使用。



为什么要有服务器呢？

先从SE阶段网络编程开始，如果一个用户A需要去请求另外一个用户B的资源文件。



如果又有一个人也希望能够访问用户B的资源文件。重复性的代码又写了一遍



这些代码完全可以当做一个基础的架构工具来使用，将这部分代码抽提，形成一个软件，称之为服务器。



## 实现简易版静态Web资源服务器

深入理解HTTP协议，写一下静态资源服务器。

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
//静态资源服务器的处理原则：根据你请求的资源做出对应的响应；如果资源文件存在，则返回文件的内容；如果文件的内容不存在，则返回404
//1.问题1 如何知道用户访问的究竟是哪个页面呢？ HTTP请求报文的请求资源时不同的
//2.问题2 如何获取请求报文的请求资源

/**
 * 请求报文的格式：
 * 请求方法 请求资源 版本协议\r\n请求头key：请求头value\r\n....\r\n\r\n请求体
 * 面向对象的思想  request对象里面
 */
public class WebServer {

    public static void main(String[] args) {
        //开启服务
        try {
            //有一个程序会监听8090端口号
            ServerSocket serverSocket = new ServerSocket(8090);
            //这一步默认情况下是阻塞的，如果有客户端连接进来，那么这一步会触发继续往下执行
            while (true){
                Socket client = serverSocket.accept();
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        //获取客户端的信息，只需要从client里面去取就可以了
                        Request request = new Request(client);
                        String requestURI = request.getRequestURI();
                        System.out.println(requestURI);
                        //希望你设计一个方法能够将所有的请求头键值对打印出来
                        //Host:localhost:8090
                        String host = request.getHeader("Host");
                        System.out.println(host);
                        //如果你希望向客户端去发送任何数据，只需要往这里面写入数据即可
                        OutputStream outputStream = null;
                        try {
                            outputStream = client.getOutputStream();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                        StringBuffer buffer = new StringBuffer();
                        File file = new File(requestURI.substring(1));
                        try {
                            if(file.exists() && file.isFile()){
                                FileInputStream inputStream = new FileInputStream(file);
                                byte[] bytes = new byte[1024];
                                int length = 0;
                                //究竟什么是协议
                                buffer.append("HTTP/1.1 200 OK\r\n");
                                buffer.append("Content-Type:text/html\r\n");
                                buffer.append("\r\n");
                                outputStream.write(buffer.toString().getBytes("utf-8"));
                                while ((length = inputStream.read(bytes)) != -1){
                                    outputStream.write(bytes, 0, length);
                                }
                            }else {
                                //返回404的响应即可
                                buffer.append("HTTP/1.1 404 Not Found\r\n");
                                buffer.append("Content-Type:text/html\r\n");
                                buffer.append("\r\n");
                                buffer.append("<h1 style='color:red'>File Not Found</h1>");
                                outputStream.write(buffer.toString().getBytes("utf-8"));
                            }
                        } catch (IOException e) {
                            e.printStackTrace();
                        }finally {
                            if(outputStream != null){
                                try {
                                    outputStream.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                        //根据请求资源做出对应的响应
                        //拿到1.html文件的输入流
                    }
                }).start();
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
import java.io.IOException;
import java.io.InputStream;
import java.net.Socket;
import java.util.HashMap;
import java.util.Map;

public class Request {

    private String method;

    private String requestURI;

    private String protocol;

    private Map<String, String> requestHeaders = new HashMap<>();


    public Request(Socket client) {
        try {
            InputStream inputStream = client.getInputStream();
            byte[] bytes = new byte[1024];
            //这一步也是阻塞步骤
            int read = inputStream.read(bytes);
            //HTTP请求报文
            String requestString = new String(bytes, 0, read);
            parseRequestLine(requestString);
            parseRequestHeaders(requestString);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    /**
     * 解析HTTP请求报文的请求头，将其拆解并存放于一个map中
     * 思路：
     * @param requestString
     */
    private void parseRequestHeaders(String requestString) {
        int begin = requestString.indexOf("\r\n");
        int end = requestString.indexOf("\r\n\r\n");
        String headerString = requestString.substring(begin + 2, end);
        String[] headerValuePairs = headerString.split("\r\n");
        for (String headerValue : headerValuePairs) {
            int i = headerValue.indexOf(":");
            String key = headerValue.substring(0, i).trim();
            String value = headerValue.substring(i + 1).trim();
            requestHeaders.put(key, value);
        }
    }

    /**
     * 将HTTP请求报文的请求行进行拆解
     * @param requestString
     */
    private void parseRequestLine(String requestString) {
        int index = requestString.indexOf("\r\n");
        String line = requestString.substring(0, index);
        String[] parts = line.split(" ");
        method = parts[0];
        requestURI = parts[1];
        protocol = parts[2];
        int i = requestURI.indexOf("?");
        if(i != -1){
            //有？
            requestURI = requestURI.substring(0, i);
        }
    }

    public String getMethod() {
        return method;
    }

    public String getRequestURI() {
        return requestURI;
    }

    public String getProtocol() {
        return protocol;
    }

    public String getHeader(String headerName){
        return requestHeaders.get(headerName);
    }
}
```



## 常见Web服务器

**JavaEE规范**

市面上其实存在很多的服务器产品。

各个公司开发的服务器产品使用方式可能都是完全不一样。

假设一个公司开发的服务器，解析请求报文得到的是一个叫做BaseRequest对象

另外一个公司开发的服务器产品，解析请求报文得到的是一个叫做GenericRequest对象

在使用的时候，如果你希望获取请求资源，在BaseRequest里面 getReqeustURI

在GenericRequest里面，叫做getRequestRecourse

JDBC

这种情况肯定不利于行业的发展。sun公司制定了一整套的标准，JavaEE规范，其中有一条就针对的就是请求报文解析之后的请求对象应该实现ServletRequest接口，接口里面定义了一个方法叫做getRequestURI()

不管是任何的服务器产品，全部都要实现该接口

我们在写代码啊的时候

ServletRequest.getRequestURI();

# Tomcat

[官方网站]([Apache Tomcat® - Welcome!](https://tomcat.apache.org/))

## Tomcat的组成结构

Tomcat本身由一系列可配置的组件构成，其中核心组件是Servlet容器组件，它是所有其他Tomcat组件的顶层容器。每个组件都可以在Tomcat安装目录/conf/server.xml文件中进行配置，每个Tomcat组件在server.xml文件中对应一种配置元素。以下用XML的形式展示了各种Tomcat组件之间的关系

```xml
<Server><!--代表整个Servlet容器组件，是最顶层元素。可以包含一个或多个Service元素-->
	<Service><!--包含一个Engine元素以及一个或多个Connector元素，这些Connector共享同一个Engine-->
        <Connector/><!--代表和客户程序实际交互的组件，负责接收客户请求，以及向客户返回响应-->
        <Engine><!--每个Service元素只能包含一个Engine元素。它处理在同一个Service中所有Connector接受到的客户请求-->
            <Host><!--在一个Engine中可以包含多个Host,它代表一个虚拟主机(网站)，它可以包含一个或多个web应用-->
                <Context/><!--使用最频繁的元素。代表了运行在虚拟主机上的单个web应用-->
            </Host>
        </Engine>
    </Service>
</Server>
```

## Tomcat体系架构

![image-20210507224747298](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210507224747298.png)

![image-20210507224750759](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210507224750759.png)

Tomcat处理HTTP请求的过程localhost/test/index.jsp：

1. 用户发送一个请求，被发送到当前机器的80端口号，被正在监听80端口号的coyote HTTP／1.1获得
2. Connector组件将收到的请求传递给Engine组件
3. Engine获得了请求地址为localhost/test/index.jsp，匹配虚拟主机
4. 匹配到名为localhost的host，如果没匹配到，也将请求交给它处理，它被定义为Engine的默认虚拟主机，该host获得/test/index.jsp，匹配它所拥有的全部Context
5. 匹配/test应用名对应的Context节点，Context节点获得index.jsp，它再去寻找响应的servlet
6. Servlet处理完逻辑
7. Context节点把执行完的结果返回给Host
8. Host将结果返回给Engine
9. Engine将结果返回给Connector组件
10. Connector将最终的响应结果返回给客户端

## Tomcat组件

​	tomcat其实是由一系列的组件所构成，组件是采用配置的形式来设置的，我们可以在conf/server.xml文件中去设置对应的标签节点，那么tomcat在启动的时候就会读取这些标签节点，然后生成对应的组件对象。

![image-20210506102918106](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210506102918106.png)



## Tomcat使用

### Tomcat安装

直接压缩包解压缩即可，放置于某个盘符根目录即可，不建议放置在很深的目录下或者中文目录下。

**bin目录：启停tomcat**

**conf目录：配置tomcat**

lib目录：服务器的支撑jar包

temp目录：tomcat运行时长生的临时文件

**logs目录：日志，排查故障**

**webapps目录：部署资源的目录**

work目录：工作目录

### Tomcat启动

windows平台：

​	1.双击startup.bat文件

​	2.在bin目录下面唤出cmd，然后执行startup

如何主动让tomcat出错 端口被占用（tomcat安装完之后，8080）

### Tomcat停止

​	1.双击shutdown.bat

​	2.ctrl + c可以结束

### Tomcat启动常见问题

如果出现启动过程tomcat窗口一闪而过，大多数情况下是因为JAVA_HOME没有正确的配置。

## Tomcat部署资源（重要，掌握）

​	部署资源的本质：根据客户端传输过来的请求地址，然后到服务器的本地硬盘上面去找到对应的文件，然后响应出去

### 直接部署

​	**直接部署在tomcat的webapps目录下，称之为直接部署**。

​	tomcat部署资源有一个要求：

​	tomcat里面存在的最小单位是应用，如果希望部署某个资源文件，比如html页面，那么应当部署在应用中

​	如何去生成一个应用呢？只需要在tomcat的webapps目录下新建一个目录，那么该目录就会形成一个应用，应用的名称就是目录的名称



​	如何访问部署的文件呢？

​	当输入http://localhost:8080时，相当于tomcat会自动定位到webapps目录，如何去访问某个资源文件，只需要去写出该文件相对于webapps的相对路径即可。

直接部署可不可以拿到文件的绝度路径？

**tomcat安装目录/webapps/app/1.txt**

直接部署除了可以部署目录之外，还可以部署一个war包（类似于一个压缩包，tomcat会自动将其解压缩为一个目录）

war包一般情况下我们用在开发完成项目之后，将代码打包成为一个war包，然后丢到服务器上面去

### 虚拟映射

正常情况下，我们部署一个资源文件都是放置于tomcat的webapps目录下的，但是如果你希望不放置在webapps目录下，也能够进行资源文件的部署，那么这个时候我们就可以利用虚拟映射来实现。虚拟地映射到tomcat的webapps目录下。

**方式一：**（**更为重要**）没有任何侵入性

需要在tomcat的conf/catalina/localhost目录下新建一个xml文件，**xml文件的文件名就会被解析成应用名**

比如再conf/catalina/localhost目录下新建一个app.xml文件，里面的内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context docBase="D:\app2"/>
<!--Context元素的path属性可以随便填，或不要，这个path属性只有在Tomcat的server.xml中配置Context时可以用来作为应用名使用-->
```

http://localhost:8080/app/index.html



比如现实生活中的地址

别名						路径

花山						湖北省武汉市洪山区花山软件新城二期



应用名 				应用的路径

app						D:\app2

/app/xxxx			D:\app2/xxxx



花山某厂---------等效的地址其实就是  湖北省武汉市洪山区花山软件新城二期某厂



**服务器的本质：在本地硬盘上面需要将该文件的绝对路径拿到**

比如再D：/app2目录下有一个资源文件，如何能够拿到该文件的绝对路径呢？



应用 ----------应用名  path   --------应用所在的路径  docBase

张家村 王五



**方式二：**

tomcat的conf/server.xml文件中进行配置

需要在Host节点下新增一个Context节点

```xml
<Context path="/app3" docBase="D:\app2"/>
```

其中关于path是应用名，docBase是应用的路径

> 在Tomcat6中，不再建议在server.xml文件中配置context元素，细节查看tomcat服务器关于context元素的说明。
>
> 让tomcat自动映射： tomcat服务器会自动管理webapps目录下的所有web应用，并把它映射成虚似目录。换句话说，tomcat服务器webapps目录中的web应用，外界可以直接访问。



![image-20210507230137683](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210507230137683.png)

在一般情况下，`<Context>`元素都会使用默认的标准Context组件，即className属性采用默认值org.apache.catalina.core.StandardContext，它除了拥有上面介绍到的属性外，还有自身专有的属性：
cachingAllowed
是否允许启用静态资源(HTML、图片、声音等)的缓存。默认值为true。
cacheMaxSize
设置静态资源缓存的最大值，单位为K。
workDir
指定Web应用的工作目录。
uppackWAR
如果为true，会把war文件展开为开放目录后再运行。为false，直接运行war文件。默认值为true。



Context元素

Tomcat提供了多种配置`<Context>`元素的途径。当其加载一个web应用时，会：
1)到Tomcat安装目录/conf/[enginename]/[hostname]/[contextpath].xml文件中查找`<Context>`元素。
2) 到Tomcat安装目录/conf/server.xml文件中查找`<Context>`元素。只适用于单个Web应用
[contextpath]：表示单个Web应用的URL入口。如果修改为ROOT，则该应用就是默认访问的应用。

```
Note：若想让程序成为默认的Web应用，即访问http://localhost:8080时自动登录到Web应用的主页，可以在此处增加名字为ROOT.xml文件，其<Context>元素的path属性应该为””
```



web.xml文件

通过web.xml文件，可以将web应用中的：
某个web资源配置为网站首页

```xml
<welcome-file-list>
         <welcome-file>hello.html</welcome-file>
         <welcome-file>index.html</welcome-file>
         <welcome-file>index.htm</welcome-file>
         <welcome-file>index.jsp</welcome-file>
</welcome-file-list>
```

将servlet程序映射到某个url地址上
……
但凡涉及到对web资源进行配置，都需要通过web.xml文件

例如：通过web.xml文件配置网站首页。

注意：web.xml文件必须放在web应用\WEB-INF目录下。





## 请求处理流程

以访问http://localhost:8080/app3/index.html为例

1.浏览器地址栏输入对应的网址，首先进行域名解析、tcp连接建立，然后生成HTTP请求报文

2.HTTP请求报文传输到达目标机器以后，到达HTTP/1.1 Connector程序

3.Connector会将HTTP请求报文进行解析，生成一个Request对象，同时还会提供一个空的Response对象来给我们写入数据使用

4.Connector会将这两个对象传给Engine，Engine会将这两个对象传给Host

5.Host的职责就是去挑选一个合适的Context  应用，查看是否有app3的应用，如果找到，则将这两个对象进一步交给该应用

6.应用的职责就是在当前应用中去查找对应的资源文件 docBase + /index.html ，该文件是否存在，文件写入到response里面

7.返回，直至到Connector，读取response里面的内容，然后生成响应报文

8.依赖底层的网络设施，将响应报文发送出去

## 额外设置

### 设置缺省应用

保底的设置。如果找不到其他的应用,那么就会交给缺省的应用来处理(只要你设置应用名叫做ROOT,那么就是缺省应用,在访问的时候直接把应用名忽略即可)

如果没有其他的应用可以选择，那么就会交给缺省的应用来处理。

缺省应用其实是ROOT

**当你在访问缺省应用时,虽然应用名叫做ROOT,但是访问的时候,直接忽略应用名不写即可.**

**如果你希望在访问某个资源文件时,不携带应用名,应该如何设置?**

**直接设置应用名为ROOT即可.**

两种：1.直接部署   2虚拟映射  conf/catalina/localhost-----ROOT.xml文件

### 设置缺省资源

默认情况下是再tomcat的conf/web.xml文件中设置的

```xml
<welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
</welcome-file-list>
```

当没有指明具体的访问资源时，tomcat会按照如下顺序依次取查找对应的文件，如果找得到，那么则加载该文件，如果找不到则最终404

### 设置默认端口号

tomcat安装成功之后，默认监听的端口号时8080，如果你希望修改端口号，可以取修改

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

说明的一点：

每个协议是有一个默认端口号的，对于http协议来说，默认端口号是80端口号（默认端口号的含义是没有设置端口号，那么会采纳默认端口号）

http://www.cskaoyan.com





通过ip地址能够直接访问到你的资源文件，应该做哪些事情？





# Servlet

## 1 Servlet简介

Servlet是用来干什么的呢？

静态资源：一成不变的。没有什么交互性。早期互联网主要的内容

动态资源：富有交互性。始终是变化的。我们登录同一个页面，显示的内容是完全不相同的。京东的个人主页地址

Java语言里面有一个技术Servlet。可以开发动态web资源。

Servlet = Server + applet.**运行再服务器里面的一个小程序**。

官方文档

http://tomcat.apache.org/tomcat-8.5-doc/servletapi/

A servlet is a small Java program that runs within a Web server.

## 2 手动编写Servlet

```java
import javax.servlet.*;


public class FirstServlet extends GenericServlet{

	public void service(ServletRequest req,
           ServletResponse res)
                      throws ServletException,
                             java.io.IOException{
            System.out.println("hello servlet");

    }
}
```

**使用javac指令进行编译**，发现编译报错

![image-20210506112542787](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210506112542787-1620397058772.png)

这个问题的原因？

类加载：

类首先是存在于本地硬盘上面的，运行的时候，类需要再内存中，这一个步骤加载类，由类加载器来完成的。

负责将本地硬盘上面的类加载到内存中。

类加载器一共可以分为三类：

BootStrap：负责加载JDK里面的核心类库

Extension：负责取加载JDK/jre/ext目录下的类库

System：负责通过外部指令的方式加载类库，比如可以通过-classpath的方式加载某个外部第三方类库



如果你希望再编译的时候能够编译通过，那么你用到的类库至少需要再内存中。

javac -classpath D:\apache-tomcat-8.5.37\lib\servlet-api.jar FirstServlet.java

编译通过，形成class文件



**使用java指令进行运行**：

![image-20210506113351621](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210506113351621-1620397058772.png)

用java指令可以运行的前提是需要由main方法



需要将写好的servlet部署再tomcat里面

如何运行servlet呢？

/FirstServlet.class方式发现无法运行的，文件被下载到本地浏览器了。

1.这个行为不是运行，而是下载

2.服务器上面的源代码能够被浏览器直接下载，很危险



针对上述做一个措施，tomcat给我们提供了一个WEB-INF目录，凡是任何不希望被浏览器能偶直接访问的文件，都放置于该目录下。



针对问题1

你今后去了公司，发现公司要求你去应酬。陪老板去见客户。

今天晚上你只要看到我摸鼻子，你就给客户敬酒。



为了让servlet能够运行，也做了这么样的一个暗号，比如设置一个路径  /servlet1，只要客户访问/servlet1，那么就意味着要运行当前servlet。

/servlet1------------------FirstServlet



配置这个暗号

```xml
<servlet>
    <servlet-name>first</servlet-name>
    <servlet-class>FirstServlet</servlet-class>
  </servlet>
  

  <!-- Define the Manager Servlet Mapping -->
  <servlet-mapping>
    <servlet-name>first</servlet-name>
      <url-pattern>/servlet1</url-pattern>
  </servlet-mapping>
```

接下来还需要做一个事情

**class文件必须得放置再WEB-INF/classes目录下，这也是一个规定。**





访问http://localhost:8080   它的应用是ROOT，究竟访问的是ROOT应用里面的哪个文件呢？tomcat的web.xml中有一个配置welcome-file

tomcat在访问某个应用时，如果没有指定究竟访问的是哪个页面， 那么会去加载那三个welcome-file



## 3 Servlet的执行流程

以访问http://localhost:8080/app/servlet1

1.浏览器生成HTTP请求报文，请求报文传输给服务器

2.被监听着8080端口号的Connector接收到，将请求报文进行解析，封装成Request对象，同时还会提供一个Response对象

3.Connector将这两个对象传给Engine，Engine进一步将其传给Host

4.Host来挑选一个合适的Context应用，（会去找一个叫做app的应用），然后将请求进行进一步下发

**5.app这个应用，根据web.xml配置的信息（/servlet1-----------FirstServlet）,发现有一个FirstServlet，通过反射实例化一个对象出来，紧接着执行该对象的service(request,response)**

注册驱动

Class.forName(com.msql.jdbc.Driver);

6.service方法执行完毕，Connector会读取response里面的数据，然后生成HTTP响应报文

7.HTTP响应报文再次传输给客户端，**客户端会取出响应体的部分，加以渲染，呈现在页面的主体部分**

## 4 借助IDEA开发Servlet

![image-20210507095424765](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210507095424765-1620397058773.png)

表示的是启动成功之后，会用默认的浏览器打开一个页面，页面的URL（如果今后大家在启动tomcat的过程中，启动结束没有打开页面，其实是启动失败了）



## 5 开发Servlet三种方式

**1.继承GenericServlet**



**2.继承HttpServlet**

A subclass of `HttpServlet` must override at least one method, usually one of these:

- `doGet`, if the servlet supports HTTP GET requests
- `doPost`, for HTTP POST requests
- `doPut`, for HTTP PUT requests
- `doDelete`, for HTTP DELETE requests
- `init` and `destroy`, to manage resources that are held for the life of the servlet
- `getServletInfo`, which the servlet uses to provide information about itself

编写一个表单页面，然后通过访问当前表单页面进一步去访问servlet

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <form action="http://localhost:8080/app/servlet2" method="post">
        <input type="text" name="username"><br>
        <input type="submit">
    </form>
</body>
</html>
```

```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class SecondServlet extends HttpServlet {

    //如果你是以get方法请求当前servlet，那么会调用doGet方法
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //super.doGet(req, resp);
        System.out.println("do get");
    }

    //如果你使用post方法请求当前servlet，那么会调用doPost方法
    //如何使用post方法访问当前servlet？
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //super.doPost(req, resp);
        System.out.println("do post");
    }
}
```

**问题1：不是说所有的servlet都有service方法吗？为什么继承HttpServlet没有呢？**

因为父类HttpServlet已经实现了该方法。

**问题2：代码究竟是如何执行到doGet或者doPost方法里面的呢？**

通过之前的描述，我们清楚，**servlet的程序执行入口是service方法**。

```java
public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
    HttpServletRequest request;
    HttpServletResponse response;
    try {
        request = (HttpServletRequest)req;
        response = (HttpServletResponse)res;
    } catch (ClassCastException var6) {
        throw new ServletException("non-HTTP request or response");
    }

    this.service(request, response);
}
```

这里面的逻辑其实就是根据请求方法的不同，然后将请求进行进行进一步细致的分发，如果是get请求，那么就分发到doGet方法中，如果是post请求，那么就到doPost中。



doGet和doPost你就可以把它当做service方法来看待

**如果你继承GenericServlet，那么程序执行入口是service方法**

**如果你继承HttpServlet，那么程序执行入口是doGet或者doPost方法**

```java
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    String method = req.getMethod();
    long lastModified;
    if (method.equals("GET")) {
        lastModified = this.getLastModified(req);
        if (lastModified == -1L) {
            this.doGet(req, resp);
        } else {
            long ifModifiedSince;
            try {
                ifModifiedSince = req.getDateHeader("If-Modified-Since");
            } catch (IllegalArgumentException var9) {
                ifModifiedSince = -1L;
            }

            if (ifModifiedSince < lastModified / 1000L * 1000L) {
                this.maybeSetLastModified(resp, lastModified);
                this.doGet(req, resp);
            } else {
                resp.setStatus(304);
            }
        }
    } else if (method.equals("HEAD")) {
        lastModified = this.getLastModified(req);
        this.maybeSetLastModified(resp, lastModified);
        this.doHead(req, resp);
    } else if (method.equals("POST")) {
        this.doPost(req, resp);
    } else if (method.equals("PUT")) {
        this.doPut(req, resp);
    } else if (method.equals("DELETE")) {
        this.doDelete(req, resp);
    } else if (method.equals("OPTIONS")) {
        this.doOptions(req, resp);
    } else if (method.equals("TRACE")) {
        this.doTrace(req, resp);
    } else {
        String errMsg = lStrings.getString("http.method_not_implemented");
        Object[] errArgs = new Object[]{method};
        errMsg = MessageFormat.format(errMsg, errArgs);
        resp.sendError(501, errMsg);
    }

}
```

**问题3：为什么要根据请求方法的不同，做不同的分发？**

根据方法不同进行进一步分发，可以做更加细致的一些工作。

比如说，你设置了登录仅允许使用post请求方式，只在doPost里面写入逻辑，doGet里面不写逻辑



**3.针对暗号的配置，还有另外一种方式**

我们之前一直都是使用web.xml来进行配置，其实还可以通过注解的方式来配置

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
//@WebServlet(name = "third", urlPatterns = "/servlet3")
//该注解可以进一步优化
//@WebServlet(urlPatterns = "/servlet3")
//还可以进一步优化，如果注解里面只有一个值，默认表示的就是urlPattern
//其中注解里面的value和urlPatterns是等价的，你可以认为是两个小名  小明  张大明
@WebServlet("/servlet3")
public class ThirdServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```



## 6 IDEA和tomcat整合

 **CATALINA_BASE**:         C:\Users\song\.IntelliJIdea2019.3\system\tomcat\Tomcat_8_5_37_servlet_2

IDEA会复制tomcat的配置文件，利用这些配置文件，重新开一个新的tomcat，利用这个新的tomcat来部署资源（了解）

`<Context path="/app" docBase="D:\ideaProjects\30th\servlet\out\artifacts\servlet_war_exploded" />`

![image-20210507110045472](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210507110045472-1620397058777.png)

实际上开发目录其实主要是为了我们开发者开发方便，最终这些文件会按照一个规则，全部复制到最终的部署目录中。

**1.src目录下面的这些源代码文件，首先会经过编译，编译之后的class文件复制到WEB-INF/classes目录下**

**2.开发目录中web蓝点目录，里面的所有文件都会直接复制到部署目录中**



## 7 IDEA Facets、artifacts

![image-20210507111729231](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210507111729231-1620397058777.png)



## 8 将SE项目改造成EE项目



## 9 Servlet生命周期

Servlet从出生到死亡整个过程

在servlet被创建的时候，会调用init方法

servlet发挥作用、功能，每次请求该servlet，都会调用service方法

servlet结束，要被销毁，会调用destroy方法来销毁servlet。



servlet的三个生命周期函数会在指定的阶段会被调用

如果你希望在指定的阶段去做一些事情，那么就可以在对应的生命周期函数中写入对应的逻辑

比如你希望在servlet被创建的时候去做一个初始化，读取配置文件，那么该代码可以写在init方法里面

比如你希望在servlet被销毁的时候，统计当前servlet处理的请求的个数，那么代码可以写在destroy方法里面



```java
import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import java.io.IOException;
@WebServlet("/life")
public class LifeCycleServlet extends GenericServlet {

    //当servlet被创建的时候，会被调用
    //说明当前servlet只会被创建一个 其实首先判断当前应用是否有当前servlet的示例对象，如果有则直接使用；如果没有，则创建一个对象
    //其实多次请求访问同一个servlet，处理的是同一个对象，那有没有线程安全问题呢？
    //但是如果你不写成员变量，其实问题不大
    @Override
    public void init() throws ServletException {
        System.out.println("init");
        //读取一个配置文件
    }

    //每次访问servlet，都会调用该方法
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("service");
    }

    //servlet被销毁的时候，会调用 立遗嘱
    @Override
    public void destroy() {
        System.out.println("destroy");
        //将一些数据进行持久化保存到本地硬盘上面
    }
}
```



其中关于servlet的生命周期init阶段，还有一点需要额外补充的，默认情况下是在第一次访问之前才会调用

但是可以通过设置一个参数load-on-startup非负数，可以让servlet在应用启动的时候就调用（时机提前了一些）

提前有什么意义呢？ 更具有容错性。

比如我希望在某个servlet做一些初始化的工作，查询数据库拿到数据，数据再共享给其他组件资源来使用

如果init第一次访问才执行，那么只有当该servlet  A被访问，那么其他的组件才可以拿到这个数据



init{

​	queryDataBase;

​     String username = xxxx;

​	//希望将这个username和其他资源进行共享

​	**share(username);**

**共享给另外一个servlet**

}



假设另外一个servlet在当前servlet  A被访问之前就需要这个数据，怎么办？

可以设置init 随着应用启动而执行，应用启动，servlet A的init方法就会执行，共享该数据，

即便后面ServletA没有被访问，那么另一个servlet也可以拿到数据。

## 10 Url-pattern注意事项

url-pattern合法的书写规则：

1. 必须是 /xxxx  ，**反面教材（xxxxx，比如servlet1）**  

   **Caused by: java.lang.IllegalArgumentException: Invalid` <url-pattern>` [url] in servlet mapping**

2. *.后缀，比如      ***.html**



**一个servlet可以设置多个url-pattern吗？**

可以设置多个

多个servlet可以映射到同一个url-pattern吗？

**IllegalArgumentException: The servlets named [com.cskaoyan.servlet.ThirdServlet] and [com.cskaoyan.servlet.url.UlrServlet] are both mapped to the url-pattern [/servlet3] which is not permitted**

强烈建议大家自己主动去复现这个错误，然后去尝试找一下。

## 11 Url-pattern优先级

1. **/xxx开头的优先级要高于`*`.后缀**
2. **如果都是/开头的，那么匹配程度越高，越优先执行**



## 12 缺省Servlet

首先，在应用中设置两个url-pattern，一个是 `/*`   一个是`/ `

紧接着，访问jsp页面以及访问html页面，观察结果

此时不管访问jsp页面还是访问html页面，均显示的是`/*`的内容



如果把`/*`注释了，仅保留`/`

jsp页面可以正常显示了，但是html页面仍然无法显示，此时页面仍然显示的是/的内容



实际上访问jsp时，访问的是一个servlet。



tomcat的conf/web.xml中有如下配置信息（相当于我们编写的所有的应用都可以使用该配置文件里面的配置），可以简单的理解为我们所有的应用都可以通过继承得到该配置信息，虽然我们的应用中并没有去做这个配置，但是这个配置信息在我们应用中是有的。

```xml
<servlet>
        <servlet-name>jsp</servlet-name>
        <servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
        <init-param>
            <param-name>fork</param-name>
            <param-value>false</param-value>
        </init-param>
        <init-param>
            <param-name>xpoweredBy</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>3</load-on-startup>
    </servlet>


 <servlet-mapping>
        <servlet-name>jsp</servlet-name>
        <url-pattern>*.jsp</url-pattern>
        <url-pattern>*.jspx</url-pattern>
    </servlet-mapping>
```

**从配置信息你可以得出什么样的结论？其实你在访问jsp时，实际上访问的是servlet。**该servlet会做一些逻辑处理，然后将jsp页面里面的内容显示出来。

为什么设置了`/*`之后，显示不了了呢？

因为`/*`的优先级要高于  `*.jsp`

当我们设置`/*`时，`/*` 的优先级要高，所以此时访问jsp时，jsp的servlet优先级没有前者高，最终显示的是`/*`的内容，但是`/`*没有实现什么逻辑，所以最终显示的就是一个`/`*输出；



把`/*`注释了之后，再次访问jsp时，因为`/*`已经没有了，所以此时`*.jsp`就可以发挥功能了，那么它的逻辑就可以被执行到，可以将页面显示出来的

把`/*`注释了之后，再次访问html页面，发现此时页面打印的是`/`，html页面仍然无法正常显示，为什么呢？



实际上，当我们在访问静态资源时，是有servlet在参与的，这个servlet主要的职责就是在指定的位置去寻找对应的文件，如果找到，则将该文件额流写出去，如果找不到，则将返回404状态码。这个servlet其实就是缺省Servlet，特征是url-pattern 为/

tomcat其实是有给我们提供一个缺省servlet的，但是如果你在应用中重新写了一个新的缺省servlet，那么tomcat就会认为你有了一个更好的实现，系统自带的就不会再给你使用了。



![image-20210507151332029](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210507151332029.png)

最终的结论：

1. **`/*`因为优先级比较高，所以在应用中，我们应当谨慎的去设置**
2. **关于/，缺省servlet，它是专门用来处理那些没有任何servlet可以处理的请求，如果你的应用中重写了/，那么tomcat自带的就不会再给你是使用了，此时如果希望能够正常发挥功能，那么你必须自己去实现缺省servlet的功能。**

```xml
<!-- The default servlet for all web applications, that serves static     -->
<!-- resources.  It processes all requests that are not mapped to other   -->
<!-- servlets with servlet mappings (defined either here or in your own   -->
```


 一般情况下，静态资源文件html等，一般是不会设置servlet的



假设在你的应用中，你设置了 一个servlet的url-pattern为 `*.html`，那么当访问html页面时，还会不会交给缺省servlet

此时不会。



## 13 请求执行流程

以访问http://localhost:8080/app/form.html为例

1.浏览器生成HTTP请求报文，然后发送给服务器

2.被监听8080端口号的Connector接收到，将其解析成Request对象，同时还有一个response

3.Connector将这两个对象传给Engine------Host，Host来挑选一个叫做app的Context应用

4.如果找到，这两个对象会交给该应用，首先先判断是否有相应的servlet url-pattern可以处理该请求

**5.最终不管有没有servlet可以处理该请求，那么最终tomcat都会给你安排一个servlet来处理你的请求（如果没有就是缺省servlet），**

**首次访问的时候进行初始化，调用该servlet的service方法执行**

6.执行的过程中response里面填充了数据，Connector读取response里面的数据，然后生成响应报文



## 14 利用ServletConfig获取Servlet初始化参数

了解的部分。

```xml
<servlet>
    <servlet-name>jsp</servlet-name>
    <servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
    <init-param>
        <param-name>fork</param-name>
        <param-value>false</param-value>
    </init-param>
    <init-param>
        <param-name>xpoweredBy</param-name>
        <param-value>false</param-value>
    </init-param>
    <load-on-startup>3</load-on-startup>
</servlet>
```

当tomcat在启动的时候，会读取这些数据，然后将这些键值对信息封装到一个叫做ServletConfig的对象中。

你只需要去调用config的某个方法来获取键值对的值即可。

```xml
<servlet>
    <servlet-name>config</servlet-name>
    <servlet-class>com.cskaoyan.servlet.config.ConfigServlet</servlet-class>
    <init-param>
        <param-name>name</param-name>
        <param-value>jingtian</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>config</servlet-name>
    <url-pattern>/config1</url-pattern>
</servlet-mapping>
```

使用config对象去获取

```java
public class ConfigServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //首先拿到servletConfig对象
        ServletConfig servletConfig = getServletConfig();
        String name = servletConfig.getInitParameter("name");
        System.out.println(name);
        //config对象的方法即可
    }
}
```

简单了解即可。一般情况下我们不会自己去写。但是如果你看别人的代码，你知道是啥意思即可

## 15 Context域共享数据

如果一个servlet在运行时产生了某些数据，需要和另外的servlet进行数据的共享，那么我们可以使用ServletContext进行数据的共享



ServletContext对象在每一个**应用**中有且只有一个。



```java
@WebServlet("/domain1")
public class DomainServlet1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.先拿到servletContext对象
        ServletContext servletContext = getServletContext();
        //如何和其他servlet进行数据共享呢？
        servletContext.setAttribute("username", "jingtian");
    }
}
```

```java
@WebServlet("/domain2")
public class DomainServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.先拿到servletContext对象
        ServletContext servletContext = getServletContext();

        String username = (String) servletContext.getAttribute("username");
        System.out.println(username);
    }
}
```



使用场景：

当前网站想做一个统计历史访问次数的功能，统计多个页面的次数，数据应该共享

实现方式一：

完全使用数据库来实现，每次访问都到数据库里面进行数据的插入



实现方式二：

先使用servletContext，再应用被卸载或者说每隔24个小时，执行一个程序，将context里面的数据存储到数据库里面（减轻了数据库的压力）

```java
@WebServlet("/page1")
public class PageServlet1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ServletContext servletContext = getServletContext();
        synchronized (servletContext){
            Integer history = (Integer) servletContext.getAttribute("history");
            if(history == null){
                history = 0;
            }
            history ++;
            servletContext.setAttribute("history", history);
            response.getWriter().println("history count:" + history);
        }
    }
}
```

```java
@WebServlet("/page2")
public class PageServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ServletContext servletContext = getServletContext();
        synchronized (servletContext){
            Integer history = (Integer) servletContext.getAttribute("history");
            if(history == null){
                history = 0;
            }
            history ++;
            servletContext.setAttribute("history", history);
            response.getWriter().println("history count:" + history);
        }
    }
}
```

## 16 获取EE项目文件绝对路径

在应用的部署目录中有一个1.txt文件，希望能够获取到该文件的绝对路径、file、流

```java
servletContext.getRealPath
```

使用方式一：

```java
String realPath = servletContext.getRealPath("");
```

先输入一个空字符串，那么会拿到部署根目录，接下来你只需要将这个路径和你相对于部署根目录的相对路径拼在一起，就形成了最终的绝对路径

比如：部署根目录下， 1.txt文件

realPath + 1.txt 就是该文件的绝对路径

使用方式二：

只需要将相对于部署根目录的相对路径传入进去作为参数，那么就会帮你拼上前面的docBase形成绝对路径

```java
String path1 = servletContext.getRealPath("1.txt");
```



假设WEB-INF目录下有一个文件，使用这种方式能不能取到？



```java
@WebServlet("/path")
public class PathServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //为什么获取的路径获取到的是tomcat的bin目录？
        //相对的是user.dir 用户的工作目录 main方法所在的目录 哪个目录调用了jvm
        //EE特点：程序全部都是没有main方法的？如何执行的呢？
        //全凭服务器tomcat来调用，tomcat使用java语言编写的 main
        File file = new File("1.txt");
        System.out.println(file.getAbsolutePath());
        // 如何能够拿到部署目录下的文件的路径？
        // 应用两个属性  path 应用名  docBase 部署根目录
        //所以tomcat应该能够帮我们提供这个路径
        ServletContext servletContext = getServletContext();
        //有一个方法，你可以认为就是对docBase的封装
        String realPath = servletContext.getRealPath("");
        System.out.println(realPath);
        File f = new File(realPath + "1.txt");
        System.out.println(f.exists());

        String path1 = servletContext.getRealPath("1.txt");
        boolean exists = new File(path1).exists();
        System.out.println(exists);
        //WEB-INF目录只是用来屏蔽浏览器的直接访问，对于服务器来说，就是一个普普通通的目录
        //可以避免外敌的入侵，但是无法避免内鬼的背叛
        String realPath1 = servletContext.getRealPath("WEB-INF/2.txt");
        boolean exists1 = new File(realPath1).exists();
        System.out.println(exists1);
    }
}
```





# IDEA创建Java Web项目

## SE项目改为Web项目

- Project Structure -> Facets -> + web

- Project Structure -> Artifacts -> + Web Application: Exploded -> From Modules
- Project Structure -> Modules -> Dependencies -> + Lrbrary -> Tomcat
- Edit Configurations -> + Tomcat -> Local -> Deployment -> + Artifact





## 直接创建

- New Project 

  -> Java Enterprise (add server and √ Web Application √ web.xml) 

  -> next...



## 导入Web项目

- Import Project 

  -> Create project from existing source 

  -> naming and next

   -> check source files and next 

  -> review libraries and next 

  -> review module structure and next 

  -> select project SDK and next 

  -> review frameworks and finish

- Project Structure 

  -> set Factes & Artifacts & Module Dependencies

- Edit Configurations for Tomcat to Deploy Artifact

