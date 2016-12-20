title: mysql日常运维

date: 2016-11-15 13:36:17
categories:
- 后端
- 运维

tags:
- 数据库
- 运维
- mysql
---


## centos安装mysql
```shell

    #下载文件
    mwget http://cdn.mysql.com/Downloads/MySQL-5.6/MySQL-5.6.25-1.linux_glibc2.5.x86_64.rpm-bundle.tar

    #查询是否安装
    rpm -qa|grep mysql

    #删除已安装的mysql
    rpm -e -allmatches -nodeps mysql-libs-5.1.73-3.el6_5.x86_64

    #启动mysql
    /etc/init.d/mysql start
```
## mysql无法登陆
```shell

    # /etc/init.d/mysql stop 
    # mysqld_safe --user=mysql --skip-grant-tables --skip-networking & 
    # mysql -u root
    mysql> use mysql
    mysql> UPDATE user SET Password=PASSWORD('root') where USER='root'; 
    mysql> FLUSH PRIVILEGES; 
    mysql> quit 
    # /etc/init.d/mysql restart 
    # mysql -uroot -p 
    Enter password: <输入新设的密码newpassword> 
    mysql> SET PASSWORD = PASSWORD('newpasswd');
```
##导出csv文件
```shell

    mysql icms -uroot -e "SELECT column AS '列名' FROM table  " >bond.xls
```
##导出数据库
```bash
mysqldump -uroot -ppassword dbname > dbname-backup-$(date +%Y%m%d).sql
```

##授权并创建用户
```bash
grant all on dbname.* to root@'localhost' identified by 'pass';
flush privileges;
```

##删除用户
```bash
drop user user@'%';
```
