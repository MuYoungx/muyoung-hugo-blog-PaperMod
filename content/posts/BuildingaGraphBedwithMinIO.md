---
title: 使用MinIO搭建图床
published: 2025-03-04T12:31:57+08:00
summary: "使用MinIO搭建图床"
cover:
  image: "https://r.muyoung.com/blogimg/202503041232888.png"
tags: [minio,图床,反向代理,Nginx]
categories: '经验分享'
draft: false 
lang: ''
---

# 安装Docker

```auto
#更新系统索引以及安装必备组件
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
添加docker源GPG
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
#添加docker源
#官方源
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

#阿里源
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/debian \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
#更新源索引以及安装docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

![img](https://r.muyoung.com/blogimg/202503041209211.png)

# 使用Docker安装MinIO

```
docker run -d--name minio
-p 9000:9000 \
-p 9001:9001 \
-e"MINIO ROOT USER=minioadmin" \
-e"MINIO ROOT PASSWORD=minioadmin" \
-v /root/minio:/data \
minio/minio server /data --console-address":9001"
```

![img](https://r.muyoung.com/blogimg/202503041213465.png)

# 放行防火墙端口

放行云服务器安全组或者防火墙9000以及9001端口

## 访问9001端口测试

![img](https://r.muyoung.com/blogimg/202503041231538.png)

默认用户名密码为之前的docker参数 建议在docker参数定义时就定义要使用的管理员用户名密码

用户名：minioadmin

密码：minioadmin

# 反向代理

9000端口为实际api访问端口 9001为web页面控制台端口

## 反向代理web页面

```
 server {
         listen    80;
         listen    443 ssl http2;
         server_name  web页面控制台访问域名;

         ssl_certificate 证书文件路径;
         ssl_certificate_key 证书密钥路径;
         ssl_session_cache shared:SSL:1m;
         ssl_session_timeout  10m;
         ssl_ciphers PROFILE=SYSTEM;
         ssl_prefer_server_ciphers on;

location / {

        proxy_pass http://127.0.0.1:9001;
        proxy_set_header Host 127.0.0.1;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_http_version 1.1;      
        add_header X-Cache $upstream_cache_status;
}
```

## 反向代理api端口

```
 server {
         listen    80;
         listen    443 ssl http2;
         server_name  api访问域名;

         ssl_certificate 证书文件路径;
         ssl_certificate_key 证书密钥路径;
         ssl_session_cache shared:SSL:1m;
         ssl_session_timeout  10m;
         ssl_ciphers PROFILE=SYSTEM;
         ssl_prefer_server_ciphers on;

location / {

        proxy_pass http://127.0.0.1:9000;
        proxy_set_header Host 127.0.0.1;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_http_version 1.1;      
        add_header X-Cache $upstream_cache_status;
}
```

# 测试访问

## 创建bucket并设置公共权限

![img](https://r.muyoung.com/blogimg/202503041227934.png)

上传一张图片到bucket进行访问测试

输入刚刚设置的api访问域名/bucket名称/文件名称 例如https://obj.minio.com/data/test.png进行测试