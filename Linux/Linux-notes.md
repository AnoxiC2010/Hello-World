# Linux

## 1 linux简介

### 1. Linux前世今生

UNIX：UNIX 操作系统由肯•汤普森（Ken Thompson）和丹尼斯•⾥奇（Dennis Ritchie）发明。

```
1965 年开始的 Multics ⼯程计划，该计划由⻉尔实验室、美国麻省理⼯学院和通⽤电⽓公司联合发
起，⽬标是开发⼀种交互式的、具有多道程序处理能⼒的分时操作系统，以取代当时⼴泛使⽤的批处理操作
系统。但是该计划最终却以失败收场。
以肯•汤普森为⾸的⻉尔实验室研究⼈员吸取了 Multics ⼯程计划失败的经验教训，于 1969 年实现了
⼀种分时操作系统的雏形，1970 年该系统正式取名为 UNIX。
但是之前的操作系统⼤多使⽤汇编语⾔编写，对硬件依赖性强，可移植性很差，1971-1972 年，肯•汤普
森的同事丹尼斯•⾥奇发明了传说中的C语⾔，UNIX 系统的绝⼤部分源代码都⽤C语⾔进⾏了重写，这为提
⾼ UNIX 系统的可移植性打下了基础。
```



LINUX：

Linux 内核最初只是由芬兰⼈林纳斯·托瓦兹（Linus Torvalds）在赫尔⾟基⼤学上学时出于个⼈爱好⽽编写的。Linux 是⼀套免费使⽤和⾃由传播的类 Unix 操作系统，是⼀个基于 POSIX 和 UNIX的多⽤户、多任务、⽀持多线程和多 CPU 的操作系统。Linux 能运⾏主要的 UNIX ⼯具软件、应⽤程序和⽹络协议。它⽀持 32 位和 64 位硬件。Linux 继承了 Unix 以⽹络为核⼼的设计思想，是⼀个性能稳定的多⽤户⽹络操作系统。

Linux 是⼀个类似 Unix 的操作系统，Unix 要早于 Linux，Linux 的初衷就是要替代 UNIX，并在功能和⽤户体验上进⾏优化，所以 Linux 模仿了 UNIX（但并没有抄袭 UNIX 的源码），使得 Linux 在外观和交互上与 UNIX ⾮常类似。



### 1.2 Linux的发⾏版本

⽬前市⾯上较知名的发⾏版有：Ubuntu、RedHat、CentOS、Debian、Fedora、SuSE、OpenSUSE、Arch Linux、SolusOS 等。

虽然Linux 的发⾏版本众多，但是系统的核⼼——内核却系出同⻔，所以只要学会使⽤其中⼀种，即可触类旁通。



### 1.3 Linux操作系统的组成

UNIX或者Linux系统⼤致可以分为以下⼏个部分：最底层的硬件，以及和硬件交互的操作系统内核；中间层是shell层；最外层是应⽤层。

![image-20210612220641212](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Linux-notes.assets\image-20210612220641212.png)

内核层
内核层是 UNIX/Linux 系统的核⼼和基础，它直接附着在硬件平台之上，控制和管理系统内各种资源（硬件资源和软件资源），有效地组织进程的运⾏，从⽽扩展硬件的功能，提⾼资源的利⽤效率，为⽤户提供⽅便、⾼效、安全、可靠的应⽤环境。

Shell层
Shell 层是与⽤户直接交互的界⾯。⽤户可以在提示符下输⼊命令⾏，由 Shell 解释执⾏并输出相应结果或者有关信息，所以我们也把 Shell 称作命令解释器，利⽤系统提供的丰富命令可以快捷⽽简便地完成许多⼯作。



### 1.4 Linux⽂件系统⽬录

在linux中，⼀切皆为⽂件。⽂件分为下⾯的⼀些类型
1. 普通⽂件
2. ⽬录⽂件
3. 链接⽂件
4. 设备⽂件
5. 管道⽂件

Linux⽂件系统⽬录结构和熟知的windows系统有较⼤区别，没有各种盘符的概念。根⽬录只有⼀个/，
采⽤层级式的树状⽬录结构。

![image-20210612220707731](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Linux-notes.assets\image-20210612220707731.png)

1. /：根⽬录，所有的⽬录、⽂件、设备都在/之下，/就是Linux⽂件系统的组织者，也是最上级的领导者。
2. /bin：bin 就是⼆进制（binary）英⽂缩写。在⼀般的系统当中，都可以在这个⽬录下找到linux常⽤的命令。系统所需要的那些命令位于此⽬录。
3. /boot：Linux的内核及引导系统程序所需要的⽂件⽬录。
4. /dev：dev 是设备（device)的英⽂缩写。这个⽬录对所有的⽤户都⼗分重要。因为在这个⽬录中包含了所有linux系统中使⽤的外部设备。但是这⾥并不是放的外部设备的驱动程序。这⼀点和常⽤的windows,dos操作系统不⼀样。它实际上是⼀个访问这些外部设备的端⼝。可以⾮常⽅便地去访问这些外部设备，和访问⼀个⽂件，⼀个⽬录没有任何区别。
5. /home：如果建⽴⼀个⽤户，⽤户名是"xx",那么在/home⽬录下就有⼀个对应的/home/xx路径，⽤来存放⽤户的主⽬录。
6. /lib：lib是库（library）英⽂缩写。这个⽬录是⽤来存放系统动态连接共享库的。⼏乎所有的应⽤程序都会⽤到这个⽬录下的共享库。因此，千万不要轻易对这个⽬录进⾏什么操作，⼀旦发⽣问题，系统就不能⼯作了。
7. /proc：存储的是当前内核运⾏状态的⼀系列特殊⽂件，⽤户可以通过这些⽂件查看有关系统硬件及当前正在运⾏进程的信息，甚⾄可以通过更改其中某些⽂件来改变内核的运⾏状态。此外还有/srv /sys三个⽬录，内核相关⽬录，不要动。
8. /root：Linux超级权限⽤户root的家⽬录。
9. /sbin：这个⽬录是⽤来存放系统管理员的系统管理程序。⼤多是涉及系统管理的命令的存放，是超级权限⽤户root的可执⾏命令存放地，普通⽤户⽆权限执⾏这个⽬录下的命令，sbin中包含的都是root权限才能执⾏的。
10. /usr：这是linux系统中占⽤硬盘空间最⼤的⽬录。⽤户的很多应⽤程序和⽂件都存放在这个⽬录下。 Unix software resource usr。类似windows系统的program files
11. /usr/local：这⾥主要存放那些⼿动安装的软件，即不是通过或apt-get安装的软件。它和/usr⽬录具有相类似的⽬录结构。
12. /usr/share ：系统共⽤的东⻄存放地，⽐如/usr/share/fonts 是字体⽬录，/usr/share/doc和/usr/share/man帮助⽂件。
13. /etc：管理所有的配置⽂件的⽬录，⽐如安装mysql的配置⽂件my.conf
14. /mnt：可供系统管理员使⽤，⼿动挂载⼀些临时设备媒体设备的⽬录。
15. /media：是⾃动挂载的⽬录。当把U盘插⼊到系统中，会⾃动挂载到该⽬录下。⽐如插⼊⼀个U盘，会⾃动到/media⽬录中挂载。
16. /opt：额外安装软件存放的⽬录。⽐如mysql的安装包就可以放在该⽬录



### 1.5 Linux vs Windows

⽬前国内 Linux 更多的是应⽤于服务器上，⽽桌⾯操作系统更多使⽤的是 Windows。主要区别如下

![image-20210612221409938](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Linux-notes.assets\image-20210612221409938.png)



## 2 Linux安装

2.1 云服务购买
⾃⼰安装服务器还是麻烦了些，现在⼀般都推荐⼤家使⽤云服务器，⽐较⽅便，价格也不贵。
⽬前国内三家主流云服务器⼚商，阿⾥云、腾讯云、华为云

每个时间点都有不同的配置跟价格。



### 2.2 虚拟机安装

#### 2.2.1 软硬件准备

软件：推荐使⽤VMware，服务器上有相关安装包
镜像：推荐使⽤ubuntu 18.04
硬件：因为是在宿主机上运⾏虚拟化软件安装ubuntu，所以对宿主机的配置有⼀定的要求。最起码I5CPU 双核，内存4G及以上



#### 2.2.2 虚拟机准备

1. 安装VMware
2. 打开VMware创建新的虚拟机

3. 选择⾃定义安装或者是典型安装模式
典型安装：VMware会将主流的配置应⽤在虚拟机的操作系统上，对于新⼿来很友好。
⾃定义安装：⾃定义安装可以针对性的把⼀些资源加强，把不需要的资源移除。避免资源的浪费。
4. 安装完虚拟机之后开启虚拟机



## 3 Linux命令操作

### 3 Linux命令操作

#### 3.2 Linux⽂件与⽬录管理

```bash
pwd 【显示当前⽬录路径】
```

```bash
ls 【list directory contents，显示当前⽂件夹下的⽬录或⽂件】
常⽤选项：
 -a 显示所有的⽂件夹和⽂件，包括隐藏⽂件
 -l 以详细的形式显示
```

例如：

```bash
[root@www /]# ls -l
total 12
drwxr-xr-x 2 ciggar ciggar 4096 Aug 2 20:58 cskaoyan
-rw-r--r-- 1 ciggar ciggar 0 Aug 2 20:47 main.txt
-rw-r--r-- 1 ciggar ciggar 14 Aug 2 20:11 test.txt
```

```bash
cd 【进⼊⼀个路径或者⽬录，绝对路径或者相对路径都可以】
⽤法： cd [路径]，⽐如 cd ~(代表进⼊家⽬录)
cd . 【当前⽬录】
cd .. 【进⼊上⼀级⽬录】
cd /home 【进⼊绝对路径home⽬录下】
```



#### 3.2 Linux⽂件与⽬录管理

我们知道Linux的⽬录结构为树状结构，最顶级的⽬录为根⽬录 /。我们需要先知道什么是绝对路径与相对路径。

- 绝对路径： 路径的写法，由根⽬录 / 写起，例如： /usr/share/doc 这个⽬录。
- 相对路径：
  路径的写法，不是由 / 写起，例如由 /usr/share/doc 要到 /usr/share/man 底下时，可以写成：
  cd ../man

##### 3.2.1 ⽬录⽂件

```bash
mkdir：创建⼀个新的⽬录
rmdir：删除⼀个空的⽬录
cp: 复制⽂件或⽬录
[root@www ~]# cp [-adfilprsu] 来源档(source) ⽬标档(destination)
-a：相当於 -pdr 的意思，⾄於 pdr 请参考下列说明；(常⽤)
-d：若来源档为连结档的属性(link file)，则复制连结档属性⽽⾮⽂件本身；
-f：为强制(force)的意思，若⽬标⽂件已经存在且⽆法开启，则移除后再尝试⼀次；
-i：若⽬标档(destination)已经存在时，在覆盖时会先询问动作的进⾏(常⽤)
-l：进⾏硬式连结(hard link)的连结档创建，⽽⾮复制⽂件本身；
-p：连同⽂件的属性⼀起复制过去，⽽⾮使⽤默认属性(备份常⽤)；
-r：递归持续复制，⽤於⽬录的复制⾏为；(常⽤)
-s：复制成为符号连结档 (symbolic link)，亦即『捷径』⽂件；
-u：若 destination ⽐ source 旧才升级 destination ！

rm: 移除⽂件或⽬录
-f ：就是 force 的意思，忽略不存在的⽂件，不会出现警告信息；
-i ：互动模式，在删除前会询问使⽤者是否动作
-r ：递归删除啊！最常⽤在⽬录的删除了！这是⾮常危险的选项！！！

mv: 移动⽂件与⽬录，或修改⽂件与⽬录的名称
[root@www ~]# mv [-fiu] source destination
-f ：force 强制的意思，如果⽬标⽂件已经存在，不会询问⽽直接覆盖；
-i ：若⽬标⽂件 (destination) 已经存在时，就会询问是否覆盖！
-u ：若⽬标⽂件已经存在，且 source ⽐较新，才会升级 (update)
```

你可以使⽤ man [命令] 来查看各个命令的使⽤⽂档，如 ：man cp。



##### 3.2.2 普通⽂件

创建⽂件命令

```bash
touch: 创建⼀个新的空⽂件
```

查看⽂件命令

```bash
cat: 以只读的⽅式打开⼀个⽂件。可以加 -n 表示带上⾏号(适合查看⽂件内容⽐较少的)
more: 和cat功能类似，不过是以分⻚的形式⼀⻚⼀⻚显示数据。最基本的指令就是按空⽩键（space）
就往下⼀⻚显示，按 b 键就会往回（back）⼀⻚显示。
+n ：从笫n⾏开始显示
-c ：从顶部清屏，然后显示

less: 也⽤来分⻚显示数据，但是功能⽐more强⼤。并不会⼀次性将全部⽂件读取才显示，⽽是根据显示
的需要加载对应的数据。
-f ：强迫打开特殊⽂件，例如外围设备代号、⽬录和⼆进制⽂件
-m ：显示类似more命令的百分⽐
-N ：显示每⾏的⾏号
操作命令
f 向后翻⼀⻚
d 向后翻半⻚
h 显示帮助界⾯
q 退出less 命令
u 向前滚动半⻚
y 向前滚动⼀⾏

head: 查看⼀个⽂件，取开头的⼀部分内容。head filename，或者添加选项： head -5 filename
tail:查看⼀个⽂件的尾部内容。tail -5 filename，查看⽂件的最后五⾏
 常⻅⽤法：tail -f filename,尾部持续不断地输出内容。Control + c退出。
 例如：tail -5f filename
```

重定向和追加

```bash
echo: 输出内容到控制台。⽐如输出Linux的环境变量到控制台
echo $PATH

>指令：输出重定向（会将原来的内容覆盖）
>>指令：追加（不会覆盖原⽂件的内容，追加到底部）
echo hello > a.txt
ls -l >> a.txt
cat a.txt > b.txt(⽂件可以存在，可以不存在)
```



压缩与解压
tar是⽤来建⽴，还原备份⽂件的⼯具程序，它可以加⼊，解开备份⽂件内的⽂件。

```bash
tar
-c:产⽣.tar⽂件
-v:显示详细信息
-z:打包同时压缩
-f:指定压缩后的⽂件名
-x:解压.tar⽂件
压缩 ：tar -zcvf combine.tar.gz 1.txt
解压 ：tar -zxvf combine.tar.gz -C java/
-C 表示解压到指定⽬录
```

注意：如果需要解压和压缩为 .zip 格式的⽂件，那么需要安装zip和unzip软件

```bash
sudo apt-get install zip
sudo apt-get install unzip
```



##### 3.2.3 ⽂本编辑

Ubuntu默认没有安装vim，需要先安装vim⼯具。

```bash
sudo apt-get install vim
```

vim有三种模式：命令模式（Command mode）、插⼊模式（Insert mode）、末⾏模式（Last Line mode）。

**命令模式**

通过指令 vim filename 进⼊命令模式。除此之外，还有⼀些其他的参数，⽐如：

```bash
-r: 恢复上次vim打开时崩溃的⽂件
-R: 把指定的⽂件以只读的⽅式放⼊vim编辑器中
+: 打开⽂件，并把光标置于最后⼀⾏的⾸部
+n: 打开⽂件，并把光标置于第n⾏的⾸部
```

命令模式快捷键：
删除

```bash
x: 删除光标所在位置的字符
dd: 少出光标所在⾏
ndd: 删除当前⾏后n⾏⽂本（包括此⾏）
dG: 删除光标所在⾏⼀直到⽂件末尾的所有内容
D: 删除光标位置到⾏尾的所有内容
```

删除的内容此时并没有被真正删除，⽽是在剪切版中，按下 p 键，可以将删除的内容粘贴回来。

**移动**

```bash
w: 光标移动⾄下⼀个单词⾸
e: 光标移动⾄下⼀个单词尾
b: 光标移动⾄上⼀个单词⾸
gg: 光标移动到⽂件开头
G: 光标移动⾄⽂件末尾
nG: 光标移动到第n⾏，n为数字
0或^:光标移动⾄当前⾏的⾏⾸
$: 光标移动⾄当前⾏的⾏尾
```

**插⼊模式**

在命令模式下，通过按下i、I、a、A、o、O这6个字⺟进⼊插⼊模式，不同的字⺟代表不同的进⼊⽅式。

```bash
i: 在当前光标位置前⾯插⼊随后输⼊的⽂本，光标后的⽂本相应向右移动
o: 在光标所在⾏下⾯插⼊新的⼀⾏，然后光标停在空⾏⾸，等待输⼊⽂本
O: 在光标所在⾏上⾯插⼊新的⼀⾏，然后光标停在空⾏⾸，等待输⼊⽂本
a: 在当前光标位置后⾯插⼊随后输⼊的⽂本，光标后的⽂本相应向右移动
A: 在光标所在⾏的⾏尾插⼊随后输⼊的⽂本
```

按下ESC键离开插⼊模式，进⼊命令模式

**末⾏模式**

在命令模式下，按下 : 键进⼊末⾏模式。在该模式下，可以使⽤⼀系列的指令，完成保存、离开vim编辑器等功能。

```bash
:wq 保存并退出vim编辑器
:wq! 保存并强制退出vim编辑器
:q 不保存退出
:q! 不保存强制退出
:w 保存不退出
:w! 强制保存不退出
:w filename 另存到filename⽂件
ZZ 直接退出
```

![image-20210612223042640](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Linux-notes.assets\image-20210612223042640.png)

#### 3.3 ⽤户管理

Linux系统是⼀个多⽤户、多任务的操作系统。多⽤户是指在linux操作系统中可以创建多个⽤户，⽽这些多⽤户⼜可以同时执⾏各⾃不同的任务，⽽互不影响。
在Linux系统中，会存在着以下⼏个概念，
1.⽤户名：⽤户的名称
2.⽤户所属的组：当前⽤户所属的组。
3.⽤户的家⽬录：当前账号登录成功之后的⽬录，就叫做该⽤户的家⽬录。



##### 3.3.1 添加⽤户

```bash
sudo useradd [选项] ⽤户名
```

eg：创建⼀个⽤户，⽤户名 test

```bash
sudo useradd test
注意这种⽅式创建出来的没有家⽬录，我们创建的时候需要带参数
sudo useradd -m test ： -m 表示在 /home⽬录下创建⼀个家⽬录
sudo useradd -m -s /bin/bash test ： 表示指定shell版本是我们熟悉的bash
```

给⽤户添加密码：

```bash
sudo passwd [⽤户名]
```

切换⽤户：

```bash
su [⽤户名]
```



##### 3.3.2 删除⽤户

```bash
sudo userdel [⽤户名]
-r: 不仅会删除该⽤户，还会删除该⽤户对应的家⽬录
```

#### 3.4 组管理

添加组： groupadd [groupname]
创建⽤户的时候加⼊组： useradd -m -s /bin/bash -g [groupname] [username]
查看⽤户及组信息：id ⽤户名
修改⽤户所属组： usermod -g [groupname] [username]



将用户添加到wheel组

为了将您的用户添加到该组，可以使用usermod或gpasswd命令。

`$ sudo usermod -aG wheel <user>` (CentOS8测试无效什么鬼)

另外，这是使用gpasswd命令的语法。

```
[root@localhost opt]# sudo gpasswd -a anoxic2010 wheel
Adding user anoxic2010 to group wheel
(CentOS8测试有效)
```

使用groups命令确保用户属于wheel组。

```
su[anoxic2010@localhost opt]$ su anoxic2010
Password: 
[anoxic2010@localhost opt]$ groups
anoxic2010 wheel
```



#### 3.5 权限管理

##### 3.5.1 ⽂件权限

查看

```bash
ciggar@ubuntu:~/Desktop/test$ ls -l
total 40
# ⽂件信息 ⽂件数 ⽤户 组名 ⼤⼩ ⽉份 ⽇期 时间 ⽂件名
drwxr-xr-x 2 ciggar ciggar 4096 Aug 2 23:52 cskaoyan
prw-r--r-- 1 ciggar ciggar 0 Aug 2 23:43 fifo_file
-rw-r--r-- 1 ciggar ciggar 26825 Aug 3 00:25 main.txt
-rw-r--r-- 1 ciggar ciggar 33 Aug 3 00:37 test.txt
drwxr-xr-x 3 ciggar ciggar 4096 Aug 3 01:27 xxx
```

前⾯10个符号表示⽂件的⼀些基本信息。
第1位：-表示是⼀个普通的⽂件；d表示是⼀个⽬录；（最常⽤）
rwx：Read、Write、Execute，读、写、执⾏权限，这个顺序不会变，如果没有权限的话就⽤-代替
第2-4位：表示⽂件所有者的权限
第5-7位：⽂件所在组的拥有的权限
第8-10位：⽂件其他组⽤户拥有的权限

**修改**

通过 chmod 指令，可以修改⽂件或者⽬录的权限

[⽅式⼀]

```bash
chmod u=rwx,g=rw,o=r filename
u:所有者 g:所有组 o:其他组,a代表全部
```

[⽅式⼆]

r=4,w=2,x=1 rwx = 4 + 2 + 1 = 7

```bash
chmod 751 等价于 u=rwx,g=rx,o=x
```



##### 3.5.2 案例

Linux⽆间道：
1. 新建⼀个police组和⼀个gang组
2. police组新增⼀个成员叫刘⼩磊，另⼀个成员⼩警，gang组新增⼀个成员叫张⼤松，另⼀个成员强哥
3. 将刘⼩磊和张⼤松的分组对调
4. 刘⼩磊创建⼀个⽂件，写下：葵涌码头，⻰⿎滩收货。该⽂件权限⾃⼰可以读写，同组其他⼈员没有权限读写，其他组可以查看
5. 张⼤松创建⼀个⽂件，写下：有内⻤，终⽌交易。该⽂件权限⾃⼰可以读写，同组其他⼈员没有权限读写，其他组可以读写
6. 将刘⼩磊的账号销毁

```
#第⼀步
sudo groupadd police
sudo groupadd gang
#第⼆步
sudo useradd -m -s /bin/bash -g police liuxiaolei
sudo useradd -m -s /bin/bash -g police xiaojing
sudo useradd -m -s /bin/bash -g gang zhangdasong
sudo useradd -m -s /bin/bash -g gang qiangge
#给定密码
sudo passwd liuxiaolei
sudo passwd xiaojing
sudo passwd zhangdasong
sudo passwd qiangge
#第三步 卧底
sudo usermod -g police zhangdasong
sudo usermod -g gang liuxiaolei
#第四步 卧底传递情报
su liuxiaolei
sudo echo "葵涌码头，⻰⿎滩收货" > liuxiaolei.txt
chmod 704 liuxiaolei.txt
su zhangdasong
sudo echo "有内⻤，终⽌交易" > zhangdasong.txt
chmod 706 zhangdasong.txt
#第五步 读取⽂件，刘⼩磊暴露，销毁刘⼩磊⽤户
```



#### 3.6 进程管理

##### 3.6.1 查看进程

Linux系统中查看进程使⽤情况的命令是ps指令
常⻅选项：
-e：显示所有进程
-f：全格式
a:显示终端上的所有进程
u:以⽤户的格式来显示进程信息
x:显示后台运⾏的进程
⼀般常⽤格式为ps -ef或者ps aux两种。显示的信息⼤体⼀致，略有区别。

**带[*]表示重要**

![image-20210612223927272](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Linux-notes.assets\image-20210612223927272.png)

```bash
UID：⽤户ID
*PID：进程ID
*PPID：⽗进程ID
C：CPU⽤于计算执⾏优先级的因⼦。数值越⼤，表明进程是CPU密集型运算，执⾏优先级会降低；数值越⼩，表明进程是I/O密集型运算，执⾏优先级会提⾼
STIME：进程启动的时间
TTY：完整的终端名称
TIME：CPU时间
*CMD：完整的启动进程所⽤的命令和参数
```

![image-20210612224012946](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Linux-notes.assets\image-20210612224012946.png)

```bash
USER：⽤户名称
*PID：程号
*%CPU：进程占⽤CPU的百分⽐
*%MEM：进程占⽤物理内存的百分⽐
VSZ：进程占⽤的虚拟内存⼤⼩（单位：KB）
RSS：进程占⽤的物理内存⼤⼩（单位：KB）
TT：终端名称（缩写），若为？，则代表此进程与终端⽆关，因为它们是由系统启动的
*STAT：进程状态，其中S-睡眠，s-表示该进程是会话的先导进程，N-表示进程拥有⽐普通优先级更低的优先级，R-正在运⾏，D-短期等待，Z-僵死进程，T-被跟踪或者被停⽌等
STARTED：进程的启动时间
TIME：CPU时间，即进程使⽤CPU的总时间
COMMAND：启动进程所⽤的命令和参数，如果过⻓会被截断显示
```



两者区别：
如果想查看进程的CPU占⽤率和内存占⽤率，可以使⽤aux
如果想查看进程的⽗进程ID和完整的COMMAND命令，可以使⽤ef



**如果想在进程列表中进⼀步筛选出想查询的进程，可以使⽤管道符**

```bash
#搜索匹配进程
ps -ef | grep [搜索内容]
```



##### 3.6.2 终⽌进程

```bash
kill [选项] 进程号
参数：
-9：操作系统从内核级别强制杀死⼀个进程
-15：可以理解为操作系统发送⼀个通知告诉应⽤主动关闭
```

kill的如果是服务，服务会换个pid重启，停止服务见下方。

##### 3.6.3 服务管理

服务本质上来说也是⼀个进程，只不过是在后台运⾏。监听着某⼀端⼝，等待该端⼝的请求到来，⽐如ssh服务，监听着22端⼝；mysql服务，监听着3306端⼝；tomcat服务，监听着80或者8080端⼝。

指令：（管理服务器的启动、停⽌、状态等）

systemctl start/stop/restart/status/reload 服务名称

例如：

```bash
#⽐如关闭ssh服务
systemctl stop sshd
```

#### 3.7 ⽹络管理

##### 3.7.1 查看⽹络设置

```bash
ifconfig
```

没有的话需要安装net-tools，系统会提示

```
sudo apt install net-tools
```



##### 3.7.2 查看⽹络端⼝占⽤情况

```bash
netstat
 -a:显示全部
 -n:以数字的形式显示
 -p:显示该连接被哪个应⽤程序占⽤PID
eg:
netstat -anp | grep 3306
```

第⼆种⽅式：

```bash
lsof -i: [端⼝号]
```

冒号之后不要有空格

##### 3.7.3 查看⽹络是否正常

```bash
ping [⽬的ip或者域名]
```



### 4 Linux远程软件安装

通过ssh协议远程连接Linux服务器。

什么是ssh协议？

SSH 为 Secure Shell 的缩写，由 IETF 的⽹络⼯作⼩bai组(Network Working Group)所制定;SSH为建⽴在应⽤层和传输层基础之上的安全协议。SSH 是⽬前较可靠，专为远程登录会话和其他⽹络服务提供安全性的协议。

如何使⽤ssh协议远程连接服务器？

1. ⾸先需要在⽬标服务器上安装ssh服务（Ubuntu 16 默认⾃带了这个服务）

   ```bash
   #查看ssh服务是否启动
   ps aux|grep ssh
   #如果出现sshd进程则表示已经启动，如果没有出现那么执⾏下⾯的命令
   #更新apt源
   sudo apt update
   #安装ssh
   sudo apt install openssh-server
   #安装完之后再次查看sshd是否已经启动，如果没有启动的话执⾏以下命令启动
   sudo service ssh restart
   ```

2. 在⽬标服务器上安装ssh服务之后需要在本机也安装ssh客户端的服务

   ```bash
   #执⾏命令
   ssh [username]@[ip]
   #例如：
   ssh ciggar@192.168.0.1
   #然后输⼊ciggar这个⽤户对应的密码即可
   ```



#### 4.1 Shell软件安装

由于每次输⼊⽤户名，ip地址，密码这些太麻烦了，所以出现了⼀些客户端软件帮我们提供ssh服务并能够帮我们保存⽤户名密码之类的，我们可以下载使⽤他们，例如xshell、SecrueCRT等等，这⾥对软件不做限制，以SecrueCRT为例
如何连接？

![image-20210612224910980](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Linux-notes.assets\image-20210612224910980.png)

![image-20210612224925092](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Linux\Linux-notes.assets\image-20210612224925092.png)

如上图:
Hostname输⼊ip地址
端⼝号22是ssh的默认端⼝号，不⽤改
Username输⼊⽤户名
然后点击continue，接下来输⼊密码即可（点击记住密码）



#### 4.2 ftp软件安装

ftp是什么？

FTP（File Transfer Protocol，⽂件传输协议） 是 TCP/IP 协议组中的协议之⼀。FTP协议包括两个组成部分，其⼀为FTP服务器，其⼆为FTP客户端。其中FTP服务器⽤来存储⽂件，⽤户可以使⽤FTP客户端通过FTP协议访问位于FTP服务器上的资源。在开发⽹站的时候，通常利⽤FTP协议把⽹⻚或程序传到Web服务器上。此外，由于FTP传输效率⾮常⾼，在⽹络上传输⼤的⽂件时，⼀般也采⽤该协议。

如何使⽤？

安装 winscp ， filezila 或者是 transmit 软件即可。
安装完软件之后就可以做到本地服务器和远程服务器之间的⽂件传输了。



### 5 Java环境搭建

#### 5.1 JDK环境搭建

从oracle官⽹下载linux的jdk8，之后⽤winscp⼯具等将jdk8上传到linux机器上。

```bash
sudo mkdir /usr/local/java
cd /usr/local/java
sudo tar -zxvf jdk-8u231-linux-x64.tar.gz
sudo mv jdk1.8.0_231/ jdk # 可改可不改
sudo vim /etc/profile
```

配置环境变量，添加下⾯⼏句

```bash
export JAVA_HOME=/usr/local/java/jdk/jdk1.8.0_231 # 路径改为自己设置的对应的路径
export JRE_HOME=/usr/local/java/jdk/jdk1.8.0_231/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH # 最后的$PATH是指在原来的PATH环境变量基础上增加新的，注意冒号分隔
```

执⾏命令 source /etc/profile 使环境变量配置⽂件⽣效
执⾏命令 java -version 查看JDK环境变量是否已经搭建好



#### 5.2 Tomcat环境搭建

```bash
#先把tomcat压缩包丢进服务器⾥⾯去
mkdir /usr/local/tomcat
sudo tar -zxvf apache-tomcat-8.5.50.tar.gz
chmod -R 777 * # 不该权限也能用
cd bin/
#启动tomcat
./startup.sh
```



#### 5.3 Mysql环境搭建

使⽤apt源上为我们提供的Linux软件安装包来进⾏安装

```bash
sudo apt update
sudo apt install mysql-server
#如果是ubuntu 16.04 那么此时会要求你输⼊密码，如果是18.04，那么就直接完成安装了
```

Ubuntu18.04 安装完之后修改mysql密码

```bash
sudo cat /etc/mysql/debian.cnf
```

```bash
anoxic2010@ubuntu:/usr/local$ sudo cat /etc/mysql/debian.cnf
# Automatically generated for Debian scripts. DO NOT TOUCH!
[client]
host     = localhost
user     = debian-sys-maint
password = JUqSB9QxVnIrEhwj
socket   = /var/run/mysqld/mysqld.sock
[mysql_upgrade]
host     = localhost
user     = debian-sys-maint
password = JUqSB9QxVnIrEhwj
socket   = /var/run/mysqld/mysqld.sock
```

使用如上的缺省账号密码登录。

【然而实测直接 sudo mysql就能直接登入了】

库mysql中的表user里有root用户但是没有设置密码，下面给root设置密码

```bash
#缺省账号登陆mysql
mysql -u debian-sys-maint -p
# 输入缺省密码
#修改⽤户名密码
use mysql;#连接到mysql数据库
update mysql.user set authentication_string=password('123456') where
User='root' and Host ='localhost' #修改密码123456是密码
update user set plugin="mysql_native_password"  #使用本地验证插件才能生效
flush privileges #生效
quit
```

```bash
#重启mysql
sudo service mysql restart
```



主机的navicat（图形界面）链接虚拟机的mysql需要做一些配置

```
cd /etc/mysql/mysql.conf.d/
sudo vim mysqld.cnf
```

```bash
# 这是mysqld.cnf文件的部分内容
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 127.0.0.1
# 这里默认绑定的主机地址是本机，修改为bind-address = 0.0.0.0 允许任意主机来连接
```

```bash
:wq # 保存退出
```

使用主机navicat连接虚拟机mysql失败，还有设置没有完成

原因是前面设置的root用户是localhost的，并不是对于任意主机都能使用的账号。

所以需要在数据中在插入一个用户，为任意主机的root用户

```bash
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION; #赋予来自所有主机的root用户所有权限
flush privileges; # 生效
```

重启mysql服务

主机图形界面即可连接虚拟机mysql

