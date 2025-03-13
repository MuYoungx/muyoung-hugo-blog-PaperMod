---
title: 在Picgo上配置Cloudflare-R2图床
published: 2025-03-12T09:51:40+08:00
summary: "在Picgo上配置Cloudflare-R2图床"
cover:
  image: "https://r.muyoung.com/blogimg/20250312095307639.png"
tags: [Picgo,R2,Cloudflare]
categories: '经验分享'
draft: false 
lang: ''
---

## 什么叫图床

图床就是将图片上传到相关服务商或者个人服务器，通过上传文件的网络地址进行远程访问。可以方便快速的将图片插入到文章中，方便后续图片二次使用、迁移、分享。

## 常用图床的几种方式

1. VPS自建：通过购买服务器搭建图床程序，比如easyimage，lsky-pro等。
   优点：方便，快捷，空间大
   缺点：速度取决于vps的线路，迁移服务时大量数据需要迁移。
2. 云端oss储存+cdn
   优点：稳定，速度快
   缺点：付费（腾讯cos/阿里oss+cdn(cdn需备案)）。免费的额度有限（Backblaze B2 + Cloudflare）。
3. Github + JsDelivr(cdn)
   优点：github绝对稳定，jsdelivr充当github的cdn加速
   缺点：虽然有加速，速度也算不上快，属中等。

本篇介绍CLoudFlare R2+Picgo 方案

CLoudFlare R2 免费用户有10GB/月的存储额度（30天内每天储存峰值的平均值），对于小网站基本足够，超额的存储是$0.015/GB/月，

- 标准储存：日常存储容量前10GB/月免费（30天内每天储存峰值的平均值）
- Daily Class A Transactions Caps：日常B类事务前100万次免费（A类事务包括下载、获取文件）
- Daily Class B Transactions Caps：日常A类事务前1000万次免费（B类事务包括创建存储桶、列举存储桶、列举文件版本、列举Keys）

![](https://r.muyoung.com/blogimg/20250312095541236.png)

## 实现目的

1.自由在MD，网站中引用图片（picgo上传）
2.防止恶意被刷流量（虽然CloudFlare只收取额外的储存费用，但被刷流量多了容易被封号）

- 自定义域名代替原域名（出现问题直接断开和原域名之间的跳转）
- 设定缓存规则（有人盗刷也是刷缓存）
- 防盗链（只在规定的网站使用）

## CloudFlare R2

### 注册账号

注册账号需要绑定一种支付方式（不扣费，只是用来选择支付方式），可以用信用卡，visa，paypal。

注册地址：https://dash.cloudflare.com/sign-up，验证过邮箱后即可使用。

![img](https://r.muyoung.com/blogimg/20250312095823613.webp)

### 开通 CloudFlare R2

点击右侧的R2对象储存。

![img](https://r.muyoung.com/blogimg/20250312100708757.webp)

在弹出的界面输入付款方式（信用卡，paypal都可以）

![img](https://r.muyoung.com/blogimg/20250312100834522.webp)

对于中小网站来说，一般超不了。
确认后就可以开通R2对象储存了。

### 创建储存桶及桶设置

![img](https://r.muyoung.com/blogimg/20250312100935006.webp)

![img](https://r.muyoung.com/blogimg/20250312101123620.webp)

### 自定义图床域名

进入桶设置界面

![img](https://r.muyoung.com/blogimg/20250312101353230.webp)

设定访问桶的域名，有两种方法，一种是有一个私有域名（**需托管在Cloudflare上**），另一个是用R2.dev子域名

#### 私有域名（二选一）

![img](https://r.muyoung.com/blogimg/20250312101130807.webp)

继续之后，点连接域，会自动生成dns记录。

![img](https://r.muyoung.com/blogimg/20250312101511888.webp)

之后就能浏览器 `https://<自定义域名>/<文件名>`访问存储桶里的文件了。

PS：如果域名不托管在CloudFlare，可以单独托管二级\三级域名，只需要给二级域名添加一条NS记录指向原托管即可。

#### R2.dev子域名（二选一）

![img](https://r.muyoung.com/blogimg/20250312102105922.webp)

点击允许访问，就可以用 `https://pub-853c2f66b8ef43ae98ecd186f4be34f8.r2.dev`访问桶

### 设定缓存规则

缓存规则一般设定两个：浏览器缓存和边缘缓存。
浏览器缓存：访问后，所需文件储存在浏览器的本地目录，在一段时间内，再次访问优先访问本地文件
边缘缓存：访问后，缓存在最近的CDN存一份，在一段时间内，优先访问CDN中的文件

进入域名页面->规则->页面规则

![img](https://r.muyoung.com/blogimg/20250312102103401.webp)

创建页面规则

![img](https://r.muyoung.com/blogimg/20250312102101155.webp)

URL 填 img.a.com/*
添加设置：缓存级别 – 缓存所有内容
添加设置：浏览器缓存 TTL – 几个小时自己选（8小时）
添加设置：边缘缓存 TTL – 一个月（图片内容只有存在或者删除两个状态，所以越长越好）
保存。此时如果有人刷流量，理论上图片都是本地缓存或者CDN缓存给的，不会走到R2对象存储。

![img](https://r.muyoung.com/blogimg/20250312102059139.webp)

### 防盗链设置

只能通过指定的网站来访问（防君子不防小人，可以伪造refer信息，照样刷流量）。
但是还是有点作用，比如别人爬取文章盗用的时候，图片是无法访问的。

安全性->WAF，创建规则

![img](https://r.muyoung.com/blogimg/20250312101529671.webp)

![img](https://r.muyoung.com/blogimg/20250312102047270.webp)

![img](https://r.muyoung.com/blogimg/20250312101547376.webp)

### 设置 CORS 策略（可选）

一般不需要设置（出于安全考虑这里也不建议设置）。如果遇到 R2 作为博客图床，但是图片打不开的情况，F12 控制台发现遇到 CORS 问题，则需要配置。

官方文档：[Configure CORS](https://developers.cloudflare.com/r2/buckets/cors/#add-cors-policies-from-the-dashboard)

进入你想设置 CORS 的存储桶的设置，拉到下面：

![img](https://r.muyoung.com/blogimg/20250312101549190.webp)

配置为允许特定源
比如要设置为允许两个域名：

```json
[
  {
    "AllowedOrigins": [
      "https://blog.a.com",
      "https://blog.b.top"
    ],
    "AllowedMethods": [
      "GET"
    ]
  }
]
```

配置为所有网站源可访问（多平台文章引用）

```json
[
  {
    "AllowedOrigins": [
      "*"
    ],
    "AllowedMethods": [
      "GET"
    ]
  }
]
```

配置为允许所有源
如果上面配置为特定源后仍然不能修复问题，或者作为随机图片 API 的图床提供服务需要设置为所有源可访问，那么需要如下设置：

```json
[
  {
    "AllowedOrigins": [
      "*"
    ],
    "AllowedMethods": [
      "GET",
      "POST",
      "PUT",
      "DELETE",
      "HEAD"
    ],
    "AllowedHeaders": [
      "*"
    ]
  }
]
```

### WEB API设置

有的api令牌就可以用软件（PICGO）自动上传图片了。

创建令牌

![img](https://r.muyoung.com/blogimg/20250312102030653.webp)

![img](https://r.muyoung.com/blogimg/20250312102028068.webp)

需要选择内容
令牌名
权限（对象读和写）
指定储存桶
TTL时间（永久）

![img](https://r.muyoung.com/blogimg/20250312101607270.webp)

记住生成的密钥，picgo软件里面设置需要。

![img](https://r.muyoung.com/blogimg/20250312101609388.webp)

## picgo设置

在插件设置中，添加常用插件。
S3插件：用来登录S3的图床
compress-next:用来压缩图片至webp。
watermark：给图片打水印
autoback：用来备份图床

![img](https://r.muyoung.com/blogimg/20250312101610766.webp)

![img](https://r.muyoung.com/blogimg/20250312102017236.webp)

安装好后，里面就新增了amazon S3的图床设置。

![img](https://r.muyoung.com/blogimg/20250312101615188.webp)

![img](https://r.muyoung.com/blogimg/20250312102012703.webp)

这里有几项配置需要尤其注意。

> - 应用密钥 ID，填写 R2 API 中的 `Access Key ID`（访问密钥 ID）
> - 应用密钥，填写 R2 API 中的`Secret Access Key`（机密访问密钥）
> - 桶名，填写 R2 中创建的 `Bucket 名称`，如创建R2的桶的名字 `img`
> - 文件路径，上传到 R2 中的文件路径，这里选择使用 `{fileName}.{extName}` (或者`{fullName}`)来保留原文件的文件名和扩展名。
> - 自定义节点，填写 R2 API 中的「为 S3 客户端使用管辖权地特定的终结点」，即 `xxx.r2.cloudflarestorage.com`格式的 S3 Endpoint
> - 自定义域名，填写上文生成的`https://xxx.r2.dev`格式的域名或自定义域名，如我配置的`https://img.a.com`
> - ForcePathStyle：`no关闭`，否则会在最终路径里面显示有桶名。
> - 拒绝无效TLS证书连接 ：`yes开启`，如果出现证书错误可以关闭
> - ACL访问控制列表：`public-read`
> - Bucket前缀：`false`

完成上述配置后，我们就可以在「上传区」直接拖入文件进行图片上传了，如上传后显示无误则为配置成功，生成的链接会自动在系统剪贴板中，直接在需要的地方粘贴即可。

PS：
1.picgo还可以自定义连接格式（Picgo→自定义链接格式）`![$fileName]($url)`,之后上传链接界面选custom即可

![img](https://r.muyoung.com/blogimg/20250312102008331.webp)

![img](https://r.muyoung.com/blogimg/20250312101929589.webp)

2.安装picgo的vscode插件-markdown transfer img，可以批量替换md中图片至云端。
3.使用piclist来平替picgo，piclist功能更多
