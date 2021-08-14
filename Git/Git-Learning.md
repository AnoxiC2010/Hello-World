# Git Notes

* git init  
  在新建的本地目录总执行，初始化本地库,初始化 .git库文件夹

* git --version  
  查看git版本

* git config --global user.name "xxxx"  
  设置全局用户名为xxxx

* git config --global user.email "xxxx@xxx.com"  
  设置全局的用户邮箱

* git add xxx.md  
  提交到本地暂存区

* git commit -m "提交说明" xxx.md  
  本地暂存区提交到本地库

  git commit -am "your message"
  
  提交所有暂存区到本地库
  
* git commit -m "解决合并冲突说明"  
  在合并中状态下使用，合并完成提交不能加文件名

* git status  
  查看所在分支工作区和暂存区的状态

* git log  
  查看提交记录日志，从近到远，如果记录太多左下角有冒号，空格键下一页，B键上一页，尾页左下显示END，按Q退出

* git log --pretty=oneline  
  git log --oneline  
  git reflog  
  三种简化查看日志

* git reset --hard 67c8975  
  版本回退到索引为67c8975的版本（本地库，暂存区，工作区）  
  可以用于恢复删除的文件（当前索引可以用HEAD表示）

* git reset --mixed  
  回退本地库指针时重置暂存区，改动工作区

* git reset --soft  
  本地库指针移动但是不改动暂存区和工作区

* git diff xxx.md  
  比对工作区和暂存区的xxx.md文件，不加文件名则比对所有文件

* git diff HEAD xxx.md  
  比对暂存区和本地库指针版本（用HEAD表示当前指针）的xxx.md文件差异

* git branch -v  
  查看本地库所有分支，带星号为当前分支

  git branch -r 
  
  查看远端所有分支
  
  git branch -a
  
  查看本地和远端所有分支
  
* git branch xxxxx (不会切换到新分支) 或 git checkout -b xxxxx (新建后会自动切换到新分支)
  创建名为xxxxx的分支(默认基于当前分支节点创建)

* git branch -d branch_name / git branch -D branch_name
  
  删除本地分支，后者大写表示强制删除，有时候当事分支上包含了未合并的改动或者当事分支是当前所在分支，则-d无法删除，需要强制删除来达到目的。
  
  删除服务器上的远程分支可以使用 git branch -d -r branch_name，其中branch_name为本地分支名。删除后，还要推送到服务器上才行，即 git push origin : branch_name
  
* git checkout xxxxx  
  切换到名为xxxxx的分支

  有时候，当前分支工作区存在修改而未提交的文件，与目的分支上的内容冲突，会导致checkout切换失败，这时候，可以使用 git checkout -f 进行强制切换。
  
  git checkout对象可以是分支，也可以是某个提交节点或者节点下的某个文件。可按需了解。
  
* git merge xxxxx  
  切换到主分支后执行此命令，合并名为xxxxx的分支到主分支，执行后如果有冲突会提示，且显示合并标签（master | MERGING)，而且查看文件会看到冲突痕迹

  ```
  <<<<<<<<HEAD
  ========
  >>>>>>>>xxxxx
  认为修改合并后add并status能看到冲突解决但仍在合并状态中
  再次git commit  -m “message you add"即可合并完成，合并标签消失，注意合并的commit命令勿加文件名
  ```

* git remote -v  
  查看远程库别名

* git remote add origin https://.....git  
  起别名

* git push origin master  
  将本地master分支向远程库推送（这时候会输入github账号密码）

* git clone https://......git  
  克隆操作，有三个作用，初始化本地库，完整克隆远程库到本地库，替我们创建远程库的别名origin

* git fetch origin master  
  将远程库内容抓取到本地，但并不更新工作区文件

* git checkout origin/master  
  查看远程库master分支

* git checkout master  
  切换到master分支

* git merge origin/master  
  把远程库master分支合并到本地库master分支，合并前先切回到本地master

* git pull origin master  
  当于fetch和merge的合并

> 免密操作
* ssh-keygen  -t rsa -C xxxx@xxx.com  
  在用户目录下执行该命令
  邮箱为github注册邮箱，执行后再三次回车确认默认值（想输入点东西也行）
  user\.ssh\目录下生成了id_rsa和id_rsa.pub文件
  id_rsa.pub里面的ssh值可复制后到github网站添加到自己的账户的SSH keys中
* git remote add origin_ssh git@github.com:…….git  
  给ssh远程地址起别名
* git push origin_ssh master  
  用ssh方式提交，不用每次都进行身份验证，但是只针对一个账号



查找资源

常用的前缀后缀

- 查找百科大全 awesome xxx
- 找例子 xxx sample
- 找空项目架子 xxx starter / xxx boilerplate
- 找教程 xxx tutorial





# Git安装

## Linux下安装Git

### 包管理器方式安装

Linux系统：Ubuntu 10.10(maverick)或更新版本，Debian(squeeze)或更新版本

```shell
$ sudo aptitude install git
$ sudo aptitude install git-doc git-svn git-email gitk
```

其中git软件包包含了大部分Git命令，是必装的软件包。

软件包git-svn、git-email、gitk本来也是Git软件包的一部分，但是因为有着不一样的软件包依赖(如更多perl模组、tk等)，所以单独作为软件包发布。



Linux系统：RHEL、Fedora、CentOS等版本：

```shell
$ yum install git
$ yun install git-svn git-email gitk
```

### 从源代码开始安装

访问Git官网：http://git-scm.com/。下载Git源码包，例如：git-2.19.0.tar.gz。

展开源码包，并进入到相应的目录中。

```shell
$ tar -jxvf git-2.19.0.tar.gz
$ cd git-2.19.0
```

安装方法写在INSTALL文件当中，参照其中的指示完成安装。下面的命令将Git安装在/usr/local/bin中。

```shell
$ make prefix=/usr/local all
$ sudo make prefix=/usr/local install
```

安装Git文档(可选)。

```shell 
$ make prefix=/usr/local doc info
$ sudo make prefix=/usr/local install-doc install-html install-info
```

### 命令补齐

Linux的shell环境(bash)通过bash-completion软件包提供命令补齐功能，能够实现在录入命令参数时按一下或两下TAB键，实现参数的自动补齐或提示。例如输入`git com`后按下TAB键，会自动补齐为`git commit`。

将Git源码包中的命令补齐脚本复制到bash-completion对应的目录中：

```shell
$ cp contrib/completion/git-completion.bash /etc/bash_completion.d/
```

重新加载自动补齐甲苯，使之在当前shell中生效：

```shell
$ ./etc/bash_completion
```

为了能够在中断开启时自动加载bash_completion脚本，需要在本地配置文件~/.bash_profile或全局文件/etc/bashrc文件内中添加下面的内容：

```shell
if [ -f /etc/bash_completion ]; then
./etc/bash_completion
fi
```

## windows下安装Git

### 注意事项：

安装Git过程中选择必要组件时，开源的git-lfs存在一些问题，建议把勾选去掉(公司提供了自己的修复版本，后续相关优化会推送到社区)。

编辑器建议默认；

修改PATH环境比那辆建议选择"Use Git Bash Only"，比米娜Git自带的工具与Windows下已有的产生冲突。

如果不清楚PATH，可以参考https://en.wikipedia.org/wiki/PATH_%28variable%29，简单来讲，就是你输出一条命令的时候，系统会从PATH这个配置中寻找实现这条命令的程序在哪里，找到后就启动程序。



### 安装TortoiseGit小乌龟

安装过程中询问要使用的SSH客户端，缺省使用内置的TortoisePLink(来自PuTTY项目)作为SSH客户端。TortoisePLink和TortoiseGit的整合性更好，可以直接通过对话框设置SSH私钥(PuTTY格式)，而无需再到字符界面去配置SSH私钥和其他配置文件。

如果本地同事安装了命令行的Git版本，可以通过TortoiseGit的设置对话框选中Git提供的ssh客户端，这样在下载SSH协议的代码仓库的时候，通过命令行与TortoiseGit图形界面都可以使用同一套公钥和密钥。



### Git基本配置

Git有三种配置，分别以文件的形式存放在三个不同的地方。可以在命令行中使用git config工具查看这些变量。

- 系统配置(对所有用户都使用)

  存放在git的安装目录下：`%Git%/etc/gitconfig`; 若使用`git config`时用`--system`选项，读写的就是这个文件：

  `git config --system core.autocrlf`

- 用户配置(只是用于该用户)

  存放在用户目录下。例如linux存放在：`~/.gitconfig`； 若使用`git config`时用`--global`选项，读写的就是这个文件：

  `git config --global user.name`

- 仓库配置(只对当前项目有效)

  当前仓库的配置文件(也就是工作目录中的`.git/config`文件)；若使用`git config`时用`--local`选项，读写的就是这个文件：

  `git config --local remote.origin.url`

注：每一个级别的配置都会覆盖上一层的相同配置，例如`.git/config`里的配置会覆盖`%Git/etc/gitconfig`中的同名变量。

#### 配置个人身份

首次Git设定身份

```bash
git config --global user.name "Zhang San"
git config --global user.email zhangsan123@qq.com
```

这个配置信息会在Git仓库中提交的修改信息中体现，但和Git服务器认证使用的密码或者公钥密码无关。

#### 文本换行符配置

假如你正在Window上写程序，又或者你正在和其他人合作，他们在Windows上编程，而你却在其他系统上，在这些情况下，你可能会遇到行尾结束如问题。这是因为Windows使用回车和换行两个字符来结束一样，而Mac和Linux只是用换行一个字符。虽然这是小问题，但它会极大地扰乱跨平台协作。

Git可以在你提交时自动地把行结束符CRLF转换成LF，而在签出代码时把LF转换成CRLF。用core.autocrlf来打开此项功能，如果实在Windows系统上，把它设置成true，这样当签出代码时，LF会被转换成CRLF：

`$ git config --global core.autocrlf true`

Linux或Mac系统使用LF作为行结束符，因此你不想Git在签出文件时进行自动的转换；当一个以CRLF为行结束符的文件不小心被引入时你肯定想进行修正，把core.autocrlf设置成imput来告诉Git在提交时把CRLF转换成LF，签出时不转换：

`$ git config --global core.autocrlf input`

这样会在Windows系统上其拿出文件中保留CRLF，会在Mac和Linux系统上，包括仓库中保留LF。

乳沟你是Window程序员，且正在开发仅运行在Window上的项目，可以设置false取消此功能，把回车符记录在库中：

`$ git config --global core.autocrlf false`

#### 文本编码配置

- i18n.commitEncoding选项： 用来让git commit log储存时，采用的编码，默认UTF-8。
- i18n.logOutputEncoding选项：查看git log时，显示采用的编码，建议设置为UTF-8。

```
# 中文编码支持
git config --global gui.encoding utf-8
git config --global i18n.commitencoding utf-8
git config --global i18n.logoutputencoding utf-8

# 显示路径中的中文
git config --global core.quotepath false
```

#### 与服务器的认证配置

常见的两种协议认证方式

- http/https协议认证

  设置口令缓存：

  `git config --global credential.helper store`

  添加HTTPS证书信任：

  `git config http.sslverify false`

- ssh协议认证

  SSH协议是一种非常常用的Git仓库访问协议，使用公钥认证、无需输入密码，加密传输，操作便利又保证安全性。

# 1    Git

版本控制工具 👉 管理文件 👉 内容 在不同的时间点下是不同的状态 👉 代码就是文件的一种

 

Github、码云(gitee) 👉 内容托管网站 👉 网盘

996icu文本内容、女装大佬、学习资料（面试资料、电子书）、开源软件

私有的软件（交保护费 👉 企业功能）

 

 

时间线 👉 版本号

 

git 👉 分布式的版本控制工具

svn 👉 集中式的版本控制工具

 

分布式最重要的特点：可以离线使用，离线也可以记录版本（内容）变化

## 1.1   集中式

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image002.jpg)

## 1.2   分布式

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image004.jpg)

## 1.3   git的由来

linus 👉 linux创始人 

开源 👉 贡献 👉 linus整合各地的开源贡献者的代码

 

找了一个代码托管的网站 👉 收费  👉 自己造一个 👉 git

C 👉 git的命令 👉 linux的影子

## 1.4   Github或gitee码云

代码托管网站 👉 网盘

使用这个“网盘”，需要使用到git这个工具

 

网盘相当于“对象” 👉 远程仓库（中央仓库）

## 1.5   核心流程

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image006.jpg)

# 2    使用Git

## 2.1   创建一个远程仓库

github、gitee 👉 推荐使用码云

[注册 - Gitee.com](https://gitee.com/signup)

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image008.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image010.jpg)

## 2.2   加入到对仓库的管理

由团队中的某人创建远程仓库，其他人加入到对仓库的管理

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image012.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image014.jpg)

## 2.3   安装包

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image016.jpg)

右键

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image018.jpg)

# 3    Git命令

## 3.1   git clone 仓库地址 👉 初始化了本地仓库

将远程仓库复制一个备份

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image020.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image022.jpg)

Java30th这个文件夹 👉 工作区

本地仓库 👉 .idea 👉 抽象的空间 👉 命令

## 3.2   git init

也可以初始化本地仓库 👉 并不建议 👉 如果按照这种方式创建本地仓库 👉 额外去和远程仓库建立联系

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image024.jpg)

## 3.3   git status

查看工作区和暂存区的变化

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image026.jpg)

## 3.4   git add 文件名

将工作区的变化 提交到暂存区(缓冲区)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image028.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image030.jpg)

 

使用add命令时可以使用通配符 👉 谨慎操作

 

文件夹

某一类

全部文件

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image032.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image034.jpg)

## 3.5   第一次执行commit之前需要去配置用户信息

执行commit时，会产生新的版本 👉 新的版本的产生是需要用户信息

### 3.5.1 执行命令

git config --global user.name “stone5j”

git config --global user.email “stone5j”

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image036.jpg)

### 3.5.2 修改配置文件

C:/users/“当前用户名“/.gitconfig

👉 如果你是一个新用户，有可能没有这个文件 👉 直接创建一个同名文件

可以使用bash命令行创建 👉 touch .gitconfig

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image038.jpg)

## 3.6   git commit -m “自定义消息”

提交 👉 是否需要指定提交暂存区里的谁呢？ 👉 不需要

将暂存区里所有的内容都提交到本地仓库

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image040.jpg)

## 3.7   git log

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image042.jpg)

## 3.8   git push

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image044.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image046.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image048.jpg)

## 3.9   git pull

本地仓库落后于远程的时候 👉 拉取远程仓库领先的内容

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image050.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image052.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image054.jpg)

## 3.10 冲突

多个设备或多名开发人员修改了同一个文件，有可能产生冲突。

git会帮我们把代码合并到一起 👉 如果没有自动合并成功，就会产生冲突 👉 需要你手动合并

后提交的人员需要处理冲突

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image056.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image058.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image060.jpg)

**找到冲突位置** **👉** **修改为你需要的业务**

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image062.jpg)

**先提交的仓库可以直接****pull****拉取变化，不需要处理冲突**

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image064.jpg)

### 3.10.1 流程图

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image066.jpg)

## 3.11  后悔药

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image068.jpg)

### 3.11.1           工作区的变化checkout（谨慎操作）

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image070.jpg)

checkout 是恢复不了

### 3.11.2           暂存区变化

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image072.jpg)

### 3.11.3           本地仓库回退为之前的某个版本

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image074.jpg)

## 3.12  .gitignore

忽略管理的配置

如果你没有管理的文件，产生了变化，我可以忽略你

 

1、特定文件：直接写文件名

2、文件夹下的 ： 文件夹名/

3、某一类文件： *.xxx

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image076.jpg)

已经管理起来的文件，产生了变化，还可以识别到的

 

如果我不小心将某一些文件已经管理起来了怎么办？

 

让远程仓库里当前版本没有这个文件 👉 本地产生删除文件的变化 👉 将这个变化push到远程

 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Git\Git-learning.assets\clip_image078.jpg)

## 3.13 分支

主干master分支

主干master分支

 

# 4    如果没有冲突

如果你想要编辑内容 i

 

👉 vim编辑器

ESC

冒号

wq或q！

回车

 
