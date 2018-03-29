# 学会科学上网

~~众所周知，中华有个传统的技术——名曰 GFW（Great Fire Wall），这便是凝聚了咱国强大的技术精髓的结晶。~~

如果汝知道什么是科学上网，并且汝想在 Linux 发行版下使用软件进行科学上网的话，那么汝可以往下看啦。

如果汝不知道什么是科学上网，并且对百度、360 etc.的厂商感到很满意，汝就没有这个需求啦。

## Linux 下科学上网的方式

### ShadowSocks

俗称ss，如果汝有 ShadowSocks 帐号的话，可以采用下面的方式：

#### CLI（命令行界面）

首先，汝需要通过下列命令安装 ```python``` 和 ```pip```：

```sudo apt update && sudo apt install python python-pip```

随后，汝便可以使用 ```pip``` 安装 ```shadowsocks``` 了：

```sudo pip install shadowsocks```

或者使用 ```pip install --user shadowsocks``` 将其安装在汝的主目录下（需要在汝的终端模拟器配置（通常为 ```~/.bashrc```） 中加入 ```export PATH=$PATH:$HOME/.local/bin``` 并在终端中输入 ```source ~/.bashrc```）。

之后，汝就可以在终端中运行 ShadowSocks 的客户端程序来运行它啦：

```sslocal -s 服务器IP -p 服务器端口 -b 127.0.0.1 -l 本地映射端口 -k 密码 -m 方式（取决于服务端的设置，如 chacha20 等）```

当然，汝也可以把这些东西写在配置文件里：

这是官方的配置文件示例：

    {
          "server":"remote-shadowsocks-server-ip-addr",
          "server_port":443,
          "local_address":"127.0.0.1",
          "local_port":1080,
          "password":"your-passwd",
          "timeout":300,
          "method":"chacha20-ietf",
          "fast_open":false,
          "workers":1
    }

在汝写完配置文件之后（保存为 ```xxx.json```），便可以通过 ```sslocal -c 配置文件路径``` 来运行它了。

#### GUI（图形交互界面）

如果汝觉得 CLI 超麻烦的，那么汝也可以试试这种方式。

一般的 ShadowSocks GUI 客户端是 Shadowsocks Qt5，一个用 Qt5 写的客户端。

安装它很方便，只需要输入…… To be continued...
