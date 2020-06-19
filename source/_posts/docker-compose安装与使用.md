---
title: docker-compose安装与使用
date: 2020年6月19日
comments: true
categories: linux
tags:
- docker
---

# docker-compose安装与使用

    Compose是用于定义和运行多容器Docker应用程序的工具。通过Compose，您可以使用YAML文件来配置应用程序的服务。然后，使用一个命令，就可以从配置中创建并启动所有服务.

<!--more-->

## 前置条件

肯定是需要安装docker了。。。

## docker-compose 安装

1. 运行以下命令以下载Docker Compose的当前稳定版本:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. 将可执行权限应用于 docker-compose 二进制文件

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

3. 测试安装

```bash
docker-compose --version
```

## 简单使用

1. 编写 docker-compose.yml 文件，如下所示：（这是一个网易云音乐客户端无版权代理到其他平台的小工具，推荐！）

```yml
  1 version: '3'
  2 
  3 services:
  4   unblockneteasemusic:
  5     image: nondanee/unblockneteasemusic
  6     container_name: neteasemusic
  7     restart: always
  8     environment:
  9       NODE_ENV: production
 10     command: -p 2233
 11     ports:
 12       - 2233:2233
```

2. docker-compose up -d
