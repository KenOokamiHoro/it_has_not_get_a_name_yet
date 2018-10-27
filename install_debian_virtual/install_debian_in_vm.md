# 在虚拟机安装 Debian

这个首先你得打开虚拟机设置里的“存储”选项，然后点这里：

![](/assets/install_debian_in_vm/vm_devices_discselect.png)

点击“选择一个虚拟光盘文件”，找到下载好的 Debian 镜像并选择它。然后确定。

接下来就是点击“启动”啦～！

然后嘛……就是这么一个界面呢：

![](/assets/install_debian_in_vm/debian_in_vm.png)

那么我们选择“Graphical Install”吧。

然后……

---

首先是选择语言呢

![](/assets/install_debian_in_vm/debian_install_1.png)

选择“中文（简体）”

![](/assets/install_debian_in_vm/debian_install_2.png)

提醒了一下没有完全翻译，继续即可。

区域和键盘布局的话，或许选默认的就好。

接着填上主机名：
![](/assets/install_debian_in_vm/debian_install_3.png)

域名一般不用填写的说～

![](/assets/install_debian_in_vm/debian_install_4.png)

设一个 `root` 密码呢，不过也可以不设，新添的用户通过 `sudo` 就可以获得权限。

**填的话一定要用个强密码！！！**

![](/assets/install_debian_in_vm/debian_install_5.png)

加一个用于一般使用的用户，这里填入全名

> 然而只能填英文的吧？

![](/assets/install_debian_in_vm/debian_install_6.png)

填上用户名，按照要求写就没问题。

![](/assets/install_debian_in_vm/debian_install_7.png)

同样地，填上密码。

![](/assets/install_debian_in_vm/debian_install_8.png)

分区时刻啊，这个看你们的，我只说一下最基本的“向导 - 使用整个磁盘”吧。

![](/assets/install_debian_in_vm/debian_install_9.png)

选择硬盘之后嘛……

![](/assets/install_debian_in_vm/debian_install_10.png)

思考一下怎么布局所有文件夹。一般新手的话，放同一分区为好。

![](/assets/install_debian_in_vm/debian_install_11.png)

再次检查一下分区设定吧，  
因为这种东西一旦设错，我也就只能默哀了。

![](/assets/install_debian_in_vm/debian_install_12.png)

还有什么 CD、DVD 什么的这时候可以拿出来当软件源。

![](/assets/install_debian_in_vm/debian_install_13.png)

选择镜像。当然是选择中国（……）

![](/assets/install_debian_in_vm/debian_install_14.png)

选择具体的站点。

![](/assets/install_debian_in_vm/debian_install_15.png)

如果你的网络要走代理的话，填上吧。

![](/assets/install_debian_in_vm/debian_install_16.png)

想一下要不要参加软件包活跃度调查？看你们的。

![](/assets/install_debian_in_vm/debian_install_17.png)

选择想要的桌面环境和小附件集（这里选了 GNOME，因为萌狼让的）

![](/assets/install_debian_in_vm/debian_install_18.png)

装上 GRUB 的确认。

![](/assets/install_debian_in_vm/debian_install_19.png)

这里选好了，选择的硬盘的主引导记录（MBR）可是会被 GRUB 覆盖掉的。

![](/assets/install_debian_in_vm/debian_install_20.png)

终于可以迎接虚拟机里的 Debian 啦！

（也许没了？）
