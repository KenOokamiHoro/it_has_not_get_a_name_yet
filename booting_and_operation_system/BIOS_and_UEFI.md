# 操作系统和启动

## BIOS 和 UEFI 究竟是什么鬼？

BIOS 的全称洋文叫“Basic Input/Output System”，根据名字就能看出这个东西是个“基础输入输出系统”，那么这个系统在哪里呢？实际上你是看不到的，除非把电脑拆开，会发现主板上有个 ROM 芯片，里面就包含了这个 BIOS 系统啦。这个系统大概在 1975 年前后被制作出来，刚开始是一个仅供 IBM PC 用的专有软件，但是之后被各种逆向工程，之后用的人多了就成了一个标准了。

BIOS 被用来在开机的时候初始检测。主板会一直从 RAM 的最开始向下加载，而 BIOS 就是最先被加载起来的，BIOS 加载起来之后会进行一系列的系统检查（POST， Power On Self Test），比如检查下内存是不是坏的，有没有键盘之类的。然后 BIOS 会从大容量存储设备（如硬盘）的第 0 个区域开始读取（即 MBR，Master Boot Record），然后操作系统的 Bootloader 才被加载出来（GRUB 等）

BIOS 自被开发出至今，已经接近 40 多年了。而 BIOS 的缺陷也逐渐的凸显出来：
安全性？ CIH 病毒？
可扩展性？ MBR 表是不支持 2 TB 以上容量的哦。
兼容性？BIOS 对 CPU 的依赖很大。

所以在 1998 年前后，BIOS 的问题逐渐凸显出来，而 Intel 开发出来了替换 BIOS 的东西——Intel Boot Initiative，而后更名为 EFI（Extensible Firmware Interface）。

在 2005 年，EFI 推出了最后一个版本 1.10，之后被废弃掉了，而后改名为 UEFI。而 The Unified Extensible Firmware Interface Forum （UEFI Forum）则成为了维护 UEFI 标准的论坛（类似如 CA 论坛等…）

还记得本文前面提到的 MBR 表么？MBR 表最大支持的磁盘是 2 TB 的，对于越来越便宜的存储设备来说，2 TB 实在是太小啦。所以 Intel 公司又开发了一个替代 MBR 的分区表标准（2010 年之后成为 UEFI 标准的一部分），叫 GPT（GUID Partition Table），GPT 表最高可以支持 8 ZB（约合 9444732965 TB） 每扇区的空间大小，反正我觉得我这辈子是看不到超过 9444732965 TB 的磁盘分区了 :(。

而在 UEFI 环境下的 Linux，则需要一个 ESP （EFI System Partition）分区来让 UEFI 启动 Linux 的 Bootloader，ESP 分区需要为 FAT32 格式以保证最大的兼容性。建议 ESP 分区为 512 MB，但是我一直用 256 MB 也没什么问题。

而不得不提到的一个东西就是 Secure Boot，这个 Secure Boot 原来是为了帮助系统只运行签过名（Signed）的内核，以避免如 Bootkit 之类的后门的入侵。不过对于大公司来说，压榨自由是他们最爱干的事情，所以这个 Secure Boot 被微软公司和电脑厂商联手做成了只允许运行他们指定的系统。不过一般发行版都已经自带了经过微软签名的内核，所以是可以安装的。即使没有签过名也不要担心，绝大部分厂商都允许你把 Secure Boot 设置为 Custom 模式，这样你就可以添加自己的公钥然后对内核进行签名，之后就可以运行啦。（以后的文章会提到用 Secure Boot 来增加系统的安全性）

## 常见的 Bootloader

Bootloader 俗称“启动引导器”，是用来引导系统启动，加载内核之类的特殊软件。常用的 Bootloader 是 GRUB2 和 Systemd-boot（前称 Gummiboot）。

GRUB：

GRUB 是很常用的一个 Bootloader，GRUB 有两个版本：一个是 GRUB Legacy（旧版）、一个是 GRUB 2（新版）。

由于 GRUB Legacy 已经停止维护了，所以本文提到的 GRUB 均指 GRUB 2。

未完待续…
