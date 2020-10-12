---
title: 基于AOP实现分页
date: 2020年10月12日
comments: true
categories: java
tags:
- 分页
- mybatis-plus
---

### 基于AOP实现分页

最近在用`mybatis-plus`，真香~
和老大讨论的时候，他建议把分页参数剥离出来，因为分页和业务没有直接联系。开搞！

主要思路呢，就是使用`AOP`前置处理的时候，检查请求参数中是否包含了分页参数，如果包括了分页参数，将分页参数和条件查询参数设置到`ThreadLocal`里面，关于`ThreadLocal`的信息，请参考：[线程局部存储](https://zh.wikipedia.org/wiki/%E7%BA%BF%E7%A8%8B%E5%B1%80%E9%83%A8%E5%AD%98%E5%82%A8#Java)

下一步就是使用了，首先看看类图：

<!--more-->

![0RQ0iR.png](https://s1.ax1x.com/2020/10/12/0RQ0iR.png)

自定义一个`BaseService`继承`Mybatis-plus`的`Iservice`，`BaseServiceImpl`继承`ServiceImpl`,然后重写`list()`方法。

代码如下：

AOP：

![0R1cUH.png](https://s1.ax1x.com/2020/10/12/0R1cUH.png)

> 前置处理：是否包含分页参数和条件查询参数，有的话，就设置到 ThreadLocal 里面
> 
> 后置处理：是否有分页，有分页的话，拼装分页返回数据

BaseService:

![0RlfnU.png](https://s1.ax1x.com/2020/10/12/0RlfnU.png)

BaseServiceImpl：

![0RlzAH.png](https://s1.ax1x.com/2020/10/12/0RlzAH.png)

> 设置条件目前只有3个：完全相等、模糊匹配、区间搜索。当然，可以根据自己需求，不断往里面加

好了，来看下效果吧。

请求：

没有任何参数 == 普通列表

![0RJTln.png](https://s1.ax1x.com/2020/10/12/0RJTln.png)

有分页参数 == 分页查询

![0RYsNF.png](https://s1.ax1x.com/2020/10/12/0RYsNF.png)

有分页参数和条件参数 == 条件搜索+分页查询

![0Rw96H.png](https://s1.ax1x.com/2020/10/12/0Rw96H.png)

哦对，使用也非常简单，只需要:

![0RtN5D.png](https://s1.ax1x.com/2020/10/12/0RtN5D.png)

仅供研究~ 目前还没有使用这种方式，因为这个是单表的哈哈哈，多表的，有时间再研究吧

![0.png](https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1403753716,1596817704&fm=26&gp=0.jpg)
