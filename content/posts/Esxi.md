---
title: vSphere Hypervisor(ESXI)
published: 2025-02-27T17:37:05+08:00
summary: "vSphere Hypervisor(ESXI)"
cover:
  image: "https://img.muyoung.com/202502271737317.png"
tags: [Vmware, 证书, 硬件直通,Esxi]
categories: '经验分享'
draft: false 
lang: ''
---

## VMware OVF Tool导出虚拟机 

1安装VMware-ovftool-4.3.0-12320924-win.x86_64.msi

2进入安装目录 C:\Program Files\VMware\VMware OVF Tool\ 输入cmd进入命令提示符

![image.png](https://img.muyoung.com/20250312134601037.png)

`

```
ovftool vi://root:@192.168.1.10/RockyLinuxServer E:\ovf\RockyLinuxServer.ova

ovftool vi://root:@192.168.1.10/WindowsServer2022 E:\ovf\WindowsServer2022.ova

ovftool vi://root:@192.168.1.10/DSM920 E:\ovf\DSM920.ova
```

`

## ESXI直通板载SATA控制器 

![image.png](https://img.muyoung.com/20250312134702011.png)



1查看”设备ID“和“供应商ID”

2SSH，不细表

3打开直通映射文件：

```
vi /etc/vmware/passthru.map
```

4. 编辑如下内容：

```
\# Union Point-H AHCI Controller

\# <供应商ID> <设备ID> d3d0 default

8086 a282 d3d0 default
```

5. 重启ESXi

6. 选中 Union Point-H AHCI Controller，点击“切换直通”，大功告成

## 修改ESXI的证书

连接SSH 进入 /etc/vmware/ssl 目录，找到 rui.key,rui.crt (注意备份原证书)选中删除，将之前修改好的ssl证书复制到该目录

如果不想重启esxi主机，可以在SSH下输入：

```
/etc/init.d/hostd restart

/etc/init.d/vpxa restart
```
