# Arch Linux notes



# TTY分辨率问题

Arch Linux 5.11.16-arch-1 (tty1)

Vmware workstation pro16

```
来自youtube，实测没有用
1.修改grub配置文件
$ sudo vim /etc/default/grub
modify: 
GRUB_GFXMODE=auto
to:
GRUB_GFXMODE=3840x2160x32
save and exit

2.把配置改动推送到boot grub目录的grub配置文件
$ sudo grub-mkconfig -o /boot/grub/grub.cfg
```



# 命令

`$ sudo !!`

用管理员权限执行上一条命令（用在上一条忘了加sudo时）

`$ neofetch`

展示系统信息，需要安装

`$ htop`

展示进程，像任务管理器，需要安装

`lsb_release -a`

展示版本信息，需要安装

