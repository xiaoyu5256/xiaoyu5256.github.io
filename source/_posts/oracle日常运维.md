title: oracle日常运维
date: 2016-04-13 20:25:39
categories:
- 后端
- 运维

tags:
- 数据库
- 运维
- oracle
---

### 创建数据库
使用DB Config Assistant创建数据库后执行
```sql
sqlplus sys/mypasswd@sid as sysdba

CREATE TABLESPACE myspace DATAFILE 'E:\myspace\myspace01.DBF'  SIZE 15G AUTOEXTEND ON NEXT 1G MAXSIZE UNLIMITED LOGGING EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;

create user myuser identified by mypasswd default tablespace myspace temporary tablespace temp quota unlimited on myspace;
grant create session to myuser;
grant Resource to myuser;
grant connect to myuser;
grant DBA to myuser;

```

### 导入导出数据库
1. 使用`exp`和`imp`导入导出
```sql
# 导出数据
exp system/passwd@sid file=d:\dump\test.dmp full=y;

# 导入数据
imp username/passwd@sid full=y file=D:/dump/test.dmp ignore=y;
```
2. 使用`expdb`和`impdb`导入导出
```sql
sqlplus system/mypasswd@sid as sysdba
# 查看目录
select * from dba_directories;
# 如果没有合适的目录，则创建一个
create directory dump_dir as 'D:\dump';
# 导出scheme
expdp system/manage@sid DIRECTORY=dump_dir schemas=orguser dumpfile=dump.dmp 
# 导入scheme
impdp system/manage@sid directory=dump_dir dumpfile=dump.dmp  schemas=orguser remap_schema=orguser:newuser remap_tablespace=orgtable:newtable table_exists_action=replace logfile=impdp.log;
```

### 查看进程执行的sql
```
SELECT sql_text FROM v$sqltext a WHERE (a.hash_value, a.address) IN
(SELECT DECODE (sql_hash_value,0, prev_hash_value,sql_hash_value ),
  DECODE (sql_hash_value, 0, prev_sql_addr, sql_address)
  FROM v$session b
  WHERE b.paddr =
    (SELECT addr FROM v$process c WHERE c.spid='$pid'
      )
      ) ORDER BY piece ASC;
```



> 参考
> [Oracle exp/expdp imp/impdp导入导出数据](http://greensky.blog.51cto.com/3347654/1377202)

