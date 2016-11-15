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
```

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
```

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
```

    mysql icms -uroot -e "SELECT source AS '受评对象', title AS '报告名称', ( SELECT cd.dict_key FROM cms_extend_data ed LEFT JOIN cms_content_dictionary cd ON ed.field_data = cd.dict_value WHERE ed.belong_content_id = content_id AND ed.belong_field_id = 11 AND cd.parent_id = 1 ) AS '等级', pub_date AS '评级时间', create_date AS '录入时间' FROM cms_content WHERE column_id = 822 AND state = 'pub' ORDER BY create_date DESC" >bond.xls
```
##导出数据库
```bash
mysqldump -ubitnami -p29d9bab753 bitnami_redmine > redmine-backup-$(date +%Y%m%d).sql
```

##授权并创建用户
```bash
grant all on icms.* to icmsuser@'10.10.7.32' identified by 'icms_db_pass';
flush privileges;
```

##删除用户
```bash
drop user icms@'%';
```
