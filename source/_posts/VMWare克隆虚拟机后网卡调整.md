title: VMWare克隆虚拟机后网卡调整
date: 2016-04-15 08:01:42
categories:
- 工具
- 系统工具
- 运维
tags:
- VMWare
- 运维
---

使用VMWare克隆虚拟机后，网卡会更换物理地址，就会找不到之前的网络配置信息。
解决办法：
#### 方法一
```
rm -rf /etc/udev/rules.d/70-persistent-net.rules
reboot
```
#### 方法二：
直接修改`/etc/udev/rules.d/70-persistent-net.rules`里的对应内容


