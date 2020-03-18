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
```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```