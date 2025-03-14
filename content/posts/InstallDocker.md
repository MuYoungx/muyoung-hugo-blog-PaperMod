---
title: 安装Docker以及其他容器
published: 2025-03-12T14:58:23+08:00
summary: ""
cover:
  image: "https://r.muyoung.com/blogimg/20250312145937578.png"
tags: [docker]
categories: 'Docker'
draft: false 
lang: ''
---

## 一键安装脚本

```
curl -fsSL https://get.docker.com -o get-docker.sh 
sudo sh get-docker.sh
```
## Redhat系安装docker

```
安装依赖
dnf -y install yum-utils device-mapper-persistent-datalvm2

导入docker源
wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
官方源

yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
阿里源

dnf makecache
更新索引

dnf install docker-ce docker-ce-cli containerd.io
安装docker组件

```
## Debian系安装docker

```
安装依赖
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

导入gpg公钥
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

添加docker源 官方源
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

阿里源
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/debian \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

更新索引
sudo apt update

安装docker组件
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
## 常规Docker组件

### Portainer
```
拉取映像
docker pull portainer/portainer

创建并运行docker
docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /root/portainer:/data portainer/portainer

```