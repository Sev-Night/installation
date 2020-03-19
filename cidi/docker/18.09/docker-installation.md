# Docker v18.09 安装文档
## 1.安装环境
- Centos7 虚拟机
- Xshell 远程连接工具
## 2.安装
### 2.1 配置仓库
- 安装yum-utils（提供yum-config-manager工具）、device-mapper-persistent-data和lvm2（是devicemapper存储驱动的依赖）。 
```
  sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
- 设置稳定的版本仓库   
设置官方镜像源
```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```   
若是速度慢，可使用阿里云镜像源   
```
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```   
### 2.2 安装最新版Docker ce
- 将软件包信息缓存到本地（为了提高搜索安装软件的速度）
```
   yum makecache fast
```   
- 安装最新稳定版docker ce
```
[root@localhost ~]# sudo yum install docker-ce docker-ce-cli containerd.io
```   
## 3.启动测试   
### 3.1 启动
```
    systemctl start docker
```  
### 3.2 配置docker国内镜像仓库   
在/etc/docker/daemon.json中添加内容
```
 {"registry-mirrors":["https://registry.docker-cn.com"]}
```
执行下面命令是配置生效   
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 3.3 简单测试 
执行下面命令，进行简单测试  
```
[root@localhost docker]# docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/    
```  
至此，简单的安装部署已结束。   
[官方安装地址](https://docs.docker.com/install/linux/docker-ce/centos/)