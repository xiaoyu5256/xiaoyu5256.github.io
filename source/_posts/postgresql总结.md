title: postgresql总结
date: 2017-08-04 09:31:06
categories:
- 后端
- 运维

tags:
- 数据库
- 运维
- postgresql
---

## centos安装postgresql
```shell
yum install https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-6-x86_64/pgdg-centos96-9.6-3.noarch.rpm
yum install postgresql96
yum install postgresql96-server
service postgresql-9.6 initdb
chkconfig postgresql-9.6 on
service postgresql-9.6 start

```

## 添加用户
```shell
sudo adduser dbuser
sudo su - postgres
psql
#进入数据库控制台，可设置密码,创建用户和数据库，授权
\password postgres
CREATE USER dbuser WITH PASSWORD 'password';
CREATE DATABASE exampledb OWNER dbuser;
GRANT ALL PRIVILEGES ON DATABASE exampledb to dbuser;
\q
```

## 控制台命令
- \h：查看SQL命令的解释，比如\h select。
- \?：查看psql命令列表。
- \l：列出所有数据库。
- \c [database_name]：连接其他数据库。
- \d：列出当前数据库的所有表格。
- \d [table_name]：列出某一张表格的结构。
- \du：列出所有用户。
- \e：打开文本编辑器。
- \conninfo：列出当前数据库和连接的信息。
- \q 退出数据库控制台
- \password 修改密码


