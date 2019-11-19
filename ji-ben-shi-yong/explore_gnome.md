# GNOME 桌面环境概览

## 桌面和窗口

在输入密码然后成功登录以后，汝大概看到的会是这个样子：

![&#x684C;&#x9762;&#x6982;&#x89C8;](../.gitbook/assets/2.png)

点击右上角可以打开系统菜单，汝可以在其中调整音量和亮度，开关 WiFi 和 蓝牙（前提是汝的设备可以支持），查看电池状态，进入设置，关机和重启等等。

![&#x7CFB;&#x7EDF;&#x83DC;&#x5355;](../.gitbook/assets/3%20%281%29.png)

点击中间的区域可以打开日历和通知列表。

![](../.gitbook/assets/4%20%281%29.png)

点击左上角（或者直接把鼠标向左上方推）可以打开活动视图，就像这样：

![](../.gitbook/assets/5%20%281%29.png)

活动视图最上方的搜索栏可以搜索应用：

![](../.gitbook/assets/6%20%281%29.png)

或是干些别的（例如计算器😂）

![](../.gitbook/assets/7%20%281%29.png)

右侧是工作区列表（就是所谓的虚拟桌面啦~），把窗口拖动到新的工作区上就能开启一个新的工作区。

![](../.gitbook/assets/8%20%281%29.png)

点击活动视图左侧边栏（叫做“收藏夹”）最下面的按钮可以打开所有程序列表：

![](../.gitbook/assets/27.png)

右键点击可以看到添加到收藏夹的选项：

![](../.gitbook/assets/28.png)

![](../.gitbook/assets/29.png)

现在咱们先从系统菜单打开设置试试：

![](../.gitbook/assets/9%20%281%29.png)

大概就是这个样子…… WiFi 旁边的开关是可以点击拨动的，就像这样 😂：

![](../.gitbook/assets/10%20%281%29.png)

可以注意到活动旁边的设置上有个倒三角形，点击可以打开应用程序菜单：

![](../.gitbook/assets/12.png)

![](../.gitbook/assets/13.png)

然而大多数程序只有一个退出而已……

点击左上角的搜索按钮就会蹦出一个搜索框。而且可以用 😂

![](../.gitbook/assets/14.png)

右上角的三条横线也是个菜单，内容多半和当前的内容相关：

![](../.gitbook/assets/15.png)

## 文件管理器

收藏夹第二个图标就是文件管理器啦~打开看一看：

![](../.gitbook/assets/44.png)

右上角的三条横线也是个菜单，内容多半和当前的内容相关：

![](../.gitbook/assets/47.png)

（它左边的按钮依次是搜索和切换列表/图标视图）

“其它位置”包含了汝电脑上的其它分区。

![](../.gitbook/assets/48.png)

\(其实和 Windows 的文件资源管理器很像吧……\)

## 添加输入源

> 可以看到大多数的文字还是英文…… 如果学有余力而且英语水平还不错的话，不妨来帮忙翻译下呗~
>
> [https://wiki.gnome.org/TranslationProject](https://wiki.gnome.org/TranslationProject)

其实就是输入法啦……

点击 "Region and Language" （区域和语言）打开选单：

![](../.gitbook/assets/16.png)

在添加输入源中依次选择 汉语 - 汉语（Rime）

![](../.gitbook/assets/17.png)

![](../.gitbook/assets/18.png)

然后右上角的系统菜单中就会多出一个切换输入源的菜单啦~

![](../.gitbook/assets/19.png)

第一次运行 Rime 时会进行部署，等到 “Rime is ready”的通知出现时……

![](../.gitbook/assets/23.png)

就可以用啦 😂

![](../.gitbook/assets/26.png)

> Rime 默认的输入方案是繁体中文的 “朙月拼音”，可以在输入框上按键盘的 F4 键进入方案选单切换输入方案。

## 优化工具初探

有没有发现窗口上只有一个关闭按钮啊 😂，可以用“优化工具”把剩下的两个按钮找回来。

（很好它的名字就叫“优化”）……

![](../.gitbook/assets/29.png)

打开之后有很多选项，咱们先找到“窗口”那一节： 在”标题栏按钮“那里就能打开（或者关闭）最大化和最小化按钮啦~

![](../.gitbook/assets/30.png)

当然要是愿意的话也可以放在左边：

![](../.gitbook/assets/31.png)

## 终端模拟器和 pacman 初探

收藏夹第三个图标就是叫终端的终端模拟器啦~

> 为啥说是终端模拟器呢？ ……

![](../.gitbook/assets/32.png)

现在来安装个浏览器试试吧……

于是就装 Iceweasel 了（就是改了商标的 Firefox），用 pacman -Ss 可以在仓库中搜索软件包。

![](../.gitbook/assets/33.png)

![](../.gitbook/assets/34.png)

可以看到还有一些 iceweasel-l10n 一类的是 Iceweasel 的语言包啦 ，用 pacman -S 可以安装指定的软件包，但是……

![](../.gitbook/assets/35.png)

还记得安装时设置的 sudo 嘛 ~

![](../.gitbook/assets/36.png)

这里输入的是汝自己的密码。

![](../.gitbook/assets/37.png)

![](../.gitbook/assets/38.png)

在活动视图的搜索框中输入 iceweasel 试试？

![](../.gitbook/assets/40.png)

![](../.gitbook/assets/41.png)

![](../.gitbook/assets/42.png)

如果想切换到中文的话，可以安装 `iceweasel-l10n-zh-cn` 然后重新启动一下。

![](../.gitbook/assets/43.png)

## 还有一件事……

随着逐渐使用，点击应用程序图标会默认显示常用应用（当然是可以切换到所有应用去的）

![](../.gitbook/assets/46.png)

