# Maven notes

# Maven

## 1. 介绍

Maven就是一个项目管理工具，可以帮助我们去管理项目。

> maven可以做什么事情呢？
>
> 1. 项目构建
>
>    maven可以帮助你去编译、打包、测试等。
>
>    编译：编译就是把.java文件变成.class文件
>
>    打包：打包就是把一堆.class文件变为一个 .jar 压缩包或者是.war压缩包
>
>    测试：我们在项目里面有很多测试类和测试方法（是指加了@Test注解的），那么maven可以帮助我们统一去运行这些测试类和			测试方法
>
> 2. 依赖管理
>
>    依赖管理其实就是帮助我们去管理这个工程的依赖（jar包）

## 2. Maven的安装

###  2.1 下载

[下载地址](https://archive.apache.org/dist/maven/maven-3/3.5.3/binaries/)

![image-20210426095850556](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426095850556.png)





### 2.2 安装

1. maven解压即安装

   maven不要解压到中文目录，目录不要带特殊字符，包括空格

2. 配置环境变量

   配置环境变量需要我们电脑上有JDK环境变量。或者说maven的运行依赖于JDK环境变量。

   第二个是我们需要配置MAVEN_HOME 以及 PATH

   ![image-20210426100954505](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426100954505.png)

   

   配置好了以后，我们可以去检测一下maven环境变量是否配置成功

   ![image-20210426101045655](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426101045655.png)

   

### 2.3 配置

![image-20210426102048520](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426102048520.png)



#### 2.3.1 配置本地仓库

打开maven文件里面的settings.xml

![image-20210426102438052](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426102438052.png)



#### 2.3.2 配置远程仓库

增加以下配置

```xml
<!-- 
这个是镜像仓库的配置，这个表示是我们下载依赖jar包从 阿里云镜像仓库的地址去下载
我们自己也搭建了这个远程仓库，那么也可以从我们王道自己的远程仓库去下载，只需要你配置就OK了
-->
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>central</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```



## 3 Maven的使用

### 3.1 Project&Module

project和module都是在idea这个编译器里面的内容

- project

  A project is a top-level organizational unit for your development work in IntelliJ IDEA. In its finished form, a project may represent a complete software solution. A project is a collection of:

  - Your work results: source code, build scripts, configuration files, documentation, artifacts, etc.
  - SDKs and libraries that you use to develop, compile, run and test your code.
  - Project settings that represent your working preferences in the context of a project.

  A project has one or more modules as its parts.

- module

  - A module is a part of a project that you can compile, run, test and debug independently.
  - Modules are a way to reduce complexity of large projects while maintaining a common (project) configuration.
  - Modules are reusable: if necessary, a module can be included in more than one project.

- project

  一个项目是IDEA开发工作的顶级组织单位。在其完成的形式中，一个项目可能代表一个完整的软件解决方案

  项目是以下内容的集合：

  - 你的工作成果：源代码、脚本、配置文件、文档、工件等。
  - 用于开发、编译、运行和测试代码的SDK和库。
  - 在项目上下文中表示你的工作偏好设置

  一个项目有一个或者多个模块作为其部件

- module

  - 模块是项目的一部分，你可以独立的编译、运行、测试和调试。
  - 模块是一种在维护公共项目配置的同事降低大型项目复杂性的方法。
  - 模块可以重复使用：如果需要，可以再多个项目中包含一个模块。

**总结一下：Project是IDEA的最顶级的结构单元，project只是起到了一个项目定义的功能，类似于工作空间的概念，这个工作空间可以去配置你的IDEA的一些公共配置，例如SDK、代码库等。module是project的一部分，可以用来单独编译运行等，每一个module项目运行的时候都是一个单独的Java进程。IDEA创建项目的时候会默认创建一个同名的module，实际上我们开发写代码都是在开发这个module。**

project就是相当于是一个单独的工作空间，从表面上看，其实就是idea这个编译器给我们重新打开一个页面。深入了来看，其实就是每一个页面idea都可以有单独的配置。

每一个module都是独立部署，独立运行的，每一个module都是一个单独的java项目



### 3.2 idea配置

#### 3.2.1 配置

配置当前项目下的maven的配置

![image-20210426112445747](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426112445747.png)



还需要配置idea的默认maven配置，这个maven的默认配置是帮助我们在打开一个maven的新的工程的时候采用的配置

![image-20210426112756984](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426112756984.png)



#### 3.2.2 使用maven

##### 3.2.2.1 手动创建一个maven工程

![image-20210426114327365](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426114327365.png)



##### 3.2.2.2 使用idea去创建maven工程

![image-20210426114933947](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426114933947.png)





在pom.xml文件里面**必须要**去配置这些坐标

```xml
<!-- 这个一般表示你的这个maven的项目是哪个公司开发的，一般是域名反转-->
<groupId>com.cskaoyan</groupId>
<!--这个是这个项目的名字 例如一个公司有多个产品，多个项目，-->
<artifactId>first-maven</artifactId>
<!--项目的版本-->
<version>1.0-SNAPSHOT</version>
```



### 3.3 项目构建

其实就是指使用maven插件去帮助我们完成去编译，打包，测试，部署等工作

![image-20210426143735956](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426143735956.png)

- **clean 这个是删除你的本地编译生成的.class文件以及包** 

  具体是指把mvn编译生成的target 包删掉

- validate 这个是使用maven 去检验你的代码是否有语法错误

- **compile 这个是编译你的项目（module）**

  这个是把module里面的.java文件以及资源文件都经过编译，放到target文件夹下

  

  文件编译之后的对应规则

  ![image-20210426151235372](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426151235372.png)

  

  还需要注意的是：我们的maven compile指令不会编译我们test包下的java文件，而package则会

- test 运行所有的你的项目里面的测试类的测试方法

  注意：我们的测试方法是有格式要求的，测试类必须是XXXTest，测试方法必须是testXXX，并且测试方法必须加上@Test注解

  

- **package 这个是帮助我们把一个module打包成一个jar文件或者是一个war文件**

  package其实就是帮助我们把 编译生成的.class文件以及资源文件压缩到 jar包或者是war里面去

  ![image-20210426151850715](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426151850715.png)

  

- verify 验证

- **install 安装**

  其实就是把打包生成的.jar文件或者是.war文件复制到本地仓库里面去

- depoly 部署

  部署其实就是把你的本地仓库里面的jar包或者是war包推送到远程仓库里面去，一般是项目部署的时候有这个需求，但是我们在实际的工作中，一般部署工具不会使用maven，而是使用一些其他的专业的部署工具来做，例如Jenkins

  

其实上述指令还可以在命令行来执行，这些指令都是maven给我们提供的命令，和idea无关

![image-20210426152337399](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426152337399.png)



其实这里的这些指令都是idea的可视化，那么我们也可以使用命令行来执行

```cmd
mvn clean
mvn compile
mvn test
mvn package
mvn install

// 也可以组合起来执行
mvn clean install

// 我们可以打开cmd窗口来执行，或者是使用idea给我们提供的terimal界面来执行命令
```



### 3.4 依赖管理

maven是如何去管理我们的依赖的呢？

其实就是通过pom.xml文件夹来管理我们的项目的依赖的

#### 3.4.1 如何导包

我们只需要去maven仓库的[官网](https://mvnrepository.com/) 去找到你需要的对应的包，然后复制这个包的坐标。

![image-20210426160307174](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426160307174.png)

然后放到你的项目的pom.xml文件里面去即可

![image-20210426160351696](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426160351696.png)

然后执行 右下角弹出的import changes 。

![image-20210426160425469](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426160425469.png)



### 3.5 依赖范围

这个指的是我们导入的jar包的作用域

![image-20210426160627037](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426160627037.png)

以这个test为例，默认是<scope>test</scope>，指只能再src/main/test这个目录里面才生效，在classpath主代码路径下不生效

默认大多数的依赖，都是compile作用域

### 3.6 依赖传递

依赖传递是依赖之间可以互相传递依赖

![image-20210426162031354](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426162031354.png)

### 3.7 依赖冲突

依赖冲突是指你在一个项目(module)中，反复的引入了同一个依赖的不同版本，那么这样就可能产生依赖冲突，版本不一致的问题。

如果产生了依赖冲突，我们需要怎样去解决呢？

#### 3.7.1 声明优先原则

谁先声明，以谁为准

![image-20210426162705945](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426162705945.png)

#### 3.7.2 就近原则

谁的依赖路径比较近，那么就以谁为准

![image-20210426163355196](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426163355196.png)

例如上图中的spring-beans依赖，我们在spring-context这个包里面有这个依赖，在spring-jdbc里面也有这个依赖，但是假如我们直接在项目中导入了spring-beans这个依赖，最后就会以离我们的项目最近的依赖为准，假如有两个或者是多个依赖他们的距离一样，那么就去遵循**声明优先原则**



#### 3.7.3 手动管理

其实就是我们可以手动的去排除我们的依赖

![image-20210426164320526](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426164320526.png)



#### 3.7.4 提取常量

我们的依赖的不同的版本会产生冲突，那么我们上面介绍了一些冲突产生的以后的解决方式，但是其实最好的解决方式就是避免冲突。如何避免冲突呢？ 我们一般可以采用提取常量的做法

![image-20210426165059952](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426165059952.png)





## 4. 使用Maven开发项目

使用了maven开发以后，我们需要把配置文件放在resources目录下， 那么这个时候我们获取文件的方式就发生了改变

配置文件存放的位置：

![image-20210426173525775](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Maven-notes.assets\image-20210426173525775.png)





文件加载的方式：

```java
static {

        try {
            // 加载配置文件
//            Properties properties = new Properties();
//            properties.load(new FileInputStream("classes/jdbc.properties"));

            // 通过类加载器去找
            Properties properties = new Properties();

            //获取类加载器
            ClassLoader classLoader = JDBCUtils2.class.getClassLoader();
            // 获取文件流
            InputStream resourceAsStream = classLoader.getResourceAsStream("jdbc.properties");

            // 加载配置文件
            properties.load(resourceAsStream);


            // 赋值
            JDBCUtils2.url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");
            driverClassName = properties.getProperty("driverClassName");

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```





# Maven+IDEA使用问题

Maven版本3.53；IDEA版本2018.3.6

## 乱码

IDEA-run窗口maven test输出中文字符乱码

```xml
<!--pom.xml-->
<!--这行处理run窗口显示的字符集-->
<properties>
    <argLine>-Dfile.encoding=UTF-8</argLine>
</properties>
```

build使用运行平台编码警告

```xml
<!--pom.xml-->
	<!--
	这行处理警告,这个可以不管（但如果用win命令行不管这个会往数据库插入乱码的中文字符）
    [WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
    -->
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

WIN命令行运行mvn的命令输出中文字符乱码

（包括 IDEA-Terminal 使用mvn test等命令输出中文字符乱码）

```
命令行执行：chcp 65001 会暂时修改cmd的输出为UTF-8
暂时解决中文显示
但如果不设置<project.build.sourceEncoding>属性为UTF-8,插入数据库的数据还是会乱码
```



## 编译环境

maven会把项目语言等级设置为JDK5，编译环境也设为1.5。就算在project structer中修改语言等级为1.8，并在settings-java compiler把1.5改为1.8，maven还会自己在后台再该回去，执行maven生命周期函数会出错。

```
[ERROR] COMPILATION ERROR : 
[INFO] -------------------------------------------------------------
[ERROR] /C:/Users/AnoxiC2010/Documents/IdeaProjects//Mybatis01/src/test/java/com/baidu/test/LoginTest.java:[63,13] try-with-resources is not supported in -source 1.5
  (use -source 7 or higher to enable try-with-resources)
```

需要再pom.xml添加以下设置

```xml
<!--pom.xml-->
<profiles>
    <profile>
        <id>jdk-1.8</id>
        <activation>
            <activeByDefault>true</activeByDefault>
            <jdk>1.8</jdk>
        </activation>
        <properties>
            <maven.compiler.source>1.8</maven.compiler.source>
            <maven.compiler.target>1.8</maven.compiler.target>
            <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
        </properties>
    </profile>

</profiles>
```



## resources路径下新建多级目录

在resources路径右键新建directory后如果像在java目录一样连续用`.`作分隔符建多级目录是不行的，这样新建的只是一个包含多个`.`的单个文件夹，在IDEA界面下看不出来错误，但运行测试会发现找不到配置文件。

所以在resources目录下请注意文件夹一级一级的新建。



## maven识别java路径下的配置文件

默认状态，maven项目中如果把配置文件放在java包里面是不会被识别到的，如果希望这样走捷径可以在pom.xml中作以下配置。就能同时识别resources和java包下的配置文件了，注意按需要修改配置文件后缀名。

```xml
<!--pom.xml-->
<!-- 编译的配置 我们可以指定我们的maven工程在编译的时候去哪个路径下找资源文件-->
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <!-- 两个* 表示目录的通配 -->
                <include>**</include>
            </includes>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
            </includes>
        </resource>
    </resources>
</build>
```

