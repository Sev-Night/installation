# Mysql v8.0.19
## 1.安装环境
### 1.1 压缩包安装环境
* Xshell
* Centos7虚拟机

### 1.2 容器安装环境


## 2.压缩包安装
### 2.1 安装
（1） **下载**
* 方式一：[Mysql Community Server yum源下载地址](https://dev.mysql.com/downloads/repo/yum/) ，浏览器下载后，通过远程连接工具传输文件。   
    在Xshell中安装lrzsz传输工具：
```
    yum install lrzsz
```   
   输入**rz**后，在弹出框中，选中文件，点击确认开始传输
```
    rz
```

* 方式二：在目标路径下，执行以下命令，直接下载：
```
    wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
```
补充：若没有wget方法，则通过**yum install wget**命令安装。    

（2）**安装Mysql yum源**    
执行下面命令后，在/etc/yum.repos.d/文件夹下，生成了mysql-community.repo和mysql-community-source.repo两个文件。
```
    sudo yum localinstall mysql80-community-release-el7-3.noarch.rpm
```   
补充：在mysql-community.repo中，通过配置对应版本的**enable=1**，即可安装指定版本。 通过下面的命令可以查看当前可以下载的mysql软件及版本。   
```
[root@localhost yum.repos.d]# yum repolist enabled | grep mysql
mysql-connectors-community/x86_64       MySQL Connectors Community           141
mysql-tools-community/x86_64            MySQL Tools Community                105
mysql80-community/x86_64                MySQL 8.0 Community Server           161
```
（3）**Yum安装Mysql**   
   执行 **rpm -ivh 安装包**命令，开始安装。
```
sudo yum install mysql-community-server
```   
补充：若是安装速度过慢，可以将镜像源改为国内镜像源，这里我选择阿里云镜像源，执行如下命令，再通过rpm命令重新安装mysql，即可
```
 wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```   
### 2.2 配置
（1）**启动**
```
[root@localhost ~]# systemctl start mysqld
[root@localhost ~]# systemctl status mysqld
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2020-03-18 19:28:07 CST; 5s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 11652 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 11729 (mysqld)
   Status: "Server is operational"
   CGroup: /system.slice/mysqld.service
           └─11729 /usr/sbin/mysqld
```    
启动过程中，主要有以下初始化事件：   
- Mysql server已经初始化。
- 在数据目录中生成了SSL证书和密钥文件。
- 安装并启用了validate_password。
- 创建了一个 **'root'@'localhost'**的超级用户，并生成一个临时密码。

(2) **查看密码**   
在/var/log/mysqld.log日志文件中，有这个超级用户的临时密码。可通过下面的命令找到：
```
[root@localhost ~]# sudo grep 'temporary password' /var/log/mysqld.log
2020-03-18T11:27:59.474810Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: JucT6!C,7rjk
```
（3）**修改超级用户密码**  
- 登陆   
```
[root@localhost ~]# mysql -uroot -p
Enter password:
```
- 修改密码
```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourpassword!';
```

### 2.3 基本测试
创建一个数据库test,并退出
```
mysql> create database test;
Query OK, 1 row affected (0.01 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.00 sec)

mysql> exit
Bye

```

## 3.容器安装
### 3.1 安装
- 创建所需文件夹   
```
mkdir /opt/data/mysql
mkdir /opt/config/mysql
```   
- 拉取镜像安装
```
$ docker pull mysql:latest

$ docker run --name mysql \
    --restart=always \
    -host mysql \
    -p 3306:3306
    -v /opt/data/mysql:/var/lib/mysql \
    -v /opt/config/mysql:/etc/mysql/conf.d \
    -e MYSQL_ROOT_PASSWORD=Mypassword! \
    -d mysql:latest
```   
说明：  
**a.** -v 将本地/opt/data/mysql挂载到容器中的/var/lib/mysql（默认保存数据的位置）上 ；   
**b.** -v 将本地的/opt/config/mysql挂载到容器中的/etc/mysql/conf.d（该容器中的）上，config-file.cnf文件会覆盖掉容器中/etc/mysql/my.cnf的相同项配置。
**c.** -e 通过设置环境变量的方式修改配置。

### 3.3 基本测试 


### 官方安装文档
[压缩包官方安装文档](https://dev.mysql.com/doc/refman/8.0/en/linux-installation.html)
[容器官方安装文档](https://hub.docker.com/_/mysql)

## 补充Mysql配置文档模板   
需要找到官方配置文档模板
```
[client]
port=3306
socket=/var/lib/mysql/mysql.sock
[mysql]
default-character-set=utf8
[mysqld]
user=mysql
port=3306
character-set-server=utf8
basedir=/usr/local/mysql
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
log-error =/var/log/mysqld.log
#是否开启慢查询日志，默认状态为开启，1 ，关闭 0
slow_query_log
#慢查询日志得超时时间
long_query_time=3
#是否记录不使用索引得查询记录（将会写入到慢查询日志中）
log_queries_not_using_indexes = 0
#慢查询日志得日志路径，需配合上面参数使用
log-slow-queries=/usr/local/mysql/log/log-slow-queries.log
pid-file =/data/mysqldata/mysql.pid
#设置默认数据库引擎
default-storage-engine=INNODB
#设置sql模式：禁止grant创建密码为空得用户，如果需要的存储引擎被禁用或未编译，那么抛出错误，一种严格得select查询GROUP BY操作，详细可参考网络上得解释
sql-mode="NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION，ONLY_FULL_GROUP_BY"
#最大连接数
max_connections=300
#指定查询结果缓存大小
query_cache_size=32M
#指定表的高速缓存，每打开一个表，表都会放入这里，可以加速访问，此值设置可以参考open_tables的值，若两者相等，则此值应该增加
table_open_cache=512
#设置线程缓存数，此值小的话，MySQL频繁创建线程，会消耗资源，配置的值参考：1G 8 、2G 16 、3G 32 。。。
thread_cache_size=38
#如果临时文件会变得超过索引，不要使用快速排序索引方法来创建一个索引
myisam_max_sort_file_size=1G
#MyISAM设置恢复表之时使用的缓冲区，REPAIR TABLE或用CREATE INDEX创建索引或ALTER TABLE过程中排序 MyISAM索引分配的缓冲区
myisam_sort_buffer_size=64M 
#设置索引块的缓冲区大小
key_buffer_size=290M 
#读查询操作的缓冲区大小
read_buffer_size = 1M 
#设置随机读缓冲区大小
read_rnd_buffer_size = 8M 
#设置MySQL执行排序的时候使用的大小
sort_buffer_size = 1M 
#如下注解
innodb_flush_log_at_trx_commit=2 
#innodb日志缓冲区大小，建议为1-8M
innodb_log_buffer_size=4M 
#innodb缓冲区大小，此值越大，可以减少磁盘IO，一般设置为内存的80%
innodb_buffer_pool_size=2G 
#设置innodb的日志文件大小，过大的话，将来做数据恢复会很慢
innodb_log_file_size=512M
# 并发数限制，设置为0则表示不限制并发
innodb_thread_concurrency=18 
#设置innodb为独立表空间模式，也就是每个表单独使用一个表空间，易于维护，
innodb_file_per_table = 1 
#设置innodb文件格式
innodb_file_format = Barracuda
#交互式连接的超时时间，单位为秒，默认8小时
interactive_timeout = 86400
#非交互式连接的超时时间
wait_timeout = 2147482 
#最大允许的包大小，
max_allowed_packet = 12M
#不使用DNS对连接进行解析
skip_name_resolve 
#以下两个选项用来设置读写IO的线程数，根据CPU核数来设置
innodb_write_io_threads = 4
innodb_read_io_threads = 4
# binlog
log-bin = /data/mysqldata/mysql-client-bin
server-id= 1
[mysqldump]  
max_allowed_packet = 512M
```
