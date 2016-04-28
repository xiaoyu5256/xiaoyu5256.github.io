title: Centos安装Python 2.7 python-pip
date: 2016-04-28 19:11:19

categories:
- 后端
- 运维
tags:
- centos
- python
---

### 准备工作

下载python包
```shell
wget http://python.org/ftp/python/2.7.11/Python-2.7.11.tgz
```

查看是否安装make工具
```shell
~#rpm -qa|grep make
automake-1.11.1-4.el6.noarch
make-3.81-20.el6.x86_64
```

如果没有安装make工具
```shell
yum -y install gcc automake autoconf libtool make
```

查看是否安装zlib库
```shell
~#rpm -qa|grep zlib
zlib-devel-1.2.3-29.el6.x86_64
zlib-1.2.3-29.el6.x86_64
```

安装zlib
```shell
yum install zlib-devel
```

检查是否安装ssl 库
```shell
~#rpm -qa|grep openssl
openssl-devel-1.0.1e-16.el6_5.x86_64
openssl-static-1.0.1e-16.el6_5.x86_64
openssl098e-0.9.8e-17.el6.centos.2.x86_64
openssl-1.0.1e-16.el6_5.x86_64
openssl-perl-1.0.1e-16.el6_5.x86_64
```

安装openssl
```shell
yum install openssl*
```

安装bzip2依赖库
```shell
yum install -y bzip2*
```


### 编译安装
```shell
tar -zxvf Python-2.7.11.tgz
vi Modules/Setup.dist
```
找到
```
#SSL=/usr/local/ssl
#_ssl _ssl.c \
#       -DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
#       -L$(SSL)/lib -lssl -lcrypto
......
#zlib zlibmodule.c -I$(prefix)/include -L$(exec_prefix)/lib -lz
```
去掉注释

编译
```
./configure --prefix=/usr/local/python2.7
make all
make install
make clean
make distclean
```
建立python2.7 软链
```shell
~#mv /usr/bin/python /usr/bin/python.bak
~#ln -s /usr/local/python2.7/bin/python2.7 /usr/bin/python2.7
~#ln -s /usr/bin/python2.7 /usr/bin/python
```

### 解决yum无法使用的问题
因为centos 6.7 下yum默认使用的是python2.6
```shell
vi /usr/bin/yum
----------------------------------------------------
#!/usr/bin/python
import sys
try:
    import yum
except ImportError:
```
修改为
```shell
#!/usr/bin/python2.6
```

### 安装python-pip工具
先安装setup-tools
```shell
wget https://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11-py2.7.egg  --no-check-certificate
chmod +x setuptools-0.6c11-py2.7.egg
sh setuptools-0.6c11-py2.7.egg
```

安装pip
```shell
wget https://pypi.python.org/packages/source/p/pip/pip-1.3.1.tar.gz --no-check-certificate
cp pip-1.3.1.tar.gz /usr/src/
tar zxvf pip-1.3.1.tar.gz
cd pip-1.3.1
python setup.py install
ln -s /usr/local/python2.7/bin/pip /usr/bin/pip
ln -s /usr/local/python2.7/bin/pip /usr/bin/pip2.7
```
