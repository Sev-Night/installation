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
创建一个数据库
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

mysql> 

```

## 3.容器安装
### 3.1 安装

### 3.2 配置

### 3.3 基本测试 


### 官方安装文档
[官方安装文档](https://dev.mysql.com/doc/refman/8.0/en/linux-installation.html)
