---
title: go-并发
date: 2020年6月16日
comments: true
categories: go
tags:
---

**协程机制**

> 定义方式 使用 go 关键字

```go
func TestMut(){
    //定义协程
    go func(){
     // code 
    }()
}
```
<!--more-->

**互斥锁**

> 使用 sync.Mutex 包

* Unlock() 释放锁，建议在defer中使用，防止异常未释放锁
* Lock() 加锁，操作之前加锁

**线程等待**

> 使用 sync.WaitGroup，类似 java 的 CountDownLatch

* 启用协程 使用 add(1)
* 协程结束后使用 Done()
* 主线程使用 Wait()

```go
func TestG(t *testing.T) {
	var waitGroup sync.WaitGroup
	var mut sync.Mutex
	var counter int
	for i := 0; i < 5000; i++ {
		waitGroup.Add(1)
		go func(i int) {
			defer func() {
				mut.Unlock()
			}()
			mut.Lock()
			counter++
			waitGroup.Done()
		}(i)
	}
	waitGroup.Wait()
	t.Log(counter)
}
```

### CSP并发机制

**channel**
> 两种机制，1. make(chan string), 2. make(chan string, 5)

* 机制1：通讯双方都必须存在，如果一方不存在，则另一方会阻塞。
* 机制2：BufferChannel，发送和接收双方松耦合，容量未满的情况下，不会阻塞

异步返回信息:

```go
func service() string {
	//time.Sleep(time.Millisecond * 50)
	return "执行业务操作中"
}

func AsyncService() chan string {
	retCh := make(chan string, 5)
	go func() {
		ret := service()
		retCh <- ret
	}()
	return retCh
}
func ohterTask() {
	fmt.Println("执行其他任务开始。")
	//time.Sleep(time.Millisecond * 100)
	fmt.Println("执行其他任务结束。")
}
func TestAsynService(t *testing.T) {
	retCh := AsyncService()
	ohterTask()
	fmt.Println(<-retCh)
}
```

**select 多路复用**

> 与 switch 结构类似，当有channel 未阻塞时，执行case 所定义的部分，与case 定义顺序无关；可用 <- time.After 实现超时，当After未达到时间的时候，After会阻塞，当达到时间的时候，After不会阻塞。

1. 结构定义示例：
```go
select{
    case ret:= <- xxx():
        // code
    case ret:= <- xxx():
        // code 
    case <- time.After(time.Second *3)
        // time out code
    default:
        // default code 
}
```

2. 示例
```go
func timeout() string {
	time.Sleep(time.Second * 3)
	return "userInfo======="
}
func userInfo() chan string {
	userCh := make(chan string)
	go func() {
		ret := timeout()
		userCh <- ret
	}()
	return userCh
}
func addressInfo() chan string {
	addCh := make(chan string)
	go func() {
		ret := func() string {
			return "address======="
		}
		addCh <- ret()
	}()
	return addCh
}
func TestSel(t *testing.T) {
	select {
	case ret := <-userInfo():
		t.Log(ret)
	//case ret := <- addressInfo():
	//	t.Log(ret)
	case <-time.After(time.Second):
		t.Error("time out")
	}
}
```

**channel 的广播和关闭**

> 定义两个 协程，一个往channel放数据，一个往channel取数据，channel放完后 close(channel)，获取的一方 判断时候正常获取。

```go
func provider(ch chan int, wg *sync.WaitGroup) {
	go func() {
		for i := 0; i < 200; i++ {
			ch <- i
		}
		close(ch)
		wg.Done()
	}()
}
func receiver(ch chan int, wg *sync.WaitGroup) {
	go func() {
		for {
			if data,ok := <- ch; ok{
				fmt.Print(data," ")
			}else{
				break
			}
		}
		wg.Done()
	}()
}
func TestChClose(t *testing.T) {
	var wg sync.WaitGroup
	ch := make(chan int)
	wg.Add(1)
	provider(ch,&wg)
	wg.Add(1)
	receiver(ch,&wg)
	wg.Add(1)
	receiver(ch,&wg)
	wg.Wait()
}
```

**任务取消**

* **context**
// TODO
