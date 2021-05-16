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

```xml
<!--在IDEA的CATALINA的localhost文件夹下生成的xml虚拟映射文件-->
<Context path="/app" docBase="D:\ideaProjects\30th\servlet\out\artifacts\servlet_war_exploded" />
```

![image-20210507110045472](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210507110045472-1620397058777.png)

实际上开发目录其实主要是为了我们开发者开发方便，最终这些文件会按照一个规则，全部复制到最终的部署目录中。

**1.src目录下面的这些源代码文件，首先会经过编译，编译之后的class文件复制到WEB-INF/classes目录下**

**2.开发目录中web蓝点目录，里面的所有文件都会直接复制到部署目录中**



## 7 IDEA Facets、artifacts

![image-20210507111729231](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210507111729231-1620397058777.png)



## 8 将SE项目改造成EE项目



## 9 Servlet生命周期

Servlet是一个供其他Java程序（Servlet引擎）调用的Java类，它不能独立运行，它的运行完全由Servlet引擎来控制和调度。

针对客户端的多次Servlet请求，通常情况下，服务器只会创建一个Servlet实例对象，也就是说Servlet实例对象一旦创建，它就会驻留在内存中，为后续的其它请求服务，直至web容器退出(或应用停止)，servlet实例对象才会销毁。

在Servlet的整个生命周期内，Servlet的init方法只被调用一次。而对一个Servlet的每次访问请求都导致Servlet引擎调用一次servlet的service方法。对于每次访问请求，Servlet引擎都会创建一个新的HttpServletRequest请求对象和一个新的HttpServletResponse响应对象，然后将这两个对象作为参数传递给它调用的Servlet的service()方法，service方法再根据请求方式分别调用doXXX方法。 



Servlet从出生到死亡整个过程

在servlet被创建的时候，会调用init方法

servlet发挥作用、功能，每次请求该servlet，都会调用service方法

servlet结束，要被销毁，会调用destroy方法来销毁servlet。



servlet的三个生命周期函数会在指定的阶段会被调用

如果你希望在指定的阶段去做一些事情，那么就可以在对应的生命周期函数中写入对应的逻辑

比如你希望在servlet被创建的时候去做一个初始化，读取配置文件，那么该代码可以写在init方法里面

比如你希望在servlet被销毁的时候，统计当前servlet处理的请求的个数，那么代码可以写在destroy方法里面



```java
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



如果在`<servlet>`元素中配置了一个`<load-on-startup>`元素，那么WEB应用程序在启动时，就会装载并创建Servlet的实例对象、以及调用Servlet实例对象的init()方法。
	举例：

```xml
<servlet>
    <servlet-name>invoker</servlet-name>
    <servlet-class>
        org.apache.catalina.servlets.InvokerServlet
    </servlet-class>
    <load-on-startup>2</load-on-startup>
</servlet>
```

用途：为web应用写一个InitServlet，这个servlet配置为启动时装载，为整个web应用创建必要的数据库表和数据。

> 从提高Servlet容器运行性能的角度出发，Servlet规范为Servlet规定了不同的初始化情形。如果有些Servlet专门负责在web应用启动阶段为web应用完成一些初始化操作，则可以让它们在web应用启动时就被初始化。对于大多数Servlet，只需当客户端首次请求访问时才被初始化。假设所有的Servlet都在web应用启动时被初始化，那么会大大增加Servlet容器启动web应用的负担，而且Servlet容器有可能加载一些永远不会被客户访问的Servlet，白白浪费容器的资源。



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

在Servlet的配置文件中，可以使用一个或多个<init-param>标签为 某个servlet配置一些初始化参数。

例如：

获得字符集编码
获得数据库连接信息



当servlet配置了初始化参数后，web容器在创建servlet实例对象时，会自动将这些初始化参数封装到ServletConfig对象中，并在调用servlet的init方法时，将ServletConfig对象传递给servlet。进而，程序员通过ServletConfig对象就可以得到当前servlet的初始化参数信息。

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
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //首先拿到servletConfig对象
        ServletConfig servletConfig = getServletConfig();
        String name = servletConfig.getInitParameter("name");
        System.out.println(name);
        //config对象的方法即可

//可以直接String name = getInitParameter("name");获得init中的参数
    }
}
```

简单了解即可。一般情况下我们不会自己去写。

## 15 Context域共享数据

WEB容器在启动时，它会为每个WEB应用程序都创建一个对应的ServletContext对象，它代表当前web应用。

ServletConfig对象中维护了ServletContext对象的引用，开发人员在编写servlet时，可以通过ServletConfig.getServletContext方法获得ServletContext对象。

由于一个WEB应用中的所有Servlet共享同一个ServletContext对象，因此Servlet对象之间可以通过ServletContext对象来实现通讯。ServletContext对象通常也被称之为context域对象。

```xml
<context-param>
    <param-name>encode</param-name>
    <param-value>GBK</param-value>
</context-param>
```

ServletContext应用

获取WEB应用的初始化参数。(多个serlvet获取相同参数)
多个Servlet通过ServletContext对象实现数据共享。
实现Servlet的转发（request）。

...

```java
统计特定网页被客户端访问的次数
public class Counter {
	private int count;
	public Counter(int count){
		this.count = count;
	}
	public Counter(){
		this(0);
	}
	public int getCount() {
		return count;
	}
	public void setCount(int count) {
		this.count = count;
	}
	public void add(int step){
		count+=step;
	}
}
----------------------------------------------
public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		ServletContext context = getServletContext();
		Counter counter = (Counter) context.getAttribute("counter");
		if(counter==null){
			counter = new Counter(1);
			context.setAttribute("counter", counter);
		}
 
		response.getOutputStream().write(("count:"+counter.getCount()).getBytes());
		counter.add(1);
	}

```

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



资源文件的路径获取或者流获取

```
资源文件通常有两种方式：
对于简单的资源文件，即包含key=value的形式，我们一般采用properties，这些文件的扩展名一般为*.properties。
对于较复杂的资源文件，采用XML格式。
通常资源文件放在src目录或者WEB-INF目录下。
在web工程中，要获得某个文件的路径，我们一般都采取相对于web工程“/”的相对路径。
如果在src下放置properites配置文件，要读取的话：

一种方式是采用ServeltContext的方法来获取某个文件的绝对路径。
Context.getRealpath("") 是应用目录
context.getResource("") 是应用目录
以上可以直接传入文件相对应用目录的相对地址即可，如：
context.getResourceAsStream("/WEB-INF/classes/db.properties")可读取类加载路径的文件，参数开头的/加和不加经测试都一样，应该是方法内部做了处理，去掉了拼接时重复的/

1、加载src下的属性资源文件
InputStream in= ServletDemo.getClass().getClassLoader().getResourceAsStream("db.properties");
2、加载与servlet同包中的资源属性文件
InputStream in=ServletDemo.getClass().getClassLoader().getResourceAsStream("com/baidu/db.properties");
1和2中的引号内传入的路径参数开始也都可以加/或者不加，实测都一样，方法内部应该也是在拼接路径时做了去重的处理。
```



# IDEA创建Java Web项目

## SE项目改为Web项目

- Project Structure -> Facets -> + web

- Project Structure -> Artifacts -> + Web Application: Exploded -> From Modules
- Project Structure -> Modules -> Dependencies -> + Library -> Tomcat
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



## 导入jar包

首先项目里有个lib目录已经add to Library了

- Facets方面

  把这个lib目录拖到WEB-INF中即可

- Artifacts方面

  Project Structure -> Artifacts -> Output Layout -> WEB-INF -> new directory lib -> right click to add copy of library

  



# ServletRequest

ServeletRequest其实就是对于请求报文的封装。

请求报文如果使用HTTP协议，那么就是HTTP协议的请求报文

如果你使用的是AJP协议，那么就是AJP协议的请求报文  （协议其实就是指报文具有的格式）



HttpServletRequest其实就是对于HTTP请求报文的封装。



一般情况下，99%的情况下发送的请求都是HTTP请求，所以呢，你可以认为ServletRequest就等价于HttpServletRequest



其实就是对于请求报文的封装，那么肯定可以获取请求报文的各个部分。



request常用方法

获得客户机信息
getRequestURL方法返回客户端发出请求时的完整URL。
getRequestURI方法返回请求行中的资源名部分。
//getQueryString 方法返回请求行中的参数部分。
getRemoteAddr方法返回发出请求的客户机的IP地址
getRemoteHost方法返回发出请求的客户机的完整主机名
getRemotePort方法返回客户机所使用的网络端口号
getLocalAddr方法返回WEB服务器的IP地址。
getLocalName方法返回WEB服务器的主机名
getMethod得到客户机请求方式



获得客户机请求头
getHeader(name)方法 
getHeaders(String name)方法 
getHeaderNames方法 



获得客户机请求参数(客户端提交的数据)
getParameter(name)方法
getParameterValues（String name）方法
getParameterNames方法 



```
Accept-Encoding   PropertyDescriptor

有许多web浏览器不发送带有“content-type”头信息的字符编码限定符，而由读取http请求的代码来决定字符的编码方式。

默认情况下，如果客户端请求未定义编码限定符，容器（如tomcat）会用“ISO-8859-1”去创建request reader和解析POST的数据
注意：自从Tomcat5.x开始，GET和POST方法提交的信息，tomcat采用了不同的方式来处理编码，对于POST请求，Tomcat会仍然使用request.setCharacterEncoding方法所设置的编码来处理，如果未设置，则使用默认的iso-8859-1编码。
而GET请求则不同，Tomcat对于GET请求并不会考虑使用request.setCharacterEncoding方法设置的编码，而会永远使用iso-8859-1编码

 <form action="" method="get">
    username:<input type="text" name="username"/><br/>
    passowrd1:<input type="text" name="password"/><br/>
    password2:<input type="text" name="password"/><br/>
    <input type="submit" value="提交"/>
    </form>
    
<!-- checkbox radio 只要一个都不选，浏览器根本不会传值给服务器,即报文中不会出现他们的name字段-->

```



## 获取请求报文

```java
@WebServlet("/request1")
public class RequestServlet1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取请求报文的各个部分
        String method = request.getMethod();
        //requestURI和requestURL地区别在于后者比前者多了访问协议、主机、端口号等
        String requestURI = request.getRequestURI();
        String requestURL = request.getRequestURL().toString();
        String protocol = request.getProtocol();
        System.out.println(method + " " + requestURI + " " + protocol);
        System.out.println(method + " " + requestURL + " " + protocol);
        //如何获取请求头呢？
        Enumeration<String> headerNames = request.getHeaderNames();
        while (headerNames.hasMoreElements()){
            String headerName = headerNames.nextElement();
            String headerValue = request.getHeader(headerName);
            System.out.println(headerName + ":" + headerValue);
        }
        //请求体暂时不处理
    }
}
```



## 获取客户机和主机的信息

```java
@WebServlet("/request2")
public class RequestServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       //获取主机的信息
        //课程的最后会做一个黑名单，指定的ip地址拦截 小黑屋 账号
        String localAddr = request.getLocalAddr();
        int localPort = request.getLocalPort();

        //获取客户机信息
        String remoteAddr = request.getRemoteAddr();
        int remotePort = request.getRemotePort();
        System.out.println("客户机：" + remoteAddr + "使用端口号：" + remotePort + "访问了主机" + localAddr + ":" + localPort);
    }
}
```



## 获取请求参数

场景：

比如前端页面登录、注册、提交了很多的表单数据，**首先需要将这些数据获取到**，保存到数据库

POST http://localhost:8080/app/register HTTP/1.1
Host: localhost:8080
Connection: keep-alive
Content-Length: 63
Cache-Control: max-age=0
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="90", "Microsoft Edge";v="90"
sec-ch-ua-mobile: ?0
Upgrade-Insecure-Requests: 1
Origin: http://192.168.7.114:8080
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36 Edg/90.0.818.51
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://192.168.7.114:8080/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6

**username=admin&password=aaa&gender=male&hobby=study&hobby=sleep**

提交的参数以key=value&key=value的形式进行提交

可以自己去拿到请求体，自己去解析，但是非常繁琐，没必要

**request给我们已经封装好了，我们其实只需要去调用对应的方法即可完成获取数据的工作**

**value = request.getParameter(key);**

```java
@WebServlet("/register")
public class RegisterServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取请求参数
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        String gender = request.getParameter("gender");
        //String hobby = request.getParameter("hobby");
        //如果需要获取多个值，那么需要使用另外一个方法
        String[] hobbies = request.getParameterValues("hobby");
        System.out.println(username);
        System.out.println(password);
        System.out.println(gender);
        //System.out.println(hobby);
        System.out.println(Arrays.toString(hobbies));
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```



## 获取请求参数2

如果前端页面提交的参数非常多，一个一个获取非常麻烦，有没有简便的方法

```java
@WebServlet("/register2")
public class RegisterServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取请求参数
        Enumeration<String> parameterNames = request.getParameterNames();
        while (parameterNames.hasMoreElements()){
            String parameterKey = parameterNames.nextElement();
            String[] parameterValues = request.getParameterValues(parameterKey);
            if(parameterValues.length == 1){
                System.out.println(parameterKey + ":" + parameterValues[0]);
            }else {
                System.out.println(parameterKey + ":" + Arrays.toString(parameterValues));
            }
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```





## 封装请求参数到一个Java对象（Java Bean）

反射。

Class.forName.newInstance();----- 实例化一个对象

class--------所有的属性值、所有的方法



set方法将参数进行赋值呢？



比如页面提交的参数为username、password、gender、hobby

java对象里面的属性值为username、password、gender、hobby

首先先取出每个参数的键值对username=xxx

会到该对象中去寻找一个叫做setUsername的方法，将xxx值注入到该方法中，



先介绍一个工具类BeanUtils，第三方的jar包，肯定不在jdk中，导包

![image-20210508094513064](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210508094513064.png)

这个报错的原因，大家可以记住，有且只有一个原因

**运行时没有找到jar包。tomcat只会到一个地方去寻找jar包，如果找不到，则直接报错。会到WEB-INF/lib目录下寻找jar包，如果找到， 没问题；如果找不到，那么报错ClassNotFoundException**。



两方面去解决：

1

![image-20210508095015840](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210508095015840.png)

web蓝点目录下面的所有文件会原封不动地复制到部署根目录里面去；WEB-INF目录下的文件也会同步过去



2.

![image-20210508095809808](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210508095809808.png)



```java
@WebServlet("/register3")
public class RegisterServlet3 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       //使用Beanutils来封装数据到java对象中
        //使用方式：会将第二个参数map里面的参数迭代出来，然后封装到第一个对象中
        User user = new User();
        try {
            //它是如何做到的呢？其实也是利用反射，利用set方法来完成赋值
            //思路：map-----》  username---xxx   password----xxx
            //会到user中找一个setUsername的方法   setPassword
            //  beanutils  dbutils
            BeanUtils.populate(user, request.getParameterMap());
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        System.out.println(user);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```





另外我们可以参考BeanUtils封装数据的方式，自己手动尝试去写一个工具类



## 获取请求参数中文乱码

浏览器是什么编码就以什么编码传送数据 
解决：request.setCharacterEncoding(“UTF-8”);//POST有效

请求体里的中文有乱码问题，要在获取参数之前给request设置setCharacterEncoding

请求行里的中文不会乱码，不需要设置request的编码



当我们使用post请求方式提交表单数据时，发现中文的确是有乱码问题的。

User{usnername='??????', password='asdasd', gender='male', hobby=[study, sleep]}



```
void setCharacterEncoding(java.lang.String env)
                          throws java.io.UnsupportedEncodingException
```

Overrides the name of the character encoding **used in the body of this request.** **This method must be called prior to reading request parameters or reading input using getReader().**

- **Parameters:**

  `env` - a `String` containing the name of the character encoding.

- **Throws:**

  `java.io.UnsupportedEncodingException` - if this is not a valid encoding



使用注意事项：

**1.只针对请求体有效**

**2.必须要求在读取请求参数之前调用**



和锟斤拷烫烫烫



## 如何获取请求行中的参数

请求行中的参数依然可以使用getParamter来获取

这个API既可以获取请求行，又可以获取请求体里面的参数

但是

参数的格式必须要求是key=value型数据

key:value不可以的

不仅可以获取到请求行中的参数，并且没有中文乱码问题

```java
@WebServlet("/register4")
public class RegisterServlet4 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        User user = new User();
        try {
            ReflectionUtils.toBean(user, request.getParameterMap());
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
        System.out.println(user);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String username = request.getParameter("username");
        System.out.println(username);
    }
}
```



## URL含中文字符乱码

//URL的编解码方式是不同的，有中文的话需要单独处理

java.net.URLDecoder;

| static String | decode(String s, String enc)使用指定的编码机制对 application/x-www-form-urlencoded 字符串解码。 |
| ------------- | ------------------------------------------------------------ |

java.net.URLEncoder;

| static String | encode(String s, String enc)使用指定的编码机制将字符串转换为 application/x-www-form-urlencoded  格式。 |
| ------------- | ------------------------------------------------------------ |

```java
//这样处理过的URL解决了中文乱码问题
String requestURI = request.getRequestURI();
requestURI = URLDecoder.decode(requestURI, "UTF-8");
```



## 网络路径写法

**1.全路径**，比如http://localhost:8080/app/servlet1这种写法，就是全路径

如果直接写全路径，那么其实不是特别的推荐，因为主机端口号部分是可变化的部分，需要经常改动。



企业中的环境

软件公司

​	自研产品：公司有自己独立的产品线

​	外包：没有自己独立的产品线，外接别人的项目来赚钱。比如alibaba，huawei会招聘很多外包工作人员。外包人员其实也可以分为两种，一种就是驻场外包（你和阿里巴巴的工作人员一起工作，同工不同酬；学到很多东西；华为外包）；直接在外包公司里面工作



开发流程：

1.项目的立项：公司的很多部分一起商讨，软件具有什么样的功能，第一版上线什么功能

2.项目的开发：

​		项目组：组长  项目主管 负责整个项目的进度

​						产品经理   需求 文档

​						UI  用户最先看到的界面

​						前端开发人员   psd--------html、css、js等静态资源页面

​						服务端开发人员   java、go、node、python  前后端分离  数据库相关工作也需要服务器开发人员去完成

​						DBA 数据库管理员   数据库方面造诣很深

​						测试 提bug  找bug

​						运维人员  运维网站的运行

​						安全测试人员	安全性测试

​		

​					项目开发：一般情况下是在每个人的个人电脑上进行，环境 localhost

​					合并，成员的代码进行合并，测试（测试的目的是为了能够将正式上线之后的代码潜在bug找出来，测试的环境应当尽可能和正式环境保持一致，linux，测试环境不会在你个人电脑上面，环境localhost合适吗，主机端口号  域名需要全部更换一遍）

​					正式上线：会有一个专门的域名，这个电脑和测试的电脑也不是一个。环境 又要变一次

结论：

全路径可以写，但是对于可变的部分一定要求以配置文件的形式配置下来，可以直接从配置文件中去获取



**2.相对路径**

相对当前页面的一个相对路径

比如当前页面http://localhost:8080/app/form.html，提交到的路径http://localhost:8080/app/servlet1

相对路径应该怎么写呢？如何从当前路径出发到达目标路径

servlet1即可

但是相对路径会过分依赖于当前页面所在的路径，如果当前页面路径发生变化，那么最终提交的路径也会变化。



3.  **/应用名/资源路径**

比如http://localhost:8080/app/register4

/app/register4即可

比较推荐的



## 转发和包含

一个Servlet对象无法获得另一个Servelt对象的引用；如果需要多个Servet组件共同协作(数据传递)，只能使用Servelt规范为我们提供的两种方式：
请求转发：Servlet(源组件)先对客户请求做一些预处理操作，然后把请求转发给其他web组件(目标组件)来完成包括生成响应结果在内的后续操作。
包含：Servelt(源组件)把其他web组件(目标组件)生成的响应结果包含到自身的响应结果中。
转发和包含的共同点
源组件和目标组件处理的都是同一个客户请求，源组件和目标组件共享同一个ServeltRequest和ServletResponse对象
目标组件都可以为Servlet、JSP或HTML文档
都依赖 javax.servlet.RequestDispatcher接口



**RequestDispather**

表示请求分发器，它有两个方法：
forward():把请求转发给目标组件
public void forward(ServletRequest request,ServletResponse response)
             throws ServletException,java.io.IOException
include():包含目标组件的响应结果
public void include(ServletRequest request,ServletResponse response)
             throws ServletException,java.io.IOException
得到RequestDispatcher对象
1、ServletContext对象的getRequestDispather(String path1)
Path1 必须即以”/”开头，若用相对路径会抛出异常IllegalArgumentException

```
当前的servlet1 : @WebServlet("/test/testServlet1")
转发到servlet2 : @WebServlet("/testServlet2")
正确 : request.getRequestDispatcher("/testServlet2")
ServletContext对象获取的getRequestDispatcher方法，只能使用/开始的基于应用目录的绝对路径
```

```
HTTP Status 500 – Internal Server Error
Message Path [test/testServlet2] does not start with a "/" character

Exception
java.lang.IllegalArgumentException: Path [test/testServlet2] does not start with a "/" character
```

2、ServletRequest对象的getRequestDispatcher(String path2)
path2可以用绝对路径也可以用相对路径

- 相对路径->是相对当前servlet的路径

  ```
  当前的servlet1 : @WebServlet("/test/testServlet1")
  转发到servlet2 : @WebServlet("/testServlet2")
  错误 : request.getRequestDispatcher("testServlet2")
  正确 : request.getRequestDispatcher("../testServlet2")
  ```

- 绝对路径

  ```
  当前的servlet1 : @WebServlet("/test/testServlet1")
  转发到servlet2 : @WebServlet("/testServlet2")
  正确 : request.getRequestDispatcher("/testServlet2")
  ```




```
1.请求数据
2.控制台与网页显示数据
3.Include包含网页时，设置头信息无效
4.request.getContextPath();
5.重定向原理及实现
```





## 转发

dispatcher.forward(request,response)的处理流程：
1、清空用于存放响应正文数据的缓冲区
2、如果目标组件为Servlet或JSP，tomcat就调用它们，把它们产生的响应结果发送到客户端；如果目标组件为文件系统中的静态HTML文档，tomcat就读取文档中的数据并把它发送给客户端。
特点：
1、由于forward()方法先清空用于存放响应正文数据的缓冲区，因此源组件生成的响应结果（无论转发前后）不会被发送到客户端，只有目标组件生成的响应结果才会被送到客户端。
2、如果源组件在进行请求转发之前，已经提交了响应结果（如调用了response的flush或close方法），那么forward（）方法会抛出IllegalStateException。为了避免该异常，不应该在源组件中提交响应结果。

```java
public class CheckServlet extends HttpServlet {

    public void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
        String username = request.getParameter("username");
        String message = null;
        if(username==null){
            message = "Please input username!";
        }else{
            message = "Hello,"+username;
        }
        request.setAttribute("msg",message);

        RequestDispatcher dispatcher = getServletContext().getRequestDispatcher("/upload.html");
        PrintWriter out = response.getWriter();
        out.println("Output from CheckServlet before forwarding request.1");
        System.out.println("Output form CheckServlet before forwarding request.2");
        dispatcher.forward(request, response);
        out.println("Output from CheckServlet after forwarding request.1");
        System.out.println("Output form CheckServlet after forwarding request.2");

    }
    -------------------------------------------------------------------------------
        public class OutputServlet extends HttpServlet {

            public void doGet(HttpServletRequest request, HttpServletResponse response)
                throws ServletException, IOException {
                String message = (String) request.getAttribute("msg");
                response.getWriter().println(message);
            }
```



要比了解要稍微多一些。

不到掌握的程度。

转发：一个servlet处理完逻辑之后，转发交给另外一个servlet或者页面

一个servlet处理完，跳转至另外一个页面，那其实就是页面的跳转，涉及到页面跳转，这部分不是重点

vue、java  前后端分离的架构

页面的跳转全部都是由前端来实现

服务器主要负责给前端页面提供数据即可

关于这部分转发的内容，今后工作中绝对不会涉及到，但是在学习EE知识过程中，我们需要用到该技术进行页面的跳转



应用场景：

一个servlet处理完逻辑之后，将请求进一步交给另外页面来处理，比如登录注册成功之后，跳转至一个页面



```java
@WebServlet("/register5")
public class RegisterServlet5 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //比如这里去写获取请求参数的逻辑，然后将这些请求参数进行保存
        //保存的方式可以为保存到文本文件中、数据库中
        //需要跳转到一个页面
        //需要传入一个地址（需要转向的页面所在的路径，网络路径）
        //全路径写法不可以，因为会自动前应用名补在前面  /app/http://localhost:8080/app/success.html
        // 相对路径写法  OK 相对于当前servlet的url
        //  /开头的路径   /资源路径即可   应用名直接忽略即可
        request.getRequestDispatcher("/success.html").forward(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

其中**关于/开头的路径**，之前说到要加应用名

但是在本案例中，缺又不加，如何记忆呢？

什么情况下是/应用名/资源路径，什么情况下是/资源路径呢？

路径的执行主体是浏览器，那么就需要加/应用名/资源路径，form的action、a标签的href、img的src等等

路径的执行主体是服务器，那么就直接/资源路径即可，**转发**



注意： dopost请求不能转发给doget， 不同的请求方法之间不能转发

```
报错：Message HTTP method POST is not supported by this URL
```



## 包含

include()方法的处理流程：
1、如果目标组件为Servlet或JSP，就执行它们，并把它们产生的响应正文添加到源组件的响应结果中；如果目标组件为HTML文档，就直接把文档的内容添加到源组件的响应结果中。
特点：
1、源组件与被包含的目标组件的输出数据都会被添加到响应结果中。
2、在目标组件中对响应状态代码或者响应头所做的修改都会被忽略。

```java
public class MainServlet extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        ServletContext context= getServletContext();
        RequestDispatcher headDispatcher = context.getRequestDispatcher("/header.html");
        RequestDispatcher greetDispatcher = context.getRequestDispatcher("/servlet/GreetServlet");
        RequestDispatcher footDispatcher = context.getRequestDispatcher("/footer.html");
        headDispatcher.include(request, response);
        greetDispatcher.include(request, response);
        footDispatcher.include(request, response);
        out.close();
    }

}
--------------------------------------------
    public class GreetServlet extends HttpServlet {
        public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
            response.getWriter().write("greet<hr/>");
        }
    }
```



## 请求范围

web应用范围内的共享数据作为ServeltContext对象的属性而存在(setAttribute)，只要共享ServletContext对象也就共享了其数据。
请求范围内的共享数据作为ServletRequest对象的属性而存在(setAttribute)，只要共享ServletRequest对象也就共享了其数据。

## request域共享空间

Context域共享空间、request域共享空间

本质：只要拿到同一个对象，那么就可以去操纵这个对象里面的map，进而就可以进行数据的共享

context对象很大，只要在当前应用下，不管任何servlet，拿到的都是同一个对象，所以全部都可以共享context域



request对象（频繁刷新浏览器多次，那么会向服务器发送多个请求，request对象是一个还是多个？）

多个，每发送一次请求，就会生成一个对应的request和reponse

即便刷新同一个页面，其实都不是同一个request对象，既然都不是同一个request对象，那么可以共享数据吗？不可以

只有转发的两个组件之间可以进行共享。



开发人员通过request对象在实现转发时，把数据通过request对象带给其它web资源处理。
setAttribute方法 
getAttribute方法  
removeAttribute方法
getAttributeNames方法



使用场景：

context域很大，request域很小；假设如果两个组件只是在一个请求中需要共享该数据，出了这个请求，那么数据就不再需要了，那么完全没有必要放在context域；直接放在request域即可，当请求响应之后，request就被销毁，数据也就清了。



应用如果卸载，context、request都被销毁

请求被响应了，request就销毁了，但是context依然在





# ServletResponse

其实就是服务器为了方便最终响应报文的输出，也为了方便我们开发者的使用，给我们提供了一个ServetResponse对象

只需要去调用该对象的某些方法，然后往这个对象里面塞入数据

最终Connector会读取对象里面的数据，然后生成响应报文

商场购物，给你提供了一个小推车，你只需要将你想买的商品放入小推车中，最终去结算就ok了

## 常用方法（掌握）

既然是为了方便我们设置响应报文

有哪些方法可以设置响应报文（每学完一个知识点，回去看一下HTTP和tomcat）

设置状态码 setStatus(int sc)

设置响应头 setHeader(String name, String value)

设置响应体 getWriter() / getOutputStream()

```java
@WebServlet("/response1")
public class ResponseServlet1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //设置响应状态码
        response.setStatus(404);
        //设置响应头
        response.setHeader("Content-Type", "text/html");
        //设置响应体
        response.getWriter().println("<h1 style='color:red'>File Not Found</h1>");
    }
}
```





## 向客户端输出字符数据（掌握）

用OutputStream(字节流)发送数据：
1、response.getOutputStream().write(“中国”.getBytes());//以默认本地编码发送数据
2、response.getOutputStream().write("中国".getBytes("UTF-8"));//以UTF-8编码发送数据，浏览器(默认用GB2312)会出现乱码
画图描述出现该问题的原因。
解决办法：

- 2.1通过更改浏览器的编码方式：IE/”查看”/”编码”/”UTF-8”(不可取)
- 2.2通过设置响应头告知客户端编码方式：response.setHeader(“Content-type”, “text/html;charset=UTF-8”);//告知浏览器数据类型及编码
- 2.3通过meta标签模拟请求头:out.write(“<meta  charset=utf-8' />".getBytes());
- 2.4通过以下方法：response.setContentType("text/html;charset=UTF-8");
  总结：程序以什么编码输出，就需要告知客户端以什么编码显示。

```
字节数据编码GBK,
字符数据用ISO-8859-1
小细节：输出字符“97”用response.getOutputStream().write(97);出现的问题？
```





用PrintWriter(字符流)发送数据：

response.getWriter().println是用于向响应体里面写入数据，响应体里面的数据会出现在浏览器的正文

示例：response.getWriter().write(“中国” );有没有乱码？

原因：以默认编码发送数据 ISO-8859-1（没有中国二字编码），此时会发生乱码
解决办法：
setCharacterEncoding(“UTF-8”);//更改编码为UTF-8
response.setHead(“Context-type”,”text/html;charset=UTF-8”);//告诉客户端编码方式
注意：不要忘记告诉客户端的编码方式。
由于经常改动编码，response提供了一种更简单的方式
response. setContentType(“text/html;charset=UTF-8”);其作用相当于以上两条代码。

```
//注意：设置 setCharacterEncoding 应该在     response.getWriter()之前，
否则拿到的PrintStream输出流还是使用默认编码集来编码。
```





## 中文乱码问题

```
void setCharacterEncoding(java.lang.String charset)
```

Sets the character encoding (MIME charset) of the response being sent to the client

```java
@WebServlet("/response2")
public class ResponseServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       //出现？？？？乱码，表示的是编码格式压根不支持中文
        //整个流程：编解码两个步骤
        //编码：服务器将中文进行编码，形成指定格式的数组  ISO-8859-1
        //解码：浏览器拿到数据之后，对数据进行解码，使用一个编码格式 GBK
        //为什么浏览器默认使用的是GBK呢？1.平台 操作系统 2.浏览器使用的语言有关 GBK
        //如何去设置编码格式呢？ request
        //设置gbk没有真正地去解决这个问题；1.海外的华人 浏览中文的网站  平台不是简体中文 浏览器使用的也不是中文语言
        //其实浏览器是支持多编码格式的，只是会采用一个默认的编码格式，如果服务器告诉浏览器使用某个编码格式，也是ok的
        //真正的方式：1.服务器设置一个编码格式  2.将编码格式告诉给浏览器,如何告诉
        response.setCharacterEncoding("utf-8");
        response.getWriter().println("你好！！！！");
    }
}
```

如何将编码格式告诉给浏览器

肯定需要通过响应报文



响应头

```java
//其实这一行代码有两层含义：1.发送一个Content-Type响应头，将编码格式告诉给浏览器  2.其实也设置了服务器的编码格式，所以respnse.setCharacterEncoding就没有必要再去写了
response.setHeader("Content-Type","text/html;charset=utf-8");
```

其实还有另外一种写法：

```java
//其实EE规范还给我们设置Content-Type头设置了一个简易的API
response.setContentType("text/html;charset=utf-8");
```

响应体

```java
@WebServlet("/response3")
public class ResponseServlet3 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //这个方法仅表示设置响应体的编码格式
        response.setCharacterEncoding("utf-8");
        response.getWriter().println("<!DOCTYPE html>\n" +
                "<html lang=\"en\">\n" +
                "<head>\n" +
                "    <meta charset=\"UTF-8\">\n" +
                "    <title>Title</title>\n" +
                "</head>\n" +
                "<body>");
        response.getWriter().println("你好！！！！");

        response.getWriter().println("</body>\n" +
                "</html>");
    }
}
```



思考：

为什么request设置了setCharacterEncoding就可以了，而response仅设置setCharacterEncoding却不可以

两者本质上来说是一致的



## 输出字节数据（掌握）

最常用的用法就是输出文件等二进制数据到客户端

![image-20210510111622975](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210510111622975.png)

两者的行为应该是差不多的

File和Servlet这两个输出流的区别应该在于写出的目的地不同，写出的过程应该是完全相同的

接下来，大家在使用ServletOutputStream的时候，整个过程和FileOutputStream完全相同的

```java
@WebServlet("/stream")
public class StreamServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //如果希望将二进制文件等数据传输给客户端，那么可以使用字节流
        ServletOutputStream outputStream = response.getOutputStream();
        //比较陌生，但是实际上它的使用和我们之前学习的FileOutputStream是一样的
        //提供文件的输入流
        ServletContext servletContext = getServletContext();
        String realPath = servletContext.getRealPath("WEB-INF/2.html");
        FileInputStream inputStream = new FileInputStream(new File(realPath));
        int length = 0;
        byte[] bytes = new byte[1024];
        while ((length = inputStream.read(bytes)) != -1){
            outputStream.write(bytes, 0, length);
        }
        //关闭流 ServletOutputStream可以关闭，也可以不关闭，如果不关闭，tomcat会帮助你关闭
        //自己创建的FileInputStream 那么应该关闭
        inputStream.close();
        outputStream.close();
    }
}
```

即便文件是存放在WEB-INF目录下面的，那么服务器也是可以将该文件响应给客户端的，只要服务器愿意



思考题：

如果希望大家能够在当前应用下实现一个缺省Servlet，应该如何实现？

1.缺省servlet的标志是url-pattern为/，一旦设置了缺省servlet，那么当前应用下的所有静态资源文件均无法正常显示了，那么需要大家做一个事情，仍然能够访问到对应的静态资源文件

要求是输入任意的资源文件均可以做出对应的响应。 有显示出来，没有显示404



提示：

路径相关的写法：

```java
@WebServlet("/url")
public class URLSummaryServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取资源路径
        String requestURI = request.getRequestURI();
        StringBuffer requestURL = request.getRequestURL();
        String servletPath = request.getServletPath();
        String contextPath = request.getContextPath();
        // URL = http:主机：端口号  + URI
        System.out.println(requestURI);
        System.out.println(requestURL);
        // URI =  contextPath + servletPath
        System.out.println(servletPath);
        //应用名
        System.out.println(contextPath);
    }
}
```

## 定时刷新页面

使用场景：

1.要求浏览器能够自动显示出时间，并且每秒钟会自动刷新

```java
@WebServlet("/refresh")
public class RefreshServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //显示出最新的时间，每秒钟自动刷新一次
        //设置一个refresh响应头即可  数字表示秒数，表示每隔多少秒刷新当前页面
        response.setHeader("refresh", "1");
        //如果希望改一下时间显示的格式  2021-05-10 11:38:00
        String formatDate = new SimpleDateFormat("YYYY-MM-dd HH:mm:ss").format(new Date());
        response.getWriter().println(formatDate);
    }
}
```

2.要求经过几秒钟以后跳转到另外一个页面

比如登录成功之后跳转到另外一个登录成功页面

```java
@WebServlet("/refresh2")
public class RefreshServle2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //表示经过多少秒之后跳转到指定的url路径 网络路径  Http://localhost:8080/app/form.html  /开头路径
        response.setHeader("refresh", "3;url=" + request.getContextPath() + "/success.html");
    }
}
```

## 重定向

通过response实现请求重定向。
请求重定向指：一个web资源收到客户端请求后，通知客户端去访问另外一个web资源，这称之为请求重定向。
地址栏会变，并发送2次请求，增加服务器负担（重定向到本服务器时，建议使用转发或者包含）
实现方式
response.sendRedirect()
实现原理：

HTTP状态码

302/307状态码和location头即可实现重定向
301 302 303 307



bing.com

服务器会返回一个重定向状态码，同时还会搭配一个Location响应头（路径），浏览器会紧接着再次向Location发送一次请求

```java
@WebServlet("/redirect")
public class RedirectServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //根据定义，我们自己实现重定向
//        response.setStatus(302);
//        response.setHeader("Location", request.getContextPath() + "/success.html");
        //但是其实没有必要自己去写，因为EE同样给我们封装好了一个简易的写法
        response.sendRedirect(request.getContextPath() + "/success.html");
    }
}
```



```
RFC1945(http://tools.ietf.org/html/rfc1945#page-34)，也就是HTTP1.0在介绍302时说，如果客户端发出POST请求后，收到服务端的302状态码，那么不能自动的向新的URI发送重复请求，必须跟用户确认是否该重发，因为第二次POST时，环境可能已经发生变化，POST操作会不符合用户预期。但是，很多浏览器（user agent我描述为浏览器以方便介绍）在这种情况下都会把POST请求变为GET请求。
303规定post请求重定向为get请求
307规定post请求重定向仍然为post请求
```



重定向

重定向机制的运作流程
1、用户在浏览器端输入特定URL，请求访问服务器端的某个组件
2、服务器端的组件返回一个状态码为302的响应结果。
3、当浏览器端接收到这种响应结果后，再立即自动请求访问另一个web组件
4、浏览器端接收到来自另一个web组件的响应结果。

HttpServeltResponse的sendRedirect(String location)用于重定向



```java
public class Check1Servlet extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
        PrintWriter out = response.getWriter();
        String username = request.getParameter("username");
        String message = null;
        if(username==null)
            message="Please input username:";
        else
            message="Hello,"+username;
        request.setAttribute("msg", message);
        out.println("Output from check1Servlet before redirecting1");
        System.out.println("Output from check1Servlet before redirecting1");
        response.sendRedirect(request.getContextPath()+"/servlet/Output1Servlet?msg="+message);//ok
        //response.sendRedirect("/servlet/Output1Servlet?msg="+message);//wrong
        //response.sendRedirect("http://localhost:8080"+request.getContextPath()+"/servlet/Output1Servlet?msg="+message);//ok
        //response.sendRedirect("http://www.itcast.cn");//ok
        out.println("Output from check1Servlet before redirecting2");
        System.out.println("Output from check1Servlet before redirecting2");
    }
}

```

特点
Servlet源组件生成的响应结果不会被发送到客户端(了解 )
      response.sendRedirect(String location)方法一律返回状态码为302的响应结果。
如果源组件在进行重定向之前，已经提交了响应结果，会抛出IllegalStateException。为了避免异常，不应该在源组件中提交响应结果。
      //Cannot call sendRedirect() after the response has been committed
在Servlet源组件重定向语句后面的代码也会执行。
源组件和目标组件不共享同一个ServletRequest对象。
对于sendRedirect(String location)方法的参数，如果以“/”开头，表示相对于当前服务器根路径的URL (不是当前应用的根目录)。以“http"//”开头，表示一个完整路径。
http://localhost/
目标组件不必是同一服务器上的同一个web应用的组件，它可以是任意一个有效网页。

```java
java.lang.IllegalStateException: Cannot call sendRedirect() after the response has been committed
```



## 页面跳转之间的区别

转发、定时跳转、重定向 这三种方法均可以用来进行页面的跳转

联系：都是用来进行页面跳转的

区别：

1.重定向的状态码是302，其他两种状态码是200

2.转发是发送了一次请求，其他都是发送两次请求

3.转发可以共享request域，其他是不可以共享的

4.转发只可以在当前应用下，另外两种技术没有任何限制

5.转发是request对象介导的，其他两种是response介导的



```java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取请求参数
        response.setContentType("text/html;charset=utf-8");
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        if("admin".equals(username) && "admin".equals(password)){
            //表示登录成功
            response.getWriter().println("登录成功，即将跳转至个人主页......");
            //发送一个refresh响应头，高速浏览器去重新发起新的请求
            //response.setHeader("refresh", "3;url=" + request.getContextPath() + "/success.html");
            response.sendRedirect(request.getContextPath() + "/success.html");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

## response细节

getOutputStream和getWriter方法分别用于得到输出二进制数据、输出文本数据的ServletOuputStream、Printwriter对象。
getOutputStream和getWriter这两个方法互相排斥，调用了其中的任何一个方法后，就不能再调用另一方法。  会抛异常。
Servlet程序向ServletOutputStream或PrintWriter对象中写入的数据将被Servlet引擎从response里面获取，Servlet引擎将这些数据当作响应消息的正文，然后再与响应状态行和各响应头组合后输出到客户端。 
Serlvet的service方法结束后，Servlet引擎将检查getWriter或getOutputStream方法返回的输出流对象是否已经调用过close方法，如果没有，Servlet引擎将调用close方法关闭该输出流对象。

```java
java.lang.IllegalStateException: getWriter() has already been called for this response
```

 

说明：

凡是关于页面相关的技术，比如转发、跳转、重定向还有后面的jsp等，

对大家的要求是了解即可

## 下载（使用场景不多）

首先对于浏览器来说，浏览器具有如下行为：

对于自己可以处理的文件，执行打开操作；对于自己无法处理的文件，那么执行下载操作，比如访问zip文件等。

指的是针对浏览器可以打开的部分文件，如果我希望浏览器可以执行下载操作，可以通过设置一个响应头，告知浏览器去执行下载而不是打开。

比如图片，正常情况下是会执行打开操作的，但是设置一个下载响应头，那么浏览器就会执行下载操作，而不是打开。



```java
@WebServlet("/down")
public class DownloadServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //当访问/down的时候，会把2.jpeg下载到本地
        //下载首先也要拿到文件的流，只需要加一个响应头就可以实现下载
        //response.setHeader("Content-Disposition", "attachment;filename=2.jpeg");
        ServletOutputStream outputStream = response.getOutputStream();
        String realPath = getServletContext().getRealPath("2.jpeg");
        FileInputStream inputStream = new FileInputStream(new File(realPath));
        int length = 0;
        byte[] bytes = new byte[1024];
        while ((length = inputStream.read(bytes)) != -1){
            outputStream.write(bytes, 0, length);
        }
        inputStream.close();
        outputStream.close();
    }
}
```

使用场景：

后台管理系统的导出数据功能



最后一步需要使用下载响应头



使用第三方的jar包来将数据写入到excel中，最后一步，需要设置该响应头，将文件下载到本地硬盘上面来。



```html
<a href="/response_war_exploded/stream">通过servlet访问图片</a>
<a href="/response_war_exploded/down">下载图片</a>
```

关于显示图片和下载图片的代码非常类似，可不可以将两个servlet进行合并呢？

里面的代码其实也可以合并



如何把多个servlet合并到一个servlet？

为什么要这么做？

软件设计里面，原则：高内聚、低耦合。功能上接近的代码应该尽可能放在一起。无关的代码应该尽可能分开

比如项目已一，电商，后台，管理员模块、用户模块、商品模块、订单模块

每个模块里面又有很多的功能点，比如管理员的新增、查询、修改、删除等功能，每个功能点都对应一个servlet，那么整个项目servlet将会非常多，后续进行查找，可以将相关的放置再一起，比如新建一个AdminServlet专门用来处理管理员模块的功能





如何合并呢？

1.一个servlet设置多个url-pattern

2.通过修改地址来实现

/response_war_exploded/pic/down 下载图片

/response_war_exploded/pic/view  显示图片

设置一个url-pattern为  /pic/*即可



# FileUpload

本地硬盘上面的文件上传到服务器的电脑上面，在服务器的电脑上面保存下来。

比如微信头像修改

选择本地的一张图片、上传到微信服务器上面去了

接下来今后在此访问头像，那么显示的都是上传修改之后的头像



将文件的二进制数据传输给服务器。

如何传输呢？

HTTP请求报文，文件只能放在请求报文的请求体中



java开发者来说，服务器怎么去处理？

你需要去拿到请求体里面的数据，然后做对应的处理，比如保存到本地硬盘上面



需要我们java开发者关注的事情其实很少

1.首先请求报文的生成、构建不需要我们去做，浏览器会帮助我们将文件的数据放置到请求报文的请求体中

2.请求报文到达服务器之后，服务器会帮我们把请求报文解析成request对象，所以此时请求体里面的数据也会在request对象中

**3.需要java开发者去做的事情，根据request给我们提供的方法，获取到请求体部分，然后去做对应的处理（比如本地IO流等）**





## 文件上传概述

实现web开发中的文件上传功能，需完成如下二步操作：
在web页面中添加上传输入项
在servlet中读取上传文件的数据，并保存到服务器硬盘中。
如何在web页面中添加上传输入项?
`<input type="file">`标签用于在web页面中添加文件上传输入项，设置文件上传输入项时须注意：
1、必须要设置input输入项的name属性，否则浏览器将不会发送上传文件的数据。
２、必须把form的enctype属值设为multipart/form-data.设置该值后，浏览器在上传文件时，将把文件数据附带在http请求消息体中，并使用ＭＩＭＥ协议对上传的文件进行描述，以方便接收方对上传数据进行解析和处理。
3、表单的提交方式要是post



如何在Servlet中读取文件上传数据，并保存到本地硬盘中?
Request对象提供了一个getInputStream方法，通过这个方法可以读取到客户端提交过来的数据。但由于用户可能会同时上传多个文件，在servlet端编程直接读取上传数据，并分别解析出相应的文件数据是一项非常麻烦的工作，示例。
为方便用户处理文件上传数据，Apache 开源组织提供了一个用来处理表单文件上传的一个开源组件（ Commons-fileupload ），该组件性能优异，并且其API使用极其简单，可以让开发人员轻松实现web文件上传功能，因此在web开发中实现文件上传功能，通常使用Commons-fileupload组件实现。
使用Commons-fileupload组件实现文件上传，需要导入该组件相应的支撑jar包：Commons-fileupload和commons-io。commons-io 不属于文件上传组件的开发jar文件，但Commons-fileupload 组件从1.1 版本开始，它工作时需要commons-io包的支持。



实现步骤
１、创建DiskFileItemFactory对象，设置缓冲区大小和临时文件目录
２、使用DiskFileItemFactory 对象创建ServletFileUpload对象，并设置上传文件的大小限制。
３、调用ServletFileUpload.parseRequest方法解析request对象，得到一个保存了所有上传内容的List对象。
４、对list进行迭代，每迭代一个FileItem对象，调用其isFormField方法判断是否是上传文件
True 为普通表单字段，则调用getFieldName、getString方法得到字段名和字段值
False 为上传文件，则调用getInputStream方法得到数据输入流，从而读取上传数据。
编码实现文件上传



核心API—DiskFileItemFactory

DiskFileItemFactory 是创建 FileItem 对象的工厂，这个工厂类常用方法：
public DiskFileItemFactory(int sizeThreshold, java.io.File repository) 
构造函数

public void setSizeThreshold(int sizeThreshold) 
设置内存缓冲区的大小，默认值为10K。当上传文件大于缓冲区大小时， fileupload组件将使用临时文件缓存上传文件。

public void setRepository(java.io.File repository) 
指定临时文件目录，默认值为System.getProperty("java.io.tmpdir").



核心API—ServletFileUpload

ServletFileUpload 负责处理上传的文件数据，并将表单中每个输入项封装成一个 FileItem 对象中。常用方法有：
boolean isMultipartContent(HttpServletRequest request) 
判断上传表单是否为multipart/form-data类型
List parseRequest(HttpServletRequest request)
解析request对象，并把表单中的每一个输入项包装成一个fileItem 对象，并返回一个保存了所有FileItem的list集合。 
setFileSizeMax(long fileSizeMax)
设置单个上传文件的最大值bytes
setSizeMax(long sizeMax) 
设置上传文件总量的最大值(多个文件同时上传时文件最大值的总和)
setHeaderEncoding(java.lang.String encoding)
设置编码格式，解决上传文件名乱码问题



核心API—FileItem

FileItem 用来表示文件上传表单中的一个上传文件对象或者普通表单对象
boolean  isFormField() 判断FileItem是一个文件上传对象还是普通表单对象
如果判断是一个普通表单对象
String   getFieldName()  获得普通表单对象的name属性
String  getString(String encoding) 获得普通表单对象的value属性
如果判断是一个文件上传对象
String  getName() 获得上传文件的文件名（有些浏览器会携带客户端路径）
InputStream getInputStream()  获得上传文件的输入流
delete()  在关闭FileItem输入流后，删除临时文件



上传文件的存放问题

文件存放位置
为保证服务器安全，上传文件应保存在应用程序的WEB-INF目录下，或者不受WEB服务器管理的目录。
为防止多用户上传相同文件名的文件，而导致文件覆盖的情况发生，文件上传程序应保证上传文件具有唯一文件名。
为防止单个目录下文件过多，影响文件读写速度，处理上传文件的程序应根据可能的文件上传总量，选择合适的目录结构生成算法，将上传文件分散存储。



## 文件上传准备工作

1.form表单 method=post

2.input type=file

```html
<form action="/app/upload" method="post">
    <input type="file" name="image"><br>
    <input type="submit">
</form>
```

紧接着点击提交，会发送请求

![image-20210510160637934](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210510160637934.png)

关注两个东西：

1.请求体里面的内容，没有上传文件，只是上传了文件的名称

2.Content-Length：12 进一步验证我们的猜想，的确上传了文件的名称，没有上传文件的内容



实际上，表单默认情况下是以key=value形式来进行提交数据，但是这种形式是无法进行文件上传的。

需要将参数的传递格式改一下。需要设置一个enctype=multipart/form-data



继续处理：

需要大家去从request中获取到请求体部分。

```java
package com.cskaoyan.upload;

import javax.servlet.ServletException;
import javax.servlet.ServletInputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

@WebServlet("/upload")
public class UploadServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //需要去从request中获取到请求体
        ServletInputStream inputStream = request.getInputStream();
        // 需要将图片保存在本地硬盘上面 输出流
        //比如放置在部署目录image目录下
        String realPath = getServletContext().getRealPath("image/2.jpeg");
        File file = new File(realPath);
        //下面的代码是为了保障父级目录一定会存在
        if(!file.getParentFile().exists()){
            //凡是该文件的上级所有目录，如果不存在，都会递归创建
            file.getParentFile().mkdirs();
        }
        FileOutputStream outputStream = new FileOutputStream(file);
        //IO流场景
        int length = 0;
        byte[] bytes = new byte[1024];
        while ((length = inputStream.read(bytes)) != -1){
            outputStream.write(bytes, 0, length);
        }
        inputStream.close();
        outputStream.close();
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

遇到了另外一个问题。



## 遇到的问题

### 问题一：仅上传文件名，不上传文件内容

```html
<form action="/app/upload" enctype="multipart/form-data" method="post">
    <input type="file" name="image"><br>
    <input type="submit">
</form>
```



### 问题二：二进制文件损坏

怎么去排查呢？为什么呢？

从另外一个角度去查找。

利用文本文件来排查。



![image-20210510162224403](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210510162224403.png)

文件上传之后，会出多来一部分符号。

这些字符直接写入到二进制文件里面，然后导致了文件的损坏。



### 问题三：为什么会有这个东西？如何去解决？

如果在上传文件的同时，还引入了表单数据

```
------WebKitFormBoundarypApeFoIUwqbxbdpd
Content-Disposition: form-data; name="username"

admin
------WebKitFormBoundarypApeFoIUwqbxbdpd
Content-Disposition: form-data; name="image"; filename="1.txt"
Content-Type: text/plain

hello
------WebKitFormBoundarypApeFoIUwqbxbdpd--
```

全部数据都进入到了文件中，并且这些符号并不是毫无意义，其实是利用这些符号进行区域的分割.



如何去解决呢?

可以通过去分割webkitformboundary这种形式来进行分割数据,取出来里面的参数.

实际操作难度是非常大的

市面上已经存在了很成熟的解决方案。

使用jar包

### 问题四:之前获取请求参数的方法也用不了

无法获取到请求参数.

原因:进能够获取key=value型数据,引入文件上传之后,此时数据格式也不再是key=value型了.





总结以下:

一切的根源在于引入了`enctype=multipart/form-data`

如果没有引入,此时请求参数是以key=value型进行提交的,获取请求参数是正常的,但是无法进行文件的上传,只会上传文件的名称

引入,可以上传文件了,但是此时二进制文件会损坏,文本文件会多出来一部分内容,获取请求参数的方法获取不到请求参数,归根结底原因在于为了能够上传文件,那么必须要改变请求体里面参数的数据格式,利用这些符号来进行分割各个部分.

## Commons-FileUpload组件（了解即可）

**引入组件之前的部分，是需要大家重点去掌握的，引入组件之后，其实只是要求大家能够根据API官方文档去解决实际应用中文件上传的功能即可。**

[官方网站](http://commons.apache.org/proper/commons-fileupload/)

```java
@WebServlet("/upload2")
public class UploadServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");		
        boolean multipartContent = ServletFileUpload.isMultipartContent(request);
        if(!multipartContent){
            //如果没有包含上传的文件，直接返回即可,其实这个只能判断是不是multi/form-data，不能判断是不是没有上传，判断没有上传需要在后面判断item.getSize()方法看看文件大小是否为0
            response.getWriter().println("没有包含上传的文件");
            return;
        }
        // 先创建一个DiskFileItemFactory
        DiskFileItemFactory factory = new DiskFileItemFactory();
        File repository = (File) getServletContext().getAttribute("javax.servlet.context.tempdir");
        factory.setRepository(repository);
        //该对象就是处理、解析请求的各个部分的核心组件对象
        ServletFileUpload upload = new ServletFileUpload(factory);
        //这一步其实就是将前端页面里面提交的每一项封装成一个FileItem
        //前端页面每出现一个input，那么就会有一个FileItem
        try {
            List<FileItem> items = upload.parseRequest(request);
            for (FileItem item : items) {
                //因为上传的文件处理逻辑和表单的处理逻辑不同，所以你应该分出来
                if(item.isFormField()){
                    processFormField(item);
                }else {
                    processUploadedFile(item);
                }
            }
        } catch (FileUploadException e) {
            e.printStackTrace();
        }

    }

    private void processUploadedFile(FileItem item) {
        String fieldName = item.getFieldName();
        String fileName = item.getName();
        String contentType = item.getContentType();
        boolean isInMemory = item.isInMemory();
        long sizeInBytes = item.getSize();
        System.out.println(fieldName + ":" + fileName + ":" + contentType + ":" + isInMemory + ":" + sizeInBytes);
        //还需要将文件保存在本地硬盘上面
        String realPath = getServletContext().getRealPath("image/" + fileName);
        File file = new File(realPath);
        if(!file.getParentFile().exists()){
            file.getParentFile().mkdirs();
        }
        try {
            item.write(file);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //处理表单数据的逻辑，只希望拿到对应的键值对即可
    private void processFormField(FileItem item) {
        //表单的name属性以及它对应的值
        String fieldName = item.getFieldName();
        String value = item.getString("utf-8");//不传如编码方式会中文乱码
        System.out.println(fieldName + ":" + value);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```



### 中文乱码问题

1.表单中文

​	如果乱码，那么不能够使用之前的request.setCharacterEncoding来设置了，原因其实在于请求体的数据结构发生了改变

request.setCharacterEncoding仅对key=value生效。



获取表单中的键值对时，不要用item.getString()，中文会乱码，使用带参数的方法传入正确的编码后就不会乱码。

org.apache.commons.fileupload

```
public interface FileItem
extends FileItemHeadersSupport
```

| `String` | `getFieldName()`Returns the name of the field in the multipart form corresponding to this file item. |
| -------- | ------------------------------------------------------------ |
| `String` | `getString(String encoding)`Returns the contents of the file item as a String, using the specified encoding. |



2.文件名中文

也是有乱码问题的。

request.setCharacterEncoding可以解决文件名中文乱码问题

或

ServletFileUpload对象.setHeaderEncoding(String encoding)





### 设置文件上传大小限制

```java
upload.setFileSizeMax(1024); //单位是byte
//需要在List<FileItem> items = upload.parseRequest(request);之前设置文件大小限制
```

## 封装数据到JavaBean

拓展性内容。理解起来有点困难，但是大家心态放平。不是说要求全部掌握，尽自己所能，掌握多少是多少

假设用户，注册、基本信息、用户名、密码等、此外还有头像，头像保存在用户里面应该保存什么？路径

```java
@WebServlet("/upload3")
public class UploadServlet3 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");
        boolean multipartContent = ServletFileUpload.isMultipartContent(request);
        if(!multipartContent){
            //如果没有包含上传的文件，直接返回即可
            response.getWriter().println("没有包含上传的文件");
            return;
        }
        // 先创建一个DiskFileItemFactory
        DiskFileItemFactory factory = new DiskFileItemFactory();
        File repository = (File) getServletContext().getAttribute("javax.servlet.context.tempdir");
        factory.setRepository(repository);
        //该对象就是处理、解析请求的各个部分的核心组件对象
        ServletFileUpload upload = new ServletFileUpload(factory);
        //这一步其实就是将前端页面里面提交的每一项封装成一个FileItem
        //前端页面每出现一个input，那么就会有一个FileItem
        User user = new User();
        try {
            //针对uploadHandler处理器进行设置，文件上传大小的限制
            // bytes
            //upload.setFileSizeMax(1024);
            List<FileItem> items = upload.parseRequest(request);

            for (FileItem item : items) {
                //因为上传的文件处理逻辑和表单的处理逻辑不同，所以你应该分出来
                if(item.isFormField()){
                    processFormField(item, user);
                }else {
                    processUploadedFile(item, user);
                }
            }
        } catch (FileUploadException e) {
            e.printStackTrace();
        }
        System.out.println(user);

    }

    private void processUploadedFile(FileItem item, User user) {
        String fieldName = item.getFieldName();
        String fileName = item.getName();
        String contentType = item.getContentType();
        boolean isInMemory = item.isInMemory();
        long sizeInBytes = item.getSize();
        System.out.println(fieldName + ":" + fileName + ":" + contentType + ":" + isInMemory + ":" + sizeInBytes);
        //还需要将文件保存在本地硬盘上面
        //比如，针对用户上传的头像，接下来上传成功，肯定希望再次去访问它，所以应该保存网络路径
        String realPath = getServletContext().getRealPath("image/" + fileName);
        File file = new File(realPath);
        if(!file.getParentFile().exists()){
            file.getParentFile().mkdirs();
        }
        try {
            item.write(file);
            user.setImage(realPath);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //处理表单数据的逻辑，只希望拿到对应的键值对即可
    private void processFormField(FileItem item, User user) {
        //表单的name属性以及它对应的值
        String fieldName = item.getFieldName();
        String value = null;
        try {
            value = item.getString("utf-8");
            if("username".equals(fieldName)){
                user.setUsername(value);
            }else if("password".equals(fieldName)){
                user.setPassword(value);
            }
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        System.out.println(fieldName + ":" + value);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

## 封装数据优化

可以使用BeanUtils，需要一个map

```java
@WebServlet("/upload4")
public class UploadServlet4 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");
        boolean multipartContent = ServletFileUpload.isMultipartContent(request);
        if(!multipartContent){
            //如果没有包含上传的文件，直接返回即可
            response.getWriter().println("没有包含上传的文件");
            return;
        }
        // 先创建一个DiskFileItemFactory
        DiskFileItemFactory factory = new DiskFileItemFactory();
        File repository = (File) getServletContext().getAttribute("javax.servlet.context.tempdir");
        factory.setRepository(repository);
        //该对象就是处理、解析请求的各个部分的核心组件对象
        ServletFileUpload upload = new ServletFileUpload(factory);
        //这一步其实就是将前端页面里面提交的每一项封装成一个FileItem
        //前端页面每出现一个input，那么就会有一个FileItem
        Map<String, Object> params = new HashMap<>();
        try {
            //针对uploadHandler处理器进行设置，文件上传大小的限制
            // bytes
            //upload.setFileSizeMax(1024);
            List<FileItem> items = upload.parseRequest(request);

            for (FileItem item : items) {
                //因为上传的文件处理逻辑和表单的处理逻辑不同，所以你应该分出来
                if(item.isFormField()){
                    processFormField(item, params);
                }else {
                    processUploadedFile(item, params);
                }
            }
        } catch (FileUploadException e) {
            e.printStackTrace();
        }
        User user = new User();
        try {
            BeanUtils.populate(user, params);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        System.out.println(user);
    }

    private void processUploadedFile(FileItem item, Map<String, Object> params) {
        String fieldName = item.getFieldName();
        String fileName = item.getName();
        String contentType = item.getContentType();
        boolean isInMemory = item.isInMemory();
        long sizeInBytes = item.getSize();
        System.out.println(fieldName + ":" + fileName + ":" + contentType + ":" + isInMemory + ":" + sizeInBytes);
        //还需要将文件保存在本地硬盘上面
        //比如，针对用户上传的头像，接下来上传成功，肯定希望再次去访问它，所以应该保存网络路径
        String relativePath = "/image/" + fileName;
        String realPath = getServletContext().getRealPath(relativePath);
        File file = new File(realPath);
        if(!file.getParentFile().exists()){
            file.getParentFile().mkdirs();
        }
        try {
            item.write(file);
            //这里面不能存取绝对路径，为什么呢？
            //你微信上传了一个照片，你是希望能够访问到该头像的  网络请求 传入一个相对路径
            //图片存放在部署根目录的image目录下的  /应用名        /image/xxxx
            params.put(fieldName, relativePath);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //处理表单数据的逻辑，只希望拿到对应的键值对即可
    private void processFormField(FileItem item, Map<String, Object> params) {
        //表单的name属性以及它对应的值
        String fieldName = item.getFieldName();
        String value = null;
        try {
            value = item.getString("utf-8");
            params.put(fieldName, value);
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        System.out.println(fieldName + ":" + value);
    }
}
```

## 进一步优化

one more step

比如用户上传头像

比如商城发布商品

90%以上的代码在进行用户头像上传以及商品发布的时候是一样的

设置一个工具类，进行代码的复用

```java
public class FileUploadUtils {

    public static Map<String, Object> parseRequest(HttpServletRequest request){
        // 先创建一个DiskFileItemFactory
        DiskFileItemFactory factory = new DiskFileItemFactory();
        File repository = (File) request.getServletContext().getAttribute("javax.servlet.context.tempdir");
        factory.setRepository(repository);
        //该对象就是处理、解析请求的各个部分的核心组件对象
        ServletFileUpload upload = new ServletFileUpload(factory);
        //这一步其实就是将前端页面里面提交的每一项封装成一个FileItem
        //前端页面每出现一个input，那么就会有一个FileItem
        Map<String, Object> params = new HashMap<>();
        try {
            //针对uploadHandler处理器进行设置，文件上传大小的限制
            // bytes
            //upload.setFileSizeMax(1024);
            List<FileItem> items = upload.parseRequest(request);

            for (FileItem item : items) {
                //因为上传的文件处理逻辑和表单的处理逻辑不同，所以你应该分出来
                if(item.isFormField()){
                    processFormField(item, params);
                }else {
                    processUploadedFile(item, params, request);
                }
            }
        } catch (FileUploadException e) {
            e.printStackTrace();
        }
        return params;
    }

    private static void processUploadedFile(FileItem item, Map<String, Object> params, HttpServletRequest request) {
        String fieldName = item.getFieldName();
        String fileName = item.getName();
        String contentType = item.getContentType();
        boolean isInMemory = item.isInMemory();
        long sizeInBytes = item.getSize();
        System.out.println(fieldName + ":" + fileName + ":" + contentType + ":" + isInMemory + ":" + sizeInBytes);
        //还需要将文件保存在本地硬盘上面
        //比如，针对用户上传的头像，接下来上传成功，肯定希望再次去访问它，所以应该保存网络路径
        String relativePath = "/image/" + fileName;
        String realPath = request.getServletContext().getRealPath(relativePath);
        File file = new File(realPath);
        if(!file.getParentFile().exists()){
            file.getParentFile().mkdirs();
        }
        try {
            item.write(file);
            //这里面不能存取绝对路径，为什么呢？
            //你微信上传了一个照片，你是希望能够访问到该头像的  网络请求 传入一个相对路径
            //图片存放在部署根目录的image目录下的  /应用名        /image/xxxx
            params.put(fieldName, relativePath);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //处理表单数据的逻辑，只希望拿到对应的键值对即可
    private static void processFormField(FileItem item, Map<String, Object> params) {
        //表单的name属性以及它对应的值
        String fieldName = item.getFieldName();
        String value = null;
        try {
            value = item.getString("utf-8");
            params.put(fieldName, value);
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        System.out.println(fieldName + ":" + value);
    }
}
```

```java
@WebServlet("/upload5")
public class UploadServlet5 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");
        boolean multipartContent = ServletFileUpload.isMultipartContent(request);
        if(!multipartContent){
            //如果没有包含上传的文件，直接返回即可
            response.getWriter().println("没有包含上传的文件");
            return;
        }

        Map<String, Object> params = FileUploadUtils.parseRequest(request);
        User user = new User();
        try {
            BeanUtils.populate(user, params);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```



## 文件重名问题

使用组件来进行文件上传的时候，组件默认情况是会忽略文件名相同的文件

微信头像上传的时候，帅哥.jpg

如何去解决文件的重名问题呢？

改名。

没有统一的标准。

1.微信id  wxid  wxid + timestamp 形成文件名

2.当前的时间戳 随即编号

3.完全随即

```java
String uuid = UUID.randomUUID().toString();
String fileName = item.getName();
fileName = uuid + "-" + fileName;
```



## 目录内文件数过多的问题

如果电脑性能不是特别好，某个盘符下面文件数非常多，硬盘转圈的声音，目录也在转圈

比如有100万个文件





网络请求也是相同

访问某个文件，这个文件在该目录内，同时还有100多w其他的文件，就需要从100w个数据中找到对应的数据



如何能够提升网络请求速度



微信---每天用户会提交10亿张图片

分流

按照某种规则，创建很多目录

1.按年月日来划分目录，一年12目录，每个月30小目录，也是可行的

也是具有不稳定性的

节假日、重大节日，某一天的图片量可能仍然是超过平时一周的图片量

不均匀



如何让文件存放均匀一些呢？

散列

hashcode

文件名-----hashcode----------0x 0 	1	2	3	6	7	A	F	----file

```java
public class FileUploadUtils {

    public static Map<String, Object> parseRequest(HttpServletRequest request){
        // 先创建一个DiskFileItemFactory
        DiskFileItemFactory factory = new DiskFileItemFactory();
        File repository = (File) request.getServletContext().getAttribute("javax.servlet.context.tempdir");
        factory.setRepository(repository);
        //该对象就是处理、解析请求的各个部分的核心组件对象
        ServletFileUpload upload = new ServletFileUpload(factory);
        //这一步其实就是将前端页面里面提交的每一项封装成一个FileItem
        //前端页面每出现一个input，那么就会有一个FileItem
        Map<String, Object> params = new HashMap<>();
        try {
            //针对uploadHandler处理器进行设置，文件上传大小的限制
            // bytes
            //upload.setFileSizeMax(1024);
            List<FileItem> items = upload.parseRequest(request);

            for (FileItem item : items) {
                //因为上传的文件处理逻辑和表单的处理逻辑不同，所以你应该分出来
                if(item.isFormField()){
                    processFormField(item, params);
                }else {
                    processUploadedFile(item, params, request);
                }
            }
        } catch (FileUploadException e) {
            e.printStackTrace();
        }
        return params;
    }

    private static void processUploadedFile(FileItem item, Map<String, Object> params, HttpServletRequest request) {
        String fieldName = item.getFieldName();
        String uuid = UUID.randomUUID().toString();
        String fileName = item.getName();
        fileName = uuid + "-" + fileName;
        String contentType = item.getContentType();
        boolean isInMemory = item.isInMemory();
        long sizeInBytes = item.getSize();
        System.out.println(fieldName + ":" + fileName + ":" + contentType + ":" + isInMemory + ":" + sizeInBytes);
        //还需要将文件保存在本地硬盘上面
        //比如，针对用户上传的头像，接下来上传成功，肯定希望再次去访问它，所以应该保存网络路径
        String basePath = "/image";
        int hashCode = fileName.hashCode();
        String hexString = Integer.toHexString(hashCode);
        char[] chars = hexString.toCharArray();
        for (char aChar : chars) {
            basePath = basePath + "/" + aChar;
        }
        String relativePath = basePath  + "/" + fileName;
        String realPath = request.getServletContext().getRealPath(relativePath);
        File file = new File(realPath);
        if(!file.getParentFile().exists()){
            file.getParentFile().mkdirs();
        }
        try {
            item.write(file);
            //这里面不能存取绝对路径，为什么呢？
            //你微信上传了一个照片，你是希望能够访问到该头像的  网络请求 传入一个相对路径
            //图片存放在部署根目录的image目录下的  /应用名        /image/xxxx
            params.put(fieldName, relativePath);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //处理表单数据的逻辑，只希望拿到对应的键值对即可
    private static void processFormField(FileItem item, Map<String, Object> params) {
        //表单的name属性以及它对应的值
        String fieldName = item.getFieldName();
        String value = null;
        try {
            value = item.getString("utf-8");
            params.put(fieldName, value);
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        System.out.println(fieldName + ":" + value);
    }
}
```



## 案例

upload页面进行数据的上传，上传成功之后要求可以在另外一个页面进行数据的回显，图片要求可以显示出来

假设用户，注册、基本信息、用户名、密码等、此外还有头像，头像保存在用户里面应该保存什么？路径

这个头像路径是给浏览器用的，应该是/appname开头的网络地址



# 乱码总结

[URL含中文字符乱码](##URL含中文字符乱码)

[post提请求交参数乱码](##获取请求参数中文乱码)

[响应中文乱码](##中文乱码问题)

[文件上传表单中文参数乱码](###设置文件上传大小限制)

[文件上传中文文件名乱码](###设置文件上传大小限制)





# 会话技术

问题：什么是会话？
	会话可简单理解为：用户开一个浏览器，点击多个超链接，访问同一个服务器多个web资源，然后关闭浏览器，整个过程称之为一个会话。
会话过程中要解决的一些问题？
每个用户在使用浏览器与服务器进行会话的过程中，不可避免各自会产生一些数据，程序要想办法为每个用户保存这些数据。
例如：用户点击超链接通过一个servlet购买了一个商品，程序应该想办法保存用户购买的商品，以便于用户点结帐servlet时，结帐servlet可以得到用户购买的商品为用户结帐。

保存会话数据的两种技术Cookie, Session



会话，现实生活中就是交谈、对话。

web访问过程中，会话的含义是指从打开浏览器页面开始，然后浏览网站的多个页面，然后最后结束，整个过程我们称之为一个会话。

web访问过程中，每个用户都会产生一些数据，比如我添加的购物车商品、我的用户名，希望服务器可以给用户去存储一些数据，但是呢HTTP协议其实是一个无状态协议，决定了HTTP不能够给用户去存储数据的，矛盾的地方。需要去引入相应的会话技术，然后去解决这个问题。

代码演示HTTP协议无状态性

```java
@WebServlet("/status")
public class NonStatusServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //把HTTP请求报文打印一遍
        //通过这个案例，我们不同用户访问当前页面，它的请求报文是完全相同的
        //HTTP协议无状态性的一个体现，所有用户的HTTP请求报文都是完全相同的
        //有没有疑惑？购物车如何实现的呢？服务器如何给我保存数据的呢？
        //我在购物车里面添加了商品，下次访问发现这些商品还在
        System.out.println(request.getMethod() + " " + request.getRequestURI() + " " + request.getProtocol());
        Enumeration<String> headerNames = request.getHeaderNames();
        while (headerNames.hasMoreElements()){
            String s = headerNames.nextElement();
            System.out.println(s + ":" + request.getHeader(s));
        }
        System.out.println("================");
    }
}
```



## HTTP协议的状态性是如何保障服务器存储数据的呢

既然HTTP协议是无状态性，那么服务器又是如何给客户端保存相应的数据的呢？

但是我们在实际访问过程中，我们发现其实并没有相关的问题

原因就在于其实HTTP协议引入了相关的会话技术。

**对于标准的HTTP协议来说，的确是无状态性；的确没法帮助用户去存储一些数据**

**我们是在HTTP协议的基础上引入了相关的会话技术，来让HTTP协议有了状态，能够帮助用户去保存相关数据。**

会话技术可以分为两大类

一类是客户端技术

一类是服务器技术

## Cookie

客户端技术。数据的产生是在服务器上面的，数据产生之后，数据会伴随着**HTTP响应报文的响应头**传输给客户端，客户端会随即将该数据进行保存，接下来当客户端再次访问服务器时，那么他就会把该数据带回给服务器（**会以HTTP请求头的形式携带回来**），服务器通过取出来这个值，就可以知道来自于哪个客户端。



![image-20210511112630228](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210511112630228.png)



**cookie会话技术的本质：**

**其实就是在HTTP协议中加入了cookie相关的请求头（cookie:key=value）和响应头(set-Cookie:key=value)**

注意：cookie中不能有空格

```
Web应用程序是使用HTTP协议传输数据的。HTTP协议是无状态的协议。一旦数据交换完毕，客户端与服务器端的连接就会关闭，再次交换数据需要建立新的连接。这就意味着服务器无法从连接上跟踪会话。即用户A购买了一件商品放入购物车内，当再次购买商品时服务器已经无法判断该购买行为是属于用户A的会话还是用户B的会话了。要跟踪该会话，必须引入一种机制。
cookie
由于服务器无法从http协议上分辨不同的用户，所以服务器要想办法给客户端们颁发一个证件，每人一个，无论谁访问都必须携带自己证件。这样服务器就能从每个人的证件上确认客户身份了。这就是Cookie的工作原理。
Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。
```



Cookie 基本API

javax.servlet.http.Cookie类用于创建一个Cookie，response接口中定义了一个addCookie方法，它用于在其响应头中增加一个相应的Set-Cookie头字段。 同样，request接口中也定义了一个getCookies方法，它用于获取客户端提交的Cookie。Cookie类的方法： 
public Cookie(String name,String value)
setValue与getValue方法 
setMaxAge与getMaxAge方法 (秒)
getName方法 



```
1.Cookie 能被访问要符合:MYURL.startWith(domain+path)完全匹配   然后再找name也匹配，此时才能访问到value

http://localhost:8080/day20_00_cookie/servlet/CookieDemo3   可以访问
http://localhost:8080/day20_00_cookie/CookieDemo2   不能
http://localhost:8080/day20_00_cookie/servlet/myservlet/CookieDemo3  可以


给不给传cookie是由浏览器决定的，取决于MYURL.startWith(domain+path)完全匹配
```



Cookie 其他API

setPath与getPath方法
setDomain与getDomain方法



Cookie细节

一个Cookie只能标识一种信息，它至少含有一个标识该信息的名称（NAME）和设置值（VALUE）value也必须是字符串类型。 
一个WEB站点可以给一个WEB浏览器发送多个Cookie，一个WEB浏览器也可以存储多个WEB站点提供的Cookie。
浏览器一般只允许存放300个Cookie，每个站点最多存放20-50个Cookie，每个Cookie的大小限制为4KB。
如果创建了一个cookie，并将他发送到浏览器，默认情况下它是一个会话级别的cookie（即存储在浏览器的内存中），用户退出浏览器之后即被删除。若希望浏览器将该cookie存储在磁盘上，则需要使用maxAge，并给出一个以秒为单位的时间。将最大时效设为0则是命令浏览器删除该cookie。
注意，删除cookie时，path必须一致，否则不会删除

```java
Map<String,Book> books =BookDB.getAllBooks();    
for(Map.Entry<String, Book>itemEntry :books.entrySet()   ){    
    Bookbook= itemEntry.getValue();
    pw.write(book.getName()+"&nbsp;&nbsp;&nbsp;&nbsp;<a href='http://localhost/MyCookieDemo/servlet/Showdetails?id="+book.getId()+"'>查看详情</a> <br>");
}

```

浏览器的容量限制

![image-20210512140334597](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210512140334597.png)



### 案例一

用户登录，登录成功可以显示用户的用户名，使用cookie来实现

```java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //先拿到用户提交过来的请求参数
        response.setContentType("text/html;charset=utf-8");
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        //不做判断，都表示登录成功
        //另外一个页面来显示用户名，所以也就是两个servlet之间进行username共享
        //context域能不能用？不可以   usnermae-----xxxx
        //request域可以用来进行用户名的共享 太小了，如果第三个页面也需要用
        //你需要做的事情：发送一个set-Cookie：name=xxxx响应头
        //再另外一个servlet中，需要去解析出Cookie：name=xxx请求头里面的数据
        //利用之前学习的response以及request关于响应头、请求头相关api的确可以完成
        //但是太过于复杂，
        //EE规范同样给我们提供了非常方便的方法，我们只需要去按照这个规则去调用即可
        //如果你希望发送一个set-Cookie响应头，那么你只需要去设置如下代码
        Cookie cookie = new Cookie("username", username);
        response.addCookie(cookie);
        response.getWriter().println("登录成功，即将跳转至个人主页");
        response.setHeader("refresh", "2;url=" + request.getContextPath() + "/info");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

```java
@WebServlet("/info")
public class InfoServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        //如何去获取cookie呢，只需要去调用如下代码就可以解析Cookie请求头
        Cookie[] cookies = request.getCookies();
        if(cookies != null){
            for (Cookie cookie : cookies) {
                if("username".equals(cookie.getName())){
                    String value = cookie.getValue();
                    response.getWriter().println("欢迎您, " + value);
                }
            }
        }
    }
}
```

### 案例二

显示用户的上次访问时间

```java
@WebServlet("/last")
public class LastLoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //cookie值里面不能有空格
        Cookie[] cookies = request.getCookies();
        if(cookies != null){
            for (Cookie cookie : cookies) {
                if("lastLogin".equals(cookie.getName())){
                    String value = cookie.getValue();
                    //这个方法可以将long类型的字符串转成long类型的数字
                    long l = Long.parseLong(value);
                    Date date = new Date(l);
                    response.getWriter().println(date);
                }
            }
        }

        Cookie cookie = new Cookie("lastLogin", System.currentTimeMillis() + "");
        response.addCookie(cookie);
    }
}
```

### 设置存活时间

默认情况下，cookie其实是存在于浏览器的内存中，只是开启浏览器的这段时间内是有效的

如果关闭浏览器，再重新打开，那么cookie失效



可以设置一个正数的时间，表示cookie在硬盘存活多少秒

```java
cookie.setMaxAge(180);//单位 秒
```



如果设置的是负数，其实和没有设置是一样的，默认的情况，也就是存在于浏览器的内存中



### 设置路径

如果没有设置，那么表示的是当访问当前域名下所有资源都会携带cookie，如果希望访问指定路径才会携带cookie，则可以设置一个path

比如设置访问html页面时携带cookie，访问js、css这些不去携带cookie



```
可在同一应用服务器内多个应用共享cookie方法：设置cookie.setPath("/");//和没设置一样的
```

只setPath，不设置domain，仅当前domain的这个path如aaa.com/path有效，关联的如sub.aaa.com/path无效

```java
@WebServlet("/path1")
public class PathServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Cookie cookie = new Cookie("path", "path2");
        //希望访问path2的时候才会携带cookie
        cookie.setPath(request.getContextPath() + "/path2");
        response.addCookie(cookie);
    }
}
```

```java
@WebServlet("/path2")
public class PathServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

### 设置域名

关于cookie的域名，有一个大的前提

cookie其实是不允许设置无关域名的cookie的，比如当前域名叫做localhost，你设置了一个域名为baidu.com的cookie，这是不允许的，浏览器也不会允许你设置成功，会拦截



域名：baidu.com

account.baidu.com属于baidu的吗，别人可以申请到这个域名吗？别人不可以申请到，这个域名还是属于baidu.com

register.account.baidu.com 同理  也是属于baidu的，别人也是申请不到的

如果你设置了一个父域名的cookie，那么当你访问子域名的资源时，该cookie会自动携带过去

比如你设置了一个baidu.com域名的cookie，接下来当你访问account.baidu.com时，浏览器依然会携带cookie过去

继承。



注意：如果不setDomain 只有当前域名如aaa.com可以，sub.aaa.com不可以；但如果setDomain为aaa.com后aaa.com和sub.aaa.com都可以。

默认cookie是同当前domain的不同应用都可以（如果不setPath的话），aaa.com的/app1和/app2都可以，但是sub.aaa.com是不可以的。



```java
@WebServlet("/domain2")
public class DomainServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Cookie cookie = new Cookie("attribute", "aaa");
        //注意：如果你设置该域名。那么一定不能在localhost、127.0.0.1主机下访问
        //执行这个servlet时，不可以通过localhost:8080/app/domain2来执行
        //应该通过http://aaa.com:8080/app/info
        cookie.setDomain("aaa.com");
        response.addCookie(cookie);
    }
}
```

### 删除cookie

并没有提供一个专门用来删除cookie的方法，如果想删除cookie，其实就是设置MaxAge=0

**注意：删除cookie的行为不是服务器去删除，而是服务器去给客户端发送一个消息，让客户端去删除。**



对cookie执行任意操作设置之后，都需要再次去调用response.addCookie()，因为cookie存储在客户端的，只是去给客户端发送一个信号，让客户端去做某些事情。



删除cookie时，还有一个注意点：

**如果cookie设置了path，接下来在删除的时候，需要再把path再写一遍**

```java
@WebServlet("/path2")
public class PathServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //希望在path2里面把"path", "path2"这个 cookie给删除
        Cookie[] cookies = request.getCookies();
        if(cookies != null){
            for (Cookie cookie : cookies) {
                if("path".equals(cookie.getName())){
                    System.out.println(cookie.getValue());
                    cookie.setMaxAge(0);
                    cookie.setPath(request.getContextPath() + "/path2");
                    response.addCookie(cookie);
                }
            }
        }
    }
}
```

### 

### cookie的优缺点

优点：

1.cookie很小，很轻便

2.cookie存储在客户端，可以很大程度减轻服务器的压力



缺点：

1.cookie很小，存储的数据容量有限制

2.cookie的值只能是字符串，如果希望存储object，还需要转成字符串

3.cookie数据存储在客户端，安全性要稍低



如果有一小部分数据需要存储，并且数据也不是特别的敏感，那么就可以放在cookie中存储



### cookie细节

cookie重名不会覆盖，服务器会一并发给浏览器，浏览器请求时只会选择带其中一个（重名的几个cookie实际上都存在浏览器内存中）。这个特点可用在服务器生成session时给对应的cookie设置存活时间，手动创建一个cookie = new Cookie("JSESSION", session.getId)并setMaxAge发给浏览器，浏览器虽然会收到两个cookie都叫JESESSIONID但是关闭后再打开只有设置了存活时间的会存在。



## Session

服务器技术。当用户访问服务器时，服务器会给用户开辟一块内存空间，这个内存空间其实就专门用来给当前客户端来使用，接下来如果当前客户端需要进行数据的存取，那么你存取这个内存空间即可。

### 概述

客户端是如何和内存空间绑定关联在一起的？

这块内存空间，应该是独一无二的，可以给这块内存空间设定一个ID编号，然后接下来将该ID编号通过cookie的形式返回给客户端

客户端会将该ID保存，当下次再次访问服务器时，会把该ID携带回来，服务器通过去查找该ID，那么就可以知道这块内存空间在哪



问题：什么是会话？
	会话可简单理解为：用户开一个浏览器，点击多个超链接，访问服务器多个web资源，然后关闭浏览器，整个过程称之为一个会话。
会话过程中要解决的一些问题？
每个用户在使用浏览器与服务器进行会话的过程中，不可避免各自会产生一些数据，程序要想办法为每个用户保存这些数据。
例如：用户点击超链接通过一个servlet购买了一个商品，程序应该想办法保存用户购买的商品，以便于用户点结帐servlet时，结帐servlet可以得到用户购买的商品为用户结帐。



保存会话数据的两种技术cookie vs Session

Cookie
Cookie是客户端技术，程序把每个用户的数据以cookie的形式写给用户各自的浏览器。当用户使用浏览器再去访问服务器中的web资源时，就会带着各自的数据去。这样，web资源处理的就是用户各自的数据了。
HttpSession
Session是服务器端技术，利用这个技术，服务器在运行时可以为每一个用户的浏览器创建一个其独享的HttpSession对象，由于session为用户浏览器独享，所以用户在访问服务器的web资源时，可以把各自的数据放在各自的session中，当用户再去访问服务器中的其它web资源时，其它web资源再从用户各自的session中取出数据为用户服务。



有状态会话：一个同学来过教室，下次再来教室，我们会知道这个同学曾经来过，这称之为有状态会话。



session

在WEB开发中，服务器可以为每个浏览器创建一个会话对象（session对象），注意：把用户数据写到用户浏览器独占的session中，当前用户使一个浏览器独占一个session对象(默认情况下)。因此，在需要保存用户数据时，服务器程序可以用session保存。同一浏览器可以从用户的session中取出该用户的数据，为用户服务。
（数据保存在服务器的Session对象中 ，内存。浏览器怎么拿到？JsessionID）
Session对象由服务器创建，开发人员可以调用request对象的getSession方法得到session对象。
跟cookie一样 从request报文中拿到Session数据？
  并不是，而是通过报文里的JsessionID 去 服务器内存里查找）



Session小实验：使用IE访问某一个servlet，其它IE可以取到这个servlet存的数据吗？



Session和Cookie的主要区别在于：
Cookie是把用户的数据写给用户的浏览器（在浏览器保存）。
Session技术把用户的数据写到用户独占的session中。（在服务器端保存）



Session 生命周期

![image-20210512141340197](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210512141340197.png)





### session使用（session域）

1.如何创建session？

​	servletRequest里面给我们封装好了一个方法，我们只需要去调用即可

​	

```
HttpSession getSession()
```

Returns the current session associated with this request, or if the request does not have a session, creates one.

- **Returns:**

  the `HttpSession` associated with this request

- **See Also:**

  [`getSession(boolean)`](http://tomcat.apache.org/tomcat-8.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html#getSession(boolean))

**总结一下：如果当前request有关联的sesison对象，那么直接返回，如果没有，那么创建一个**



其实除了上面这个午参数的形式，还有一个有参数的形式

```
HttpSession getSession(boolean create)
```

Returns the current `HttpSession` associated with this request or, if there is no current session and `create` is true, returns a new session.

If `create` is `false` and the request has no valid `HttpSession`, this method returns `null`.

总结一下：**如果当前request有关联的session对象，那么直接返回**；如果没有，并且create是true，那么创建一个；如果此时create是false，那么直接返回null



2.session的使用

session的执行流程和context，session域。

session对象中包含了一个map，如果你能够拿到同一个session对象，那么就可以拿到同一个map，就可以进行map的共享



哪些情况下拿到的是同一个session对象呢？

session是和浏览器关联的，如果是同一个浏览器，那么它进行的访问是不是用的都是一个session

### 登录案例

```java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        HttpSession session = request.getSession();
        session.setAttribute("username", username);
        response.getWriter().println("登录成功，即将跳转至个人主页");
        response.setHeader("refresh", "2;url=" + request.getContextPath() + "/info");

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

```java
@WebServlet("/info")
public class InfoServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        //如何去获取cookie呢，只需要去调用如下代码就可以解析Cookie请求头
        HttpSession session = request.getSession();
        Object username = session.getAttribute("username");
        response.getWriter().println("欢迎您，" + username);
    }
}
```

### getSession()执行逻辑

判断有没有session，其实完全依赖于当前请**求是否携带了一个有效的Cookie:JSESSIONID=xxxx,**如果携带了，则根据这个id去到session列表中找到对应的session对象；如果没有携带该cookie头，那么就会创建一个session对象

![image-20210511160303949](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210511160303949.png)



### 关闭浏览器session会被销毁吗，数据会丢失吗

```java
@WebServlet("/session1")
public class SessionServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //如何创建session对象呢？
        //第一次访问当前servlet，当前request对象肯定没有关联的session对象，所以肯定会创建一个
        // Set-Cookie: JSESSIONID=57E9B12CF320ACCDF39E376F04306196; Path=/session_war_exploded; HttpOnly
        //如果接下来第二次再次访问当前servlet，还会不会创建sesssion？
        //第二次访问时，发现的确是不会有set-Cookie:JSESSIONID=XXX这个响应头，说明没有生成session对象
        HttpSession session = request.getSession();
        System.out.println("session1:" + session);
        System.out.println("session1:" + session.getId());
        session.setAttribute("username", "ssss");
    }
}
```

```java
@WebServlet("/session2")
public class SessionServlet2 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //如何创建session对象呢？
        //第一次访问当前servlet，当前request对象肯定没有关联的session对象，所以肯定会创建一个
        // Set-Cookie: JSESSIONID=57E9B12CF320ACCDF39E376F04306196; Path=/session_war_exploded; HttpOnly
        //如果接下来第二次再次访问当前servlet，还会不会创建sesssion？
        //第二次访问时，发现的确是不会有set-Cookie:JSESSIONID=XXX这个响应头，说明没有生成session对象
        HttpSession session = request.getSession();
        System.out.println("session2:" + session);
        System.out.println("session2:" + session.getId());
        Object username = session.getAttribute("username");
        System.out.println(username);
    }
}
```

访问session1打印

session1:org.apache.catalina.session.StandardSessionFacade@70450898
session1:3883EC1D9D5C90447B0E6EBE3068D847

访问session2打印

session2:org.apache.catalina.session.StandardSessionFacade@70450898
session2:3883EC1D9D5C90447B0E6EBE3068D847
ssss

关闭浏览器，再次访问session2打印

session2:org.apache.catalina.session.StandardSessionFacade@e631fbf
session2:CABE8275D8AD4AC67A1306CE7512183F
null

关闭浏览器session对象不会被销毁。为什么会出现session的地址、id以及数据全部都没了的现象呢？

**主要的原因在于请求携带的cookie请求头没了，会创建一个新的session对象**。

### 关闭服务器session会销毁吗，数据会丢失吗

关闭服务器，session对象会被销毁；数据不会丢失。

数据会不会丢失我们需要去验证一下。

验证的方式，注意：**不要使用关闭IDEA的tomcat来验证，否则你得不出正确的结论**。

配置：

1.

到本地安装的tomcat  conf/tomcat-users.xml文件中增加如下设置

```xml
<role rolename="manager-gui"/>
<user username="tomcat" password="tomcat" roles="manager-gui"/>
```

2.本地的tomcat webapps目录需要有manager应用



去访问该管理系统：

http://localhost:8080/manager



访问 session1

访问session2

session2:org.apache.catalina.session.StandardSessionFacade@e631fbf
session2:CABE8275D8AD4AC67A1306CE7512183F
ssss

重启应用

11-May-2021 16:34:30.764 璀﹀憡 [http-nio-8080-exec-31] org.apache.tomcat.util.descriptor.web.WebXml.setVersion Unknown version string [4.0]. Default version will be used.
11-May-2021 16:34:30.816 淇℃伅 [http-nio-8080-exec-31] org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.

访问sesion2

session2:org.apache.catalina.session.StandardSessionFacade@31ee950
session2:CABE8275D8AD4AC67A1306CE7512183F
ssss

总结：

观察到应用重启之后，session对象被销毁了（地址），但是session的id没有变，session里面的数据依然可以访问到

实际上是在被销毁之前，把数据转移到了本地硬盘上面（序列化）

应用重新被启动的时候，重新将该序列化形式的文件读取到内存中，生成新的session对象，会将原先的session的id以及session的数据塞入到这个新的session对象中。



可以直接将IDEA里面的应用直接用本地tomcat来部署



注意：session域中保存的对象必须实现了Serializable接口才能在重启之后恢复，否则会丢失。



CATALINA_BASE里存放session序列化后的本地文件，在tomcat启动读取文件后会删除它。

注意IDEA下的tomcat是每次启动都会拷贝本地tomcat的配置文件，所以在IDEA界面的tomcat下停止服务器虽然也会在拷贝的CATALINA_BASE里生成session序列化的本地文件，但是再次启动IDEA的tomcat时IDEA会先删除所有旧的配置文件后再到本地tomcat去重新拷贝配置文件，IDEA的这种行为导致不能再IDEA的tomcat控制界面下完成这个session序列化的测试

### session生命周期

session对象的创建：

request.getSession()  session对象被创建



应用被卸载、服务器关闭  session对象被销毁



session对象被销毁并不意味着数据的丢失

数据的丢失只有两个因素有关

1.session的数据有一个有效期，有效期到达，session里面的数据自动消失（30min，如果30min内没有访问该session，那么数据丢失）

2.主动调用session.invalidate()方法将数据被销毁



### 三个域总结

![image-20210511172639361](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210511172639361.png)

选择：

request域：很小，如果我们希望在一次请求内两个组件之间进行共享，request域



session域：一个浏览器和一个session对象对应。一个浏览器访问很多个servlet时，对应的都是同一个session对象。和用户具有特异性的数据。用户特有的数据，购物车、浏览记录、用户名



context域：最大的，一个应用只有一个。一般情况下我们可以存放一些全局性的配置，和用户无关的。比如商城商品的分类，应用的配置信息。



请求执行流程

![请求执行流程](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\请求执行流程.png)

## 应用案例

首页，首页可以展示出商品的超链接



点击超链接，进入商品详情页，商品toString



商品详情页还有三个a标签，一个是加入购物车、一个是查看购物车、一个返回首页



点击加入购物车，会将商品加入到购物车中

点击查看购物车，会显示当前用户的购物车商品



实现历史足迹

不同要求：

最低要求：不要求按照时间降序来；也不要求将久远的剔除出去

实现真正的历史足迹：按照时间降序来，仅显示最近的20条浏览记录



## 浏览器禁用Cookie后的session处理

session的存储依赖于cookie，浏览器可以禁用cookie的

如果禁用了cookie，此时session会怎么样呢？

进行URL重写

实验演示禁用Cookie后servlet共享数据导致的问题。
解决方案：URL重写
response. encodeRedirectURL(java.lang.String url) 
用于对sendRedirect方法后的url地址进行重写。
response. encodeURL(java.lang.String url)
用于对表单action和超链接的url地址进行重写 
附加：
Session的失效  invalidate()立刻实效
Web.xml文件配置session失效时间





# Web组件

在EE规范中，sun公司一共给我们提供了三种类型的组件

Servlet：开发动态web资源

Listener：监听器，监听对象、监听某个事件

Filter：过滤器，过滤、拦截



## Listener

监听器。

现实生活中的监听器



被监听对象：明星艺人

监听者：朝阳人民群众

监听事件：吸毒嫖娼

触发事件：报警



web中的监听器

被监听对象：比如ServletContext对象

监听者：自己去编写了一个监听器

监听事件：ServletContext对象的创建和销毁

触发事件：触发自己写的监听器里面对应的方法



### 使用（掌握）

1.编写一个类实现ServletContextListener接口

2.声明注册该listener（注解、web.xml）

```java
@WebListener
public class MyServletContextListener implements ServletContextListener {

    //该方法会在servletContext对象被创建的时候调用
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        System.out.println("context init");
    }

    //该方法会在servletContext对象被销毁的时候调用
    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {

    }
}
```



使用场景：

最开始出现的时候，只有servlet组件，没有listener，listener是后来加入的

购物车案例，初始化商品的逻辑----写在index servlet里面的，分析的略微有一些不太合适的

全局性的设置，为什么要设置在某一个servlet中呢？

为何不放在一个更加全局性的设置里面呢？完全可以将全局性的初始化设置放在listener里面来做



可以将之前放置在某个serlvet中的初始化设置，挪动到listener里面来做

比如查询数据库，拿到一些配置文件

读取配置文件里面的参数

这些都可以在listener里面来做



**学习Spring阶段，Spring初始化就会用到该listener**





原理（**不要求掌握**）

tomcat的代码是90年代的代码

你写的代码是21年的代码

90年的代码是如何调用21年的代码的呢？





爸爸妈妈带宝宝，宝宝哭，爸爸打宝宝，妈妈打爸爸

假设工作日 爸爸妈妈需要上班，爷爷奶奶来带

宝宝再哭，爷爷抱宝宝，奶奶做吃的



如果需求发生变更的时候，需要频繁的去变更现有的代码逻辑，这个代码其实是有很大问题的

代码如何设计？能够让需求变更的时候，最好不要修改代码

![image-20210512111041675](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210512111041675.png)

```java
class ServletContext{
    List<ServletContextListener> listeners = new Arrayist;
    
    void addListener(ServletContextListener listner){
        listeners.add(listener);
    }
    
    void init(){
       //servletContext init 
        listeners.for----{listener.contextInitialized()}
    }
    
    void destroy(){
        // servletCOntext destroy
        listeners.for------{listener.contextDstroyed()}
    }
    
}
```



## Filter

sun公司制定的三大web组件之一。

过滤、拦截



过滤器位于客户端和Servlert之间

可以检查修改request对象

也可以检查修改response对象

打游戏   我*****你

### 简介

过滤器是Servlet规范的高级特性。
过滤器（Filter）技术是从Servlet2.3规范开始引入的。过滤器是一种Web应用程序组件，可以部署在Web应用程序中。
过滤器由Servlet容器调用，用来拦截以及处理请求和响应。过滤器本身并不能生成请求和响应对象，但是可以对请求和响应对象进行检查和修改。



 过滤器的工作原理

过滤器介于客户端与Servlet/JSP等相关的资源之间，对于与过滤器关联的Servlet来说，过滤器可以在Servlet被调用之前检查并且修改request对象。在Servlet调用之后检查并修改response对象。 

![image-20210512195126946](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210512195126946.png)

以上过程可分为以下步骤：
1．客户端将请求发送给Web容器；
2．Web容器根据客户端发送的请求生成请求对象request和响应对象response。
3．Web容器在调用与过滤器相关联的Web组件（例如Servlet/JSP）之前，先将request对象以及response对象发送给过滤器。
4．过滤器对request对象进行必要的处理；
5．过滤器把处理过的request对象以及response对象传递给Web组件；
6．Web组件调用完成后，再次通过过滤器，此时过滤器对response对象进行必要的处理；
7．过滤器把处理过的response对象传递给Web容器；
8．Web容器将响应的结果返回到客户端，并在浏览器上显示。



过滤器 

Servlet规范中使用javax.servlet.Filter接口支持过滤器的使用
创建过滤器必须实现Filter接口，该接口中定义了三个方法

- void init(FilterConfig config)：

  用于初始化过滤器，当容器装载并初始化过滤器时调用。Web容器为此方法传递一个FilterConfig对象，FilterConfig对象可以获取web.xml文件中过滤器初始化参数的配置；利用FilterConfig对象也可以获取当前Filter的名称以及相关联的ServletContext对象。

- void doFilter(ServletRequst request, ServletResponse response, FilterChain chain)：
  此方法是Filter接口的核心方法，用于对请求对象和响应对象进行检查和处理。此方法包括三个输入参数。其中，ServletRequest对象为请求对象，包括表单数据、Cookie以及HTTP请求头等信息；ServletResponse对象为响应对象，用于响应使用ServletRequest对象访问的信息；FilterChain用来调用过滤器链中的下一个资源，即将ServletRequest对象以及ServletResponse对象传递给下一个过滤器或者是其它的Servlet/JSP等资源。

- void destroy( )：
  此方法用于销毁过滤器，当容器要销毁过滤器实例时调用此方法，Servlet过滤器占用的资源会被释放。



过滤器的使用步骤

创建过滤器的步骤如下：
1．创建一个实现Filter接口的Java类；
2．实现init( )方法，如有必要，读取过滤器的初始化参数；
3．实现doFilter( )方法，完成对ServletRequest对象以及ServletResponse对象的检查和处理；
4．在doFilter( )方法中调用FilterChain接口对象chain的doFilter( )方法，以便将过滤器传递给后续的过滤器或资源。
5．在web.xml中注册过滤器，设置参数以及过滤器要过滤的资源。 



Filter Chain

FilterChain 是 servlet 容器为开发人员提供的对象，它提供了对某一资源的已过滤请求调用链的视图。过滤器使用 FilterChain 调用链中的下一个过滤器，直到最后一个过滤器。



Filter链介绍

多个Filter对同一个资源进行了拦截，那么当我们在开始的Filter中执行chain.doFilter(request,response)时，是访问下一下Filter,直到最后一个Filter执行时，它后面没有了Filter,才会访问web资源。
关于多个FIlter的访问顺序问题
如果有多个Filter形成了Filter链，那么它们的执行顺序是怎样确定的？
它们的执行顺序取决于`<filter-mapping>在web.xml`文件中配置的先后顺序
注解配置，按照类名的ASCII码表顺序



Filter生命周期

当服务器启动，会创建Filter对象，并调用init方法，只调用一次.
当访问资源时，路径与Filter的拦截路径匹配，会执行Filter中的doFilter方法，这个方法是真正拦截操作的方法.
当服务器关闭时，会调用Filter的destroy方法来进行销毁操作.



Filter配置详解

```xml
Filter基本配置介绍
<filter>
    <filter-name>filter名称</filter-name>
    <filter-class>filter类全名</filter-class>
</filter>
<filter-mapping>
    <filter-name>filter名称</filter-name>
    <url-pattern>映射路径</url-pattern>
</filter-mapping>
```

关于url-pattern配置
1.完全匹配 
	要求必须以"/"开始.
2.目录匹配
	要求必须以"/"开始，以`*`结束.			
3.扩展名匹配
	不能以“/”开始，以`*.xxx`结束. （或`*`）

关于servlet-name配置
	针对于servlet拦截的配置  `<servlet-name>`配置
	在Filter中它的配置项上有一个标签
	`<servlet-name>`它用于设置当前Filter拦截哪一个servlet
是通过servlet的name来确定的。



### Filter编写

1.编写一个类，实现Filter接口

2.声明该filter

```java
public class FirstFilter implements Filter {

    //init直接会随着应用的启动而加载
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init");
    }

    //类比为servle的service方法，每访问一次filter，那么就会执行一次
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("filter doFilter");
    }

    //应用被卸载、服务器被关闭 
    @Override
    public void destroy() {
        System.out.println("destroy");
    }
}
```

### filter功能（过滤、拦截）

**对于与过滤器关联的Servlet来说**，过滤器可以在Servlet被调用之前检查并且修改request对象。在Servlet调用之后检查并修改response对象。 



**如何将filter和servlet关联在一起呢**？

最简单的方式就是将servlet的url-pattern赋值给filter即可（注意事项：我们之前说servlet之间不可以设置相同的url-pattern，但是**filter可以设置和servlet相同的url-pattern**）



但是修改过后，filter可以被执行，servlet缺又执行不到了？？？？？

原因在于filter默认情况下执行的是拦截操作，如果希望filter执行放行，那么需要如下代码

```java
//这行代码对于放行至关重要，如果没有这行代码，那么执行的就是拦截操作
filterChain.doFilter(servletRequest, servletResponse);
```



既然filter可以检查修改request对象，那么可不可以将这些代码放在filter里面呢？





**filter可不可以设置/*呢？**

一个filter对应多个servlet-----------将编码格式的代码放到filter里面来做



filter完全可以设置/*     可以将编码格式的代码写在filter里面



**多个filter可以设置相同的url-pattern吗？**

完全可以设置相同的url-pattern



**多个filter的掉用先后顺序是什么样的呢？**

如果是web.xml，按照filter-mapping声明的先后顺序来

如果是注解，那么按照类名首字母的ASCII先后顺序来

多个filter调用时，和url-pattern的优先级没有关系，只要能够处理该请求，那么他们都是相同地位的，至于谁先调用，那么就看上面的规则



### 请求处理流程（最终的整合效果）

![请求执行流程](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\请求执行流程-1620819761105.png)

以访问Http://localhost:8080/app/servlet1为例

1.浏览器生成HTTP请求报文，传输到目标服务器主机，被监听8080端口号的Connector程序接收到

2.将请求报文解析成为Request对象， 同时生成一个response对象，传给engine

3.engine将这两个对象进一步传给host，host挑选一个叫做app的context

4.这个时候有效的请求路径为/servlet1,首先判断有没有合适的Filter可以处理该请求，如果有的话，则将其加入到一个链表内；如果有多个，那么按照调用的先后顺序加入到链表中

5.再次去判断有没有合适的servlet可以处理该请求，如果有的话，则将该servlet也加入到该链表内；如果没有，则调用缺省servlet

6.形成链表之后，依次调用链表的每个组件，调用filter的doFilter方法，servlet的service方法，同时将request、response作为参数传递进去，往response里面写入数据

7.Connector读取response里面的数据，生成HTTP响应报文，传输给客户端。



### filter的doFilter方法被执行了两次

但是为什么里面的日志只打印了一个呢？

​	程序代码执行的时候，执行到filterChain.doFilter方法的时候，会进入到下一个组件执行，全部执行完毕之后，再次原路返回。

递归的方式进入，然后再回来。



**去的过程只执行filterChain上面的代码**

**回的过程只执行filterChain下面的代码**

![image-20210512142546890](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210512142546890.png)



### 案例

1.设置编码格式



2.如果希望filter帮我们进行页面的拦截，比如未登录的情况下，访问京东的我的页面

拦截。

filter涉及到拦截   filterChain.doFilter  这个方法决定了是否拦截还是放行现有的请求



我的页面逻辑：

没有登录的情况下拦截

登录的情况下放行



如何判断是否登录呢？

session联系在一起



但是这里面有一个点，就是不是所有的页面都拦截；登录页面是不拦截的。



首先判断页面是否需要验证登录状态，如果不需要（比如登录页面），那么直接放行

如果需要验证登录状态（访问我的页面），验证是否登录，如果登录，直接放行；如果没有登录，拦截

建议大家在学习的时候，自己去画流程图

![image-20210512144057514](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-web-notes.assets\image-20210512144057514.png)





# MVC

## JSON

json就是指的是js里面的对象。

**json里面如果是一个对象，那么对应的是一个{}**

**如果是一个数组或者集合，那么对应的是一个[]**

[{},{}]

json目前主流的应用场景就是作为数据交换的格式

比如前后端分离的项目

前端传输数据给服务器，可以以json的格式来进行传输，当然key=value也可以，有局限性，中国下面的省份，省份里面又包含了城市信息，key=value无法描述出其中的关系



```js
var country = {name:"中国", province:[{name:"黑龙江",cities:["哈尔滨","大庆"]},
                                        {name:"广东",cities:["广州","深圳"]},
                                        {name:"台湾",cities:["台北","高雄"]}]}
```



 var country ={
            "name":"中国",
             "province":[{"name":"黑龙江",”cities”:["哈尔滨","大庆"]},
		          {"name":"广东","cities":["广州","深圳","珠海"]},
		          {"name":"台湾","cities":["台北","高雄"]},
		          {"name":"新疆","cities":["乌鲁木齐"]}
                                   ]
                   }



上面和下面的区别在于下面的属性值上面加了引号。

上下两者的关系你可以理解为java对象和java对象的toString的关系

一个json，一个是json字符串。

实际上我们在进行数据的传输过程中，都是以json字符串的形式来进行传输的



### 将java对象转成json字符串

比如需要将商品的信息传输给前端，可以以json字符串的形式返回。





### 将json字符串转成java对象

前端页面登录，然后将提交的数据以json字符串的形式提交给服务端，解析出里面的数据，封装成为java对象，保存到数据库等。

```
//jackson解析不了null "" 和 非jason格式字符串，均会抛出异常
ObjectMapper objectMapper = new ObjectMapper();
        String a = null or "" or "abc";
        Type type = objectMapper.readValue(a, Type.class);
        System.out.println(type);
```



```java
package com.cskaoyan.json;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.type.CollectionType;
import com.fasterxml.jackson.databind.type.TypeFactory;
import org.junit.Test;

import java.util.ArrayList;
import java.util.List;

/**
 * junit单元测试用例
 * 1.导包junit的包 junit 4.12
 * 2.编写一个类，类名称一般叫做XXXTest
 * 3.里面编写对应的方法，方法的名称一般叫做testXxxxx
 * 4.方法的权限public，返回值void，方法的参数为无参
 */
public class JsonTest {

    //在java语言里面操纵json字符串，一般会用如下三个jar包
    //1.jackson  2.gson 3. fastjson
    @Test
    public void testJavaObjectToJson(){
        User user = new User();
        user.setUsername("admin");
        user.setPassword("admin123");
        // user-----json字符串  {"username":"admin","password":"admin123"}
        // bejson.com
        String jsonStr = "{\"username\":\"" + user.getUsername() + "\",\"password\":\"" + user.getPassword() + "\"}";
        System.out.println(jsonStr);
    }

    @Test
    public void testJavaObjectToJson2() throws JsonProcessingException {
        User user = new User();
        user.setUsername("admin");
        user.setPassword("admin123");

        User user2 = new User();
        user2.setUsername("admin");
        user2.setPassword("admin123");

        //使用的入口类ObjectMapper
        List<User> users = new ArrayList<>();
        users.add(user);
        users.add(user2);
        ObjectMapper objectMapper = new ObjectMapper();
        String content = objectMapper.writeValueAsString(user);
        String usersString = objectMapper.writeValueAsString(users);
        System.out.println(content);
        System.out.println(usersString);
    }

    @Test
    public void testJsonToObject() throws JsonProcessingException {
        String content = "{\"username\":\"admin\",\"password\":\"admin123\"}";
        ObjectMapper objectMapper = new ObjectMapper();
        //如何做到的呢？利用反射
        User user = objectMapper.readValue(content, User.class);
        System.out.println(user);
    }

    @Test
    public void testJsonToObject2() throws JsonProcessingException {
        String content = "[{\"username\":\"admin\",\"password\":\"admin123\"},{\"username\":\"admin\",\"password\":\"admin123\"}]";
        ObjectMapper objectMapper = new ObjectMapper();
        TypeFactory typeFactory = objectMapper.getTypeFactory();
        CollectionType type = typeFactory.constructCollectionType(List.class, User.class);
        List<User> users = objectMapper.readValue(content, type);
        for (User user : users) {
            System.out.println(user);
        }
    }

}
```



## MVC

### 从注册登录案例引出MVC

要求：先使用json文件来存储用户的数据，后面需求变更，使用数据库来存储用户的数据。整体业务逻辑不做改变。

```java
package com.cskaoyan.user;

import com.cskaoyan.user.utils.StringUtils;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.type.CollectionType;
import com.fasterxml.jackson.databind.type.TypeFactory;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.lang.reflect.InvocationTargetException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

@WebServlet("/user/*")
public class UserServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //如何区分是register还是login呢？
        String requestURI = request.getRequestURI();
        String replace = requestURI.replace(request.getContextPath() + "/user/", "");
        if("register".equals(replace)){
            register(request, response);
        }else if("login".equals(replace)){
            login(request, response);
        }
    }

    //登录的业务逻辑
    private void login(HttpServletRequest request, HttpServletResponse response) {

    }

    //注册的业务逻辑
    private void register(HttpServletRequest request, HttpServletResponse response) throws IOException {
        //1.首先应该获取请求参数 做一些常规的校验
        Map<String, String[]> map = request.getParameterMap();
        User register = new User();
        try {
            BeanUtils.populate(register, map);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        //常规的校验，判空  密码的确认密码是否相同
        if(StringUtils.isEmpty(register.getUsername()) || StringUtils.isEmpty(register.getPassword()) || StringUtils.isEmpty(register.getConfirmPass())){
            response.getWriter().println("参数不能为空");
            return;
        }
        if(!register.getPassword().equals(register.getConfirmPass())){
            response.getWriter().println("两次密码不一致");
            return;
        }
        //逻辑：取出users.json文件里面的数据，然后解析成为java对象，通过比较username是否相同， 是否已经被注册
        String realPath = getServletContext().getRealPath("WEB-INF/users.json");
        File file = new File(realPath);
        FileInputStream inputStream = new FileInputStream(file);
        //如何以字符的形式显示文件里面的内容？
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        int length = 0;
        byte[] bytes = new byte[1024];
        while ((length = inputStream.read(bytes)) != -1){
            outputStream.write(bytes, 0, length);
        }
        String content = outputStream.toString("utf-8");
        List<User> users = new ArrayList<>();
        ObjectMapper objectMapper = new ObjectMapper();
        if(!StringUtils.isEmpty(content)){
            //不为空，表示里面有数据，才去解析
            TypeFactory typeFactory = objectMapper.getTypeFactory();
            CollectionType type = typeFactory.constructCollectionType(List.class, User.class);
            users = objectMapper.readValue(content, type);
            for (User u : users) {
                if(register.getUsername().equals(u.getUsername())){
                    //用户名已经被占用
                    response.getWriter().println("当前用户名已经被注册");
                    return;
                }
            }
        }
        //为空 直接注册
        users.add(register);
        //最后需要把users数据写回users.json文件中
        FileWriter writer = new FileWriter(file);
        writer.write(objectMapper.writeValueAsString(users));
        writer.flush();
        writer.close();
        response.getWriter().println("注册成功，跳转至登录页面");
        //2.存放到json文件里：判断当前应用中是否存在你输入的用户名；如果不存在，则直接注册，如果存在，则需要更换
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```

接下来，需要将使用json文件 来存储用户相关数据改为使用数据库，分析哪些代码需要变化？



需求变更的时候，如果需要频繁的去变更现有的代码逻辑，这个逻辑写的是有问题的



### MVC概念

MVC设计模式。模式其实经过实践总结出的一套比较好的编码的习惯

MVC根据概念，其实是需要将我们的程序代码进行一个分离解耦。分为几个模块

Model：数据模型（用户），基于对这些数据模型的操作，比如对于**用户的操作**，注册、登录等，这些其实都是属于模型的部分

view：显示、视图。比如html、jsp。response输出的内容

controller：控制器。一般请求分发到controller中，然后controller会调用模型的一个或者多个方法，比如调用用户的注册相关代码，返回返回一个结果，根据这个结果，进行进一步调用不同的视图。作用就是用来将model模型和视图view进行解耦合。



```java
package com.cskaoyan.user;

import com.cskaoyan.user.model.User;
import com.cskaoyan.user.model.UserJsonModel;
import com.cskaoyan.user.model.UserSqlModel;
import com.cskaoyan.user.utils.StringUtils;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.type.CollectionType;
import com.fasterxml.jackson.databind.type.TypeFactory;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.lang.reflect.InvocationTargetException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

@WebServlet("/user/*")
public class UserServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //如何区分是register还是login呢？
        String requestURI = request.getRequestURI();
        String replace = requestURI.replace(request.getContextPath() + "/user/", "");
        if("register".equals(replace)){
            register(request, response);
        }else if("login".equals(replace)){
            login(request, response);
        }
    }

    //登录的业务逻辑
    private void login(HttpServletRequest request, HttpServletResponse response) {

    }

    //注册的业务逻辑
    private void register(HttpServletRequest request, HttpServletResponse response) throws IOException {
        //1.首先应该获取请求参数 做一些常规的校验
        Map<String, String[]> map = request.getParameterMap();
        User register = new User();
        try {
            BeanUtils.populate(register, map);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        //常规的校验，判空  密码的确认密码是否相同
        if(StringUtils.isEmpty(register.getUsername()) || StringUtils.isEmpty(register.getPassword()) || StringUtils.isEmpty(register.getConfirmPass())){
            response.getWriter().println("参数不能为空");
            return;
        }
        if(!register.getPassword().equals(register.getConfirmPass())){
            response.getWriter().println("两次密码不一致");
            return;
        }
        boolean result = UserSqlModel.register(register, request);
        if(!result){
            response.getWriter().println("当前用户名已经被注册");
            return;
        }
        response.getWriter().println("注册成功，跳转至登录页面");
        //2.存放到json文件里：判断当前应用中是否存在你输入的用户名；如果不存在，则直接注册，如果存在，则需要更换
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
}
```



总结一下：

如果希望今后需求变更的时候代码改动尽可能少，你应当如何去设计呢？

1.方法的名称相同

2.参数也相同

3.返回值也相同

遵循这三点，其实只需要变更UserJsonModel------->UserSqlModel即可，其他全部的代码都不用变



这三点像不像规范？

接口。interface。天然就具有上述三个点。

上面其实只是一个建议，建议如果写成这样，那么你的更改将尽可能少

为何不设计成一个接口，设计成接口以后，就可以强制性约束必须这么写



将在MVC设计模式的基础上进行进一步的优化。引入三层架构

### 三层架构

MVC设计模式和三层架构不是一回事。

MVC设计模式说的是需要将程序的代码进行分层、分模块。分成controller、view、model，这样就已经符合MVC设计模式了

但是我们发现其实还是有可以进一步优化的空间的

```java
boolean result = UserSqlModel.register(register, request);
if(!result){
    response.getWriter().println("当前用户名已经被注册");
    return;
}
```

这部分完全可以把它写成接口和实现类的方式。

三层架构可以认为是在MVC设计模式的基础上，进行进一步的分离解耦，将我们的程序代码进行进一步的细分。

将model的代码比如UserJsonModel和UserSqlModel进行进一步的细分。

首先先将model改一个名字，其实都是和数据打交道的，一般称之为DAO  Data Access Object

将之前的UserJsonModel和UserSqlModel改名改成UserJsonDao和UserSqlDao，同时引入接口的概念



访问的时候通过

```java
UserDao userDao = new UserJsonDao();
userDao.register();
```

如果今后再进行登录

```java
UserDao userDao = new UserJsonDao();
userDao.register();
```

变成如上代码

UserDao userDao = new UserJsonDao();

这行代码写了多份，如果今后发生需求的变更，那么从json变化到sql，每个地方都要变

为了改动尽可能少，统一写成成员变量

![image-20210513165002397](C:\Users\AnoxiC2010\Desktop\wdJava30th\EE\Day8 MVC\note\MVC.assets\image-20210513165002397.png)



#### 展示层

展示层你可以理解为就是之前的controller + view，此时充当了展示层的功能

#### 数据层 DAO

此时是将之前的model中关于模型的操作的部分，抽提成了dao  数据层

#### 业务层 Service

现阶段的逻辑：

servlet也就是controller中，我们调用dao的方法

假设一个场景：

某一个功能点：



需要使用

dao.method1()

dao.method2()

dao.method3()

搭配一起使用，可以完成该功能



后面发现有另外一种实现方式，但是不知道新的实现方式性能如何，做一个性能测试

新的方式

dao.method1()

dao.method2()

dao.method4()

实现该功能





```java
private void method(){
        userDao.mehtod1();
        userDao.method2();
        userDao.method3();
        //测试新的实现方式性能
//        userDao.method1();
//        userDao.method2();
//        userDao.method4();
    }
```





#### 为什么要有service层

其实主要是负责和当前的功能具体相关的业务场景逻辑，绝大多数的业务相关代码，应当封装在当前方法中。



最终效果：

通过扩增现有的代码逻辑来修改我们的业务功能而不是通过修改现有的代码逻辑

这个就是一个比较好的实现方式





展示层调用业务层，业务层调用数据层





controller：获取请求参数、参数的校验，调用service的代码，根据结果，调用视图。4s门店

service：和当前业务逻辑息息相关的代码都应该写在service中，逻辑运算、处理，service发起一个一个的dao方法调用，然后再service进行组装。汽车工厂，向各个汽车零部件工厂发起订单，零部件进行组装形成一个整车

dao：单纯的一个一个的sql语句，查询、修改、新增等。零部件工厂





## 关于路径的一个补充

如果我想获取EE项目的一个文件的路径，除了可以使用context来获取之外还可以使用另外一种方式来获取

利用类加载器来获取



编写的java文件，编译形成class文件，class文件存储在本地硬盘上面的

运行时候可以找到该class，说明了啥？

硬盘上面的class文件被加载到了内存中

这个过程称之为磁盘IO，IO代码

类被加载到内存中是由来加载器来完成的

既然类加载器可以将本地硬盘上面的class文件加载到内存中，那么可不可以将本地硬盘上面的其他的文件加载到内存中呢？

肯定也是可以的



我们可以使用类加载器来将我们需要加载的文件绝对路径拿到



```java
@WebServlet("/test")
public class TestServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //拿到类加载器
        ClassLoader classLoader = TestServlet.class.getClassLoader();
        //输入一个相对于classpath的相对路径，返回一个绝对路径
        // classpath一般情况下就是和你的全类目的类最外层package的父目录
        //文件存放：就是和你们的全类目的最外层package保持平级，比如com目录平级
        //有时候，我只想拿到path
        String path = classLoader.getResource("application.properties").getPath();
        System.out.println(path);
        InputStream inputStream = classLoader.getResourceAsStream("application.properties");
        Properties properties = new Properties();
        properties.load(inputStream);
        String username = properties.getProperty("username");
        System.out.println(username);
    }
}
```





# MVN+IDEA 整合EE项目

New Project -> Mvn -> project ifno and finish
main包下新建webapp包
Projcet Structure -> Facets ->  + web and choose module -> check Deployment Descriptors path and Web Resource Directories path, filanny apply 
pom.xml -> `<packaging>war</packaging>` -> then Artifacts will be automatically set with war and war exploded (部署用这个展开形式即可)， 查看output layout没有lib文件夹，没关系，加入maven依赖会自动被添加到WEB-INF下的lib，编译时其实不需要引入jar包到项目，只需要把jar包的路径引入到classpath，运行时Artifacts会把依赖的jar包复制到lib中 

# 项目一

## Day1 项目启动

### 项目概述

http://115.29.141.32:8085/admin.html

http://115.29.141.32:8085



项目一是一个商城。可以分为两部分，一部分后台管理系统，一部分是前台用户系统。

前台用户系统指的就是用户购物的一个系统，比如接触的京东、淘宝等这些前台用户系统

后台管理系统指的是商城工作人员使用的一个系统，主要去维护前台用户系统里面的相关数据，比如用户数据，用户忘了密码；商品数据，发布一个新品；订单系统，物流信息；留言、订单的评价

### 项目架构-前后端分离

究竟什么是前后端分离呢？

页面里面的内容，其实可以分为两个部分，一个是单纯的页面，一个是数据

页面是来自于8085系统

数据来自于8084系统

页面和数据不在同一个系统内，一个负责前端页面，一个负责对应的数据

前后端分离

![image-20210514095636570](C:/Users/AnoxiC2010/Desktop/wdJava30th/EE/Day9 项目启动/项目一.assets/image-20210514095636570.png)

你今后的工作其实主要是给前端开发人员提供对应的数据



### 接口文档

人员组成。

前端开发人员    服务端开发人员

前端开发人员主要负责将psd图片转成html页面，加上css样式，加上js等

数据来源于java等语言的服务端开发人员来提供

开发人员之间要进行对接、交互。全部都以口头的形式进行对接，效率非常低。并且有时候还会出现一些模棱两可的事情

比如前端开发人员说了一句，这个请求应该给我返回什么样的数据格式，你的确也是以这种格式返回的；但是前端忘了，解析取数据的时候不是按照这个格式来进行读取的

最好再对接的时候以纸质的形式记录下来，对照着文档来操作。

文档一般称之为**接口**文档。

这里面的接口和我们java语言里面学习的interface不是一个概念，但是从功能上去分析的话，其实很类似。

interface： Animal animal = new Pig();

​					animal.run();

相当于一个黑盒一样，只需要去调用animal.run()，接下来就会给我返回对应的数据，我并不需要去关心究竟是如何实现的



请求的响应过程：

发起请求，需要有一个请求的地址，请求的方法，请求的参数，遵循着这个要求，往这个地址去发起一个请求，那么就会给我返回对应的结果

一般情况下，我们将请求的处理响应过程也称之为一个接口

接口一般会涉及：请求的资源路径、请求的方法、请求的参数，返回对应的数据格式

我们需要将这些格式以文档的形式约束下来，请求应该怎么发，响应的数据应该如何去响应，字段应该叫什么字段。都应该写的清清楚楚。

建议大家一定要去写。

### 项目如何进行

在企业中，项目的进行是由前后端开发人员进行交互、协商一起去进行。

但是在项目一，不可能给大家配置一个专门的前端开发人员来进行对接，我们采用的方式是先在公网服务器上面预先搭建好了一套已经实现好了的代码，大家只需要根据这个公网服务器上面的要求来即可。

本地的前端代码和公网服务器上面的前端代码基本是一模一样的，请求的发送格式、响应的格式，

你可以认为项目是前端开发人员已经写好了一个接口文档，然后大家需要去根据这个要求去加以实现即可。



更通俗的去说，

通过去抓取公网服务器上面的请求报文和响应报文

公网服务器上面的请求报文和本地的前端发送的请求报文应该是一致的

公网服务器上面要求的返回数据类型其实和本地也是一样的

**就是去抓取公网服务器上面的响应报文，然后本地加以复现。你的最终响应报文要和公网服务器一致就可以了。**

### 功能分析

发给大家的前端代码是一个压缩包，解压缩，

1.需要在package.json所在的目录执行cnpm install 或者npm install

但是npm的服务器在国外，可能网速比较慢，直接npm修改源即可。

npm config set registry https://registry.npm.taobao.org

通过这种方式设置，和设置cnpm基本等效

2.执行npm run dev

分析代码，主要关注src目录

叫做api目录-----其实就是一个一个的接口

admin.js

​	const res = axios.post('**/api/admin/admin/login**',data);

其实axios是一个发送HTTP请求的工具，会以post请求方式往这个地方去发送一个请求，拿到对应的结果



其中admin.js是后台管理系统的所有接口（先从后台管理系统开始）

client.js是前台用户系统所有的接口

/api/admin/admin/login里面的路径是不包含主机端口号的，那么发送的主机端口号在哪个配置的呢？

在src/config目录下

axios-admin.js

axios-client.js

axios.defaults.baseURL = 'http://localhost:8084';



3.新建Maven的EE项目，监听8084端口号，启动

通过页面点击登录按钮

![image-20210514111437234](C:/Users/AnoxiC2010/Desktop/wdJava30th/EE/Day9 项目启动/项目一.assets/image-20210514111437234.png)

#### 问题：跨域

什么叫跨域？

访问页面的时候请求的地址Http://localhost:8085,但是紧接着发送了一个到Http://localhost:8084的请求

8085和8084虽然主机名相同，但是端口号不同，这个时候也是跨域

默认情况下浏览器是不允许跨域的，如果希望能够跨域，需要服务器的相关支持，需要服务器返回几个响应头

响应头类似于一个通行凭证，今后每次去访问都需要有这个通行凭证才可以正常访问。

在跨域的时候，浏览器会发送两个请求，一个是OPTIONS请求，一个是真正的请求

OPTIONS你可以认为是一个试探性请求，服务器是否支持跨域，如果支持，给他返回了对应的响应头， 那么就可以正常发送真正的请求；如果没有返回对应的响应头，那么则表示不允许跨域，则请求发送失败

#### 登录逻辑分析

本质还是去抓包。

去公网服务器上面去抓请求报文（本地抓请求报文也可以），以及响应报文

然后想法设法和它返回的一致即可。



登录的请求报文：

POST http://localhost:8084/api/admin/admin/login HTTP/1.1
Host: localhost:8084
Connection: keep-alive
Content-Length: 31
sec-ch-ua: "Google Chrome";v="89", "Chromium";v="89", ";Not A Brand";v="99"
Accept: application/json, text/plain, */*
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36
Content-Type: application/json;charset=UTF-8
Origin: http://localhost:8085
Sec-Fetch-Site: same-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost:8085/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9

**{"email":"admin","pwd":"admin"}**

着重关注请求体，因为我们的请求参数是在请求体中

之前我们在给大家演示的时候，都是通过form表单，参数都是以key=value形式进行提交的，此时不再是了，而是json字符串

request.getParameter无法获取到了。

登录的响应报文：（登录成功）

HTTP/1.1 200
Vary: Origin
Vary: Access-Control-Request-Method
Vary: Access-Control-Request-Headers
Access-Control-Allow-Origin: *
Content-Type: application/json;charset=UTF-8
Date: Fri, 14 May 2021 03:27:59 GMT
Content-Length: 50

**{"code":0,"data":{"token":"admin","name":"admin"}}**

登录失败的响应报文

HTTP/1.1 200
Vary: Origin
Vary: Access-Control-Request-Method
Vary: Access-Control-Request-Headers
Access-Control-Allow-Origin: *
Content-Type: application/json;charset=UTF-8
Date: Fri, 14 May 2021 03:31:25 GMT
Content-Length: 43

**{"code":10000,"message":"密码不正确!"}**



**你需要做的事情就是本地的响应报文和公网上面的一致即可**。



### BO、VO、POJO

数据库里面的表----对应的应该有一个java对象  POJO  plain old java object

请求发送过来的数据，数据和数据库里面的字段有关联吗？请求发送过来的字段可以是任意的

比如请求发送过来的是email和pwd，而我的数据库里面的表字段是username和password，完全合理的

新建一个新的对象用来接收传递过来的请求参数的封装，一般称之为BO business object



数据库里面的字段的信息，要显示给客户端，发送出去，数据库里面的字段信息可能叫做username、password

但是页面需要的字段可能是email和pwd，这也是非常合理的

需要将POJO里面的字段信息转成VO  view Object  显示的



关于路径：

**如果要用classLoader去获取文件的路径、流，需要将文件放置再classpath相关路径下**



使用servletContext来获取，路径的写法相对来说比较容易写，需要引入servletContext对象





前两天进度应该是不快的

管理员、用户

遇到问题一定要自己先去思考（自己不思考，一个是自己死钻牛角尖，30min~1h没有头绪）

勤总结，bug--分析原因



