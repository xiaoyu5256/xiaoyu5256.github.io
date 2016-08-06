title: owncloud私有云盘安装笔记
date: 2016-08-06 15:32:06
categories:
- 工具
- 私有云
tags:
- owncloud
- 私有云
- 网盘
---

## 安装笔记
服务器环境centos6.8,其他的操作系统会不同，请看官方文档。
1. php的版本必须5.4或5.4以上，安装php,mysql,apache我用操作系统自带的，操作系统若有之前版本php需卸载。
```
yum install centos-release-SCL
yum install php54 php54-php php54-php-gd php54-php-mbstring
yum install php54-php-mysqlnd
service httpd restart
chkconfig httpd on

yum install mysql-server mysql mysql-devel
service mysqld start
chkconfig mysqld on


```

2. 下载owncloud
```
wget https://download.owncloud.org/community/owncloud-9.1.0.tar.bz2 --no-check-certificate
tar -jxvf owncloud-9.1.0.tar.bz2
mv owncloud /var/www/
```

3. 目录权限，执行以下脚本即可。
```
#!/bin/bash
ocpath='/var/www/owncloud'
htuser='apache'
htgroup='apache'
rootuser='root'
printf "Creating possible missing Directories\n"
mkdir -p $ocpath/data
mkdir -p $ocpath/assets
mkdir -p $ocpath/updater
printf "chmod Files and Directories\n"
find ${ocpath}/ -type f -print0 | xargs -0 chmod 0640
find ${ocpath}/ -type d -print0 | xargs -0 chmod 0750
printf "chown Directories\n"
chown -R ${rootuser}:${htgroup} ${ocpath}/
chown -R ${htuser}:${htgroup} ${ocpath}/apps/
chown -R ${htuser}:${htgroup} ${ocpath}/assets/
chown -R ${htuser}:${htgroup} ${ocpath}/config/
chown -R ${htuser}:${htgroup} ${ocpath}/data/
chown -R ${htuser}:${htgroup} ${ocpath}/themes/
chown -R ${htuser}:${htgroup} ${ocpath}/updater/
chmod +x ${ocpath}/occ
printf "chmod/chown .htaccess\n"
if [ -f ${ocpath}/.htaccess ]
then
chmod 0644 ${ocpath}/.htaccess
chown ${rootuser}:${htgroup} ${ocpath}/.htaccess
fi
if [ -f ${ocpath}/data/.htaccess ]
then
chmod 0644 ${ocpath}/data/.htaccess
chown ${rootuser}:${htgroup} ${ocpath}/data/.htaccess
fi

```

4. 安装owncloud
```
 sudo -u apache php occ maintenance:install --database "mysql" --database-name "owncloud" --database-user "root" --database-pass "" --admin-user "admin" --admin-pass "owncloudpasswd"
```

5. 配置apache服务。
```
vim /etc/httpd/conf.d/owncloud.conf
```
添加内容：
```
nameVirtualHost *:80
<VirtualHost *:80>
    ServerAdmin webmaster@cloud.me
    DocumentRoot /var/www/owncloud
    ServerName cloud.me
    ErrorLog logs/cloud.me-error_log
    CustomLog logs/cloud.me-access_log common
</VirtualHost>
```
查看日志，执行`tail -f /var/log/httpd/cloud.me-error_log`即可。

6. 修改owncloud配置。
```
vim /var/www/owncloud/config/config.php
```
修改`overwrite.cli.url`成自己的域名，`trusted_domains`添加自己的域名。

7. 开启https。
```
yum install mod_ssl
mkdir /etc/httpd/ssl
```
生成证书
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/httpd/ssl/apache.key -out /etc/httpd/ssl/apache.crt

```
添加apache的ssl配置
```
 vi /etc/httpd/conf.d/owncloud-ssl.conf
```
内容如下：
```
<VirtualHost *:80>
  ServerAdmin youremail
  ServerName  youapp.com
  Redirect permanent / https://cloud.showmecode.cn/
</VirtualHost>

<IfModule mod_ssl.c>
  <VirtualHost *:443>
    ServerAdmin youremail
    ServerName youapp.com
    DirectoryIndex index.html index.php
    DocumentRoot /var/www/owncloud
    SSLEngine on
    SSLCertificateFile      /etc/httpd/ssl/apache.crt
    SSLCertificateKeyFile   /etc/httpd/ssl/apache.key
    <FilesMatch "\.(cgi|shtml|phtml|php)$">
       SSLOptions +StdEnvVars
    </FilesMatch>
    <Directory /var/www/owncloud/>
       Options +FollowSymlinks
       AllowOverride All

      <IfModule mod_dav.c>
        Dav off
      </IfModule>

      SetEnv HOME /var/www/owncloud
      SetEnv HTTP_HOME /var/www/owncloud

    </Directory>
    <Directory /usr/lib/cgi-bin>
       SSLOptions +StdEnvVars
    </Directory>

    BrowserMatch "MSIE [2-6]" \
                                nokeepalive ssl-unclean-shutdown \
                                downgrade-1.0 force-response-1.0
    # MSIE 7 and newer should be able to use keepalive
    BrowserMatch "MSIE [17-9]" ssl-unclean-shutdown

     <IfModule mod_headers.c>
        Header always set Strict-Transport-Security "max-age=15768000; includeSubDomains; preload"
      </IfModule>
  </VirtualHost>
</IfModule>
```


8. 浏览器打开网页，访问自己域名。


>参考
[Owncloud　安装实录(Apache2.4+PHP7+MariaDB10)](http://huifeng.me/2016/05/03/owncloud-install/)
[How To Create a SSL Certificate on Apache for CentOS 6](https://www.digitalocean.com/community/tutorials/how-to-create-a-ssl-certificate-on-apache-for-centos-6)
