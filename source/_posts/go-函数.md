---
title: go-函数
comments: true
categories: go
tags:
---

1. 可以有多个返回值
2. 所有参数都是值传递
3. 函数可以作为变量的值
4. 函数可以作为参数和返回值

```go

package _func

import (
	"fmt"
	"math/rand"
	"testing"
	"time"
)

func returnMultiValues() (int, int) {
	return rand.Intn(10), rand.Intn(20)
}

func timeSpent(inner func(op int) int) func(op int) int {
	return func(n int) int {
		start := time.Now()
		ret := inner(n)

		fmt.Println("time spent:", time.Since(start).Seconds())
		return ret
	}
}
func slowFun(op int) int {
	time.Sleep(time.Second* 1)
	return op
}

func TestFn(t *testing.T) {
	a, b := returnMultiValues()
	t.Log(a, b)
	tsSF := timeSpent(slowFun)
	t.Log(tsSF(10))
}


```