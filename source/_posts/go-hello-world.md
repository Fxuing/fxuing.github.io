---
title: go-Hello World
comments: true
categories: go
tags:
---

```go
package main // 包，声明代码所在的木块

import "fmt" //引入代码依赖

func main(){ // 功能实现
	fmt.Println("hello world")
}
```

> 应用程序入口

* 必须是main包
* 必须是main方法
* 文件名不一定是main.go


 **与其他语言的差异**

> 退出返回值

* go 中main函数不支持任何返回值
* 通过 os.exit来返回状态

> 获取命令行参数

* main函数不支持传入参数

```
func main(arg [] string)
```

* 在程序中直接通过 os.Args 获取命令行参数
