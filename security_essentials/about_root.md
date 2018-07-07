# 关于 root 权限的二三事

相信大部分人对“root 权限”的认识，是从 Android 开始的。大多数人的印象大概是：root 权限是一个非常高也非常危险的权限，使用这个权限的时候，一不小心就会把系统搞坏。

前半句没错，后半句——GNU/Linux（尤其是给桌面用户用的那些发行版），在这方面还是比较友好的。

## 什么是 root 权限？

这里的 root 权限，实际上指的是名为 root 的账户所具有的权限。“获取 root 权限”，指的实际是获取 root 账户的使用权。root 是系统中权限最高的用户。和 Windows 的 Administrator 相比，权限不知道高到哪里去了。（*注：在 Windows 中想获取较高的权限，可以使用 NSudo*）举个例子：Administrator 是删除不了  `explorer.exe`  文件的，而 root 可以删除你当前正在使用的 shell 。

## 切换到 root 账户
使用命令  `su` 。

Android 上的软件，实际上就是这样获取你授予它的 root 权限的。当然，会有一定的验证措施。在手机上通常是弹框确认，而在电脑上，通常会要求用户输入 root 账户的密码。

## 我不知道 root 账户的密码，怎么办？
在许多桌面发行版（如 Ubuntu Desktop）中，root 账户默认是不被用户直接接触的。系统会给 root 账户生成一个随机的密码（并且不告诉用户）。在这种情况下，用户通过  `sudo` 命令获取 root 账户的使用权。

对 `sudo apt-get ...` 不陌生吧？对，这就是 `sudo` 的用法了。**注意**，在使用 `sudo` 的时候，它询问的是**你自己账户的密码**，而不是 root 账户的密码。

想切换到 root 权限的 shell？也用这样的方法， `sudo bash（或者你喜欢的其它 shell）` 

## sudo 可以让任何人使用 root 账户？那岂不是太不安全了！
并不是的呢。 sudo 君有一个小名单，只有名单上的人才可以使用 sudo。这个名单就是 `/etc/sudoers` 。

在 Redhat 系统上，很多人在使用 sudo 时会发现这样的提示：
> 用户不在 sudoers 文件中。此事将被报告。

为什么呢？ Redhat 系的发行版，似乎不默认把安装时建立的用户加入 sudoer，sudo 君自然就不会放行啦。

如想编辑 `/etc/sudoers` ，建议使用 `visudo`。

### wheel 组
很多系统在 `/etc/sudoers` 都有这条预置策略： `wheel` 组的所有成员都可以使用 `sudo`。所以，如果想让一位用户使用 `sudo`，最简单的方法就是将此用户加入`wheel`组：

```
usermod -G wheel <用户名>
```

## su 和 sudo 怎么拿到 root 权限的？

让我们看一看 `ls -l /bin/su` 和 `ls -l /usr/bin/sudo` 的权限：

```
-rwsr-xr-x 1 root root （其后省略）
```

这里的小写字母 s 是一个有点特殊的权限：SUID。通俗地说，拥有这个权限的可执行程序，在运行时会拥有其所有者的权限（即 Set UID），于是 `su` 和 `sudo` 就得到了 root 权限（当然，不一定会给所有调用它们的人）。
