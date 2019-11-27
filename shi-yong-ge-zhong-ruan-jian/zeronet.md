# ZeroNet - 无中心化网络

## ZeroNet 是啥？

ZeroNet 是一个目前用户相对比较多的去中心化网络。使用到了 BitTorrent 协议（就是种子啦）和 Namecoin （和比特币 Bitcoin 算是同类）。内部已经有了许多功能各异的网站，像是论坛、留言板、博客、视频网站、种子站点甚至是 Git 托管服务（这个好像还正在开发）。

由于用户较多，根据世界的一贯运行规律，ZeroNet 的连接在本文主要读者群所在地区已经[受到了干扰](https://github.com/HelloZeroNet/ZeroNet/issues/1401)。不过，和许多情况下一样，我们仍然可以通过某种方式来解决这类问题。相关材料还请各位自备，魔法书编写部门暂不提供此类服务。

## 安装！

ZeroNet 是用 Python 写的，就和原来风靡 GitHub 国内用户的那啥 Agent 一样。

请在[**这里**](https://github.com/HelloZeroNet/ZeroNet/blob/master/README-zh-cn.md#%E5%A6%82%E4%BD%95%E5%8A%A0%E5%85%A5-)下载适合你所使用的操作系统的文件包。

下载完毕之后，Windows 用户双击 `ZeroNet.exe`、 Linux 用户运行一下 `ZeroNet.sh` 就可以啦。

刚才下载的内容主要是环境和基础组件。首次运行 ZeroNet ，会下载 ZeroNet 本体。本体下载完成，你的浏览器就会打开 ZeroNet 的管理界面 ZeroHello 啦！顺便说一下，这个管理界面，也是托管在 ZeroNet 上的哦！

（如果没有自动打开的话，请手动访问 [http://127.0.0.1:43110](http://127.0.0.1:43110)。）

### 找不到节点？

点击左上角的 `CONFIG`，进入[ZeroNet Config](http://127.0.0.1:43110/Config/)。最下面的 `Proxy for tracker connections` 选择 `Custom`，然后 `Custom socks proxy address for trackers` 填入你的魔法门的 socks 地址（例如 127.0.0.1:1080），点击下面大黄色的 `Save Settings` ，重新启动 ZeroNet 就可以了。

## 探索

ZeroHello 的界面大概是这样的：

![Screenshot](https://i.imgur.com/H60OAHY.png)

左侧可以管理访问（并保存）的站点、文件和查看统计信息。这些站点都保存到了你的电脑上，是可以离线访问的。

最开始大概只有 ZeroHello （这个管理界面） 和 ZeroName （Namecoin 的 `.bit` 域名到 ZeroNet 站点地址的映射，这个列表由 ZeroNet 的作者维护，bot 会自动从 Namecoin 处抓取。有点类似于 DNS 服务器）。之后你就可以自己来访问一些站点了。

向左拖动右上方的按钮，可以查看一些站点相关信息，包括目前所连接到的 peer 的数量及位置等。

### 身份系统

在大多数论坛/留言板发言时，需要在网站所支持的“可信授权提供商”中注册一个用户并使用此用户发布内容。使用这种设计，很大程度上是出于对 anti-spam 的考虑。

* **最广泛使用**：[ZeroID](http://127.0.0.1:43110/zeroid.bit)，其中一个、也是最大的一个用户站点，通用于绝大多数论坛。
* [ZeroVerse](http://127.0.0.1:43110/zeroverse.bit)，功能同上。相对 ZeroID 而言，支持 ZeroVerse 的站点较少。

**注意：在 ZeroID 或 ZeroVerse 等身份验证服务上，网页注册 ZeroNet 身份，可能会暴露你的 IP 地址（见** [**ZeroNet 中此页面**](http://127.0.0.1:43110/zeroverse.bit/faq.html)**的 “Why was ZeroVerse created?” 一条）！如果对此有顾虑的话，可以通过 BitMessage 等其它方式注册身份。**

你的身份保存在本地的 `ZeroNet/data/users.json`，请记得备份好这个文件以免注册的身份丢失。

**使用方式**：在需要选择身份的地方，点击 `登录`，在右上角选择身份即可。

## 深入了解 ZeroNet

* [建立自己的站点，或克隆他人的站点](http://127.0.0.1:43110/NewGFWTalk.bit/?Topic:18_13Z7XxTa7JuFat3KzzMWu3onwM6biLuurJ/) （中文，ZeroNet 上内容）
* [通过 Tor 连接到 ZeroNet](https://zeronet.readthedocs.io/en/latest/faq/#how-to-use-zeronet-with-tor) （英文，readthedocs.io）

