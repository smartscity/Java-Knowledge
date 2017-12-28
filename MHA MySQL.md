# MHA MySQL



###  环境

1主1从，manager放在从库；

主库：192.168.0.10 

从库：192.168.0.20

### 添加从库



```mysql
mysql> show master status;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000001 |      154 |              |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
```

```mysql

然后在主库（192.168.0.10）
添加复制账号以及mha用的账号
grant replication slave on *.* to 'repl'@'192.168.0.10'  identified by 'repl';
grant replication slave on *.* to 'repl'@'192.168.0.20'  identified by 'repl'; 

grant all on *.* to 'root'@'192.168.0.20'  identified by '123';  
grant all on *.* to 'root'@'192.168.0.10'  identified by '123';  

```

```mysql
从库（192.168.0.10 ）change到mysql-bin.000001，pos点154
CHANGE MASTER TO  MASTER_HOST='192.168.0.10',MASTER_USER='repl',MASTER_PASSWORD='repl',MASTER_LOG_FILE='mysql-bin.000001',MASTER_LOG_POS=154;
```

```shell
ssh 互信
ssh-keygen -t rsa
ssh-copy-id -i  /root/.ssh/id_rsa.pub  '-p 22 192.168.0.10'
ssh-copy-id -i  /root/.ssh/id_rsa.pub  '-p 22 192.168.0.20'
```

安装MHA软件

```shell
首先安装epel源
rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
yum install perl-DBD-MySQL perl-Config-Tiny perl-Log-Dispatch perl-Parallel-ForkManager perl-Time-HiRes -y

安装MHA软件（两台机器）
tar xf mha4mysql-node-0.56.tar.gz 
cd mha4mysql-node-0.56
perl Makefile.PL
make && make install

tar xf mha4mysql-manager-0.56.tar.gz
cd mha4mysql-manager-0.56
perl Makefile.PL
make && make install
```

#### MHA Config

在从库（192.168.0.20）创建目录：

```shell
mkdir /data/mha/3306/log
cd /data/mha/3306/touch mha.cnf
```

```mysql
[server default]
client_bindir=/usr/local/mysql/bin/
manager_log=/data/mha/3306/log/manager.log
manager_workdir=/data/mha/3306/log
master_binlog_dir=/data/mysql/3306/binlog/
master_ip_failover_script=/usr/local/bin/master_ip_failover
master_ip_online_change_script= /usr/local/bin/master_ip_online_change
report_script=/usr/local/bin/send_report
init_conf_load_script=/usr/local/bin/load_cnf
remote_workdir=/data/mysql/3306/mysqltmp
#secondary_check_script= /usr/local/bin/masterha_secondary_check -s 192.168.0.30 -s 192.168.0.40
user=root
ping_interval=3
repl_user=repl
ssh_port=22
ssh_user=root
max_ping_errors=40

[server1]
hostname=192.168.0.10
port=3306

[server2]
candidate_master=1
check_repl_delay=0
hostname=192.168.0.20
port=3306
```

编辑文件 /usr/local/bin/load_cnf 里面的密码修改成对应的密码

```shell
#!/usr/bin/perl

  print "password=123\n";
  print "repl_password=repl\n";
```

执行chek命令查看复制是否正常：

```
masterha_check_repl --conf=/data/mha/3306/mha.cnf 
```

主库（192.168.0.10）执行命令，启动vip：

```
/sbin/ifconfig eth1:1 192.168.0.88/16
```

在线切换，把主库切到192.168.0.20

```shell
masterha_master_switch --master_state=alive --conf=/data/mha/3306/mha.cnf --new_master_host=192.168.0.20 --new_master_port=3306 --orig_master_is_new_slave
```

关于配置文件中的参数：
max_ping_errors=40，这个是修改了源码，增加了检测次数的定义，默认是3次，太容易误切换。

启动管理进程：

```shell
/usr/bin/nohup /usr/local/bin/masterha_manager --conf=/data/mha/3306/mha.cnf --ignore_last_failover > /data/mha/3306/log/manager.log 2>&1 &
```

在主库（192.168.0.10）封掉ip，可以看到日志输出。

```
iptables -I INPUT -s 192.168.0.20 -j DROP
```



### 参考

* http://www.cnblogs.com/gomysql/p/6547797.html