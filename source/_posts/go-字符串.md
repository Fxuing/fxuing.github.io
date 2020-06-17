---
title: go-字符串
date: 2020年6月16日
comments: true
categories: go
tags:
---

***与其他语言的差异***

1. string 是数据类型，不是引用或指针类型
2. string是只读的byte sclice,len函数可以返回它包含的byte长度
3. string的byte数可以存放任何数据
<!--more-->

**Unicode UTF-8**

1. Unicode 是一种字符集
2. UTF-8是unicode 的存储实现

常用转换

```go
func TestStringConv(t *testing.T) {

	// 整型转string
	s := strconv.Itoa(10)
	t.Logf("s type is %[1]T value is %[1]s", s)
	// 字符串转整型
	val,_ :=strconv.Atoi("10")
	t.Log(val+10)

	t.Log(Str2DEC("1000"))
}
```