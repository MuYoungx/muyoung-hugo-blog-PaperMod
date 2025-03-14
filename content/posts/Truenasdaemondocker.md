---
title: TrueNAS24.10docker开机自动替换源
published: 2025-02-27T17:34:36+08:00
summary: ""
cover:
  image: "https://r.muyoung.com/blogimg/202502280936178.png"
tags: [truenas, docker]
categories: 'TrueNAS'
draft: false 
lang: ''
---

存放于/root目录下

![img](https://r.muyoung.com/blogimg/202502271732370.png)  

```bash
#!/bin/bash
sleep 30
# 备份原始文件
sudo cp /etc/docker/daemon.json /etc/docker/daemon.json.bak

# 使用 jq 添加新的键值对并写入临时文件
sudo jq '. + { "registry-mirrors": ["https://docker.1panel.live"] }' /etc/docker/daemon.json | sudo tee /etc/docker/daemon.json.tmp > /dev/null

# 检查 jq 是否成功
if [ $? -eq 0 ]; then
    # 替换原始文件
    sudo mv /etc/docker/daemon.json.tmp /etc/docker/daemon.json
    echo "成功更新 daemon.json 文件。"
else
    echo "更新 daemon.json 文件失败，恢复备份。"
    sudo mv /etc/docker/daemon.json.bak /etc/docker/daemon.json
fi

# 重启 Docker 服务
sudo systemctl restart docker
```