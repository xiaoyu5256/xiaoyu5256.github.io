title: VMWare调整硬盘类别和大小
date: 2016-02-15 14:10:57
categories:
- 工具
- 系统工具
- 运维
tags:
- VMWare
- 运维
---

### 调整硬盘类别
建立虚拟机选择了硬盘类别为预分配后，发现物理硬盘占用太大，于是调整为自增长的类别。步骤如下：
1.打开cmd，切换到VMware Workstation安装目录。
2.执行`vmware-vdiskmanager -r  "D:\vm\CENTOS6.6模板\CENTOS6.6模板_3.vmdk" -t 0 "D:\vm\CENTOS6.6模板\CENTOS6.6模板_new.vmdk"`。
3.将`CENTOS6.6模板_new.vmdk`修改成`CENTOS6.6模板_3.vmdk`。

### 调整CentOS磁盘大小
虚拟机使用一段时间后，发现磁盘大小不够用，做扩容，步骤如下：
1.在虚拟机设置里修改磁盘大小，或者使用`vmware-vdiskmanager`做扩容。
2.打开虚拟机，执行`df -h`查看硬盘大小，发现磁盘大小未改变。
3.新建磁盘分区，执行`fdisk /dev/sda`先选`p`查看分区情况，然后选`n`,最后选择`w`保存分区，重启机器。
4.扩展LVM,执行`pvcreate "/dev/sda3"`，`vgextend /dev/mapper/VolGroup /dev/sda3`,`lvextend -L +20G /dev/mapper/VolGroup-lv_root`,`resize2fs /dev/mapper/VolGroup-lv_root`

>参考
[VMware虚拟机更改硬盘大小之扩大篇](http://www.linuxidc.com/Linux/2012-09/69568.htm)
