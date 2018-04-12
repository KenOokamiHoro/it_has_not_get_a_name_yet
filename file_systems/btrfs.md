# Btrfs

## 介绍

> Btrfs (Butter FS, _Better FS_, B-tree FS) 是一款支持 _CoW_ 特性的 Linux 现代文件系统，使用 **GPL** 开源协议授权，实现了包括_可写快照_、_RAID_、_子卷 (subvolumes)_ 等高级特性，同时，Btrfs 专注于**高容错**、**易修复**和**易于管理**。自 2.6.29，Btrfs 已并入 Linux 内核主线，GRUB2 & Syslinux 也已支持 Btrfs。

## 准备

Linux 内核支持 Btrfs 文件系统，但如果要使用 Btrfs 用户空间工具，你需要安装一些软件包。

Debian 6 (_squeeze_) ~ Debian 8 (_jessie_)

`apt install btrfs-tools`

Debian 9 (_stretch_) ~

`apt install btrfs-progs`

## 分区和配置

### 常规

在分区 /dev/partition 上创建 Btrfs 文件系统

`mkfs.btrfs -L label_name /dev/partition`

在单文件或目录上禁用 CoW

`chattr +C /dir/file`

> _注：仅对新创建的文件有效，若要对已存在文件禁用 CoW，请新建另一空目录设置+C，并将原文件复制到该目录_

轻量拷贝文件 (_Lightweight copy_)

> _注：创建轻量(Lightweight)拷贝引用（指向）原数据_

`cp --reflink source dest`

对已存在的文件/目录进行压缩

`btrfs filesystem defragment -r -v -calg /`

alg 可以是 zlib, lzo, zstd 或 no (不压缩)

> _注：GRUB & rEFInd 暂时缺少 zstd 支持，请不要对其所在的 boot 分区进行 zstd 压缩_

查看 Btrfs 磁盘使用率

`btrfs filesystem usage /`

> _该方法比通用 df 命令结果更准确_

Btrfs 手动磁盘整理

`btrfs filesystem defragment -r /`

### 子卷

创建新的 Btrfs 子卷

`btrfs subvolume create /path/to/subvolume`

查看目录下所有子卷

`btrfs subvolume list -p /path/with/subvolumes`

删除一个子卷

`btrfs subvolume delete /path/to/a/subvolume`

设置默认子卷为 subvol-id

`btrfs subvolume set-default subvol-id /`

> _btrfs subvolume list 命令可列出子卷 ID_

### 磁盘配额

> Under Construction ...

### RAID

> Under Construction ...

### SSD Trim

Btrfs 支持 discard 和 fstrim 以释放未被使用的块

### Scrub

这是一个 Btrfs 文件系统检查工具

后台开始检查目录任务

`btrfs scrub start /`

查看目录检查状态

`btrfs scrub status /`

### 快照 Snapshot

新建一个快照/只读快照

`btrfs subvolume snapshot source [dest/]name`

`btrfs subvolume snapshot -r source [dest/]name`

## 从 Ext3/4 迁移到 Btrfs

从 CD Live 环境引导以离线转换磁盘，此部分请阅读本魔法书其他部分，转换前请**务必**备份您的重要数据

对 /dev/partition 分区进行 Btrfs 转换

`btrfs-convert /dev/partition`

> _转换后确保修改 /etc/fstab 中的挂载类型和磁盘 UUID，您可以 chroot 进您的 Debian 系统重建您的 GRUB 引导，如果您转换的是根分区，还需要重建 initramfs_

确认无误且系统正常引导后可删除文件系统转换备份子卷

`btrfs subvolume delete /ext2_saved`

最后 blance 整个文件系统来回收可用空间

`btrfs blance start /`

## 已知问题 Known Issues

> Under Construction ...

## 参考资料

[Btrfs - ArchWiki](https://wiki.archlinux.org/index.php/Btrfs)
[Btrfs - DebianWiki](https://wiki.debian.org/Btrfs)
[Btrfs Wiki](https://btrfs.wiki.kernel.org/index.php/Main_Page)
