# WSL2 Notes

# Installation

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

# WSL2 with Docker

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



