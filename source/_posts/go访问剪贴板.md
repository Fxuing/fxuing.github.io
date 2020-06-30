---
title: 生成微信小程序码
date: 2020年6月30日
comments: true
categories: go
tags:
---

# go访问剪贴板

访问剪贴板的库：github.com/atotto/clipboard，操作非常简单

<!--more-->

使用 clipboard.WriteAll() 写入到剪贴板

使用 clipboard.ReadAll() 读取剪贴板内容

示例：

```go
package main

import (
    "github.com/atotto/clipboard"
)

func main() {
	// 写入
    clipboard.WriteAll(`复制这段内容到剪切板`)

    // 读取
    content, err := clipboard.ReadAll()
    if err != nil {
        panic(err)
    }
    println(content)
}
```