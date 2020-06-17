---
title: go-运算符
comments: true
categories: go
tags:
---

* Go 语言没有前置的 ++ -- 如： ++1,--1
* 用 == 比较数组
1. 相同维度数组且含有相同个数元素的数组才可以比较
2. 每个元都有相同的才相等
```go
func TestCompareArray(t *testing.T) {
	a := [...]int{1, 2, 3, 4}
	//b := [...]int{1, 2, 3, 4, 5}
	c := [...]int{1, 2, 3, 4}
	d := [...]int{1, 2, 3, 6}
	//t.Log(a == b)
	t.Log(a == c,a==d)
}
```

* 位运算符

> &^ 按位清零，对于两个操作数来说，只要是右边位数为1，无论左边操作位数是0还是1，都会把左边位数清零，如果右边位数为0，那左边原来是什么就是什么

1 &^ 0 -- 1

1 &^1 --0

0 &^1 --0

0 &^ 0 --0

```go
const (
	Readable = 1 << iota // 1 << 0 = 1
	Writable			// 1 << 1 = 2
	Executable			// 1 << 2 = 4
)
func TestBitClean(t *testing.T) {
	a := 7 // 0111
	a = a&^ Readable
	t.Log(a&Readable == Readable, a&Writable == Writable, a&Executable == Executable)
	t.Log(a &^ Readable)
}

```