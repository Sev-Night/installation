# Redis v5.0.8 安装文档
## 1.安装环境
### 1.1 压缩包安装环境
- Centos7虚拟机
- Xshell远程连接工具

### 1.2 容器安装环境
- docker-ce 19

## 2.压缩包安装
### 2.1 下载
- 方式一，在目标路径下，执行下面命令下载
```
wget http://download.redis.io/releases/redis-5.0.8.tar.gz
```   
- 方式二，在本地浏览器下载后，通过远程传输工具传输到指定位置   [官方下载地址](https://redis.io/download)  
安装**lrzsz**工具，通过**rz**命令传输本地文件至虚拟机中。
```
 $ yum install lrzsz

 $ rz
```
### 2.2 安装
- 安装依赖（Centos 7 默认不安装gcc）  
```
$ yum install -y epel-release

$ yum install -y gcc

$ yum install tcl
```
- 解压   
```
$ tar xzf redis-5.0.8.tar.gz
```   
- 编译，测试
```
$ cd redis-5.0.8
$ make MALLOC=libc

$ make test
```

### 2.3 配置
- 打开配置文件
```
[root@localhost redis-5.0.8]# vi redis.conf
```
- 允许远程连接,注释下面一行
```
#bind 127.0.0.1 
```
- 后台运行进程
```
daemontize yes
```
- 日志输出文件(创建/var/log/redis/redis.log文件后，修改下面配置)
```
logfile "/var/log/redis/redis.log"
```
- 配置密码(设置为test，可任意设置)
```
requirepass test
```
- 复制配置文件
```
[root@localhost redis-5.0.8]# cp redis.conf /etc/redis/redis.conf
```   
- 复制redis 相关命令至 /usr/local/bin中
```
[root@localhost redis-5.0.8]# cp src/redis-server /usr/local/bin

[root@localhost redis-5.0.8]# cp src/redis-cli /usr/local/bin

[root@localhost redis-5.0.8]# cp src/redis-sentinel /usr/local/bin

[root@localhost redis-5.0.8]# cp src/redis-benchmark /usr/local/bin
```  
- 配置启动服务脚本
```
$ cat > /usr/lib/systemd/system/redis.service <<-EOF
[Unit]
Description=Redis 6379
After=syslog.target network.target
[Service]
Type=forking
PrivateTmp=yes
Restart=always
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli -h 127.0.0.1 -p 6379 -a jcon shutdown
User=root
Group=root
LimitCORE=infinity
LimitNOFILE=100000
LimitNPROC=100000
[Install]
WantedBy=multi-user.target
EOF
```  
- 启动redis，并设置开机自启动
```
$ systemctl daemon-reload

$ systemctl start redis

$ systemctl status redis
● redis.service - Redis 6379
   Loaded: loaded (/usr/lib/systemd/system/redis.service; disabled; vendor preset: disabled)
   Active: active (running) since Thu 2020-03-19 13:39:40 CST; 4s ago
  Process: 22508 ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
 Main PID: 22509 (redis-server)
   CGroup: /system.slice/redis.service
           └─22509 /usr/local/bin/redis-server *:6379

Mar 19 13:39:40 localhost.localdomain systemd[1]: Starting Redis 6379...
Mar 19 13:39:40 localhost.localdomain systemd[1]: Started Redis 6379.

$ systemctl enable redis
Created symlink from /etc/systemd/system/multi-user.target.wants/redis.service to /usr/lib/systemd/system/redis.service.
```

### 2.4 基本测试
```
$ redis-cli
127.0.0.1:6379> auth test  // 在配置文件中设置的密码
OK
127.0.0.1:6379> set foo bar
OK
127.0.0.1:6379> get foo
"bar"
```
[压缩包官方安装文档](https://redis.io/download)

## 3.容器安装
### 3.1 配置
-创建文件夹及相应配置文件(采用压缩包安装方式的一样配置)
```
$ mkdir /opt/data/redis

$ mkdir /opt/config/redis/

$ vi /opt/config/redis/redis.conf
```

### 3.2 安装
- 拉取镜像
```
$ docker pull redis // 最新稳定版

$ docker pull redis:
```
- 安装
```
$ docker run --name redis-server -d  \
-p 6378:6379 \
-v /opt/data/redis:/data  \
-v /opt/config/redis/redis.conf:/usr/local/etc/redis/redis.conf \
redis sh -c 'redis-server /usr/local/etc/redis/redis.conf --appendonly yes'
```

### 3.3 基本测试 
- 以交互方式进入容器
```
$ docker exec -it redis-server bash
```   
- 连接redis server，并执行简单测试
```
root@4cc2ed0d4e74:/data# redis-cli
127.0.0.1:6379> set foo hello
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth test   //在redis.conf中配置的密码
OK
127.0.0.1:6379> set foo hello
OK
127.0.0.1:6379> get foo
"hello"
127.0.0.1:6379> exit   //退出redis
root@4cc2ed0d4e74:/data# exit   //退出容器
exit

```  
至此，简单的容器安装测试结束


## 4 官方安装文档
- [压缩包方式安装文档](https://redis.io/download)
- [容器方式官方安装文档](https://hub.docker.com/_/redis)
