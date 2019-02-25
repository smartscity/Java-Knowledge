# Install 12c

### Requirement

* JDK 
* Download URL

### Procedure

* 系统依赖包

  ```text
  yum -y install binutils compat-libcap1 \
          compat-libstdc++-33 compat-libstdc++-33*.i686 \
          elfutils-libelf-devel gcc gcc-c++ glibc*.i686 glibc \
          glibc-devel glibc-devel*.i686 ksh libgcc*.i686 libgcc \
          libstdc++ libstdc++*.i686 libstdc++-devel libstdc++-devel*.i686 \
          libaio libaio*.i686 libaio-devel libaio-devel*.i686 make \
          sysstat unixODBC unixODBC*.i686 unixODBC-devel unixODBC-devel*.i686 \
          libXp
  ```

* 建立用户和组

  ```text
  groupadd oinstall  
  groupadd dba  
  groupadd oper  
  useradd -g oinstall -G dba,oper oracle  
  echo "123456" | passwd --stdin oracle #oracle用户的登录密码，后续登录要用，记着。
  ```

* 创建安装目录

  ```text
  mkdir -p /orcl/app/oracle/product/12.1.0/db_1  
  chown -R oracle:oinstall /orcl/app  
  chmod -R 775 /orcl/app
  ```

* 修改内核参数vi /etc/sysctl.conf，添加：

  ```text
  vi /etc/sysctl.conf

  fs.aio-max-nr = 1048576  
  fs.file-max = 6815744  
  kernel.shmall = 2097152  
  kernel.shmmax = 1200000000    
  kernel.shmmni = 4096  
  kernel.sem = 250 32000 100 128  
  net.ipv4.ip_local_port_range = 9000 65500  
  net.core.rmem_default = 262144  
  net.core.rmem_max = 4194304  
  net.core.wmem_default = 262144  
  net.core.wmem_max = 1048576

  WQ!

  sysctl -p #使她生效
  ```

* 一种说法：上面的kernel.shmmax = 1200000000可能会有问题，可以改成4098955264。我在安装时有警告，但选择忽略后，安装能正常进行。
* 改文件限制：vi /etc/security/limits.conf，添加：

  ```text
  oracle soft nproc 2047  
  oracle hard nproc 16384  
  oracle soft nofile 1024  
  oracle hard nofile 65536  
  oracle soft stack 10240 
  ```

* 以及vi /etc/pam.d/login，添加：

  ```text
  vi /etc/pam.d/login
  session required pam_limits.so
  ```

* 修改ulimit：vi /etc/profile，添加：

  ```text
  if [ $USER = "oracle" ]; then  
  if [ $SHELL = "/bin/ksh" ]; then  
  ulimit -p 16384  
  ulimit -n 65536a  
  else  
  ulimit -u 16384 -n 65536  
  fi  
  fi
  ```

* 修改环境变量。vi ~oracle/.bash\_profile，添加：

  ```text
  ORACLE_BASE=/orcl/app/oracle  
  ORACLE_HOME=$ORACLE_BASE/product/12.1.0/db_1  
  ORACLE_SID=orcl  
  export ORACLE_BASE ORACLE_HOME ORACLE_SID  
  PATH=$ORACLE_HOME/bin:$PATH  
  export PATH  
  ```

* * 以oracle用户登录，开始安装

  ```text
  # su - oracle  
  $ cd /orcl/app/oracle  
  $ unzip linuxamd64_12102_database_se2_1of2.zip  
  $ unzip linuxamd64_12102_database_se2_2of2.zip  
  $ export LANG="en_US"  
  $ cd /orcl/app/oracle/database  
  $ ./runInstaller
  ```

  注：/orcl/app/oracle/下是2个目录：product和database。解压后的安装文件放在database下。

* 然后就会出现安装界面，配置过程从略。需要注意的是字符集要选择unicode。
* **start**：

  ```text
  sh database/runInstaller -silent -ignorePrereq  -responseFile /orcl/app/oracle/database/rsp/db_install.rsp

  $ORACLE_HOME/bin/netca -silent -responseFile /home/oracle/rsp/netca.rsp 
  正在对命令行参数进行语法分析:
  参数"silent" = true
  参数"responsefile" = /home/oracle/rsp/netca.rsp
  完成对命令行参数进行语法分析。
  Oracle Net Services 配置:
  正在配置监听程序:DG1
  监听程序配置完成。
  Oracle Net 监听程序启动:
      正在运行监听程序控制: 
        /u01/app/oracle/product/12.1.0/db_1/bin/lsnrctl start DG1
      监听程序控制完成。
      监听程序已成功启动。
  完成概要文件配置。
  成功完成 Oracle Net Services 配置。退出代码是0




  $ORACLE_HOME/bin/dbca -silent -createDatabase -responseFile /home/oracle/rsp/dbca.rsp -sysPassword 123456 -systemPassword 123456


  #su - oracle #切换到 oracle 用户且切换到它的环境
  $lsnrctl status #查看监听及数据库状态
  $lsnrctl start #启动监听
  $sqlplus / as sysdba #以 DBA 身份进入 sqlplus
  ```

* **stop**

  ```text
  #su - oracle #切换到 oracle 用户且切换到它的环境
  $lsnrctl stop #停止监听
  ```

* **login**

  ```text

  ```

  


> [https://blog.csdn.net/lbyyy/article/details/52870630](https://blog.csdn.net/lbyyy/article/details/52870630)

