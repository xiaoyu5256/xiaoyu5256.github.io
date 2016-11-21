title: centos安装php5.6
date: 2016-11-21 19:27:15
categories:
- 后端
- 运维
tags:
- centos
- php
---

### 配置yum源
centos6
```shell
# rpm -Uvh http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
# rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
```

centos7
```shell
# yum install epel-release
# rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
```

查看可安装的包
```shell
# yum list --enablerepo=remi --enablerepo=remi-php56 | grep php
```

### 安装php5.6
```
yum install --enablerepo=remi --enablerepo=remi-php56 php php-opcache php-devel php-mbstring php-mcrypt php-mysqlnd php-phpunit-PHPUnit php-pecl-xdebug php-pecl-xhprof php-soap php-fpm
```
