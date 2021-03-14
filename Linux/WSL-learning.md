# WSL2 Notes

# WSL2 Installation

[Official Instruction](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10#simplified-installation-for-windows-insiders)

* Windows version 2004 or higher

* 2 windows features to be turned on

  Virtual Machine Platform

  Windows Subsystem for Linux

* Install WSL2 Linux kernel update package

* Install Ubuntu from Microsoft Store

* Launch Ubuntu after downloading

* Set username and password

  * $ uname -a

    $ lsb_release -a

    to check release info

* Run PowerShell to config

  * wsl -l -v

    To check the WSL mode

  * wsl --set-default-version 2

    To set v2 as the default version for future installations

  * wsl --set-version Ubuntu-20.04 2(or 1)

    To upgrade your existing Linux distro to v2/v1
  
  * wsl --set-default Ubuntu-20.04
  
    To change your default WSL to certain distro

**安装好后开始菜单找到该安装版运行即可进入子系统窗口**

> 子系统可在linux命令窗口运行外也可以在PowerShell中运行

```powershell
> wsl
//直接在PowerShell中运行子系统，执行后提示符变为linux风格
$ lsb_release -a
//然后检查linux版本信息，执行说明已经运行了子系统
$ exit
//exit退出子系统回到PowerShell
> wsl -l -v
//查看子系统说明已经在PowerShell中，可以使用cmd命令
> wsl --help
//查看帮助
> wsl --shutdown
//关闭所有子系统
```

# 和vmware等虚拟机同时用的问题

可以同时使用，但是在VMware的处理器中启用嵌套虚拟技术会启动不了

> VMware Player does not support nested virtualization on this host

VMware和wsl2同时使用会变慢

# 子系统和主系统交互

## 本地使用VSCode连接linux操作文件

在Linux命令行启动WIN中的vscode

```bash
~$ code .
```

VSCode在WIN中被启动且提示安装"Remote - WSL"插件并安装

在Linux命令行再次启动WIN中的vscode

```bash
~$ code .
```

Linux开始安装VS Code Server for x64，完成后开始WIN中的vscode

VSCode启动后便已连接且能看到Linux的目录，可以开始在WIN下的VSCode创建和编辑文件了

Linux终端能同步看到VSCode创建和处理的文件



# File access to each other

**Linux to browse Windows files**

  ```bash
$cd /mnt/c
$sudo ls
//use Linux commands to retrive windows file system
$find . | grep <filename>
  ```

**Windows to browse Linux files**

***explorer.exe***

explorer.exe allows me to explore files on the Linux system using Window application

```bash
$cd ~
$explorer.exe .
```

Now it is free to use Windows editor to edit a Linux file.
Charset Problem
I inputted Chinese character to a Linux hello.c file by Windows editor, after being compiled in Linux the ./a.out will not output the Chinese character correctly.  Again in Windows, after changing the charset by Notepad, I could see the correct output from the compiled hello.c in Linux within WSL2.

***VS Code***

```bash
$code <filename>
```

Starting VS Code in Windows and building remote connect to Linux with the Linux file be directly opened and ready to be edited on Windows. 



# WSL2 with Docker

## Docker for WSL2 Installation

[Offical Instruction](https://docs.docker.com/docker-for-windows/wsl/)

> with WIN10 1903 or higher
>
> with WSL2 enabled and installed

* Download Docker Desktop Installer 

* install and confirm to enable WSL2 windows features

* succeeded & close

* start Docker Desktop

* click icon and show dashboard

* prompt: no containers running and with a command offered

  docker run -d -p 80:80 docker/getting-started

* Setting > Resources > √ Enable integration with my default WSL distro

* General > √ Use the WSL2 based engine

* PowerShell: wsl -l to show the default Linux distro

* start Linux in WSL2 and try

  $ docker run -d -p 80:80 docker/getting-started

  $ docker run -it ubuntu bash -> to run a ubuntu in Docker

  $ apt update > atp install lsb-release > lsb_release -a -> to check the version

  it's working now

> In Setting > General,  it says that WSL2 provides better performance than legacy Hyper-V backend

## Docker Websites on WIN10

```bash
$docker ps
```

Linux | to show docker containers currently running

```
localhost:8080
```

WIN web browser|to show web server currently running on that port.  (the default port number is 80, just input localhost if the server is mapped to default localhost).  It'll time out if there's nothing running on that port.

```bash
$docker run -dp 8080:80 docker/getting-started:pwd
```

Linux | to start a docker container and to map port 8080 on the outside to port 80 on the docker container.

so when I connect to port 8080 on my localhost, it's going to map to port 80 on the docker container, the docker container has a web server running .

this will start the "getting-started" container.

```bash
$docker ps
```

Linux | now it shows this container with this name is currently created and running , and the local port 8080 is mapped to port 80 on the docker container

```
localhost:8080
```

WIN web browser | the website now displays.

```bash
$ docker run -dp 8081:80 --name mywebsite nginx
```

Linux | and try to run another container at the same time on local port 8081

```bash
$docker ps
```

Linux | to show the docker containers running

```
localhost:8081
```

WIN web browser | it shows the website is available

```bash
$docker run -dp 8082:80 uzyexe/tetris
```

Linux | to start another one

```
localhost:8082
```

WIN web browser | now tetris game is available

```bash
$docker
```

Linux | to see the options

```
$docker stop <CONTAINER ID>
```

Linux | to stop a container, `$docker ps` shows the container ID.

> get more docker containers from [Docker Website](www.hub.docker.com)

```bash
$docker pull ubuntu
```

Linux | to pull ubuntu docker official images

```bash
$docker run -it ubuntu bash	
```

Linux | to run ubuntu container and open up a bash shell

```bash
$lsb_release -a
```

Ubuntu container | it doesn't work  because lsb-release is not installed

```bash
$cat /etc/os-release
```

Ubuntu container | to see OS release information

> **explore more containers from Docker Hub**



# Windows Terminal

Customize more in settings

| Hotkeys          |                          |
| ---------------- | ------------------------ |
| alt+shift+"+"    | to split vertical pane   |
| alt+shift+"-"    | to split horizontal pane |
| ctrl+shift+w     | to close a tab           |
| alt+arrows       | to switch pane           |
| alt+shift+arrows | to adjust pane           |
|                  |                          |

