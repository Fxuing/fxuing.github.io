---
title: centos 安装 docker
date: 2020年6月19日
comments: true
categories: linux
tags:
- docker
---

docker 是一种开源容器化技术，用于构建和容器化应用程序

<!--more-->

# centos 安装 docker

## 安装yum-utils

```bash
yum install -y yum-utils
```

## 设置存储库

    自行选择源

```bash
## 官方源
yum-config-manager \
--add-repo \
https://download.docker.com/linux/centos/docker-ce.repo

## 阿里云
yum-config-manager \
--add-repo \
http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

## 安装 docker

```bash
yum install docker-ce docker-ce-cli containerd.io
```

## 启动 docker

```bash
systemctl start docker
```

    运行 hello-world 验证是否安装成功

```bash
docker run hello-world
```



## 参考官方文档

[Install Docker Engine on CentOS | Docker Documentation](https://docs.docker.com/engine/install/centos/)
