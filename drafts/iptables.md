# 防火墙工具 iptables

# iptables是个啥
iptables是一个运行在用户空间的应用软件，通过控制Linux内核netfilter模块，来管理网络数据包的流动与转送。iptables的操作需要用到超级用户的权限。

简而言之，iptables是Linux上自带的防火墙，然而它的功能远远不止于此，通过控制 netfilter 模块来管理网络数据包的流动与转送，iptables哈可以做到端口转发。
# iptables的结构
## 表 tables
“表”和不同的数据包处理有关。

我们常说，iptables是“四表五链”(或者三表五链)，实际上iptables一共有五个表：
* raw 用于配置数据包，raw 中的数据包不会被系统跟踪。
* filter 是用于存放所有与防火墙相关操作的默认表。
* nat 用于 网络地址转换（例如：端口转发）。
* mangle 用于对特定数据包的修改（参考 损坏数据包）。
* security 用于 强制访问控制 网络规则
我们平时一般主要会用到 **filter** 和 **nat**，其中 filter 链会拿来做咱们传统意义上的“防火墙”，nat链一般就拿来做转发了。
## 链 chains
决定数据包是否可以穿越的是 "链"。

iptables中共有五个链：
* PREROUTING
* INPUT
* FORWARD
* OUTPUT
* POSTROUTING

默认来说， filter 表包含 INPUT， OUTPUT 和 FORWARD 3条内建的链，这3条链作用于数据包过滤过程中的不同时间点。实在是太好理解了，INPUT, FORWARD, OUTPUT 就是对包的入、转发、出进行定义的三个过滤链，我们通过操作这三个链可以实现防火墙功能；nat 表包含PREROUTING， POSTROUTING 和 OUTPUT 链。
需要注意的是，每个表之间存在优先级，mangle > nat > filter
## 规则 rules
一条 "规则" 在链里面则可以决定是否送往下一条链（或其它的动作），比如说ACCEPT， DROP，我猜大家都懂的。

# 前世今生
![](/assets/iptables_ufw/Netfilter-packet-flow.svg)
图片来自维基百科，Arch Linux 上有一个[更简洁的图](https://www.frozentux.net/iptables-tutorial/images/tables_traverse.jpg)

我们大致可以看出：包首先要由 bridge check 来判断属于链路层还是网络层，之后包都会遇到 NAT 或者 mangle 表的 PREROUTING 链，从这 pre 前缀咱能才出来，这是要在路由之前要过的链。

接着当包通过了 PREROUTING 之后，便到了 decision 这个分支点，这里有一个对目的地址的判断，如果包的目的地是本地, 那么包向上走，走入 INPUT 链处理，然后进入 LOCAL PROCESS，之后再进入对应的 OUTPUT、POSTROUTING；

如果非本地，那么就进入 FORWARD 链进行转发，之后再走 POSTROUTING，我们在这里就不说 FORWARD 链的事了。

我们可以看到，filter 表在包的处理流程中，处于中间的位置，这部分也就是防火墙的主要功能所在。

# iptables 的基本用法

**警告：错误的使用 iptables 可能会造成很悲剧的结果哦**

由于 iptables 用法比较复杂，咱下面就列几个比较常见的用法

## 阻断某端口入网

## 允许 TCP 80 入网
`iptables –A INPUT –p tcp –dport 80 –j ACCEPT`

## 允许 ping
```bash
iptables -A OUTPUT -p icmp -j ACCEPT
iptables -A INPUT -p icmp -j ACCEPT  
```

## 拒绝22入网
`iptables -A INPUT -p tcp --dport 22 -j ACCEPT   `

## 开放 iptables
```bash
# 首先，我们先把 INPUT、FORWARD、OUTPUT 这三个链的规则设置成 ACCEPT 免得把自己锁在门外：
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT

# 然后，我们清除 NAT、Mangle 表，清除链，删除所有的非默认链
sudo iptables -t nat -F        #清除NAT表
sudo iptables -t mangle -F    #清除mangle表
sudo iptables -F    #清除所有链的规则
sudo iptables -X    #删除非默认链
```

参考资料：
https://xstarcd.github.io/wiki/Linux/iptables_forward_internetshare.html
