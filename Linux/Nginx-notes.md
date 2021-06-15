# 1 Nginx简介    



## 1.1 什什么是Nginx   

Nginx (engine x) 是⼀个⾼性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。Nginx是由伊⼽尔·赛索耶夫为俄罗斯访问量第⼆的Rambler.ru站点（俄⽂：Рамблер）开发的，第 ⼀个公开版本0.1.0发布于2004年10⽉4⽇。

其将源代码以类BSD许可证的形式发布，因它的稳定性、丰富的功能集、示例配置⽂件和低系统资源的 消耗⽽闻名。2011年6⽉1⽇，nginx 1.0.4发布。

Nginx是⼀款轻量级的Web 服务器/反向代理服务器及电⼦邮件（IMAP/POP3）代理服务器，在BSD-like 协议下发⾏。其特点是占有内存少，并发能⼒强，事实上nginx的并发能⼒在同类型的⽹⻚服务器 中表现较好，中国⼤陆使⽤nginx⽹站⽤户有：百度、京东、新浪、⽹易、腾讯、淘宝等。

## 1.2 Nginx的核⼼心功能              

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Nginx-notes.assets\clip_image001.gif)

Nginx最核⼼心的两个功能：

1. ⾼高性能的静态web服务器器（io效率高 零拷贝）

2. 反向代理理

正向代理理*vs*反向代理理

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Nginx-notes.assets\clip_image005.gif) 

 如上图，上边是正向代理，下边是反向代理
正向代理：代理服务器是代表⽤户客户端去访问后端服务器，代理的对象是前⾯的⽤户
反向代理：代理服务器是代表后端服务器供客户端去访问，对于前⾯的⽤户来说是⽆感知的，代理的对象是后⾯的后台服务器



## 1.3 Nginx的优势    

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Nginx-notes.assets\clip_image007.gif)

1. ⾼高并发、⾼高性能（⼀一个进程可以处理理多个请求）

2. 扩展性好（模块化设计）

3. 异步⾮非阻塞的事件驱动模型

4. ⾼高可靠性（热部署、7*24）



# 2 Ngnix的使用             

## 2.1 Nginx的安装    

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Nginx-notes.assets\clip_image002.gif)

Ubuntu安装Nginx ⾮常⽅便，可以直接使⽤apt源来安装

```bash
sudo add-apt-repository ppa:nginx/stable
sudo apt-get update
sudo apt-get install nginx
```

执⾏以上三条指令即可完成安装
执⾏命令查看是否安装成功

```bash
nginx -v
```

```bash
ll /etc/nginx
```

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Nginx-notes.assets\clip_image012.gif)

如上所示，主配置⽂件是nginx.conf，Nginx的指令放在/usr/sbin/nginx，⽇志⽂件放在 /var/log/nginx 中

## 2.2 Nginx常⽤用命令                

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Nginx-notes.assets\clip_image007.gif)

找到Nginx执⾏行行命令，以Ubuntu18.04为例例

```bash
cd /usr/sbin
#启动命令
./nginx
#关闭命令
./nginx -s stop
#重启命令
./nginx -s reload
```



## 2.3 Nginx配置⽂文件                

核⼼心配置⽂文件就是nginx.conf，打开这个核⼼心配置⽂文件

配置⽂文件中有很多#， 开头的表示注释内容，我们去掉所有以 # 开头的段落，精简之后的内容如下：

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Nginx-notes.assets\clip_image016.jpg)

根据上述⽂件，我们可以很明显的将 nginx.conf 配置⽂件分为三部分：

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Nginx-notes.assets\clip_image018.jpg)

### 2.3.1 全局配置              

从配置⽂件开始到 events 块之间的内容，主要会设置⼀些影响 nginx 服务器整体运⾏的配置指令，主要包括配置运⾏ Nginx 服务器的⽤户（组）、允许⽣成的 worker process 数，进程 PID 存放路径、⽇志存放路径和类型以及配置⽂件的引⼊等。
⽐如上⾯第⼀⾏配置的：

```shell
user www-data; #运⾏worker⼦进程的⽤户
worker_processes auto; #⼦进程的个数
pid /run/nginx.pid; #运⾏master的pid⽂件存放的路径
include /etc/nginx/modules-enabled/*.conf; #将其他配置⽂件包含进来
```

这是 Nginx 服务器并发处理服务的关键配置，worker_processes 值越⼤，可以⽀持的并发处理量也越多，但是会受到硬件、软件等设备的制约



### 2.3.2 events配置           

⽐比如上⾯面的配置：

```bash
events {
 worker_connections 768;
}
```

events 块涉及的指令主要影响 Nginx 服务器与⽤户的⽹络连接，常⽤的设置包括是否开启对多 work process 下的⽹络连接进⾏序列化，是否允许同时接收多个⽹络连接，选取哪种事件驱动模型来处理连接请求，每个 word process 可以同时⽀持的最⼤连接数等。上述例⼦就表示每个 work process ⽀持的最⼤连接数为 768， 这部分的配置对 Nginx 的性能影响较⼤，在实际中应该灵活配置。

### 2.3.3 http配置             

这算是 Nginx 服务器配置中最频繁的部分，代理、缓存和⽇志定义等绝⼤多数功能和第三⽅模块的配置都在这⾥。
需要注意的是：http 块也可以包括 http 全局块、server 块。

1. http全局块
http 全局块配置的指令包括⽂件引⼊、MIME-TYPE 定义、⽇志⾃定义、连接超时时间、单链接请求数上限等。
2. server块
这块和虚拟主机有密切关系，虚拟主机从⽤户⻆度看，和⼀台独⽴的硬件主机是完全⼀样的，该技术的产⽣是为了节省互联⽹服务器硬件成本。
每个 http 块可以包括多个 server 块，⽽每个 server 块就相当于⼀个虚拟主机。
⽽每个 server 块也分为全局 server 块，以及可以同时包含多个 locaton 块。

**配置详解**

```bash
########### 每个指令必须有分号结束。#################
#user administrator administrators; #配置⽤户或者组，默认为nobody nobody。
#worker_processes 2; #允许⽣成的进程数，默认为1
#pid /nginx/pid/nginx.pid; #指定nginx进程运⾏⽂件存放地址
error_log log/error.log debug; #制定⽇志路径，级别。这个设置可以放⼊全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
events {
    accept_mutex on; #设置⽹路连接序列化，防⽌惊群现象发⽣，默认为on
    multi_accept on; #设置⼀个进程是否同时接受多个⽹络连接，默认为off
    #use epoll; #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
    worker_connections 1024; #最⼤连接数，默认为512
}
http {
    include mime.types; #⽂件扩展名与⽂件类型映射表
    default_type application/octet-stream; #默认⽂件类型，默认为text/plain
    #access_log off; #取消服务⽇志 
    log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #⾃定义格式
    access_log log/access.log myFormat; #combined为⽇志格式的默认值
    sendfile on; #允许sendfile⽅式传输⽂件，默认为off，可以在http块，server块，location块。
    sendfile_max_chunk 100k; #每个进程每次调⽤传输数量不能⼤于设定的值，默认为0，即不设上限。
    keepalive_timeout 65; #连接超时时间，默认为75s，可以在http，server，location块。
    upstream mysvr { 
        server 127.0.0.1:7878;
        server 192.168.10.121:3333 backup; #热备
    }
    error_page 404 https://www.baidu.com; #错误⻚
    server {
        keepalive_requests 120; #单连接请求上限次数。
        listen 4545; #监听端⼝
        server_name 127.0.0.1; #监听地址 
        location ~*^.+$ { #请求的url过滤，正则匹配，~为区分⼤⼩写，~*为不区分⼤⼩写。
            #root path; #根⽬录
            #index vv.txt; #设置默认⻚
            proxy_pass http://mysvr; #请求转向mysvr 定义的服务器列表
            deny 127.0.0.1; #拒绝的ip
            allow 172.18.5.54; #允许的ip 
        }
    }
}
```



## 2.4 Nginx核⼼心功能                

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Nginx-notes.assets\clip_image002.gif)

**2.4.1 反向代理理**   **反向代理理配置**

```bash
http{
    ...
    ...
    #这部分是被代理服务器的设置 ciggar只是⼀个代号
    upstream ciggar{
    	server 192.168.45.151:8080;
    }
    #这部分是nginx作为反向代理服务器的配置
    server{
        #nginx监听的端⼝
        listen 80;
        #虚拟服务器的识别标记，⼀般配置为本机ip
        server_name 192.168.45.151;
        #代理设置地址
        location / {
        	proxy_pass http://ciggar;
        }
    }
}
```

**负载均衡配置**

```bash
#负载均衡策略
# 1 轮询（默认）
# 2 weight
# 3 ip_hash
# 4 least_conn 最少连接⽅式
# 5 fair(第三⽅) 响应时间
# 6 url_hash (第三⽅)
```

```bash
#weight weight 代表权重,默认为 1,权重越⾼被分配的客户端越多
...
upstream ciggar{
    server 192.168.45.151:8080 weight=2;
    server 192.168.45.151:8081 weight=1;
}
...
```

```bash
#ip_hash 每个请求按访问 ip 的 hash 结果分配，这样每个访客固定访问⼀个后端服务器，可以解决session 的问题。例如：
...
upstream ciggar{
    ip_hash;
    server 192.168.45.151:8080;
    server 192.168.45.151:8081;
}
...
```



### 2.4.2 缓存            

Nginx从0.7.48版本开始，⽀持了类似Squid的内容缓存功能。这个缓存是把URL及相关组合当作Key，⽤md5编码哈希后保存在硬盘上，所以它可以⽀持任意URL链接，同时也⽀持404/301/302这样的⾮200状态码。
nginx缓存配置

```bash
...
http{
    ...
    #声明⼀个cache缓存节点的内容，levels 在 /path/to/cache/ 设置了⼀个两级层次结构的⽬录。设置Web缓存区名称为my_cache，内存缓存空间⼤⼩为200MB，1天没有被访问的内容⾃动清除，硬盘缓存空间⼤⼩为30GB。
    proxy_cache_path /data0/proxy_cache_dir levels=1:2 
    keys_zone=my_cache:200m inactive=1d max_size=30g;

    server{
        ...
        location / {
            proxy_cache my_cache;
            proxy_cache_key $uri;
            proxy_cache_valid 200 206 304 301 302 10d;
        }
        ...
    }
}
```

