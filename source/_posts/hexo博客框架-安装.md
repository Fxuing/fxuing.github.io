---
title: hexo博客框架-安装
date: 2020年6月19日
comments: true
categories: 
tags:
- hexo
---

    hexo 是一个快速、简洁且高效的博客框架。hexo使用Markdown解析文章，在几秒内，即可利用靓丽的主题生成静态网页


<!--more-->


## 方式一： 参考官方文档

* 安装前提
  
  > 1. node.js > 8.10,建议10.0+版本
  > 2. Git 

* 安装 hexo

```bash
npm install -g hexo-cli
```

* 初始化 hexo

```bash
$ hexo init blog
$ cd blog
$ hexo install
```

* 启动 hexo

```bash
$ hexo server
```

访问 ip:4000即可看到

![UTOOLS1592555022132.png](https://user-gold-cdn.xitu.io/2020/6/19/172cbad2f1f7a4af?w=1885&h=988&f=png&s=530336)

## 方式二：docker部署

1. 编写 docker-compose.yml
* /data/blog 是我存放的目录，我是整个目录挂载，可根据需求，自行变更
  
   执行 ` docker exec blog ls / ` 可以看到，需要挂载的目录是 docker-hexo 
  
   ![UTOOLS1592555337252.png](https://user-gold-cdn.xitu.io/2020/6/19/172cbb1fb6041a85?w=645&h=408&f=png&s=11908)

yml如下：

```yml
 24   blog:
 25     image: zeusro/hexo
 26     container_name: blog
 27     restart: always
 28     ports:
 29       - 4000:4000
 30     volumes:
 31       - /data/blog:/docker-hexo
```

2. docker-compose up -d

3. 访问ip:4000，效果同上

// TODO 主题修改，自行参考官方文档

[文档 | Hexo](https://hexo.io/zh-cn/docs/)


