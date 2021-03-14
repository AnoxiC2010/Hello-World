# WSL2 Notes

# WSL2 Installation

## [Official Instruction](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10#simplified-installation-for-windows-insiders)

* Windows version 2004 or higher

* 2 windows features to be turned on via Control Panel or PowerShell

  Control Panel: √ Virtual Machine Platform

  `PowerShell`

  `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`

  Control Panel: √ Windows Subsystem for Linux

  `PowerShell`

  `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`

* Download the Linux kernel update package

* restart and set WSL 2 as the default version when installing a new Linux distribution

  `PowerShell`

  `wsl --set-default-version 2`

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

## Get Windows Terminal (it's nice)

Customize more in settings

| Hotkeys          |                          |
| ---------------- | ------------------------ |
| alt+shift+"+"    | to split vertical pane   |
| alt+shift+"-"    | to split horizontal pane |
| ctrl+shift+w     | to close a tab           |
| alt+arrows       | to switch pane           |
| alt+shift+arrows | to adjust pane           |
|                  |                          |



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

* 

## 和vmware等虚拟机同时用的问题

可以同时使用，但是在VMware的处理器中启用嵌套虚拟技术会启动不了

> VMware Player does not support nested virtualization on this host

VMware和wsl2同时使用会变慢

## GUI for WSL2

**A walking around ubuntu GUI before Microsoft release the built-in GUI for WSL2**

`$sudo apt update && sudo apt -y upgrade`

Ubuntu | update and upgrade

`$sudo apt install xrdp`

Ubuntu | to install rdp

`$sudo apt install -y xfce4`

Ubuntu | to install a light weight graphical user interface xfce4

After installed, Package Configuration will ask to choose a display manager between `gdm3` and `lightdm`, here I'm going to use `gdm3`  as David Bombal recommended.

`$sudo apt install -y xfce4-goodies`

Ubuntu | to install additional soft wares `xfce4-goodies`. 

`sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak`

Ubuntu | in order to do some configuration of xrdp, to back up the xrdp configuration file first.

(for restoring the backup if made mistakes.)

`$sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini`

Ubuntu | to change the default port of 3389 to 3390, to use a different port number to the default, maker sure there's no conflicts

(not necessary, just to follow Dalao's step)

`$sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini`

`$sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini`

Ubuntu | to increase the resolution by increasing the bits per pixel, the 2 commands just allow the quality of rdp session to be better

(not necessary, for basically rdping to myself locally, make sense to have good quality as Dalao demonstrated. )

`$echo xfce4-session > ~/.xsession`

Ubuntu | to save that to xsession.

`$sudo nano /etc/xrdp/startwm.sh`

```
//go right to the end to find these last two lines
test -x /etc/X11/Xsession && exec /etc/X11/Xsession
exec /bin/sh /etc/X11/Xsession
//to comment out the two lines
# test -x /etc/X11/Xsession && exec /etc/X11/Xsession
# exec /bin/sh /etc/X11/Xsession
//to add additional two lines.  ctrl+x to exit and save this file.
# xfce
startxfce4
```

Ubuntu | to edit this xrdp startup script by nano.

`$sudo /etc/init.d/xrdp start`

Ubuntu | to start up xrdp.

***That's been down, the RDP server is now started.***

**To test the connection:**

`Start Menue > Remote Desktop Connection`

WIN10 | to open remote desktop connection

`Computer: localhost:3390 or <server ip>:3390`

`Username: None Specified`

Remote Desktop Connection | to use the local host port 3390 I changed for RDP server to connect.

Click the connect and ignore the warning.

`Session: Xorg`

`username: *****`

`password: *****`

RDP session | I got an RDP session to my Ubuntu server now. To login in with the Ubuntu username and password.

Click Ok to login to the Ubuntu server.

Then I've RDPed or opened a remote desktop to my Ubuntu computer.

`$lsb_release -a`

`$ip addr`

Ubuntu CLI && RDP session terminal|  to check release info and IP address, the info shows they are the same server.

For it's a light weight GUI interface rather than the default, error shows when trying to open the browser.

`$sudo apt install firefox`

RDP session terminal | to install firefox as the default browser.

The web browser is now ready.

`Settings Manager > Appearance > Settings > Window Scaling > select 2 as the scaling factor`
`Settings Manager > Window Manager > Style > select Default-xhdpi theme`

RDP session | enable high dpi to fit my 3840x2160 display resolution.

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





