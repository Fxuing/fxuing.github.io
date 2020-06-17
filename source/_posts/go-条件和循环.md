---
title: go-条件和循环
comments: true
categories: go
tags:
---

**循环**

* 只有一个 for 关键字

```go
// while循环
func TestWhileLoop(t *testing.T) {
	n:=0
	for n<5  {
		t.Log(n)
		n++
	}
}
```
**条件**

* if 条件

1. condition 表达式必须布尔值
2. 支持变量赋值：
```
if var declaration; condition{
    
}
```
例：

```go
func TestIf(t *testing.T) {
	if a:=1==1; a{
		t.Log("1==1")
	}
}
```

* switch条件

1. 条件表达式不限制为常量或者整数
2. 单个case中，可以出现多个结果选项，使用逗号分割
3. 于C ，java等规则相反，Go语言不需要使用break来明确退出一个case
4. 可以不设定switch之后的条件表达式，在这种情况下，整个switch结构与多个if...else...的逻辑作用等同

```go
func TestSwitchMultiCase(t *testing.T)  {
	for i := 0; i < 5; i++ {
		switch i {
		case 0, 2:
			t.Log("Even")
		case 1, 3:
			t.Log("Odd")
		default:
			t.Log("it is not 0-3")
		}
	}
}

func TestSwitchCaseCondition(t *testing.T) {
	for i := 0; i < 5; i++ {
		switch {
		case i%2 == 0:
			t.Log("Even")
		case i%2 == 1:
			t.Log("Odd")
		default:
			t.Log("unknown")

		}
	}
}
```