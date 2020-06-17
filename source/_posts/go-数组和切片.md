---
title: go-数组和切片
date: 2020年6月16日
comments: true
categories: go
tags:
---

**数组声明和遍历**

```go
func TestArrayInit(t *testing.T) {
	var arr [3]int
	arr1 := [4]int{1, 2, 3, 4}
	arr2 := [...]int{1, 2, 3, 4, 5, 6}
	t.Log(arr[0], arr[1])
	t.Log(arr1,arr2)
}

func TestArrayTravel(t *testing.T) {
	arr3 := [...]int{1, 2, 3, 4, 5}
	for i,e := range arr3{
		t.Log(i,e)
	}
}
```
<!--more-->
**数组截取**
* a[开始索引(包含),结束索引(不包含)]
* a[3:],a[:3] 

```go
func TestArraySection(t *testing.T) {
	arr3 := [...]int{1, 2, 3, 4, 5}
	arr3_sec := arr3[3:]
	t.Log(arr3_sec)
}
```

**切片slice**
```go
func TestSliceInit(t *testing.T) {
	var slice []int
	t.Log(len(slice), cap(slice))
	slice = append(slice, 1)
	t.Log(len(slice), cap(slice))

	s1 := []int{1, 2, 3, 4}
	t.Log(len(s1), cap(s1))

	s2 := make([]int, 3, 5)
	t.Log(len(s2), cap(s2))
	t.Log(s2[0], s2[1], s2[2])
}
```
**切片扩容**
> 容量会 *2 扩展
```go
func TestSliceGrowing(t *testing.T) {
	var s []int
	for i := 0; i < 10; i++ {
		s = append(s, i)
		t.Log(len(s),cap(s))
	}
}
```

**切片共享存储结构**

> 修改一个值会影响到其他的值

```go
func TestSliceShareMemory(t *testing.T) {
	year := []string{
		"一月", "二月", "三月", "四月",
		"五月", "六月", "七月", "八月",
		"九月", "十月", "十一月", "十二月",
	}
	Q2 := year[3:6]
	t.Log(Q2, len(Q2), cap(Q2))
	summer := year[5:8]
	t.Log(summer,len(summer),cap(summer))
	summer[0] = "Unknow"
	t.Log(Q2)
	t.Log(year)
}
```

**数组和切片的区别**
1. 容量是否可伸缩
2. 是否可以进行比较