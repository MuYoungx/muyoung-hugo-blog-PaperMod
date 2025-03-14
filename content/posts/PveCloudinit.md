---
title: Proxmox VE 中使用 Cloud 系统镜像快速创建虚拟机
published: 2025-03-14T10:13:42+08:00
summary: ""
cover:
  image: "https://r.muyoung.com/blogimg/202503041350331.png"
tags: [pve, cloudinit]
categories: 'PVE'
draft: false 
lang: ''
---

# 序

之前我出了一个套娃的教程，我说用PVE是为了可以快速的创建Linux虚拟机，这期就讲一下如何创建。

# 获取 Cloud Images

- CentOS: https://cloud.centos.org/centos/
- Ubuntu: https://cloud-images.ubuntu.com/releases/
- Debian: https://cloud.debian.org/images/cloud/
- Fedora: https://alt.fedoraproject.org/cloud/
- RedHat: https://access.redhat.com/downloads/
- openSUSE: http://download.opensuse.org/repositories/Cloud:/Images:/

下载qcow2的镜像

下载你需要的系统对应的 Cloud Images 镜像（Proxmox VE 支持两种 Cloud Images 类型，分别为 `nocloud v1` 和 `configdrive v2`），这里以 Debian 11 为例。可以直接使用 `wget` 在 PVE 里下载

```
wget https://cloud.debian.org/images/cloud/bullseye/latest/debian-11-nocloud-amd64.qcow2
```

# 安装

创建一个虚拟机，注意 SCSI 控制器必须是 `VirtIO SCSI`，无需创建硬盘，如果创建了自行分离删除
![h7t3.png](https://www.truenasscale.com/usr/uploads/2022/05/1960894046.png)




![htCD.png](https://www.truenasscale.com/usr/uploads/2022/05/564683641.png)





![hEWm.png](https://www.truenasscale.com/usr/uploads/2022/05/3638516927.png)



在创建的虚拟机硬件设置里添加 CloudInit 设备

![hLwA.md.png](https://www.truenasscale.com/usr/uploads/2022/05/3367534873.png)


使用 SSH 或者 Xftp 工具将镜像文件上传到 PVE 服务器（`wget` 下载的跳过此步），使用下面的命令将磁盘镜像导入到虚拟机，成功后 PVE 面板的 VM 硬件里会出现未使用的磁盘。

```
# 100 为虚拟机ID
qm importdisk 100 debian-11-nocloud-amd64.qcow2 local-lvm
```

![图片.png](https://www.truenasscale.com/usr/uploads/2022/05/508923776.png)



双击这个未使用磁盘启用它，总线/设置选择 `SCSI`，然后在 选项-引导顺序 中，将此磁盘设置为第一项

![图片.png](https://www.truenasscale.com/usr/uploads/2022/05/1272233665.png)


![图片.png](https://www.truenasscale.com/usr/uploads/2022/05/488918232.png)



修改 Cloud-Init，填入你需要设置的信息，Cloud-Init 会将 VM 的名字作为主机名

![图片.png](https://www.truenasscale.com/usr/uploads/2022/05/494586860.png)



扩容硬盘，启动 VM 即可，初次启动可能比较慢，如果卡了尝试重启试试。

![图片.png](https://www.truenasscale.com/usr/uploads/2022/05/2178628645.png)


如果 Cloud-Init 配置没有生效，使用 PVE 的控制台登录虚拟机，使用 `cloud-init -v` 命令查看是否安装了 Cloud-Init。如果没有返回值，则使用下方的安装命令安装即可：

```
# Centos
yum install cloud-init -y
# Debian
apt install cloud-init -y
```

# 常见问题

- hosts 文件 `/etc/hosts` 重启会重置修改 `/etc/cloud/cloud.cfg` 文件内容，将 `update_etc_hosts` 注释或删除掉即可
- DNS 配置文件 `/etc/resolv.conf` 重启会重置

# 排错

![图片.png](https://www.truenasscale.com/usr/uploads/2022/05/2501758491.png)



输入

```
ssh-keygen -A
```