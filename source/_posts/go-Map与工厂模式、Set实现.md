---
title: go-Map与工厂模式、Set实现
date: 2020年6月16日
comments: true
categories: go
tags:
---

**Map与工厂模式**

* map的value可以是一个方法
```go
func TestMapWithFunValue(t *testing.T) {
	m := map[int]func(in int) int{}
	m[1] = func(in int) int { return in }
	m[2] = func(in int) int { return in * in }
	m[3] = func(in int) int { return in * in * in }

	t.Log(m[1](3), m[2](3), m[3](3))
}
```
* 与Go的 Dock type 接口方式一，可以方便的实现单一方法的工厂模式
<!--more-->

**实现Set**

* go的内置集合中没有Set实现，可以map[type]bool
1. 元素的唯一性
2. 基本操作（添加元素，判断是否存在，删除元素，元素个数)

```go
func TestMapForSet(t *testing.T) {
	mySet := map[int]bool{}
	// 添加
	mySet[1] = true
	// 判断元素是否存在
	if mySet[1] {
		t.Log("key 1 存在")
	}
	// 元素个数
	t.Log(len(mySet))
	// 删除元素
	delete(mySet,1)
}
```
