---
description: 这些都是啥？
---

# UNIX 和 GNU/Linux

## Unix

**UNIX**，一种多用户、多进程的计算机[操作系统](https://zh.wikipedia.org/wiki/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F)，源自于从20世纪70年代开始在美国 [AT&T](https://zh.wikipedia.org/wiki/AT%26T) 公司的[贝尔实验室](https://zh.wikipedia.org/wiki/%E8%B4%9D%E5%B0%94%E5%AE%9E%E9%AA%8C%E5%AE%A4)开发的 [AT＆T](https://zh.wikipedia.org/wiki/AT%26T) Unix 。按照操作系统的分类，属于分时操作系统，最早由Ken Thompson、Dennis Ritchie和Douglas McIlroy于1969年在 AT&T 的贝尔实验室开发。

目前它的商标权由国际开放标准组织所拥有，只有符合单一UNIX规范的UNIX系统才能使用UNIX这个名称，否则只能称为类 UNIX（UNIX-like）。

有不少操作系统可以当做类 UNIX 系统，例如[ BSD 系统](https://zh.wikipedia.org/wiki/BSD) ， Apple Darwin（macOS 和 iOS 的内核）和 GNU/Linux 等等。

## GNU/Linux

这个名字要分成两个部份来看，一个以自由为目标的操作系统 GNU 和一个操作系统内核 Linux。

Linux 是一种自由和开放源码的类 UNIX 操作系统的内核，由林纳斯·托瓦兹在1991年10月5日首次发布。Linux也是自由软件和 开放源代码软件发展中最著名的例子。只要遵循 GNU 通用公共许可证\(GPL\)，任何个人和机构都可以自由地 使用Linux的所有底层源代码，也可以自由地修改和再发布。

Linux 严格来说是单指操作系统的内核，因操作系统中包含了许多用户图形接又和其他实用工具。由于这些支持用户空间的系统工具和库主要由 理查德·斯托曼于1983年发起的 GNU 计划提供，自由软件基金会提议将其组合系统命名为GNU/Linux，但 Linux不属于 GNU 计划，这个名称并没有得到社群的一致认同。

GNU计划 \(英语: GNU Project\)，是一个自由软件集体协作计划，1983年9月27日由理查德·斯托曼在麻省理工学院公开发起。它的目标是创建一套完全自由的操作系统，称为GNU。理查德·斯托曼最早在 net.unix- wizards 新闻组上公布该消息，并附带一份[《GNU宣言》](https://www.gnu.org/gnu/manifesto.zh-cn.html)等解释为何发起该计划 的文章，其中一个理由就是要“重现当年软件界合作互助的团结精神”。

GNU是“GNU is Not Unix”的递归缩写。

> 在黑客（Hacker，并不是骇客（Cracker）界中，把缩写放在全称中是一种很常见的做法，这就是递归缩写啦~ 再例如 PHP（PHP Hypertext Processor，PHP 超文本解析器）

为避免与单词 gnu \(非洲牛羚，发音与 “new”相同\)混淆，斯托曼宣布GNU发音应为“Guh-NOO”\( /ˈgnuː/ \)，与 “canoe”发音相似。其中，Emacs就是由这个计划孵化而出。

为保证GNU软件可以自由地“使用、复制、修改和发布”，所有GNU软件都包含一份在禁止其他人添加任何限制 的情况下，授权所有权利给任何人的协议条款，GNU通用公共许可证\(GNU General Public License，GPL\)。 这个就是被称为“公共著作权”的概念。GNU也针对不同场合，提供GNU宽通用公共许可证与GNU自由文档许可 证这两种协议条款。

### 为啥要说 GNU/Linux ?

简单来说 Linux 只是一个内核啦，只靠一个内核貌似大多数事情都做不成……

GNU 的内核 Hurd，虽然从 1990 年就开始开发了，[GNU Hurd 在 2001 年开始可以稳定工作](https://www.gnu.org/software/hurd/hurd-and-linux.html)，但是距离能够被人们正常使用还有很长的路要走。

庆幸的是，我们不必再等Hurd了，因为有了 Linux 。当Linus Torvalds在 1992 年使 Linux 成了自由软件，它填补了 GNU 系统的一个重要空白。人们可以[把 Linux 和 GNU 系统结合起来](http://ftp.funet.fi/pub/linux/historical/kernel/old-versions/RELNOTES-0.01)组成一个完整的自由系统—一个带有 Linux 的 GNU 系统。换句话说，就是 GNU/Linux 系统。

因为混淆和宣传等等原因， GNU/Linux 经常会被“错误”地称作“ Linux（操作）系统” 或者仅仅简单地称作 “Linux”。（啊这本书最大（？）的作者和 Stallman 一样是这样认为的）GNU 计划特别[编写了一篇文章](https://www.gnu.org/gnu/gnu-linux-faq.zh-cn.html)来详细的解释这一问题。

而汝经常听说的 “Linux 系统”或者“Linux 发行版”上，其实大多数软件都来自 GNU 计划（例如 Bash 和 nano ，前者是一个 Shell，看起来就像汝 Windows 上的 PowerShell 一样。后者是一个小型文字编辑器）。 为了鼓励人们做那些需要做的工作，当汝选择一个包含 Linux 的 GNU 操作系统时，这个系统就叫做 GNU/Linux，不要称其为 “Linux”。改掉这一个习惯不会太久，也是个宣传 GNU 和自由软件运动的好手段，是呗~

> 当您选择一个包含 Linux 的 GNU 操作系统时，这个系统就叫做 [GNU/Linux](https://www.gnu.org/gnu/linux-and-gnu.html)，不要称其为 “Linux”。一旦人们注意到我们已经做的事情，而不是认为是别人的，他们会 [更加支持我们当前和未来的努力](https://www.gnu.org/gnu/why-gnu-linux.html)。改掉这个习惯只会用您很少的时间。
>
> -- [帮助 GNU - GNU 工程 - 自由软件基金会](https://www.gnu.org/help/help.zh-cn.html)







