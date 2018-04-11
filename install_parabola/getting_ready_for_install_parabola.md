# 在 Windows 上准备安装 Parabola

> 其实条件允许的话，最好还是找一台电脑单独装（<s>双系统什么的最麻烦的啦~</s>）

确定启动类型
----------------------------------------

* 首先打开设置 ( Windows 8/8.1 叫做 "电脑设置"),然后通过 "更新和恢复" -> "恢复" -> "高级启动" 重启电脑.

如果是 UEFI 启动的话,大概是这个样子:

![UEFI 系统启动之后大概像这样](/assets/getting_ready_for_install_parabola/0.png)

没错就是有个 "使用设备" 的选项 😂

* 或者同时按下键盘上的 Windows 徽标键（就是有 Windows 标志那个） 和 R 键，会打开“运行” 对话框。

![“运行”对话框](/assets/getting_ready_for_install_parabola/1.png)


在里面输入 :code:`msinfo32` 然后回车（按 Enter 键）确认，打开”系统信息”应用。

![“系统信息”窗口](/assets/getting_ready_for_install_parabola/2.png)

看“BIOS模式”里是不是 UEFI 😂😂，还有下面那个 “安全启动状态”是不是“以关闭”（咱这台电脑的 UEFI 太旧所以显示的是不支持）


    具体怎么关因为每种电脑的方法不一样于是汝要自己 STFW (Search the f**king Web，搜索一下) 了😂

* 再或者打开磁盘管理（Windows 8 以后的系统可以通过按下 Windows + X 的菜单里找到 “磁盘管理”）

![磁盘管理在这~](/assets/getting_ready_for_install_parabola/01.png)

嗯，大概就是这样子的呗 (虽然具体的磁盘分区可能和咱的不一样)

![大概长这样~](/assets/getting_ready_for_install_parabola/02.png)

看汝的硬盘上有没有一个 EFI 系统分区 😂😂😂

在硬盘上准备一块空闲空间
---------------------------------------

这里拿来演示的是 Windows 7 以后都自带的 “磁盘管理” 程序，应该能解决大多数问题 \_(:з」∠)\_

* Windows 8 以后的系统可以通过按下 Windows + X 的菜单里找到 “磁盘管理”

![磁盘管理在这~](/assets/getting_ready_for_install_parabola/01.png)
* 嗯，大概就是这样子的呗 (虽然具体的磁盘分区可能和咱的不一样)

![大概长这样~](/assets/getting_ready_for_install_parabola/02.png)


* 汝哪个硬盘分区比较空闲? 右键点击它,有一个"压缩卷的选项"

![”压缩卷“ 在这~](/assets/getting_ready_for_install_parabola/03.png)

* 输入压缩的大小 \_(:з」∠)\_

![](/assets/getting_ready_for_install_parabola/04.png)

* 然后就多了一块未分配的空间 😂

![](/assets/getting_ready_for_install_parabola/05.png)

制作启动盘
-------------------------

但前提是汝的电脑能从 U 盘启动 😂 （不过最近几年生产的电脑都应该可以了吧……

Windows 下咱比较推荐一个叫 rufus 的软件，`官方网站在这 <https://rufus.akeo.ie/>`_

下载完以后双击运行，需要管理员权限，记得看有没有数字签名。（有数字签名时用户账户控制的对话框是蓝色的）

Rufus 自带多国语言（当然也包括中文啦），如果汝系统语言不是中文的话，点击那个地球图标就可以修改语言了啦~

![选择语言](/assets/getting_ready_for_install_parabola/3.png)


然后戳有点像光盘的按钮选择刚下载好的 ISO 镜像

![选择映像](/assets/getting_ready_for_install_parabola/4.png)

然后选择一种启动类型，UEFI 就选最后一个，不是的话就选第一个。

![选择启动类型](/assets/getting_ready_for_install_parabola/5.png)

写入方式选推荐的就好 （´＿｀）

![选择写入方式](/assets/getting_ready_for_install_parabola/6.png)

确认（要知道汝按下确认以后就没有回头路了，所以记得提前备份 U 盘上的资料 😂）

![](/assets/getting_ready_for_install_parabola/7.png)

然后坐等完成，完成以后汝的 U 盘卷标应该是 "PARA_201804" 这样的 （后面四位年份和两位月份），不要改成别的，万一不对记得照 ISO 改回来 😂😂
