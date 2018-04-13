# 取得 Debian 的安装映像

## 何谓安装映像？

> 安装映像是一种磁盘映像，大部分安装映像是 ISO 映像（当然也有像 AOSC 那样放出 tarball （伪安装映像）的异类）。
> 咱的 Debian 便是释出了 ISO 映像。

## 下载 Debian 的安装映像

还是用汝喜欢的浏览器打开 [https://www.debian.org/distrib/](https://www.debian.org/distrib/) 呗~然后……没有大大的 Download 按钮。

呐，汝不要慌张啦。汝看下面这张图，这是在网站左上角的一部分：

![](/assets/get_debian_installation_image/download_part.png)

    或者汝这个时候看到的网站是有中文翻译的，那就对着下面的英语看意思吧 😂

其中有两个粗体的链接：```small installation image``` 和 ```complete installation image```，这两个都是映像下载链接，但是却略有不同。

```small installation image``` 是小型的安装映像，通常会在启动映像后通过网络下载剩余的资源。

```complete installation image``` 是完整的安装映像，包含了更多的包，在安装过程中一般无需再下载包。

这里咱们戳那个“complete installation image”进入完整映像的下载界面。

![Debian on CDs](/assets/get_debian_installation_image/debian_cds.png)

这五个选项分别是：

* 通过 HTTP 下载 CD/DVD 映像文件 (Download CD/DVD images using HTTP) ，就是直接下载啦~ 
* 购买 Debian CD-ROM 产品 (Buy finished Debian CD-ROMs) ，嗯虽然这次用不上的样子……
* 使用 jigdo 下载 CD/DVD 映像文件 (Download CD/DVD images with jigdo) 。"Jigdo" 系统协助您从全世界三百个 Debian 镜像挑选最快的站点开始下载。它最大的特色就是可以简单的选择镜像以及“升级”旧的映像文件到最新的版本。同时，这也是下载所有硬件架构 DVD 映像文件的唯一管道。（但首先要有个 GNU/Linux …… 下次再说吧 😂）
* 通过 BitTorrent 下载 CD/DVD 映像文件 (Download CD/DVD images with BitTorrent)。BitTorrent 点对点系统让许多用户同时一起合作下载映像文件，仅占用服务器最少资源。DVD 映像文件只有一些特定硬件架构。如果汝能熟练的使用 Bitorrent 下载的话说不定可以用。
* 通过 HTTP、FTP 或 itTorrent 下载实时映像文件 (Download live images using HTTP, FTP or BitTorrent )，其实就是 LiveCD 啦（但是这次用不上…… 😂）

所以这次选第一个：

![通过 HTTP 下载 CD/DVD 映像文件](/assets/get_debian_installation_image/get_cd_http.png)

嘛咱毕竟不是真的要去刻录一个 CD/DVD 出来，所以就选那个 “Official CD/DVD images of the "stable" release”下面的 amd64 的 DVD 咯 （其实就是 https://cdimage.debian.org/debian-cd/current/amd64/iso-dvd/ ）

![cdimage.debian.org](/assets/get_debian_installation_image/cdimage_debian_org.png)

然后翻到最下面把第一片（debian-9.4.0-amd64-DVD-1.iso ）下载下来就好。

    其实你也可以从镜像站下载的😂😂😂

## 验证文件

你需要验证 下载来的 Debian CD/DVD 映像文件，以确保没下载来个假货😂
所以你需要这么做：

Linux:
```console
$ export DEBIAN_CDIMAGE_BASE=https://cdimage.debian.org # 方便起见
$ wget $DEBIAN_CDIMAGE_BASE/debian-cd/current/amd64/iso-dvd/SHA512SUMS(,.sign) # 或 SHA256SUMS（MD5 / SHA-1 有弱点，不建议）
$ sha512sum -c SHA512SUMS
$ # 文件全部 OK，然而这还没完。
$ gpg --verify SHA512SUMS.sign
$ # 如果需要导入密钥，参考 https://www.debian.org/CD/verify
```
Windows: （待续）
