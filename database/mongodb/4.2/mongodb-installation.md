# MongDB v4.2 安装手册
## 1.安装环境
### 1.1 压缩包安装环境
- Centos7 虚拟机
- Xshell 远程连接工具

### 1.2 容器安装环境
- Docker19.0.3

## 2.Yum安装
### 2.1 安装   
- 配置yum源   
创建/etc/yum.repos.d/mongodb-org-4.2.repo文件，配置官方yum源
```
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc
```   
如果速度过慢，可以配置国内阿里云MongoDB yum源
```
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=http://mirrors.aliyun.com/mongodb/yum/redhat/7Server/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc
```
- yum安装MongoDB
```
$ sudo yum install -y mongodb-org // 安装最新稳定版本

$ sudo yum install -y mongodb-org-4.2.3 mongodb-org-server-4.2.3 mongodb-org-shell-4.2.3 mongodb-org-mongos-4.2.3 mongodb-org-tools-4.2.3   // 安装指定版本
```   
安装时，MongoDB的数据默认存储在**/var/lib/mongo**路径下，默认的日志数据存储在/var/log/mongodb路径下。

### 2.2 配置

### 2.3 基本测试
```
[root@localhost lib]# mongo
MongoDB shell version v4.2.3
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("5a6bc8ad-5704-44e7-8228-3c9845fe239f") }
MongoDB server version: 4.2.3
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings: 
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] 
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] 
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] 
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] 
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-03-20T23:18:34.665+0800 I  CONTROL  [initandlisten] 
---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display
metrics about your deployment (disk utilization, CPU, operation statistics, etc).

The monitoring data will be available on a MongoDB website with a unique URL accessible to you
and anyone you share the URL with. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command: db.enableFreeMonitoring()
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---

> 2+2
4
> 3+6
9
> show users;
> db.system.users.find()
> use test
switched to db test
> db
test
> show dbs;
admin   0.000GB
config  0.000GB
local   0.000GB
> db.test.insert({"name":"简单测试"})
WriteResult({ "nInserted" : 1 })
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
test    0.000GB
```


## 3.容器安装
### 3.1 安装

### 3.2 配置

### 3.3 基本测试 


### 官方安装文档
[官方安装文档]()
