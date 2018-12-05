# 进行一些基本设置

## 启动和登录

重启以后汝大概首先会看到这个：

![](/assets/post_installation_configuration/grub.png)

选择第一项启动，或者汝什么都不做等着它也行。

然后汝就会看到……

![](/assets/post_installation_configuration/login.png)

输入 root 和汝刚刚设置的密码登录：

![](/assets/post_installation_configuration/tty.png)

## 创建用户和设置 sudo 

    所以这又是个啥玩意？ （´＿｀）

    因为 root 用户的权力很大而且很危险，所以轻易不会用到它 （。＞ω＜）。

    所以就有了 sudo(substitute user do) 使得系统管理员可以授权特定用户或用户组作为 root 
    或其他用户执行某些（或所有）命令，同时还能够对命令及其参数提供审核跟踪。 

    （话风突变）

sudo 应该已经作为 base-devel 的一部分装上去了，如果没有的话汝也可以自己手动安装一下：

```bash
[root@archlinux /] # pacman -S sudo 
```

让咱们首先从创建用户开始，可以使用 `useradd` 命令：

    -m 为新用户创建一个目录，-s 设置用户的登录 Shell -G 会把新建的用户追加到某个组中
  （这里是刚刚 sudo 里的那个 wheel），记得最后是用户名就好 😂

```bash
[root@archlinux /]# useradd -m -G wheel -s /bin/bash horo
```
然后设置密码（和 root 那时候一样输入两次）
```bash
[root@archlinux /]# passwd horo
```
sudo 的配置文件是 /etc/sudoers ，但是咱们不会直接去编辑它（因为一旦搞坏了不好修）。
所以有一个 visudo 的命令用来代理编辑它（就是先编辑一个临时文件，然后检查有没有错误，
一切 OK 后再覆盖）。

嗯…… visudo？ 如果汝觉得 vi 哪里眼熟的话，没错！ 这个命令默认会用 vi 去编辑那个文件。
如果汝想用其他的编辑器（例如刚刚用的 nano）的话，可以通过一个环境变量：

```bash
[root@archlinux /]# EDITOR=nano visudo
```

现在大概像这个样子：

```text

GNU nano 3.2                                        /etc/sudoers.tmp                                                  

## sudoers file.
##
## This file MUST be edited with the 'visudo' command as root.
## Failure to use 'visudo' may result in syntax or file permission errors
## that prevent sudo from running.
##
## See the sudoers man page for the details on how to write a sudoers file.
##

##
## Host alias specification
##
## Groups of machines. These may include host names (optionally with wildcards),
## IP addresses, network numbers or netgroups.
# Host_Alias    WEBSERVERS = www1, www2, www3

##
## User alias specification
##
                                                [ Read 97 lines ]
^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo       M-A Mark Text
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Spell    ^_ Go To Line  M-E Redo       M-6 Copy Text
```

虽然这个文件有很多行，但是咱们还是先从让它能够工作开始来最小的修改它。

找到下面的这一行，然后把 %wheel 前面的注释符号（#）去掉，不过百分号要留下：


```bash
## Uncomment to allow members of group wheel to execute any command
# %wheel ALL=(ALL) ALL
```
然后就可以保存退出啦~ （效果就是注释里说明的，给 wheel 组执行所有命令的权限）

最后一步是用 `passwd -l` 来锁住 root 用户（毕竟以后几乎咱们不会用 root 登录了啦~）

```bash
root@yoitsu_optiplex ~ # passwd -l root
passwd: password expiry information changed.
```

接下来可以注销然后用新创建的用户登录试试？

如果汝想进一步了解 sudo 的配置的话，还是去看 ArchWiki: https://wiki.archlinux.org/index.php/Sudo

## 安装 GNOME 桌面环境

* 安装桌面环境需要的基础包 （就是 xorg 啦）

```bash

[root@archlinux /]# pacman -S xorg gnome
:: There are 80 members in group xorg:
:: Repository extra
1) xf86-input-evdev  2) xf86-input-joystick  3) xf86-input-keyboard  4) xf86-input-libinput
5) xf86-input-mouse  6) xf86-input-synaptics  7) xf86-input-vmmouse  8) xf86-input-void
9) xf86-video-amdgpu  10) xf86-video-ark  11) xf86-video-ati  12) xf86-video-dummy
13) xf86-video-fbdev  14) xf86-video-glint  15) xf86-video-i128  16) xf86-video-intel
17) xf86-video-mach64  18) xf86-video-neomagic  19) xf86-video-nouveau  20) xf86-video-nv
21) xf86-video-openchrome  22) xf86-video-r128  23) xf86-video-savage  24) xf86-video-siliconmotion
25) xf86-video-sis  26) xf86-video-tdfx  27) xf86-video-trident  28) xf86-video-vesa
29) xf86-video-vmware  30) xf86-video-voodoo  31) xorg-bdftopcf  32) xorg-docs  33) xorg-font-util
34) xorg-fonts-100dpi  35) xorg-fonts-75dpi  36) xorg-fonts-encodings  37) xorg-iceauth
38) xorg-luit  39) xorg-mkfontdir  40) xorg-mkfontscale  41) xorg-server  42) xorg-server-common
43) xorg-server-devel  44) xorg-server-xdmx  45) xorg-server-xephyr  46) xorg-server-xnest
47) xorg-server-xvfb  48) xorg-server-xwayland  49) xorg-sessreg  50) xorg-setxkbmap
51) xorg-smproxy  52) xorg-x11perf  53) xorg-xauth  54) xorg-xbacklight  55) xorg-xcmsdb
56) xorg-xcursorgen  57) xorg-xdpyinfo  58) xorg-xdriinfo  59) xorg-xev  60) xorg-xgamma
61) xorg-xhost  62) xorg-xinput  63) xorg-xkbcomp  64) xorg-xkbevd  65) xorg-xkbutils  66) xorg-xkill
67) xorg-xlsatoms  68) xorg-xlsclients  69) xorg-xmodmap  70) xorg-xpr  71) xorg-xprop
72) xorg-xrandr  73) xorg-xrdb  74) xorg-xrefresh  75) xorg-xset  76) xorg-xsetroot  77) xorg-xvinfo
78) xorg-xwd  79) xorg-xwininfo  80) xorg-xwud

Enter a selection (default=all):
```
这时会让汝选择需要哪些软件包啦,其实大多数时候默认的就行……

* 然后安装中文字体（ 同样 pacman -S  😋）

    Google Noto Fonts 系列： noto-fonts noto-fonts-cjk noto-fonts-emoji

    思源黑体：adobe-source-han-sans-otc-fonts

    文泉驿：wqy-microhei wqy-zenhei

更多的字体可以在 https://wiki.archlinux.org/index.php/Fonts_(简体中文) 找到。

* 以及中文输入法：（ `ibus-libpinyin` 或 `ibus-rime` ）

    待会儿会从设置中添加输入法。

## 激活需要的服务（显示管理器啦）

    systemd 是 Arch Linux 现在唯一的 init 程序，当内核加载完毕后，首先被加载的
    就是 init （这里就是 systemd 啦）。然后 systemd 会接着加载剩下的部分。

    https://wiki.archlinux.org/index.php/systemd

可以使用 `systemctl enable` 来设置服务在开机时启动：

    gdm 是 GNOME 的显示管理器， NetworkManager 用来在 GNOME 中管理网络连接。

```bash
root@yoitsu_optiplex ~ # systemctl enable gdm NetworkManager
Created symlink /etc/systemd/system/display-manager.service → /usr/lib/systemd/system/gdm.service.
Created symlink /etc/systemd/system/dbus-org.freedesktop.NetworkManager.service → /usr/lib/systemd/system/NetworkManager.service.
Created symlink /etc/systemd/system/multi-user.target.wants/NetworkManager.service → /usr/lib/systemd/system/NetworkManager.service.
Created symlink /etc/systemd/system/dbus-org.freedesktop.nm-dispatcher.service → /usr/lib/systemd/system/NetworkManager-dispatcher.service.
Created symlink /etc/systemd/system/network-online.target.wants/NetworkManager-wait-online.service → /usr/lib/systemd/system/NetworkManager-wait-online.service.
```

## 通过 gdm 登录

再次重启以后汝大概就能见到 gdm 了：

![](/assets/post_installation_configuration/gdm_0.png)

点击汝的用户名，然后输入用户名和密码登录：

    下面的齿轮图标可以选择登录到哪一个桌面，大多数情况下第一个 GNOME 就够用了 

![](/assets/post_installation_configuration/gdm_1.png)

----

## 欢迎来到 Arch Linux 的世界！


    Cheers！ 汝刚刚成功的安装 Arch Linux 到汝的电脑中了呐！

那接下来要干啥呢？

* [ArchWiki](https://wiki.archlinux.org/) 是 Arch Linux 文档的集中地，
  遇到了问题或是只想找些好玩的？去翻翻吧，例如 
  [常见操作](https://wiki.archlinux.org/index.php/General_recommendations_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
  和 [(应用列表](https://wiki.archlinux.org/index.php/List_of_applications_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

* 汝希望找个组织？可以来 [中文社区交流群](http://fars.ee/~readme.html) ,
  或者 [更国际化（？）的 IRC 频道](https://wiki.archlinux.org/index.php/IRC_channel)。
