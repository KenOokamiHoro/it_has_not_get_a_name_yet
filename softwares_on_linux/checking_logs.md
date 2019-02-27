# 日志是什么？在哪里？有什么用？

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

总而言之，如果程序或者系统出了错误，日志很可能会对错误的解决会有帮助。如果想向他人或者社区描述自己遇到的问题，也记得要（以合适的方式，参见后文）附上日志哟。

## 如何在设备上找到我的日志？

这是一本关于 GNU/Linux 的魔法书，我们就不提及 Windows 的系统日志通常可以在 `eventvwr.msc` 中查询啦。

从日志存储位置来分类，大体上可以把日志分为两种类型，一种是系统与服务的日志，一种是应用程序的日志。前者在多数较新的 Linux 发行版上目前由 systemd 处理并存储，后者通常由应用程序自行处理和存储。

### 应用程序日志

很多应用程序在运行中都会产生日志。有的会把日志存储下来，有的则会把日志丢弃。试着在终端运行一些图形程序 （例如 KDE 的文件管理器 `dolphin`），在程序启动后，你可能会在终端中看到类似这样的输出：

```
kf5.kio.core: We got some errors while running testparm "Load smb config files from /etc/samba/smb.conf\nError loading services.\n"
kf5.kio.core: We got some errors while running 'net usershare info'
kf5.kio.core: "Can't load /etc/samba/smb.conf - run testparm to debug it\n"
```

这些就是 `dolphin` 在运行时会列印出的日志的一部分。很多程序在运行时都会列印出日志，例如这个文件管理器。如果你从桌面上运行，该产生的日志仍然会产生，只不过因为没有人收集而被丢掉了。

这类日志可能有以下特征：

* 很长
* 如果存储在硬盘上的话，通常会以 `.log` 结尾
* 如果集中存储在一个文件夹里的话，文件夹的名字通常以 `log` 或者 `logs`开头/结尾。

不过，为了防止日志太多太杂，有些程序默认只会在出现错误或者警告的时候产生日志。如果你觉得日志太简洁了，看看程序有没有调整日志详细等级（verbosity）的选项或者类似 `--verbose`（常被简写为`-v`） 的参数，它们可能会让程序产生的日志更详细，对问题的解决更有帮助。

### 系统日志 （如果由 systemd 管理）

如果你安装的是比较新的 Linux（这里对“比较新”的定义是发布在 2014/15 年或以后，具体参见[维基百科](https://en.wikipedia.org/wiki/Systemd#Adoption)），那么这一章节对你适用。

> 还是不确定？试着在终端中执行  `systemctl` 或者 `journalctl`，如果没提示“命令未找到”的话，那就没问题哦。

在终端运行 `journalctl`  试试。你大概会看到类似这样的内容：

```
-- Logs begin at Sun 2019-02-24 22:01:15 HKT, end at Sun 2019-02-24 22:04:29 HKT. --
Feb 24 22:01:15 satania unbound[21229]: [21229:0] debug: Forward zone server list:
Feb 24 22:01:15 satania unbound[21229]: [21229:0] debug:    ip4 8.8.8.8 port 53 (len 1>
Feb 24 22:01:15 satania unbound[21229]: [21229:0] debug: Forward zone server list:
```

我们来分别说一下。

第一行会标明这段日志的开始时间和结束时间。由于默认会从最老的日志开始显示，这个时间可能会距离你查看日志时的时间很远，不要担心。

后三行就是日志的例子啦。分别是：

```
[日期] [时间] [主机名] [进程名][进程ID]: [日志内容]
```

（是的，这里日志的主机名夹杂了一点私货，Satania 最高！）

按上下左右方向键可以向各个方向翻页。按 q 退出，按 h 会显示帮助和一些“日志查看器”的命令（没错，这个日志查看器就是 `man` 文档的查看器 `less`）。

以上就是查看日志的基本方法。想查看特定服务/时间的日志？也可以！几个例子：

* 查看 `dnscrypt-proxy.service` 服务的日志：
  
  `jounalctl -u dnscrypt-proxy.service`

* 查看计算机在这次启动时的日志：

  `journalctl --boot=-1`

  （上一次就是 `--boot=-2`,依次类推）

## 如何向他人分享我的日志？

很短很短的日志可以直接粘贴到聊天室。如果日志太长（例如 5 行以上），建议通过在线的 pastebin 服务来分享。

首先将日志导出。可以直接使用类似于 `> /tmp/output.txt` 这样的方法啦。例如导出 `unbound` 的日志：

```shell
journalctl -u unbound.service > /tmp/output.txt
```

当然也可以手动复制粘贴到文件中（笑）。

在上传之前，检查一下自己的日志有没有包含什么敏感信息（例如自己的 IP）。

之后就可以上传啦。可以使用 [Fedora Pastebin](https://paste.fedoraproject.org/) 之类的站点。通常在站点上可以设置粘贴内容的保存时间。

当然我们也有比较 geeky 的办法！这里我们以 farseerfc 老师的 ptpb 实例 fars.ee 举个例子。

* 如果确定日志里没有什么敏感信息，又不想费劲存储到临时文件，可以用管道重定向直接让 curl 上传，例如：

  ```
  $ journalctl -u dnsmasq.service | curl -F c=@- http://fars.ee/
  ```

  返回内容中 `url` 那行，就是上传文本的链接啦。把这个链接发送给要分享的人或群组就行。

* 如果需要从临时文件上传日志，可以用类似这样的方式（`your.log`换成你要上传的文件名啦）：

  ```
  curl -F c=@- http://fars.ee/ < your.log
  ```

  和上面一样，返回内容中 `url` 那行是上传文本的链接。

  ## 如何进行日志清理？

  （这些命令需要 root 权限。）
* `journalctl --vacuum-time=<时间>` - 清理早于<时间>的所有日志

   可以接受 s/m/h/days/months/weeks/years 后缀。

* `journalctl --vacuum-size=<大小>` - 清理日志直至日志总大小不大于<大小>

   可以接受 K/M/G/T 后缀。

* `journalctl --vacuum-files=<文件数>` - 清理日志直至文件数不大于<文件数>

所有的日志清理操作均会按从早到晚的顺序进行清理，直至满足所有条件（是的，这几个参数可以同时使用）。
