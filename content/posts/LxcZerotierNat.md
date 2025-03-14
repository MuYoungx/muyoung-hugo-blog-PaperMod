---
title: lxc搭建zerotier转发nat内网
published: 2025-03-12T14:46:06+08:00
summary: ""
cover:
  image: "https://r.muyoung.com/blogimg/202503041350331.png"
tags: [lxc, zerotier,nat]
categories: 'PVE'
draft: false 
lang: ''
---

宿主机 nano /etc/pve/lxc/lxc-id.conf
文件最后添加
```
lxc.cgroup2.devices.allow: c 10:200 rwm 
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

安装zerotier 
```
curl -s https://install.zerotier.com | sudo bash
```

加入
```
zerotier-cli join
```
设置转发
```
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf && sysctl -p
```
配置nat
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
开机自动nat 持久化保存
```
apt install -y iptables-persistent && bash -c iptables-save > /etc/iptables/rules.v4
```