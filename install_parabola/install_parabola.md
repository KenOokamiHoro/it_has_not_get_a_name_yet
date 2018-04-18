# 安装 Parabola GNU/Linux Libre

准备启动
--------------------------

重启电脑，然后让电脑从 U 盘启动。

    具体怎么搞还是要看电脑的硬件啦 😂

* BIOS 成功启动以后像这样

![BIOS](/assets/install_parabola/parabola_mbr.png)

选第一项。

* UEFI 成功启动以后像这样

![UEFI](/assets/install_parabola/parabola_uefi.png)

还是选第一项。😂

然后等待一会以后会出现……

![Shell](/assets/install_parabola/parabola_shell.png)

这就表示已经启动完毕啦  ~(>_<~)

> root是用戶名，前面那個數字是上一個命令的exit status啦，如果正常結束的命令exit status是0，就不會顯示出來，你有 1 2 127 這種都是某種東西報錯了.

> ---- 现任 Arch Linux TU 之一的 [farseerfc](https://www.zhihu.com/people/farseerfc) 在 https://www.zhihu.com/question/45329752/answer/98733823 中写到……

联网
----------------------------

首先当然是联网啦，如果是自动获取 IP 地址的有线网络，那么应该啥也不用做，ping 一下试试？

``` bash
root@parabolaiso ~ # ping archlinux.org
```
（把电脑用网线接到家里的路由器上就有相同的效果）

如果没网的话…… 😂

----

* 先用 `ip link` 确定一下网卡

``` bash
root@parabolaiso ~ # ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp4s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN mode DEFAULT group default qlen 1000
    link/ether c8:9c:dc:a8:ab:c3 brd ff:ff:ff:ff:ff:ff
3: wlp0s29u1u1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DORMANT group default qlen 1000
    link/ether 44:94:fc:0f:63:b9 brd ff:ff:ff:ff:ff:ff
```

这个例子里，lo 是本地环回不用管它，enp 开头的是有线网卡，wlp 开头的是无线网卡。

如果汝明明有无线网卡却没识别的话，有可能汝是大多数只有私有驱动的无线网卡厂商受害者😂😂

这时可以：

* 有 Android 手机的话，手机连 WiFi ，然后用“USB 网络共享”共享给电脑。
* 找个 USB 无线网卡插上 😂
* 连有线😂😂

如果有无线网卡的话，试试连接到 WiFi ……

* 输入 `wifi-menu` ,等一下会看到找到的 WiFi 网络的列表

![WIFI 列表](/assets/install_parabola/wifi_menu_0.png)

* 选择一个网络，保存网络配置文件

![保存配置文件](/assets/install_parabola/wifi_menu_1.png)

* 如果有密码的话，输入密码

![输入密码](/assets/install_parabola/wifi_menu_2.png)

* 然后按 Enter 确认，连到 WiFi 的话会返回 Shell。

谁叫 Parabola 和 Arch 一样，连不上网的话都装不了 😂

时间同步
---------------------

用 `timedatectl set-ntp true` 保证时间同步 。

``` bash

    root@parabolaiso ~ # timedatectl set-ntp true
    root@parabolaiso ~ # timedatectl status
        Local time: Fri 2016-10-28 17:39:42 UTC
    Universal time: Fri 2016-10-28 17:39:42 UTC
            RTC time: Fri 2016-10-28 17:39:42
        Time zone: UTC (UTC, +0000)
    Network time on: yes
    NTP synchronized: no
    RTC in local TZ: no

准备硬盘空间
-----------------------

这里用 cgdisk (UEFI)/ cfdisk (MBR) 来给硬盘分区。

两个看起来差不多所以咱偷会儿懒😂

首先输入 `lsblk` 看看汝的硬盘是哪个设备：

``` bash
root@parabolaiso ~ # lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 463.9G  0 disk
├─sda1   8:1    0   512M  0 part
├─sda2   8:2    0    16M  0 part
├─sda3   8:3    0 103.4G  0 part
└─sda4   8:4    0 253.4G  0 part
sdb      8:16   1   7.5G  0 disk
└─sdb1   8:17   1   7.5G  0 part /run/parabolaiso/bootmnt
sr0     11:0    1  1024M  0 rom
loop0    7:0    0 346.1M  1 loop /run/parabolaiso/sfs/airootfs
```

比如咱这里 sda 是咱的硬盘，于是运行 cgdisk 时加上 /dev/sda 这个参数：

> /dev 是一个虚拟文件夹（也就是并不在硬盘上），它会把电脑上的设备映射成一个个文件  _(:з」∠)_

``` bash
root@parabolaiso ~ # cgdisk /dev/sda
```

```

                                            cgdisk 1.0.1

                                        Disk Drive: /dev/sda
                                    Size: 972906545, 463.9 GiB

Part. #     Size        Partition Type            Partition Name
----------------------------------------------------------------
            1007.0 KiB  free space
1           512.0 MiB   EFI System                EFI system partition
2           16.0 MiB    Microsoft reserved        Microsoft reserved partition
3           103.4 GiB   Microsoft basic data      Basic data partition
4           253.4 GiB   Microsoft basic data      Basic data partition
            106.6 GiB   free space





    [ Align  ]  [ Backup ]  [  Help  ]  [  Load  ]  [  New   ]  [  Quit  ]  [ Verify ]  [ Write  ]
```
cgdisk 的界面大概像这样啦，用上下方向键把光标移动到汝之前的空闲空间上去（例如咱这里是最后一个）

新硬盘的话应该只有一个 free space 😂

用左右方向键把下面一排按钮上的光标移动到 New 上，然后按 Enter。

（这里看不出光标😂，黑色背景下光标应该是白的吧😂😂）

接下来会问几个问题（# 开头的是咱加上的注释😂）：

``` bash
# 数字可能和汝看到的不一样😂
# 起始扇区的位置，直接 Enter 就行
First sector (749424640-972906511, default = 749424640):
# 大小，可以是扇区数，也可以是实际的大小（例如 100M，20G一类的），要用掉整个剩余空闲空间的话，直接 Enter 就行。
Size in sectors or {KMGTP} (default = 223481872):
# 分区类型，默认的就好
# 但是如果要建立新的 EFI 系统分区的话 ，分区类型是 `ef00`
# 但是如果要建立新的 交换空间（就是虚拟内存啦）的话 ，分区类型是 `8200`
Current type is 8300 (Linux filesystem)
Hex code or GUID (L to show codes, Enter = 8300):
# 设置卷标，不设置也行。
Current partition name is ''
Enter new partition name, or <Enter> to use the current name:
```

然后汝应该会发现下面的空闲空间变成 Linux filesystem 了呗~

要保存分区表的话，用左右方向键把下面一排按钮上的光标移动到 Write 上，然后按 Enter。

``` bash
Are you sure you want to write the partition table to disk? (yes or no):

            Warning!! This may destroy data on your disk!
```
在这里输入 `yes` （就是 yes，不是 y Y YES 啥的😂），然后按 Enter。

然后下面会闪过一行 "The operation has completed successfully" ,这时就可以退出了。

用左右方向键把下面一排按钮上的光标移动到 Quit 上，然后按 Enter。

然而汝以为这样就结束了？还没格式化呢 (╯°Д°）╯︵/(.□ . )

创建文件系统+挂载
----------------------

首先还是用 `lsblk` 确定一下分区的名称，为了以防万一记得加上 -f 参数：

```
root@parabolaiso ~ # lsblk -f
NAME   FSTYPE   LABEL       UUID                                 MOUNTPOINT
sda
├─sda1 vfat               3C44-B4ED
├─sda2
├─sda3 ntfs               42E243C5E243BBC3
├─sda4 ntfs   新加卷      58741F29741F0A00
└─sda5
sdb
└─sdb1 vfat   PARA_201610 EAC8-F012                            /run/parabolaiso/bootmnt
sr0
loop0  squashfs                                                  /run/parabolaiso/sfs/airootfs
```
第一排分别表示设备名称，文件系统类型，卷标，UUID和挂载点。

咱这里的话 sda1 那个 vfat 分区就是 EFI 系统分区啦，sda5 就是刚刚新建的分区啦~（因为还没格式化所以没有文件系统😂）

用 `mkfs.ext4` 把那个分区格式化成 ext4 文件系统咯~

    记得自己看清楚是哪个分区别格式化错了 😂

``` bash
root@parabolaiso ~ # mkfs.ext4 /dev/sda5
mke2fs 1.43.3 (04-Sep-2016)
Creating filesystem with 27935234 4k blocks and 6987776 inodes
Filesystem UUID: a3943e57-6217-4a5f-8e57-ade5771315c0
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
    4096000, 7962624, 11239424, 20480000, 23887872

Allocating group tables: done
Writing inode tables: done
Creating journal (131072 blocks): done
Writing superblocks and filesystem accounting information: done

root@parabolaiso ~ #
```
等一排文字闪过就格式化完了……

> 如果要格式化新的 EFI 系统分区的话，用 `mkfs.vfat`
> 
> 如果要格式化新的 交换空间的话，用 `mkswap`

接下来用 mount 挂载分区啦~ （。＞ω＜）。

``` bash
# mount <设备名称> <目标文件夹>
# /mnt 挺合适的
root@parabolaiso ~ # mount /dev/sda5 /mnt
# 如果要挂载 EFI 系统分区的话，建议挂载到 /mnt/boot
# 所以先建立相应的文件夹
root@parabolaiso ~ # mkdir /mnt/boot
root@parabolaiso ~ # mount /dev/sda1 /mnt/boot
# 有交换空间的话不用挂载，用 swapon 命令。
root@parabolaiso ~ # swapon /dev/sda6
```

选择软件仓库镜像
----------------------------

    软件仓库（在Debian系发行版中，又叫做“软件源”）是软件包存储的地方。通常我们所说的软件仓库指在线软件仓库，亦即用户从互联网获取软件的地方。

用 nano 打开 `/etc/pacman.d/mirrorlist`

```bash

    root@archiso ~ # nano /etc/pacman.d/mirrorlist


      GNU nano 2.9.5                                   /etc/pacman.d/mirrorlist                                             

# Parabola GNU/Linux-libre - Last Updated: Wed Dec 20 02:59:35 GMT 2017

# Location: London, UK
# Responsible: Parabola Project
# Note: Not really a mirror, automatically redirects you to an Arch
#       mirror when possible. Works best specifying it a few times in a
#       row (404s workaround).
# Server = http://redirector.parabola.nu/$repo/os/$arch
Server = https://redirector.parabola.nu/$repo/os/$arch

# Location: Reykjavík, Iceland
# Responsible: Parabola Project
# Note: Not really a mirror, automatically redirects you to a Parabola
#       mirror that has the file you are looking for.
# Server = http://repomirror.parabola.nu/$repo/os/$arch
Server = https://repomirror.parabola.nu/$repo/os/$arch

# Location: Falkenstein, Germany
# Server = http://mirror.grapentin.org/parabola/$repo/os/$arch
                                                   [ Read 54 lines ]
^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo       M-A Mark Text
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Spell    ^_ Go To Line  M-E Redo       M-6 Copy Text
```

这是 GNU nano 的主界面，然而 Parabola 国内并没有镜像……于是就看样子把需要的行前的 # 号删除吧……

（所谓的 ^K 就是 Ctrl+K ，就是这两个键一起按😂）

输入完以后按下 Ctrl+O 写入，按 Enter 确定，再按 Ctrl+X 退出。

然后用 `pacman -Syy 刷新一下软件包数据库。


更新钥匙环
--------------------------

保险起见……

``` bash
root@parabolaiso ~ # pacman -Sy parabola-keyring
```

安装基本系统
-----------------------------

用 pacstrap 安装基本系统，默认会安装 base 组，要通过 AUR 或者 ABS 编译安装软件包,还需要安装 base-devel 啦：

    如果需要连接无线网络的话，记得装上 iw dialog wpa_supplicant wpa_actiond

``` bash
root@parabolaiso ~ # pacstrap /mnt base base-devel iw dialog wpa_supplicant wpa_actiond
```

这个组并没有包含全部 live 环境中的程序，有些需要额外安装，
其他软件以后会用 pacman 再安装啦~

安装完以后大概会是这个样子 (\´・ω・\｀)

``` bash
pacstrap /mnt base base-devel   29.09s user 2.61s system 85% cpu 37.271 total
```

准备进入 chroot 环境
-----------------------------

生成 fstab 啦 ~

``` bash

# genfstab
usage: genfstab [options] root

Options:
    -L             Use labels for source identifiers (shortcut for -t LABEL)
    -p             Exclude pseudofs mounts (default behavior)
    -P             Include printing mounts
    -t TAG         Use TAG for source identifiers
    -U             Use UUIDs for source identifiers (shortcut for -t UUID)

    -h             Print this help message

genfstab generates output suitable for addition to an fstab file based on the
devices mounted under the mountpoint specified by the given root.

root@parabolaiso ~ # genfstab -U /mnt >> /mnt/etc/fstab
```

然后向新系统出发~
``` bash
root@parabolaiso ~ # arch-chroot -help
usage: arch-chroot chroot-dir [command]

    -h                  Print this help message
    -u <user>[:group]   Specify non-root user and optional group to use

If 'command' is unspecified, arch-chroot will launch /bin/bash.
root@parabolaiso ~ # arch-chroot /mnt /bin/bash
```

设置基本系统
-------------------

    # 开头只表示以 root 用户运行，汝不用把 # 输入到终端里啦~

* 设置时区（中国的时区是 Asia/Shanghai，如果想设置成其它时区的话，可以 `ls /usr/share/zoneinfo ` 查看详细的时区列表。）

``` bash
# ln -s <源文件> <目标> 创建一个符号链接
# ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

* 设置时间标准 为 UTC，并调整 时间漂移:

``` bash
    # hwclock --systohc --utc
```
* /etc/locale.gen 是一个仅包含注释文档的文本文件。指定您需要的本地化类型，去掉对应行前面的注释符号（＃）就可以啦，用 nano 打开，建议选择帶UTF-8的項：

``` bash

    # nano /etc/locale.gen

    en_US.UTF-8 UTF-8
    zh_CN.UTF-8 UTF-8
    zh_TW.UTF-8 UTF-8

```
* 执行 locale-gen 以生成 locale 讯息：

``` bash
    # locale-gen
```

* 创建 locale.conf 并提交您的本地化选项：

    将系统 locale 设置为en_US.UTF-8，系统的 Log 就会用英文显示，这样更容易问题的判断和处理。用户可以设置自己的 locale。

    警告: 不推荐在此设置任何中文locale，或导致tty乱码。

``` bash
    # echo 用来输出某些文字，后面的大于号表示把输出保存到某个文件里啦~
    # 或者可以用文字编辑器新建这个文件加上这一行。
    # echo LANG=en_US.UTF-8 > /etc/locale.conf
```
* 设置一个喜欢的主机名（用汝的主机名代替 myhostname ）：

``` bash
    # echo myhostname > /etc/hostname
```

* 设置 root 的密码（输入密码的时候就是啥也没有 ╮(￣▽￣)╭ ）：

``` bash
    [root@parabolaiso /]# passwd
    New password:
    Retype new password:
    passwd: password updated successfully
```
* 安装启动管理器（例如 GRUB ）：

    * UEFI 用户先再安装几个必要的软件包咯~

    > \# pacman -S efibootmgr dosfstools

    * 然后安装 GRUB

    >\# pacman -S grub os-prober

* 把 GRUB 安装到硬盘：

``` bash
    # MBR 用户这么做 （记得用汝自己硬盘的名称代替 sda ，不要带上表示分区的数字啦~）：

    # grub-install --target=i386-pc /dev/sda --recheck

    # UEFI 用户这么做：

    # grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub --recheck
```

EFI 安装成功以后大概像这样 😂

``` bash

    [root@parabolaiso /]# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub --recheck
    Installing for x86_64-efi platform.
    Installation finished. No error reported.
```

然后生成必要的配置文件：

``` bash

    [root@parabolaiso /]# grub-mkconfig -o /boot/grub/grub.cfg
    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-linux
    Found initrd image(s) in /boot: initramfs-linux.img
    Found fallback initrd image(s) in /boot: initramfs-linux-fallback.img
    done
```

安装桌面环境 
------------------------------------------

* 安装桌面环境需要的基础包 （就是 xorg 啦）

``` bash
    [root@parabolaiso /]# pacman -S xorg
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

* 接下来装 GNOME 桌面：

> \# pacman -S gnome

* 桌面环境大多数使用 NetworkManager ：

> \# pacman -S networkmanager

* 然后安装中文字体（这里安装的是 Google 的 Noto 系列，更多的字体可以在 https://wiki.archlinux.org/index.php/Fonts_(简体中文) 找到。
）
> \# pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji

* 和中文输入法 （这里安装的是 [中州韵输入法引擎](http://rime.im) ，因为 GNOME 和 ibus 结合的最好，所以更多的 ibus 输入法引擎可以在 https://wiki.archlinux.org/index.php/ibus_(简体中文) 找到）。

> \# pacman -S ibus-rime

## 新建一个用户
``` bash
    -m 为新用户创建一个文件夹，-s 设置用户的登录 Shell
    记得最后是用户名就好 😂
    # useradd -m -s /bin/bash  horo
    然后设置密码
    # passwd horo
```

## 为新用户设置 sudo 

    sudo(substitute user do) 使得系统管理员可以授权特定用户或用户组作为 root 或他用户执行某些（或所有）命令，同时还能够对命令及其参数提供审核跟踪。

    用户可以选择用 su 切换到 root 用户运行命令，但是这种方式会启动一个 root shell 并允许用户运行之后的所有的命令。而 sudo 可以针对单个命令、仅在需要时授予临时权限，减少因为执行错误命令损坏系统的可能性。sudo 也能以其他用户身份执行命令并且记录用户执行的命令，以及失败的权限申请。

出于安全考虑，大多数时候都应该用 `visudo` 命令来修改 /etc/sudoers 文件而不是直接修改那个文件。可以通过设置 EDITOR 环境变量来指定使用啥编辑器（默认是 vim）

``` bash
    EDITOR=nano visudo
```

这里咱把这一行取消注释，然后保存：

``` text
# %wheel      ALL=(ALL) NOPASSWD: ALL
```

接着把刚才新建的用户添加到 wheel 组中：

``` bash
# gpasswd -a 用户名 要添加到的用户组
# -a 就是 --add 啦 ~
gpasswd -a horo wheel
```

## 激活需要的服务（显示管理器啦，GNOME 里用的是 GDM）
``` bash
    # systemctl enable gdm
```
当然还有 NetworkManager：
``` bash
    # systemctl enable NetworkManager
```
    （这个里面有大写😂）

完工啦~
---------------

* 离开 chroot 环境： `# exit`

* 卸载挂载的分区，（其实不是必须的，因为马上就重启啦~） `# umount -R /mnt`

* 重新启动，准备迎接新的系统吧  ~(>_<~)