title: redis总结.md
date: 2016-07-23 13:09:34
categories:
- 后端
- redis
tags:
- redis
- 缓存
- 总结
---

## 安装
1 检查依赖，需要安装`gcc-c++`,`tcl`
```
yum install gcc-c++
yum install -y tcl
```
2 下载安装redis
```
wget http://download.redis.io/releases/redis-3.2.1.tar.gz
tar -zxvf redis-3.2.1.tar.gz
cd redis-3.2.1
make&&make install

```
3 安装完成后`/usr/local/bin`下会生成几个可执行文件

命令|描述
---|---
redis-server|Redis服务器端启动程序
redis-cli|Redis客户端操作工具。也可以用telnet根据其纯文本协议来操作
redis-benchmark|Redis性能测试工具
redis-check-aof|数据修复工具
redis-check-dump|检查导出工具

4 配置redis,修改daemonize配置项为yes，使Redis进程在后台运行
```
cp redis.conf /etc/
vi /etc/redis.conf
```

5 启动redis
```
redis-server /etc/redis.conf
```
6 添加开机启动
```
echo "/usr/local/bin/redis-server /etc/redis.conf" >>/etc/rc.local
```

7 配置参数说明
---|---
daemonize|是否以后台daemon方式运行
pidfile|pid文件位置
port|监听的端口号
timeout|请求超时时间
loglevel|log信息级别
logfile|log文件位置
databases|开启数据库的数量
save * * | 保存快照的频率，第一个*表示多长时间，第三个*表示执行多少次写操作。在一定时间内执行一定数量的写操作时，自动保存快照。可设置多个条件。
rdbcompression|是否使用压缩
dbfilename|数据快照文件名（只是文件名）
dir|数据快照的保存目录（仅目录）
appendonly|是否开启appendonlylog，开启的话每次写操作会记一条log，这会提高数据抗风险能力，但影响效率。
appendfsync|appendonlylog如何同步到磁盘。三个选项，分别是每次写都强制调用fsync、每秒启用一次fsync、不调用fsync等待系统自己同步
