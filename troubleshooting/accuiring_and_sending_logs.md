# 获得和发送可以用于调试和问题排除的信息

## 那么问题来了，啥样的东西可以帮助调试呢？

大概有详细输出、日志和堆栈跟踪吧，看情况而定。

## 所以日志是啥玩意？

> **log**
>
> US [lɔɡ] / UK [lɒɡ]
>
> ① n. 日志
>
> ② v. 记录
> 
> （其余释义略）

根据维基百科的说法（原文稍有改动），日志是一个或多个自动创建和维护的日志文件，其中包含其所执行活动的列表。这样的一个“列表”，在平时可能只被看作占用硬盘空间的无用文件，但在设备发生问题的时候，日志经常可以提供很关键的信息，帮助维护者定位和解决问题。

除了应用自己可以在不同的地方里写日志以外， Linux 内核还提供了 syslog 接口（？）用来把日志放在一起（？？），当然作为
加入了 Systemd 全家桶的 Arch，当然用的是 Systemd 提供的 journal 咯~

总而言之，如果程序或者系统出了错误，日志很可能会对错误的解决会有帮助。如果想向他人或者社区描述自己遇到的问题，也记得要（以合适的方式，参见后文）附上日志哟。

## 等等啥叫详细输出？

大多数程序都可以通过一些命令行选项来控制往终端或是其它什么输出信息的详（luo）细（suo）程度，
通常是 `-v` 或者 `--verbose` 一类的参数

> openssh： -vvv 了解一下？

```text
$ ssh -vvvvv horo.yoitsu.moe
OpenSSH_7.9p1, OpenSSL 1.1.1a  20 Nov 2018
debug1: Reading configuration data /home/horo/.ssh/config
debug1: /home/horo/.ssh/config line 9: Applying options for horo.yoitsu.moe
debug1: Reading configuration data /etc/ssh/ssh_config
debug2: resolving "horo.yoitsu.moe" port 22
debug2: ssh_connect_direct
debug1: Connecting to horo.yoitsu.moe port 22.
debug1: Connection established.
debug1: identity file /home/horo/.ssh/id_yoitsu type -1
debug1: identity file /home/horo/.ssh/id_yoitsu-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_7.9
debug1: Remote protocol version 2.0, remote software version OpenSSH_7.9
debug1: match: OpenSSH_7.9 pat OpenSSH* compat 0x04000000
debug2: fd 3 setting O_NONBLOCK
debug1: Authenticating to horo.yoitsu.moe:22 as 'horo'
debug3: hostkeys_foreach: reading file "/home/horo/.ssh/known_hosts"
debug3: record_hostkey: found key type ECDSA in file /home/horo/.ssh/known_hosts:72
debug3: load_hostkeys: loaded 1 keys from horo.yoitsu.moe
```
以下省略若干行……

有些 GUI 应用其实也是会往标准输出写些字的，如果汝在终端模拟器里运行的话应该也会看到：

```text
$ firefox-nightly --verbose

###!!! [Child][RunMessage] Error: Channel closing: too late to send/recv, messages will be lost

IPDL protocol Error: Received an invalid file descriptor
IPDL protocol Error: Received an invalid file descriptor
IPDL protocol Error: Received an invalid file descriptor
IPDL protocol Error: Received an invalid file descriptor
[Parent 31983, Gecko_IOThread] WARNING: pipe error (62): 连接被对方重设: file /builds/worker/workspace/build/src/ipc/chromium/src/chrome/common/ipc_channel_posix.cc, line 357

###!!! [Parent][MessageChannel] Error: (msgtype=0x1E0089,name=PBrowser::Msg_Destroy) Closed channel: cannot send/recv


###!!! [Child][MessageChannel] Error: (msgtype=0x34010E,name=PContent::Msg_DetachBrowsingContext) Closed channel: cannot send/recv

[Parent 31983, Gecko_IOThread] WARNING: pipe error (146): 连接被对方重设: file /builds/worker/workspace/build/src/ipc/chromium/src/chrome/common/ipc_channel_posix.cc, line 357
[Parent 31983, Gecko_IOThread] WARNING: pipe error (166): 连接被对方重设: file /builds/worker/workspace/build/src/ipc/chromium/src/chrome/common/ipc_channel_posix.cc, line 357
```

## 堆栈跟踪又是啥？

    # TODO ：不过汝都知道堆栈跟踪了……

## 所以我到底该怎么看到日志呢？

> 首先去看应用开发者的文档啊（大嘘）

按照文件系统层次结构标准的话， `/var/log` 是应用放置日志的主要目录。

```bash
$ ls /var/log
audit     btmp    cups     firewalld  glusterfs  journal  libvirt  old         private  speech-dispatcher  teamviewer14
boot.log  btmp.1  faillog  gdm        httpd      lastlog  mongodb  pacman.log  privoxy  swtpm              wtmp
```

除了 journal ，大部分都是文本文件，用 cat 一类可以和文本文件互动的命令或者文字编辑器都可以打开，例如 pacman 的日志：

```bash
$ cat /var/log/pacman.log 
...
[2019-03-01 15:19] [ALPM] running 'systemd-sysusers.hook'...
[2019-03-01 15:19] [ALPM] running 'systemd-tmpfiles.hook'...
[2019-03-01 15:19] [ALPM] running 'systemd-udev-reload.hook'...
[2019-03-01 15:19] [ALPM] running 'systemd-update.hook'...
[2019-03-01 15:19] [ALPM] running 'texinfo-install.hook'...
[2019-03-01 15:19] [ALPM] running 'update-appstream-cache.hook'...
[2019-03-01 15:19] [ALPM-SCRIPTLET] AppStream cache update completed successfully.
[2019-03-01 15:19] [ALPM] running 'update-desktop-database.hook'...
[2019-03-01 15:19] [ALPM] running 'update-mime-database.hook'...
[2019-03-01 15:19] [ALPM] running 'update-vlc-plugin-cache.hook'...
```

对于一些服务型的应用（例如有 Systemd 系统单元的），可以通过 `journalctl` 命令查找日志：

> 为啥刚刚说除了 /var/log/journal 以外都能用文字编辑器查看呢，因为 journald 是用一种定制的二进制格式存储的。

直接运行 journalctl 的话，大概是这个效果

```bash
$ journalctl
-- Logs begin at Tue 2018-11-27 14:45:18 CST, end at Fri 2019-03-01 15:57:43 CST. --
11月 27 14:45:18 archlinux kernel: Linux version 4.19.2-arch1-1-ARCH (builduser@heftig-16038) (gcc version 8.2.1 20180831 (GCC)) #1 SMP PREEMPT Tue Nov 13 21:16:19 UTC 2018
11月 27 14:45:18 archlinux kernel: Command line: initrd=\initramfs-linux.img rd.luks.name=d4fb86ef-aa66-4a19-8df8-dcd2309eb51e=sysroot root=/dev/mapper/arch-base rootflags=compress=lzo
11月 27 14:45:18 archlinux kernel: KERNEL supported cpus:
11月 27 14:45:18 archlinux kernel:   Intel GenuineIntel
11月 27 14:45:18 archlinux kernel:   AMD AuthenticAMD
11月 27 14:45:18 archlinux kernel:   Centaur CentaurHauls
11月 27 14:45:18 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x001: 'x87 floating point registers'
11月 27 14:45:18 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x002: 'SSE registers'
11月 27 14:45:18 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x004: 'AVX registers'
11月 27 14:45:18 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x008: 'MPX bounds registers'
11月 27 14:45:18 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x010: 'MPX CSR'
11月 27 14:45:18 archlinux kernel: x86/fpu: xstate_offset[2]:  576, xstate_sizes[2]:  256
11月 27 14:45:18 archlinux kernel: x86/fpu: xstate_offset[3]:  832, xstate_sizes[3]:   64
11月 27 14:45:18 archlinux kernel: x86/fpu: xstate_offset[4]:  896, xstate_sizes[4]:   64
11月 27 14:45:18 archlinux kernel: x86/fpu: Enabled xstate features 0x1f, context size is 960 bytes, using 'compacted' format.
```

它会调用预设的查看器来展示系统日志（默认是 less），如果汝有用过这个命令或者是查看过手册页的话应该不会陌生。 可以用键盘的方向键来上下移动，如果看完了可以按 q 键退出。

    更多的操作可以在 less 里输入 h 来打开 less 的帮助来查看。

想查看特定服务/时间的日志？也可以！几个例子：

* 查看某个服务的日志（例如 libvirtd）：
  
```bash
$ jounalctl -u libvirtd.service
$ journalctl -u libvirtd
-- Logs begin at Tue 2018-11-27 14:45:18 CST, end at Fri 2019-03-01 15:59:30 CST. --
12月 04 14:37:58 yoitsu_optiplex systemd[1]: Started Virtualization daemon.
12月 04 14:37:58 yoitsu_optiplex libvirtd[5931]: 2018-12-04 06:37:58.460+0000: 5953: info : libvirt version: 4.8.0
12月 04 14:37:58 yoitsu_optiplex libvirtd[5931]: 2018-12-04 06:37:58.460+0000: 5953: info : hostname: yoitsu_optiplex
12月 04 14:37:58 yoitsu_optiplex libvirtd[5931]: 2018-12-04 06:37:58.460+0000: 5953: error : virFirewallValidateBackend:191 : direct firewall backend requested, but /sbin/ebtables is not available: No such file or directory
...
```

* 显示从某个日期 ( 或时间 ) 开始的消息:

    journalctl --since="2012-10-30 18:17:16"

* 显示从某个时间 ( 例如 20分钟前 ) 的消息:

    journalctl --since "20 min ago"

* 查看计算机在这次启动时的日志：

```bash
$ journalctl -b -1
-- Logs begin at Tue 2018-11-27 14:45:18 CST, end at Fri 2019-03-01 16:01:14 CST. --
3月 01 13:27:38 archlinux kernel: Linux version 5.0.0-rc5-mainline (builduser@lilydjwg) (gcc version 8.2.1 20181127 (GCC)) #1 SMP PREEMPT Tue Feb 5 02:06:10 CST 2019
3月 01 13:27:38 archlinux kernel: Command line: initrd=\initramfs-linux-mainline.img rd.luks.name=ba0891b2-c49d-47f3-8b21-322844031127=sysroot root=/dev/mapper/sysroot rootflags=subvol=arch_surfacebook_root quiet splash loglevel=3 rd.udev.log-priority=3 vt.global_cursor_default=0 intel_iommu=on
3月 01 13:27:38 archlinux kernel: KERNEL supported cpus:
3月 01 13:27:38 archlinux kernel:   Intel GenuineIntel
3月 01 13:27:38 archlinux kernel:   AMD AuthenticAMD
3月 01 13:27:38 archlinux kernel:   Hygon HygonGenuine
3月 01 13:27:38 archlinux kernel:   Centaur CentaurHauls
3月 01 13:27:38 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x001: 'x87 floating point registers'
3月 01 13:27:38 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x002: 'SSE registers'
3月 01 13:27:38 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x004: 'AVX registers'
3月 01 13:27:38 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x008: 'MPX bounds registers'
3月 01 13:27:38 archlinux kernel: x86/fpu: Supporting XSAVE feature 0x010: 'MPX CSR'
...
```

* 如果需要的话，可以在最后加上 -r 参数来让最新的记录在最上面：

```bash
$ journalctl -u privoxy -b -1 -r
-- Logs begin at Tue 2018-11-27 14:45:18 CST, end at Fri 2019-03-01 16:03:48 CST. --
3月 01 14:26:03 yoitsu-surfacebook systemd[1]: Stopped Privoxy Web Proxy With Advanced Filtering Capabilities.
3月 01 14:26:03 yoitsu-surfacebook systemd[1]: privoxy.service: Failed with result 'exit-code'.
3月 01 14:26:03 yoitsu-surfacebook systemd[1]: privoxy.service: Main process exited, code=exited, status=15/n/a
3月 01 14:26:03 yoitsu-surfacebook systemd[1]: Stopping Privoxy Web Proxy With Advanced Filtering Capabilities...
3月 01 13:28:06 yoitsu-surfacebook privoxy[578]: 2019-03-01 13:28:06.532 7fd130a95100 Info: Program name: /usr/bin/privoxy
3月 01 13:28:06 yoitsu-surfacebook privoxy[578]: 2019-03-01 13:28:06.531 7fd130a95100 Info: Privoxy version 3.0.26
3月 01 13:28:06 yoitsu-surfacebook systemd[1]: Started Privoxy Web Proxy With Advanced Filtering Capabilities.
```

<s>emmm 好像失败了哈</s>

## 那我要怎么把这一把字发给找我要这些的人呢？

如果汝是通过 IM 发送这些的话：

* 不要直接大段大段复制然后粘贴到聊天软件的对话框中（特别是某些协议没有多行支持的 IM）

* 不要拍照（摩尔纹和频闪伤不起啊）

* 不要截图（因为不少终端模拟器的默认宽度可能放不下一整行）

    <s>那还有别的方法嘛？？</s>

* 方法还是有的，那就是老生常谈的 Pastebin 啦~ 
    
    然后把 Pastebin 给汝生成的链接贴到聊天软件里去，最好再注明一下。

如果汝是通过邮件发送日志的话：

* 看邮件列表的要求，有的邮件列表要求日志作为附件发送，有的可能会要求放在正文里。

* 不要截断原来日志里的行，或者直接以纯文本格式发送。

## 发送多少合适？

其实并没有一个硬性的要求提交的日志一定要多长或者多短，不过太长的话
看着费劲，太短的话又有可能发现不了问题在哪里…… 😅

* 最关键的是不要自己修改或者删减日志（万一汝把暴露汝问题的那行删掉了呢？）

    不过删除一些敏感信息（例如 IP 地址和 UUID 什么的）大概是可行的
    （只要汝``确切``的知道汝在干啥）

* 如果可以的话，尽可能提供问题发生时期的日志。

    例如解决刚刚发生的问题的话，上个星期的日志可能就没什么用处了。
    可以考虑用 `tail` 或者 `awk` 一类的命令限制输出的范围，对于 `journal` 的话，充分利用 `--since` / `-u` 一类的参数限制输出范围。

> 这样做的用处至少有三点。 第一，表现出你为简化问题付出了努力，这可以
> 使你得到回答的机会增加； 第二，简化问题使你更有可能得到有用的答案；
> 第三，在精炼你的 bug 报告的过程中，你很可能就自己找到了解决方法或权宜之计。