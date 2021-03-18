# Manjaro Linux

# VMware中安装

[设置参考](https://mrswolf.github.io/zh-cn/2019/05/24/manjaro%E8%B8%A9%E5%9D%91%E8%AE%B0/#%E5%AE%89%E8%A3%85base-devel)

VirtualBox下没有3840*2080分辨率 VMware下可选

更改pacman镜像 

利用pacman-mirrors选择镜像，或者手动在/etc/pacman.d/mirrorlist里添加

sudo pacman -S pacman-mirrors

中国区镜像排序，一般选择前两个镜像

sudo pacman-mirrors -i -c China -m rank

同步更新

新系统先更新一下

sudo pacman -Syy

sudo pacman -Syu

安装base-devel基础开发工具 

sudo pacman -S base-devel （含gcc）

Sudo pacman -S vim

不能铺满屏幕的问题：

安装后有bug只能在很小800X600左右分辨率窗口显示（不拉伸的话），更新后能适应屏幕，但是重启又变很小，[参考方法](https://github.com/Feliz-SZK/Linux-Decoded/blob/master/Fix%20Resolution%20Issue%20on%20Manjaro(Vmware%20Guest).Md)：

```bash
$sudo pacman -S open-vm-tools
$sudo pacman -Su xf86-input-vmmouse xf86-video-vmware mesa gtk2 gtkmm
$sudo echo needs_root_rights=yes >>/etc/X11/Xwrapper.config
$sudo systemctl enable vmtoolsd
$sudo systemctl start vmtoolsd
$sudo systemctl restart vmtoolsd //登录不全屏就直接执行
```

# ssh

开启ssh服务

`$ systemctl start sshd.service`

ssh服务开机自启

`$ systemctl enable sshd.service`

重启ssh服务

`$ systemctl restart sshd.service`



