---
title: go-数据类型
comments: true
categories: go
tags:
---

**基本数据类型**
1. bool
2. string
3. int int8 int16 int 32 int 64
4. uint uint8 uint16 uint32 uint64 uintptr
5. byte
6. rune
7. float32 float64
8. complex64 complex128


**类型转化和其他主要编程语言的差异**
1. Go语言不允许隐式类型转换
2. 别和原有类型也不能进行隐式转换
<!--more-->
```go
package type_test

import "testing"

type MyInt int64
func TestImplicit(t *testing.T) {
	var a int32 = 1
	var b int64
	b = int64(a)
	var c MyInt
	c = MyInt(b)
	t.Log(b,c)
}
```

**类型的预定义值**

1. math.MaxInt64
2. math.MaxFloat64
3. math.MaxUint32

**指针类型**

1. 不支持指针运算
2. string是值类型，其默认的初始化为空字符串，而不是nil


```go
func TestPoint(t *testing.T) {
	a := 1
	aPtr := &a
	//aPtr++
	t.Log(a, aPtr)
	t.Logf("%T %T", a, aPtr)
}

func TestString(t *testing.T) {
	var s string
	t.Log("*" + s + "*")
	if s == "" {
		t.Log(len(s))
	}
}

```