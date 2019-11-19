# BIOS 和 UEFI

## BIOS 和 UEFI 究竟是什么鬼

BIOS 的全称洋文叫“Basic Input/Output System”，根据名字就能看出这个东西是个“基础输入输出系统”，那么这个系统在哪里呢？实际上你是看不到的，除非把电脑拆开，会发现主板上有个 ROM 芯片，里面就包含了这个 BIOS 系统啦。这个系统大概在 1975 年前后被制作出来，刚开始是一个仅供 IBM PC 用的专有软件，但是之后被各种逆向工程，之后用的人多了就成了一个标准了。

BIOS 被用来在开机的时候对硬件进行初始化，并且加载下一步的Payload（一般情况下是 Second-stage Bootloader）。对于大部分计算机系统，在上电之后会从一个可以直接寻址的ROM的头部开始取指执行。此处的BIOS代码会初始化Cache和内存，以建立Stack，建立C语言所需的运行环境，并且进行“Relocate”把自己从ROM拷贝入RAM以加快运行速度。之后开始初始化总线控制器，枚举 PCI 设备，运行 PCI Rom 上的代码，探测 USB ，网卡和其他外部设备，并且根据用户的配置调整硬件选项。最后会根据用户的配置从外部设备如硬盘，USB大容量存储设备乃至互联网上寻找Payload，加载到内存，并执行。对于传统的块设备例如硬盘，BIOS可以从其第 0 个区扇区读取 MBR （Master Boot Record） 来寻找 Payload。

同时BIOS也会提供一定的配置信息表和API保留在内存中给之后的 Second-stage Bootloader 和 操作系统。例如 ACPI 的 DSDT SSDT 表 可以告诉操作系统硬件的读取操作方式。而 INT10h API 则可以在操作系统的图形设施建立之前提供简易的屏幕绘图方式。

BIOS 自被开发出至今，已经接近 40 多年了。而 BIOS 的缺陷也逐渐的凸显出来： 安全性？ CIH 病毒？ 可扩展性？ 在X86的实模式下仅有1M内存可用 同时 MBR 表是不支持 2 TB 以上容量的哦。

所以在 1998 年前后，BIOS 的问题逐渐凸显出来，而 Intel 开发出来了替换 BIOS 的东西——Intel Boot Initiative，而后更名为 EFI（Extensible Firmware Interface，可扩展固件接口）。

在 2005 年，EFI 推出了最后一个版本 1.10，之后被废弃掉了，而后改名为 UEFI。而 The Unified Extensible Firmware Interface Forum （UEFI Forum）则成为了维护 UEFI 标准的论坛（类似如 CA 论坛等…）

还记得本文前面提到的 MBR 表么？MBR 表最大支持的磁盘是 2 TB 的，对于越来越便宜的存储设备来说，2 TB 实在是太小啦。所以 Intel 公司又开发了一个替代 MBR 的分区表标准（2010 年之后成为 UEFI 标准的一部分），叫 GPT（GUID Partition Table），GPT 表最高可以支持 8 ZB（约合 9444732965 TB） 每扇区的空间大小，反正我觉得我这辈子是看不到超过 9444732965 TB 的磁盘分区了 :\(。

UEFI 不仅能读取分区表，还能自动支持文件系统。所以它不像 BIOS，已经没有仅仅 440 字节可执行代码即 MBR 的限制了，它完全用不到 MBR。

而 UEFI 需要一个 ESP （EFI System Partition）分区，里面有 UEFI 所要用到的一些文件。计算机供应商可以在 /EFI// 文件夹里放官方指定的文件，还能用固件或它的 shell，即 UEFI shell，来启动引导程序。ESP 分区需要为 FAT32 格式以保证最大的兼容性。建议 ESP 分区为 512 MB。

UEFI 主流都支持 MBR 和 GPT 分区表。Apple-Intel Macs 上的 EFI 还支持 Apple 专用分区表。绝大部分 UEFI 固件支持软盘上的 FAT12，硬盘上的 FAT16、FAT32 文件系统，以及 CD/DVDs 的 IS09660 和 UDF。Intel Macs 的 EFI 还额外支持 HFS/HFS+ 文件系统。

而不得不提到的一个东西就是 Secure Boot，这个 Secure Boot 原来是为了帮助系统只运行签过名（Signed）的内核，以避免如 Bootkit 之类的后门的入侵。不过对于大公司来说，压榨自由是他们最爱干的事情，所以这个 Secure Boot 被微软公司和电脑厂商联手做成了只允许运行他们指定的系统。不过一般发行版都已经自带了经过微软签名的内核，所以是可以安装的。即使没有签过名也不要担心，绝大部分厂商都允许你把 Secure Boot 设置为 Custom 模式，这样你就可以添加自己的公钥然后对内核进行签名，之后就可以运行啦。

```text
有些 GNU/Linux 用户会把这种功能叫做“限制启动”（Restricted Boot），以表达对 M$ 的不满（雾）
```

## 前面说了那么多废话，那么问题来了，怎么区分咱现在是那一种启动方式呢~

在 Windows 中的话：

首先打开设置 \( Windows 8/8.1 叫做 "电脑设置"\),然后通过 "更新和恢复" -&gt; "恢复" -&gt; "高级启动" 重启电脑.

如果是 UEFI 启动的话,大概是这个样子:

![UEFI &#x7CFB;&#x7EDF;&#x542F;&#x52A8;&#x4E4B;&#x540E;&#x5927;&#x6982;&#x50CF;&#x8FD9;&#x6837;](https://github.com/KenOokamiHoro/it_have_not_get_a_name_yet/tree/13f2a0be412010681614294aa5d8915f297e4c99/assets/getting_ready_for_install_parabola/0.png)

没错就是有个 "使用设备" 的选项 😂

* 或者同时按下键盘上的 Windows 徽标键（就是有 Windows 标志那个） 和 R 键，会打开“运行” 对话框。

![&#x201C;&#x8FD0;&#x884C;&#x201D;&#x5BF9;&#x8BDD;&#x6846;](https://github.com/KenOokamiHoro/it_have_not_get_a_name_yet/tree/13f2a0be412010681614294aa5d8915f297e4c99/assets/getting_ready_for_install_parabola/1.png)

在里面输入 `msinfo32` 然后回车（按 Enter 键）确认，打开”系统信息”应用。

![&#x201C;&#x7CFB;&#x7EDF;&#x4FE1;&#x606F;&#x201D;&#x7A97;&#x53E3;](https://github.com/KenOokamiHoro/it_have_not_get_a_name_yet/tree/13f2a0be412010681614294aa5d8915f297e4c99/assets/getting_ready_for_install_parabola/2.png)

看“BIOS模式”里是不是 UEFI 😂😂

* 再或者打开磁盘管理（Windows 8 以后的系统可以通过按下 Windows + X 的菜单里找到 “磁盘管理”）

![&#x78C1;&#x76D8;&#x7BA1;&#x7406;&#x5728;&#x8FD9;~](https://github.com/KenOokamiHoro/it_have_not_get_a_name_yet/tree/13f2a0be412010681614294aa5d8915f297e4c99/assets/getting_ready_for_install_parabola/01.png)

嗯，大概就是这样子的呗 \(虽然具体的磁盘分区可能和咱的不一样\)

![&#x5927;&#x6982;&#x957F;&#x8FD9;&#x6837;~](https://github.com/KenOokamiHoro/it_have_not_get_a_name_yet/tree/13f2a0be412010681614294aa5d8915f297e4c99/assets/getting_ready_for_install_parabola/02.png)

看汝的硬盘上有没有一个 EFI 系统分区 😂😂😂

