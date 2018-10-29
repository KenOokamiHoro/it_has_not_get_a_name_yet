# 在安装过程中设置加密卷

> 其实就是在安装过程中加密根目录咯~ 

## 如果通过向导自动配置

如果汝更中意让向导帮忙的话：

![选择 “向导 - 使用整个磁盘并配置加密的 LVM”](/assets/encrypt_installation/auto_1.png)

在磁盘分区一节中选择 “向导 - 使用整个磁盘并配置加密的 LVM”。

![选择要分区的磁盘](/assets/encrypt_installation/auto_2.png)

因为咱是拿虚拟机做的例子，所以这里可以放心的选择那唯一的一块磁盘。**汝自己操作的时候记得看清楚是哪一块磁盘啦~**

![选择分区方案](/assets/encrypt_installation/auto_3.png)

选择一种分区方案，和没加密的时候一样，随汝喜欢啦。

![将修改写入磁盘并配置 LVM](/assets/encrypt_installation/auto_4.png)

确认汝所做的修改，这一下以后几乎就无法回头了呐。

![使用随机数据覆盖磁盘](/assets/encrypt_installation/auto_5.png)

然后安装器就会开始用随机数据覆盖汝选择的磁盘啦，如果汝并不需要这么做（例如这是块新硬盘、或者汝自己手动擦除过、或者像咱一样这是块虚拟硬盘等等），汝可以点击下面的取消来跳过。

![为加密分区选择密码](/assets/encrypt_installation/auto_6.png)

为加密的分区输入一个密码啦，还是和以前一样唠叨，选择一个自己好记别人难猜强度又高的密码非常重要、非常重要、非常重要(ﾟДﾟ*)ﾉ

![分区完成](/assets/encrypt_installation/auto_7.png)

分区完成，点击“分区设定结束并将修改写入磁盘”完成修改：

![分区完成](/assets/encrypt_installation/auto_8.png)

确认，然后就是和正常安装一样继续啦~

## 手动分区时进行加密

如果汝更中意自己安排分区的话：

![选择一块空闲空间](/assets/encrypt_installation/manual_0.png)

选择一块空闲空间创建分区，分区类型选择“要加密的物理卷”。

![分区类型选择“要加密的物理卷”](/assets/encrypt_installation/manual_1.png)

然后在分区菜单中选择“配置加密卷”- “创建加密卷”。

![配置加密卷](/assets/encrypt_installation/manual_2.png)

选择汝刚刚设置的那个分区作为要加密的分区。

![选择加密的分区](/assets/encrypt_installation/manual_3.png)

完成，然后确认擦除（要知道这一下以后就完全没有回头路了呐~）

![完成](/assets/encrypt_installation/manual_4.png)

![确认擦除](/assets/encrypt_installation/manual_5.png)

然后安装器就会开始用随机数据覆盖汝选择的磁盘啦，如果汝并不需要这么做（例如这是块新硬盘、或者汝自己手动擦除过、或者像咱一样这是块虚拟硬盘等等），汝可以点击下面的取消来跳过。

![擦除进度](/assets/encrypt_installation/manual_6.png)

为加密的分区输入一个密码啦，还是和以前一样唠叨，选择一个自己好记别人难猜强度又高的密码非常重要、非常重要、非常重要 (╯‵□′)╯︵┻━┻

![设置密码](/assets/encrypt_installation/manual_7.png)

设置完成的加密卷就像这个样子，接下来就是在加密卷上创建分区咯~

![加密卷设置完成](/assets/encrypt_installation/manual_8.png)

例如这样（手动滑稽）

![改变加密卷的文件系统示例](/assets/encrypt_installation/manual_9.png)

## 那启动时怎么解密呢？

![启动时解密加密卷](/assets/encrypt_installation/unlock.png)

会有地方让汝输入密码的 😂