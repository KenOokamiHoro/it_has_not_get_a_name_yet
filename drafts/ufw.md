# ufw
UFW 全称为 Uncomplicated Firewall，是 Ubuntu 系统上默认的防火墙组件, 为了轻量化配置 iptables 而开发的一款工具。
UFW 提供一个非常友好的界面用于创建基于IPV4，IPV6的防火墙规则。

但是，UFW 是没有图形化用户界面的，它使用指令列操作，所以，操作起来就不是那么的方便，有人帮它写了个图形化用户界面，名字就叫做“Gufw”。

# 基本用法
ufw的用法十分简单，简单可以概括为如下内容
```bash
# 默认入网规则全部拒绝
ufw default deny
# 允许22号、TCP/UDP协议、从任何位置入网请求
ufw allow 22
# 允许SSH协议、从任何位置入网请求
ufw allow ssh
ufw allow http
ufw allow https
# 启用防火墙
ufw enable
# 显示防火墙状态和规则数字
ufw status numbered
# 删除2号规则
ufw delete 2
# 阻止192.168.1.5对所有端口的入网请求
ufw deny from 192.168.1.5 to any
# 阻止192.168.1.5对所有443（HTTPS）的入网请求
ufw deny from 192.168.1.5 to any port 443
# 查看ufw日志
less /var/log/ufw.log
# 重置ufw
ufw reset
```

# 注意事项
ufw 的规则是匹配模式。 意味着它的规则是从上到下，匹配到符合规则的就停止。
举个例子，如果你的网站开启了 ufw 允许80入网，然后通过`ufw deny from 192.168.1.5 to any`会发现不生效的
聪明的你一定想到了，那插入规则到第一行不就得了。
## 禁止14.3.20.228访问443，插入规则到第一行
`ufw insert 1 deny from 14.3.20.228 to any port 443`

# 除了deny和allow……
除了 deny，还有 reject 和 limit 两种语法，根据 manual，他们的区别是：
* limit： connection rate limiting ufw 会允许连接到该端口，但是一个 IP 如果在 30 秒钟之内尝试 6 次，那么就暂时封禁。这也就意味着`ufw limit ssh`在某些情况下是有必要的
* reject：给出拒绝原因而不是 deny 直接拒绝什么提示都没有（客户端会等待到超时）
