---
title: Docker Compose 从入门到实践
published: 2025-03-14T10:07:40+08:00
summary: ""
cover:
  image: "https://r.muyoung.com/blogimg/20250312145937578.png"
tags: [docker,docker-compose]
categories: 'Docker'
draft: false 
lang: ''
---

# 介绍

Docker Compose 是一个用于定义和运行多容器 Docker 应用的工具。通过使用一个 YAML 文件，您可以配置应用程序需要的所有服务。然后，只需一个命令，就可以创建并启动所有服务。

TrueNAS SCALE 24.10 开始使用docker和docker compose部署应用，所以写这一篇简单的介绍一下如何编写docker compose文件（即TrueNAS SCALE 的自定义应用）

# 编写`docker-compose.yaml` 文件

`docker-compose.yaml` 文件使用 YAML 格式定义服务、网络和卷。以下是文件的基本结构：

```yaml
services:
  service_name:
    image: image_name:tag
    build: .
    ports:
      - "host_port:container_port"
    volumes:
      - host_path:container_path
    environment:
      - ENV_VAR=value
    depends_on:
      - other_service
```

有过docker-compose经验的同学可能已经发现了不对劲的地方，为什么第一行少了`version:'3'`，因为新版已经去掉了这个版本信息，直接写service就行

## 示例：部署一个简单的 Web 应用

我们以nginx举例

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - /mnt/SSD/apps/nginx:/usr/share/nginx/html
```

解释：

- **services**：定义服务列表，这里定义了一个名为`web` 的服务。
- **image**：指定使用的镜像，这里使用官方的 Nginx 镜像。
- **ports**：将主机的 8080 端口映射到容器的 80 端口。
- **volumes**：将本地的`/mnt/SSD/app/nginx` 目录挂载到容器内的`/usr/share/nginx/html`

注意：主机端口不要与其他应用冲突，`/mnt/SSD/app/nginx` 这个路径需要提前创建，你可以创建数据集或者直接命令行里创建目录，数据集的路径按下图示例：

![](https://www.truenasscale.com/usr/uploads/2024/10/3231654518.png)



在图片显示的路径前面要加上`/mnt`，也就是`/mnt/SSD/apps/nginx` 这样

![](https://www.truenasscale.com/usr/uploads/2024/10/1064205830.png)



点击保存，系统就会开始拉取镜像进行安装

然后我们可以尝试在映射的目录里写入一些东西

```shell
echo "Hello world" > /mnt/SSD/apps/nginx/index.html
```

然后访问[http://NAS](http://nas/) IP:8080/

![](https://www.truenasscale.com/usr/uploads/2024/10/3478431399.png)



更多用法可以点击[这里](https://chatgpt.com/share/66fe3e42-48dc-800c-bd61-63f4b920c2cc)