---
title: TrueNAS SCALE 套娃安装PVE虚拟机，并挂载NFS
published: 2025-03-14T10:19:00+08:00
summary: ""
cover:
  image: "https://obj.muyoung.com/blogimg/202502280936178.png"
tags: [pve,truenas,nfs]
categories: 'TrueNAS'
draft: false 
lang: ''
---

# 序

很多人看到这个标题可能就觉得没必要，或者部署很理解，实际上我其实一开始是玩一玩，试一下SCALE支不支持嵌套虚拟，我试过了是支持的。因为SCALE的虚拟机并不是非常好用，只能以zvol的方式当做硬盘，而且没有模板之类的东西，所以对于做实验什么的不是很方便，所以我才套娃一个PVE。

实际上用下来感觉还可以，做一些实验用的虚拟机没有什么问题。而且还可以挂载scale 的NFS，使用scale的快照之类的。

# 安装

首先去下载镜像，可以去[官网](https://www.proxmox.com/en/downloads)下载，该教程使用的版本为7.2-1

然后像安装普通的镜像一样安装

![图片.png](https://obj.muyoung.com/blogimg/20250314103120382.png)

![图片.png](https://obj.muyoung.com/blogimg/20250314103124162.png)

我这里直接给了24核心，这里根据自己的需求设置

![图片.png](https://obj.muyoung.com/blogimg/20250314103125381.png)

大小自己随意设置，后面我们可以挂载NFS的

![图片.png](https://obj.muyoung.com/blogimg/20250314103134173.png)

这里选择桥接网卡

![图片.png](https://obj.muyoung.com/blogimg/20250314103138143.png)

这里找到我们的ISO镜像

下面全部默认保存即可

开机，点击展示

![图片.png](https://obj.muyoung.com/blogimg/20250314103139936.png)

直接回车

![图片.png](https://obj.muyoung.com/blogimg/20250314103146717.png)

一路下一步，设置好密码

![图片.png](https://obj.muyoung.com/blogimg/20250314103151762.png)

这里的主机名要带`.local`，最后点击install等待安装

安装好之后我们直接点PowerOff

![图片.png](https://obj.muyoung.com/blogimg/20250314103156991.png)

![图片.png](https://obj.muyoung.com/blogimg/20250314103200510.png)


点击设备把CD删除，然后再开机

然后看vnc显示的地址，浏览器打开，输入账户密码即可登录

![图片.png](https://obj.muyoung.com/blogimg/20250314103202032.png)

# 挂载NFS

先创建一个数据集，点击共享，添加NFS

![图片.png](https://obj.muyoung.com/blogimg/20250314103204378.png)

## 设置数据集权限

因为PVE的nfs不支持填写账号密码，所以我们需要设置一下权限

![图片.png](https://obj.muyoung.com/blogimg/20250314103214047.png)

![图片.png](https://obj.muyoung.com/blogimg/20250314103209915.png)

确保这里是NFSV4

![图片.png](https://obj.muyoung.com/blogimg/20250314103207329.png)

![图片.png](https://www.truenasscale.com/usr/uploads/2022/05/2235625831.png)

![图片.png](https://obj.muyoung.com/blogimg/20250314103219064.png)

需要添加一条everyone的权限

![图片.png](https://obj.muyoung.com/blogimg/20250314103221157.png)

点击nfs

![图片.png](https://obj.muyoung.com/blogimg/20250314103222951.png)

内容有很多种，可以自己创建多个nfs都挂载