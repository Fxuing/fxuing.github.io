---
title: docker 搭建 MinIO
date: 2020年10月16日
comments: true
categories: linux
tags:
- docker
- MinIO
---

## docker 搭建 MinIO

### 简介

> MinIO 是一个基于Apache License v2.0开源协议的对象存储服务。它兼容亚马逊S3云存储服务接口，非常适合于存储大容量非结构化的数据，例如图片、视频、日志文件、备份数据和容器/虚拟机镜像等，而一个对象文件可以是任意大小，从几kb到最大5T不等。

<!--more-->

### docker-compose 方式

docker-compost.yml

```yml
minio:
  image: minio/minio
  container_name: minio
  restart: always
  ports:
    - 9000:9000
  volumes:
    - /mut/data:/storage
    - /mnt/config:/root/.minio
  environment:
    MINIO_ASSESS_KEY: minio
    MINIO_SECRET_KEY: 12345678a
  command: server start object storage server
```

需要注意的几点：

1.  volumes  的 storage 和 command 的storage 一直，映射到容器里的目录 可以不是 storage

2.  MINIO_ASSESS_KEY 是登录页面的账号，MINIO_SECRET_KEY 是登录页面的密码

3.  如果访问不到，检查安全组是否打开了端口

### nginx 配置修改

通过上面的步骤后，已经可以使用 ip:port 访问到了，有域名的同学，可以配置下 nginx 转发

conf.conf

```nginx
location /minio {
    proxy_pass http://172.19.0.6:9000/minio;
    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    client_max_body_size 30m;
    client_body_buffer_size 256k;
    proxy_connect_timeout   90; 
    proxy_send_timeout      180;
    proxy_read_timeout      180;
    proxy_buffer_size       256k;
    proxy_buffers           16 256k;
    proxy_busy_buffers_size 1024k;
    proxy_temp_file_write_size      1024k;
}
```

加了上面这个配置后，可以通过 域名/minio 访问到了，但是还有个小问题，就是分享文件的时候会有问题，此时再加上一段

```nginx
location /share {
    proxy_pass http://172.19.0.6:9000/share;
    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    client_max_body_size 30m;
    client_body_buffer_size 256k;
    proxy_connect_timeout   90; 
    proxy_send_timeout      180;
    proxy_read_timeout      180;
    proxy_buffer_size       256k;
    proxy_buffers           16 256k;
    proxy_busy_buffers_size 1024k;
    proxy_temp_file_write_size      1024k;
}
```

>  share 是 bucket name
> 
>  tips: 通过nginx转发的话，可以关掉安全组的端口了 ：）



可惜带宽不够，不然就可以当做个人网盘用了，哈哈哈哈， 当然了，MinIO还是非常强大的，提供了好几种 SDK, 具体用法参考官网了： [MinIO Docs | MinIO快速入门指南](https://docs.min.io/cn/minio-quickstart-guide.html)


