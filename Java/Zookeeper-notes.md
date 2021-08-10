# Zookeeper notes

# Zookeeper入门

## 概述

Zookeeper是一个开源的分布式的，为分布式框架提供协调服务的Apache项目。



工作机制

Zookeeper从设计模式角度来理解：是一个基于观察者模式设计的分布式服务管理框架，它负责储存和管理大家都关心的数据，然后接收观察者的注册，一旦这些数据的状态发生变化，Zookeeper就将负责通知已经在Zookeeper上注册的那些观察者做出相应的反应。



Zookeeper特点

![image-20210802222348017](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210802222348017.png)

![image-20210802222423238](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210802222423238.png)



数据结构

Zookeeper数据模型的结构与Unix文件系统很类似，整体上可以看作是一棵树，每个节点称作一个ZNode。每个ZNode默认能够储存1MB的数据，每个ZNode都可以通过其路径唯一标识。

![image-20210803105949258](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210803105949258.png)



应用场景

提供的服务包括：统一命名服务、统一配置管理、统一集群管理、服务器节点动态上下线、软负载均衡等。



统一命名服务

在分布式环境下，经常需要对应用/服务进行统一命名，便于识别。

例如：IP不容易技术，而域名容易记住。

<img src="C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210803110643805.png" alt="image-20210803110643805" style="zoom: 25%;" />



## Linux安装使用zookeeper

官网下载→上传到linux→解压缩

修改conf文件夹下的示例配置文件zoo-sample.cfg名字为zoo.cfg

修改zoo.cfg里的dataDir的示例路径为有效路径

进入bin目录

启动服务端 `$ ./zkServer.sh start`

> 没有配置环境的话不加`./`会command not found

![image-20210803213242277](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210803213242277.png)

查看zookeeper进程 `$ jps -l`

> 如果jps出现command not found但`java -version`显示安装了java，需要安装Java开发环境

![image-20210803213337876](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210803213337876.png)

启动客户端 `$ ./zkCli.sh`

> 不需要加 start

退出客户端`quit`

![image-20210803213749148](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210803213749148.png)

![image-20210803214208524](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210803214208524.png)

![image-20210803215146072](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210803215146072.png)

查看zookeeper状态

![image-20210803214419131](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Zookeeper-notes.assets\image-20210803214419131.png)

退出zookeeper `$ ./zkServer.sh stop`



## 配置参数解读