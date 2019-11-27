# 有关 root 的二三事

相信大部分人对“root 权限”的认识，是从 Android 开始的。大多数人的印象大概是：root 权限是一个非常高也非常危险的权限，使用这个权限的时候，一不小心就会把系统搞坏。

前半句没错，后半句——GNU/Linux（尤其是给桌面用户用的那些发行版），在这方面还是比较友好的。

## 什么是 root 权限？

这里的 root 权限，实际上指的是名为 root 的账户所具有的权限。“获取 root 权限”，指的实际是获取 root 账户的使用权。root 是系统中权限最高的用户。和 Windows 的 Administrator 相比，权限不知道高到哪里去了。（_注：在 Windows 中想获取较高的权限，可以使用 NSudo_）举个例子：Administrator 是删除不了 `explorer.exe` 文件的，而 root 可以删除你当前正在使用的 shell 。

## 切换到 root 账户

使用命令 `su` 。

Android 上的软件，实际上就是这样获取你授予它的 root 权限的。当然，会有一定的验证措施。在手机上通常是弹框确认，而在电脑上，通常会要求用户输入 root 账户的密码。

## 我不知道 root 账户的密码，怎么办？

在许多桌面发行版（如 Ubuntu Desktop）中，root 账户默认是不被用户直接接触的。如 Ubuntu，其 root 账户默认是被禁用的。在这种情况下，用户通过 `sudo` 命令获取 root 账户的使用权。

对 `sudo apt-get ...` 不陌生吧？对，这就是 `sudo` 的用法了。**注意**，在使用 `sudo` 的时候，它询问的是**你自己账户的密码**，而不是 root 账户的密码。

想切换到 root 权限的 shell？也用这样的方法， `sudo -i`

## sudo 可以让任何人使用 root 账户？那岂不是太不安全了！

并不是的呢。 sudo 君有一个小名单，只有名单上的人才可以使用 sudo。这个名单就是 `/etc/sudoers` 。

在 Redhat 系统上，很多人在使用 sudo 时会发现这样的提示：

> 用户不在 sudoers 文件中。此事将被报告。

为什么呢？ Redhat 系的发行版，似乎不默认把安装时建立的用户加入 sudoer，sudo 君自然就不会放行啦。

如想编辑 `/etc/sudoers` ，建议使用 `visudo`。

### /etc/sudoers

sudo 的配置文件是 `/etc/sudoers` 。visudo会锁住 sudoers 文件，保存修改到临时文件，然后检查文件格式，确保正确后才会覆盖 sudoers 文件。必须保证 sudoers 格式正确，否则 sudo 将无法运行。

```text
# visudo
```

看名字就知道它会用 vi 啦，如果汝想换一个自己喜欢的编辑器的话，可以用 `EDITOR` 环境变量设置：

```text
# EDITOR=vim visudo
```

### wheel 组

很多系统在 `/etc/sudoers` 都有这条预置策略： `wheel` 组的所有成员都可以使用 `sudo`。所以，如果想让一位用户使用 `sudo`，最简单的方法就是将此用户加入`wheel`组：

```text
# usermod -G (追加用户到组) {组名} {用户名}
usermod -G wheel <用户名>
# gpasswd 也有类似的作用
gpasswd -a <用户名> wheel
```

### 编辑 sudoers 文件

```text
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
## 主机名别名
## Groups of machines. These may include host names (optionally with wildcards),
## IP addresses, network numbers or netgroups.
# Host_Alias    WEBSERVERS = www1, www2, www3

##
## User alias specification
## 用户别名
## Groups of users.  These may consist of user names, uids, Unix groups,
## or netgroups.
# User_Alias    ADMINS = millert, dowdy, mikef

##
## Cmnd alias specification
## 命令别名
## Groups of commands.  Often used to group related commands together.
# Cmnd_Alias    PROCESSES = /usr/bin/nice, /bin/kill, /usr/bin/renice, \
#                 /usr/bin/pkill, /usr/bin/top
# Cmnd_Alias    REBOOT = /sbin/halt, /sbin/reboot, /sbin/poweroff

##
## Defaults specification
## 默认参数（例如保持各种环境变量等等，按需取消注释即可）
## You may wish to keep some of the following environment variables
## when running commands via sudo.
##
## Locale settings
# Defaults env_keep += "LANG LANGUAGE LINGUAS LC_* _XKB_CHARSET"
##
## Run X applications through sudo; HOME is used to find the
## .Xauthority file.  Note that other programs use HOME to find   
## configuration files and this may lead to privilege escalation!
# Defaults env_keep += "HOME"
##
## X11 resource path settings
# Defaults env_keep += "XAPPLRESDIR XFILESEARCHPATH XUSERFILESEARCHPATH"
##
## Desktop path settings
# Defaults env_keep += "QTDIR KDEDIR"
##
## Allow sudo-run commands to inherit the callers' ConsoleKit session
# Defaults env_keep += "XDG_SESSION_COOKIE"
##
## Uncomment to enable special input methods.  Care should be taken as
## this may allow users to subvert the command being run via sudo.
# Defaults env_keep += "XMODIFIERS GTK_IM_MODULE QT_IM_MODULE QT_IM_SWITCHER"
##
## Uncomment to use a hard-coded PATH instead of the user's to find commands
# Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
##
## Uncomment to send mail if the user does not enter the correct password.
# Defaults mail_badpass
##
## Uncomment to enable logging of a command's output, except for
## sudoreplay and reboot.  Use sudoreplay to play back logged sessions.
# Defaults log_output
# Defaults!/usr/bin/sudoreplay !log_output
# Defaults!/usr/local/bin/sudoreplay !log_output
# Defaults!REBOOT !log_output

##
## Runas alias specification
##

##
## User privilege specification
##
## 按用户分配权限。
## 用户名 主机名 = 命令
root ALL=(ALL) ALL

## 要为某个用户可以执行所有命令，在配置文件中加入：
## 用户名   ALL=(ALL) ALL

## 如果只想允许以某个主机名登录用户执行命令：
## 用户名   主机名=(ALL) ALL

## 允许wheel用户组成员无密码使用sudo：
## %wheel      ALL=(ALL) NOPASSWD: ALL

## 要不询问某个用户的密码:
## Defaults:USER_NAME      !authenticate

## 只为用户启用部分命令的执行权限：
## 用户名 主机名=/sbin/halt,/sbin/poweroff,/sbin/reboot,/usr/bin/pacman -Syu

## 按组分配权限
## Uncomment to allow members of group wheel to execute any command
%wheel ALL=(ALL) ALL

## 如果希望不需要密码使用 sudo
## Same thing without a password
# %wheel ALL=(ALL) NOPASSWD: ALL

## Uncomment to allow members of group sudo to execute any command
# %sudo    ALL=(ALL) ALL

## Uncomment to allow any user to run sudo if they know the password
## of the user they are running the command as (root by default).
# Defaults targetpw  # Ask for the password of the target user
# ALL ALL=(ALL) ALL  # WARNING: only use this together with 'Defaults targetpw'

## 从 /etc/sudoers.d/ 中读取追加设置（这里的 # 不是注释）
## Read drop-in files from /etc/sudoers.d
## (the '#' here does not indicate a comment)
#includedir /etc/sudoers.d
```

更多的设置范例可以参考 [sudo 用户手册](http://www.gratisoft.us/sudo/man/sudoers.html)

## su 和 sudo 怎么拿到 root 权限的？

让我们看一看 `ls -l /bin/su` 和 `ls -l /usr/bin/sudo` 的权限：

```text
-rwsr-xr-x 1 root root （其后省略）
```

这里的小写字母 s 是一个有点特殊的权限：SUID。通俗地说，拥有这个权限的可执行程序，在运行时会拥有其所有者的权限（即 Set UID），于是 `su` 和 `sudo` 就得到了 root 权限（当然，不一定会给所有调用它们的人）。

