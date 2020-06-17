---
title: go-Map声明、访问、遍历
date: 2020年6月16日
comments: true
categories: go
tags:
---

**声明方式**

> map[key类型]value类型{},如
map[string]string{}

**获取key**
> 在访问的key不存在时，仍会然回零值，不能通过返回nil来判断元素是否存在
<!--more-->
```go
func TestGetKey(t *testing.T) {
	m1 := map[int]int{}
	t.Log(m1[100])

	m1[2] = 0
	t.Log(m1[2])
	m1[20] = 2000
	if value,exist := m1[20]; exist{
		t.Log("key 存在",value)
	}else{
		t.Log("key 不存在")
	}
}
```

**map遍历**
* 遍历也是使用range,与数组遍历一样

```go
func TestTravelMap(t *testing.T) {
	m1 := map[string]string{"name": "小张", "password": "123456"}

	for k, v := range m1 {
		t.Logf("key is %s,value is %s", k, v)
	}

}
```
