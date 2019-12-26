---
title: golang/Mutex
date: 2019-12-26 15:56:47
tags: golang
---

# Mutex

* Mutex 是互斥锁。
* 0值就是 unlocked 状态的 Mutex
* Mutex 在第一次使用之后不能被复制

## func(* Mutex) Lock
```golang
    func (m *Mutex) Lock()
```

## func(* Mutex) Unlock
```golang
    func (m *Mutex) Unlock()
```

解锁已经解锁的Mutex, 运行时候报错

允许一个线程去锁,然后另外一个线程去解锁它

```golang
    package main

import (
    "sync"
    "time"
)

// Mutex lock 可以被别的线程 unlock --------
func main() {
    var mu sync.Mutex
    go func() {
        mu.Lock()
        time.Sleep(10 * time.Second)
        mu.Unlock()
    }()
    time.Sleep(time.Second)
    mu.Unlock()
    select {}
}
```