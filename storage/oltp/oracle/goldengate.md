---
description: OGG 安装配置
---

# GoldenGate

## OGG 

### 下载安装

* 下载   中文和英文版本号 有些不同
  * 英文 [https://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html](https://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html) 
  * 中文 [https://www.oracle.com/technetwork/cn/middleware/goldengate/downloads/index.html](https://www.oracle.com/technetwork/cn/middleware/goldengate/downloads/index.html)
* 上传到服务器
* 创建OGG安装目录

  ```text
  mkdir -p /data/ogg_app
  chown -R oracle:oinstall /data/ogg_app
  tar -xvf 
  ```

* 添加环境变量

  ```text
  vi /etc/profile
  export JAVA_HOME=/usr/java/jdk1.8.0_201-amd64
  export OGG_HOME=/data/ogg_app
  export PATH=$PATH:$OGG_HOME
  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$OGG_HOME:$JAVA_HOME/lib/amd64/libjsig.so:$JAVA_HOME/lib/amd64/server/libjvm.so:$JAVA_HOME/lib/amd64/server:$JAVA_HOME/lib/amd64
  wq!

  # 生效
  source /etc/profile


  ```

### 配置



### 管理

* 启动

  ```text
  ./ggsci

  # 创建工作目录
  create subdirs


  ```



## 数据库配置

### 源端配置

```text
# 源数据库开启归档模式
sqlplus / as sysdba
ALTER DATABASE ADD SUPPLEMENTAL LOG DATA; # 开启附加日志
ALTER DATABASE FORCE LOGGING;  # 开启强制日志记录：
SHUTDOWN IMMEDIATE
STARTUP MOUNT
ALTER DATABASE ARCHIVELOG;    # 开启归档
ALTER DATABASE OPEN;
ALTER SYSTEM SWITCH LOGFILE;   
ALTER SYSTEM SET ENABLE_GOLDENGATE_REPLICATION=TRUE SCOPE=BOTH; # 设置ogg参数
EXIT



创建OGG单独使用的表空间:
SQL>create tablespace ogg_tbs datafile '+DATA' size 5G autoextend on next 64m;
创建ogg管理用户:
SQL>create user ogg identified by ogg$123 default tablespace ogg_tbs;
赋予权限：
SQL>grant connect, resource,CREATE SESSION to ogg;  
SQL>exec dbms_goldengate_auth.grant_admin_privilege('ogg');  
SQL>exec dbms_goldengate_auth.grant_admin_privilege(grantee=>'ogg');  
SQL>grant select any dictionary to ogg;  

```

