# Docker 最新版本 安装文档
## 1.安装环境
- Centos7 虚拟机
- Xshell 远程连接工具
## 2.安装
### 2.1 卸载旧版本docke
```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 2.2 配置仓库
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
### 2.3 安装最新版Docker ce
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
[root@localhost ~]# docker version
Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b
 Built:             Wed Mar 11 01:27:04 2020
 OS/Arch:           linux/amd64
 Experimental:      false
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


## 4 安装指定版本Docker
### 4.1 搜索可安装版本
在执行2.2节的步骤之后，执行下面命令，查看可以安装的docker版本   
```
[root@localhost yum.repos.d]# yum list docker-ce --showduplicates|sort -r
 * updates: mirrors.aliyun.com
Loading mirror speeds from cached hostfile
Loaded plugins: fastestmirror
Installed Packages
 * extras: mirrors.aliyun.com
docker-ce.x86_64            3:19.03.8-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.8-3.el7                    @docker-ce-stable
docker-ce.x86_64            3:19.03.7-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.6-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.5-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.4-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.3-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.2-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.1-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:19.03.0-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.9-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.8-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.7-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.6-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.5-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.4-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.3-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.2-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.1-3.el7                    docker-ce-stable 
docker-ce.x86_64            3:18.09.0-3.el7                    docker-ce-stable 
docker-ce.x86_64            18.06.3.ce-3.el7                   docker-ce-stable 
docker-ce.x86_64            18.06.2.ce-3.el7                   docker-ce-stable 
docker-ce.x86_64            18.06.1.ce-3.el7                   docker-ce-stable 
docker-ce.x86_64            18.06.0.ce-3.el7                   docker-ce-stable 
docker-ce.x86_64            18.03.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            18.03.0.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.12.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.12.0.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.09.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.09.0.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.06.2.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.06.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.06.0.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.03.3.ce-1.el7                   docker-ce-stable 
docker-ce.x86_64            17.03.2.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable 
docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable 
 * base: mirrors.aliyun.com
Available Packages
[root@localhost yum.repos.d]# yum list docker --showduplicates|sort -r
 * updates: mirrors.aliyun.com
Loading mirror speeds from cached hostfile
Loaded plugins: fastestmirror
 * extras: mirrors.aliyun.com
docker.x86_64             2:1.13.1-109.gitcccb291.el7.centos              extras
docker.x86_64             2:1.13.1-108.git4ef4b30.el7.centos              extras
docker.x86_64             2:1.13.1-103.git7f2769b.el7.centos              extras
docker.x86_64             2:1.13.1-102.git7f2769b.el7.centos              extras
 * base: mirrors.aliyun.com
Available Packages

```   
### 4.2 安装指定版本
- 若是要安装docker 1.13.1.109，则代替2.3节内容执行下面命令，其余步骤与上面相同
```
$ yum -y install docker-1.13.1
$ docker version
Client:
 Version:         1.13.1
 API version:     1.26
 Package version: 
```  
- 若是出现下面问题，则是docker未启动。
```
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running? 
```  
- 启动docker，并设置未开机自启动。
```
$ systemctl start docker

$ systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
```
