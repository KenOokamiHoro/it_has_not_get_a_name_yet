# 磁盘和分区？

## WHAT？

如果用住宅和房间来比喻的话，每套住宅大概就像一块磁盘，里面有各种功能的房间，就相当于各个分区啦……

## 那 MBR 和 GPT 是啥？

主引导记录（Master Boot Record，缩写：MBR），又叫做主引导扇区，是计算机开机后访问硬盘时所必须要读取的首个扇区，它在硬盘上的三维地址为（柱面，磁头，扇区）＝（0，0，1）。在深入讨论主引导扇区内部结构的时候，有时也将其开头的446字节内容特指为“主引导记录”（MBR），其后是4个16字节的“磁盘分区表”（DPT），以及2字节的结束标志（55AA）。不过 MBR 只支持四个主分区（于是后来可以把第四个主分区做成扩展分区来放更多的分区），而且要是大于 2.2TiB 的硬盘使用 MBR 分区表的话，后面的空间就用不上啦（这就是又搞出一个 GPT 的原因？？）

GUID磁碟分割表（GUID Partition Table，缩写：GPT）是一个實體硬盘的分区表的结构布局的标准。它是可扩展固件接口（EFI）标准（被Intel用于替代个人计算机的BIOS）的一部分，被用于替代BIOS系统中的一32bits来存储逻辑块地址和大小信息的主開機紀錄（MBR）分区表。
随着 UEFI 的流行，GPT 用的越来越多，相对的 MBR 就少啦……

## 那么怎么区分呢？

在 GNU/Linux 上，可以用 fdisk 命令来查看：

```bash

# -l 就是 --list 啦，用来列出现有的磁盘
$ fdisk -l

Disk /dev/sda: 894.3 GiB, 960197124096 bytes, 1875385008 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 861DC223-ED68-4A91-7497-87243A07180A

Device          Start        End    Sectors   Size Type
/dev/sda1        2048    1050623    1048576   512M EFI System
/dev/sda2     1050624 1665667071 1664616448 793.8G Linux filesystem
/dev/sda3  1665667072 1875380223  209713152   100G Microsoft basic data
/dev/sda4  1875380224 1875384974       4751   2.3M BIOS boot

```

输出中的 Disklabel type 就是磁盘的分区表类型啦（比如这里的就是 GPT）

## 还有下面那个 /dev/sda 啥的是啥玩意？

毕竟 GNU/Linux 也被称作类 Unix 系统嘛，自然继承了 UNIX 一切皆文件的思想，
上面哪些 /dev/sda 啥的其实就是映射出来的设备“文件”啦……

现在的电脑基本都使用的是 SATA 接口的硬盘啦，于是第一块硬盘就是 /dev/sda 啦
（同理第二块就是 /dev/sdb ，以此类推……）。一些还在用 ATA 接口的老式电脑上可能会有
/dev/hda 一类的硬盘。

后面的数字就是分区啦，比如 /dev/sda1 就是这个硬盘上的第一个分区。
有些只有一个分区的设备（例如 U 盘）插进去的时候没有数字，只有整个设备也是正常的啦~

有些比较新的电脑带上了读卡器，或者在一些 eMMC 闪存的设备上（例如某些平板电脑），
汝也许会看到 /dev/mmcblk0p1 一类的，大概就是第一块闪存上的第一个分区啦（为啥控制器从 0
开始数但分区不是 ……）

有些比较新的电脑使用的是 NVM Express 协议的硬盘，这个时候汝也许会看到 /dev/nvme0n1p1 这样的，
就是第一个 NVMe 控制器上的第一个磁盘的第一个分区咯（依样画葫芦……）

