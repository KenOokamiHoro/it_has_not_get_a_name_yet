# 虚拟化和虚拟机的二三事

    在计算机技术中，虚拟化（技术）或虚拟技术（英语：Virtualization）是一种资源管理技术，是将计算机的各种实体资源（CPU、内存、磁盘空间、网络适配器等），予以抽象、转换后呈现出来并可供分区、组合为一个或多个电脑配置环境。由此，打破实体结构间的不可切割的障碍，使用户可以比原本的配置更好的方式来应用这些电脑硬件资源。这些资源的新虚拟部分是不受现有资源的架设方式，地域或物理配置所限制。一般所指的虚拟化资源包括计算能力和数据存储。

    --- https://zh.wikipedia.org/wiki/虚拟化

### 啥是虚拟机，都有哪些实现手段（Hypervisor）？

简单的来说，**虚拟机是对于操作系统的模拟**。  
通过虚拟机技术，你可以在一台电脑上运行多个 OS，比如说最常见的例子是在 MacBook 上通过 Bootcamp 和/或 Parallels Desktop 运行 Windows

我们将通过虚拟机运行的操作系统称之为 Guest OS (区分于后文提到的 Guest OS)

而 Hypervisor 则是用来创建、监控、运行虚拟机的软件/固件/硬件。通过架构，我们又可将 hypervisors 区分为两类：

![Types of hypervisors](/assets/Virtualization/Hypervisor.png)

- Type 1 hypervisor
    - 直接运行在硬件上
    - 例子有 Xen, Hyper-V, Xbox One, VMWare ESXi
- Type 2 hypervisor
    - 像一个常规软件一样运行在传统的操作系统上 (我们将这个系统称之为 Host OS)
    - 例子有 VMWare Workstation, VirtualBox, Parallels Desktop

显然，type 1 hypervisors 具有更高的性能，但在 setup 上不那么友好。以 ESXi 举例，ESXi 初次安装需要从另一台电脑远程配置安装 guest os。而 Hyper-V 仅在 pro 版本的 Windows 中作为额外功能提供。

不过对于 Type-2 hypervisors 来说，通过支持硬件虚拟化的主板**以及**CPU，也可以最大化 Guest OS 的性能；  
具有一定动手能力的同学还可以折腾 PCIe 显卡直通 以使用独显在虚拟机中 ~~玩游戏~~ 机器学习

对于用户而言，**最新手友好** 的虚拟机实现方式大致有：

| 实现手段 | 优点 | 缺点 | 适用场景 |
| ------- | ---- | --- | -------- |
| Hyper-V | 微软官方出品，支持动态内存分配 | 与 Windows Professional 或 Windows Server 一同提供；坊间传闻对于 Linux 支持极差 | 对于想在 Windows 机器上虚拟 Windows 的人（不在本篇讨论范围） |  
| VMWare Workstation Pro | 性能相对优于VBox，更加稳定，不容易crash | 付费，很贵；免费版本功能被阉割的非常严重 | 具有支付意愿与支付能力，且非常注重虚拟机的稳定性和性能的人 |
| VirtualBox | 开源，免费；支持从其他平台迁移 | 性能略差（不过可以接受） | 追求开源的用户们以及绝大多数用户们 |

### 为啥需要虚拟机？



### 我应该用虚拟机嘛？
