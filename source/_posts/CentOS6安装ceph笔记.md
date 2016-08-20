title: CentOS6安装ceph笔记
date: 2016-08-20 12:46:43
categories:
- 后端
- 运维
tags:
- centos
- ceph
- 存储
---

## 安装前准备
- 准备三台机器，分别修改hostname,`vim /etc/sysconfig/network`,分别修改成`ceph-node1`,`ceph-node2`,`ceph-node3`
```
    HOSTNAME=ceph-node1
 ```
- 在ceph-node1上设置免密码ssh登录
```
ssh-keygen
ssh-copy-id ceph-node2
ssh-copy-id ceph-node3
```

- 在ceph-node1的机器上修改hosts,`vim /etc/hosts`
```
192.168.57.101 ceph-node1
192.168.57.102 ceph-node2
192.168.57.103 ceph-node3
```
- 将hosts复制到cehp-node2,ceph-node3上
```
scp /etc/hosts root@cehp-node2:/ect/hosts
scp /etc/hosts root@cehp-node3:/ect/hosts
```

- 三台机器分别使用国内的163的yum镜像
```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
cd /etc/yum.repos.d/
wget http://mirrors.163.com/.help/CentOS6-Base-163.repo
mv CentOS6-Base-163.repo CentOS-Base.repo
yum makecache
```

- 三台机器分别升级系统
```
yum update
```

- 三台机器同步时间
```
ntpdate pool.ntp.org
service ntpd restart
chkconfig ntpd on
```

- 添加ceph的yum源，`vim /etc/yum.repos.d/ceph.repo`,内容如下：

- 使用国内ceph镜像,ceph最版本不提供centos6的包，只能安装hammer之前的版本。
```
export CEPH_DEPLOY_REPO_URL=http://mirrors.163.com/ceph/rpm-hammer/el6
export CEPH_DEPLOY_GPG_URL=http://mirrors.163.com/ceph/keys/release.asc
```

## 安装
- 在ceph-node1上执行,指定安装1.5.33,安装1.5.34会有问题：
```
yum install ceph-deploy-1.5.33-0.noarch
```

- 在ceph-node1上安装集群，执行：
```
mkdir /etc/ceph
cd /etc/ceph/
ceph-deploy new ceph-node1
```
如果报错提示为`Error in sys.exitfunc:`,则添加环境变量`export CEPH_DEPLOY_TEST=YES`即可。
如果报错提示为`No section: 'ceph';`,则执行`yum remove ceph-release`

- 在其他机器上安装ceph，在ceph-node1上执行：
```
ceph-deploy install --release hammer  ceph-node1 ceph-node2 ceph-node3
```

- 在ceph-node1上安装监控，执行:
```
ceph-deploy mon create-initial
```

- 准备磁盘，测试环境可以直接使用目录，如在三台机器上分别建立3个目录，ceph-node1上建立`/var/local/osd0`,后两台依次,然后执行:
```
ceph-deploy osd prepare ceph-node1:/var/local/osd0  ceph-node2:/var/local/osd1 ceph-node3:/var/local/osd2
```

- 启用osd
```
ceph-deploy osd activate ceph-node1:/var/local/osd0 ceph-node2:/var/local/osd1 ceph-node3:/var/local/osd2


>参考
[基于centos6.7的Ceph分布式文件系统安装指南](http://blog.csdn.net/yhao2014/article/details/51394815)
[ceph 测试环境搭建](http://files.cppblog.com/runsisi/ceph%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.pdf)
