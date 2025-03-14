---
title: Pve开源虚拟化常见配置
published: 2025-03-04T13:50:14+08:00
summary: ""
cover:
  image: "https://r.muyoung.com/blogimg/202503041350331.png"
tags: [pve,硬件直通,swap]
categories: '[PVE]'
draft: false 
lang: ''
---

## 删除local-lvm

```
lvremove pve/data

lvextend -l +100%FREE -r pve/root
```

## 挂载swap文件

删除pve自带swap分区

```
swapoff -a

lvremove /dev/pve/swap

lvresize -l +100%FREE /dev/pve/root
以文件形式

#创建一个16G的swap，bs * count =16G

dd if=/dev/zero of=/swapfile bs=1G count=16

#配置安全的权限

chmod 0600 /swapfile

#格式化成swap

mkswap /swapfile

#挂载swap

swapon /swapfile

#验证

free -h

nano /etc/fstab      开机自动挂载

/swapfile  swap      swap    defaults   0       0

调整swap阈值

vi /etc/sysctl.conf 

sysctl -p
```



## 硬件直通

```
Intel CPU

对于Intel CPU，添加 intel_iommu=on，操作如下：

1、Shell 里面输入命令：nano /etc/default/grub

root@pve:~# nano /etc/default/grub

2、在里面找到：GRUB_CMDLINE_LINUX_DEFAULT="quiet"

然后修改为

GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"

编辑完成后，使用快捷键 Ctrl + O 回车保存文件，Ctrl + X 退出编辑器。

3、使用命令 update-grub 保存更改并更新grub

root@pve:~# update-grub

4、更新完成后，使用命令 reboot 重启PVE系统

root@pve:~# reboot

从命令行运行 dmesg | grep -e DMAR -e IOMMU 如果没有输出，则说明有问题。

如果有,可基本确认这个过程顺利完成! 接下来就可以为虚拟机正常的添加硬件直通了。
```

\-----------------------------------------------------------------------------------------------

## 增加虚拟化驱动，加载vifo系统模块

这仅在必要时启用IOMMU转换，将iommu分组相关的内核模块启用，从而可以提高VM中未使用的PCIe设备的性能。

然后是修改 /etc/modules 文件

root@pve:~# nano /etc/modules

```
添加如下内容

vfio

vfio_iommu_type1

vfio_pci

vfio_virqfd

CTRL +X 退出编辑，按Y保存，按回车退出。

更新initramfs

update-initramfs -u -k all
```

## 修改系统源

```
# 将此文件的中的所有内容注释掉

nano /etc/apt/sources.list.d/pve-enterprise.list

# 下载中科大的GPG KEY

wget https://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bookworm.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg

# 使用Proxmox非企业版源

echo "deb https://mirrors.ustc.edu.cn/proxmox/debian bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list

# 将Debian官方源替换为中科大源

sed -i 's|^deb http://ftp.debian.org|deb https://mirrors.ustc.edu.cn|g' /etc/apt/sources.list

sed -i 's|^deb http://security.debian.org|deb https://mirrors.ustc.edu.cn/debian-security|g' /etc/apt/sources.list

# 替换Ceph源

echo "deb https://mirrors.ustc.edu.cn/proxmox/debian/ceph-quincy bookworm no-subscription" > /etc/apt/sources.list.d/ceph.list

# 替换CT镜像下载源

sed -i 's|http://download.proxmox.com|https://mirrors.ustc.edu.cn/proxmox|g' /usr/share/perl5/PVE/APLInfo.pm
```



## 删除订阅弹窗

```
sed -Ezi.bak "s/(Ext.Msg.show\(\{\s+title: gettext\('No valid sub)/void\(\{ \/\/\1/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js && systemctl restart pveproxy.service
```

\# 执行完成后，浏览器Ctrl+F5强制刷新缓存