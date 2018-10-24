title: CentOS7安装Oracle11g笔记
date: 2016-08-20 12:46:43
categories:
- 后端
- 运维
tags:
- oracle
- 数据库
---

- 下载安装包[下载](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/112010-linx8664soft-100572.html),解压
- 添加host`192.168.192.130  vm_c7`
- 关闭selinux,防火墙
- 安装依赖包
    ```
    yum install gcc make binutils gcc-c++ compat-libstdc++-33elfutils-libelf-devel elfutils-libelf-devel-static ksh libaio libaio-develnumactl-devel sysstat unixODBC unixODBC-devel pcre-devel –y
    ```
- 添加安装用户和组
    ```
    groupadd oinstall
    groupadd dba
    useradd -g oinstall -G dba oracle
    passwd oracle
    id oracle
    ```
- 修改内核参数配置文件,`vim /etc/sysctl.conf`,添加内容,执行`sysctl -p`,其中`kernel.shmmax = 1073741824`为物理内存的一半
    ```
    fs.aio-max-nr = 1048576
    fs.file-max = 6815744
    kernel.shmall = 2097152
    kernel.shmmax = 1073741824
    kernel.shmmni = 4096
    kernel.sem = 250 32000 100 128
    net.ipv4.ip_local_port_range = 9000 65500
    net.core.rmem_default = 262144
    net.core.rmem_max = 4194304
    net.core.wmem_default = 262144
    net.core.wmem_max = 1048576
    ```
- 修改用户限制文件`vim /etc/security/limits.conf`,添加内容：
    ```
    oracle           soft    nproc           2047
    oracle           hard    nproc           16384
    oracle           soft    nofile          1024
    oracle           hard    nofile         65536
    oracle           soft    stack           10240
    ```
- 修改/etc/pam.d/login文件，添加内容：
    ```
    session required  /lib64/security/pam_limits.so
    session required   pam_limits.so
    ```
- 修改/etc/profile文件,添加内容：
    ```
    if [ $USER = "oracle" ]; then
    if [ $SHELL = "/bin/ksh" ]; then
    ulimit -p 16384
    ulimit -n 65536
    else
    ulimit -u 16384 -n 65536
    fi
    fi
    ```
- 创建安装目录
    ```
    mkdir -p /data/oracle/product/11.2.0
    mkdir /data/oracle/oradata
    mkdir /data/oracle/inventory
    mkdir /data/oracle/fast_recovery_area
    chown -R oracle:oinstall /data/oracle
    chmod -R 775 /data/oracle
    ```
- 设置oracle用户的环境变量
    ```
    su - oracle
    vim .bash_profile
    ```
    添加内容:
    ```
    ORACLE_BASE=/data/oracle
    ORACLE_HOME=$ORACLE_BASE/product/11.2.0
    ORACLE_SID=ora11
    PATH=$PATH:$ORACLE_HOME/bin
    export ORACLE_BASE ORACLE_HOME ORACLE_SIDPATH
    ```
- 编辑静默安装响应文件
    ```
    cp -R /data/oracle/oraclesetup/database/response/ .
    cd response/
    vim db_install.rsp
    ```
    设置内容如下：
    ```
    oracle.install.option=INSTALL_DB_SWONLY
    ORACLE_HOSTNAME=CentOS
    UNIX_GROUP_NAME=oinstall
    INVENTORY_LOCATION=/data/oracle/inventory
    SELECTED_LANGUAGES=en,zh_CN
    ORACLE_HOME=/data/oracle/product/11.2.0
    ORACLE_BASE=/data/oracle
    oracle.install.db.InstallEdition=EE
    oracle.install.db.DBA_GROUP=dba
    oracle.install.db.OPER_GROUP=dba
    DECLINE_SECURITY_UPDATES=true
    ```
- 根据响应文件静默安装Oracle11g
    ```
    cd /data/oracle/oraclesetup/database/
    ./runInstaller -silent -responseFile /home/oracle/response/db_install.rsp -ignorePrereq
    ```
- 安装成功后，按照脚本要求，打开终端，执行脚本
    ```
    sh /data/oracle/inventory/db_1/orainstRoot.sh
    sh /data/oracle/product/11.2.0/db_1/
    ```
- 以静默方式配置监听
    ```
    netca /silent /responseFile /home/oracle/response/netca.rsp
    ```
- 检查端口是否开启
    ```
    netstat -tnulp | grep 1521
    ```
- 以静默方式建立新库，同时也建立一个对应的实例,`vim /home/oracle/response/dbca.rsp`
  修改配置：
  ```
  GDBNAME= "ora11"
  SID =" ora11"
  SYSPASSWORD= " system@2016"
  SYSTEMPASSWORD= "system@2016"
  SYSMANPASSWORD= " system@2016"
  DBSNMPPASSWORD= " system@2016"
  DATAFILEDESTINATION=/data/oracle/oradata
  RECOVERYAREADESTINATION=/data/oracle/fast_recovery_area
  CHARACTERSET= "ZHS16GBK"
  TOTALMEMORY= "1638"

  ```

- 执行配置 ` dbca -silent -responseFile /home/oracle/response/dbca.rsp`
- 检查实例进程 `ps -ef | grep ora_`
- 查看监听状态 ` lsnrctl status`
- 若找不到监听，修改`/data/oracle/product/11.2.0/network/admin/listener.ora`,添加
    ```
    SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = ora11)
      (SID_NAME = ora11)
    )
  )
    ```
- 登录后查看实例 `sqlplus / as sysdba`,执行sql`SQL> select status from v$instance;`
- 修改 `/data/oracle/product/11.2.0/bin/dbstart`,将`ORACLE_HOME_LISTNER=$1`修改为O`RACLE_HOME_LISTNER=$ORACLE_HOME`
- 修改 `/data/oracle/product/11.2.0/bin/dbshut`,将`ORACLE_HOME_LISTNER=$1`修改为O`RACLE_HOME_LISTNER=$ORACLE_HOME`
- 修改`/etc/oratab`文件,将`orcl:/data/oracle/product/11.2.0:N`中最后的N改为Y，成为`orcl:/data/oracle/product/11.2.0:Y`
- 切换root用户，设置开机启动
- 设置开机启动
    ```
    su -
    vim /etc/rc.d/init.d/oracle
    ```
    添加内容:
    ```
    #!/bin/bash
    #oracle: Start/Stop Oracle Database 11g R2
    #chkconfig: 345 90 10
    #description: The Oracle Database is an Object-Relational Database ManagementSystem.
    #
    . /etc/rc.d/init.d/functions
    LOCKFILE=/var/lock/subsys/oracle
    ORACLE_HOME=/data/oracle/product/11.2.0
    ORACLE_USER=oracle

    case "$1" in
    'start')
    if [ -f $LOCKFILE ];then
    echo $0 already running.
    else
    echo -n $"StartingOracle Database:"
    su - $ORACLE_USER -c"$ORACLE_HOME/bin/lsnrctl start"
    su - $ORACLE_USER -c"$ORACLE_HOME/bin/dbstart $ORACLE_HOME"
    su - $ORACLE_USER -c"$ORACLE_HOME/bin/emctl start dbconsole"
    touch $LOCKFILE
    fi
    ;;

    'stop')
    if [ ! -f $LOCKFILE ]; then
    echo $0 already stopping.
    else
    echo -n $"StoppingOracle Database:"
    su - $ORACLE_USER -c"$ORACLE_HOME/bin/lsnrctl stop"
    su - $ORACLE_USER -c"$ORACLE_HOME/bin/dbshut"
    su - $ORACLE_USER -c"$ORACLE_HOME/bin/emctl stop dbconsole"
    rm -f $LOCKFILE
    fi
    ;;

    'restart')
    $0 stop
    sleep 5
    $0 start
    ;;

    'status')
    if [ -f $LOCKFILE ]; then
    echo $1 started.
    else
    echo $0 stopped.
    fi
    ;;
    *)
    echo "Usage: $0[start|stop|status]"
    exit 1
    esac
    exit 0
    ```
    修改权限`chmod 755 /etc/init.d/oracle`

- 设置开机启动`chkconfig oracle on`
