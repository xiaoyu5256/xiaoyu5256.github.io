title: OSX下VMware虚拟网络ip设置
date: 2016-03-10 09:41:02
categories:
- 工具
- 系统工具
tags:
- VMWare
- 开发工具
---
虚拟机的网络访问方式分NAT模式和桥接模式。
NAT模式即使用的物理机的ip访问外部网络，通过虚拟网卡做nat转换，在window下可以通过界面直接修改虚拟ip设置。OSX下如果不使用dhcp自动分配虚拟机ip，则需要把虚拟机的ip设置到`192.168.71.0`网段下,网关设置成`192.168.71.2`。如果想自己指定网段，则需要修改：
```
# sudo vi /Library/Preferences/VMware\ Fusion/networking
# sudo vi /Library/Preferences/VMware\ Fusion/networking/vmnet8/dhcpd.conf
# sudo vi /Library/Preferences/VMware\ Fusion/networking/vmnet8/nat.conf
```

桥接模式相对简单，即使用物理主机同网段的ip即可。选择桥接模式，虚拟机的网关和物理机网关设置成一样，ip设置到物理机的同网段下。
