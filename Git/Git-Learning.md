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

* git branch xxxxx  
  创建名为xxxxx的分支

* git checkout xxxxx  
  切换到名为xxxxx的分支

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

 
