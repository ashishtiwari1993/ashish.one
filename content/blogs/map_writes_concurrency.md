---
title: "[SOLVED] Golang fatal error: concurrent map writes"
date: 2020-02-04T01:14:03+05:30
type: "post"
ogtype: article
tags: ["golang","map","fatal","error","concurrent map writes","concurrent"]
draft: false
slug: "fatal-error-concurrent-map-writes"
topics: "Golang"
---

![concurrent_map_writes](/img/golang/concurrent_map_writes.jpg)


## The Problem: 

Suddenly got below errors which killed my daemon:

```sh
fatal error: concurrent map writes

goroutine 646 [running]:
runtime.throw(0x75fd38, 0x15)
        /usr/local/go/src/runtime/panic.go:774 +0x72 fp=0xc000315e60 sp=0xc000315e30 pc=0x42ecf2
runtime.mapdelete_fast64(0x6f0800, 0xc00008ad50, 0x2b3e)

goroutine 1 [sleep]:
runtime.goparkunlock(...)
        /usr/local/go/src/runtime/proc.go:310
time.Sleep(0x12a05f200)
        /usr/local/go/src/runtime/time.go:105 +0x157
webhook/worker.Manager()

goroutine 6 [IO wait]:
internal/poll.runtime_pollWait(0x7fc308de6f08, 0x72, 0x0)
        /usr/local/go/src/runtime/netpoll.go:184 +0x55
internal/poll.(*pollDesc).wait(0xc000110018, 0x72, 0x0, 0x0, 0x75b00b)
        /usr/local/go/src/internal/poll/fd_poll_runtime.go:87 +0x45
internal/poll.(*pollDesc).waitRead(...)
        /usr/local/go/src/internal/poll/fd_poll_runtime.go:92
internal/poll.(*FD).Accept(0xc000110000, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0)
        /usr/local/go/src/internal/poll/fd_unix.go:384 +0x1f8
net.(*netFD).accept(0xc000110000, 0xc000050d50, 0xc000046700, 0x7fc308e426d0)
        /usr/local/go/src/net/fd_unix.go:238 +0x42
net.(*TCPListener).accept(0xc000126000, 0xc000050d80, 0x40dd08, 0x30)
        /usr/local/go/src/net/tcpsock_posix.go:139 +0x32
net.(*TCPListener).Accept(0xc000126000, 0x72f560, 0xc0000f0180, 0x6f4f20, 0x9c00c0)
        /usr/local/go/src/net/tcpsock.go:261 +0x47
net/http.(*Server).Serve(0xc0000f4000, 0x7ccbe0, 0xc000126000, 0x0, 0x0)
        /usr/local/go/src/net/http/server.go:2896 +0x286
net/http.(*Server).ListenAndServe(0xc0000f4000, 0xc0000f4000, 0x8)
        /usr/local/go/src/net/http/server.go:2825 +0xb7
net/http.ListenAndServe(...)
        /usr/local/go/src/net/http/server.go:3080
webhook/handler.HandleRequest()

```

### Expected behaviour

In starting for a few seconds it was working smoothly. 

![goroutine_race_condition](/img/golang/Go-Routines_race_condition.png)

### Actual behaviour

After few seconds my service got kill with above mentioned error.

![goroutine_race_condition_error](/img/golang/Go-Routines_race_condition_error.png)

## Code Overview:

Initialized one global variable with the type 'map'. Where the key is `int` and value is `channel`.

```sh
var ActiveInstances = make(map[int](chan string))
```

Having two functions

1. `go SetValue()`
2. `go DeleteValue()`

### In `SetValue()`

```sh
ActiveInstances[id] = make(chan string, 5)
```  

### In `DeleteValue()`

```sh
delete(ActiveInstances, id)
```

Both functions running in multiple goroutines.

## Observation:

The error itself says `concurrent map writes`, By which I got the idea that something is wrong with my map variable `ActiveInstances`. Both functions in multiple goroutines are trying to access the same variable(`ActiveInstances`) at the same time. Which created race condition. After exploring a few blogs & documentation, I become to know that **_"Maps are not safe for concurrent use"_**. 

As per golang doc   

> Map access is unsafe only when updates are occurring. As long as all goroutines are only reading—looking up elements in the map, including iterating through it using a for range loop—and not changing the map by assigning to elements or doing deletions, it is safe for them to access the map concurrently without synchronization.

## Solution:

Here we need to access `ActiveInstances` synchronously. We want to make sure only one goroutine can access a variable at a time to avoid conflicts, This can be easily achieved by `sync.Mutex`. This concept is called mutual exclusion which provides methods `Lock` and `Unlock`. 

We can define a block of code to be executed in mutual exclusion by surrounding it with a call to `Lock` and `Unlock`
It is as simple as below:

```sh
var mutex = &sync.Mutex{}

mutex.Lock()
//my block of code
mutex.Unlock()
```

## Code Modifications:

```sh
var mutex = &sync.Mutex{}

mutex.Lock()
ActiveInstances[i_id] = make(chan string, 5)
mutex.Unlock()
```

```sh
mutex.Lock()
delete(ActiveInstances, id)
mutex.Unlock()
```

This is how we successfully fix this problem. 

## Code to Reproduce

To reproduce, Comment Mutex related all operation like line no. 12, 30, 32, 44, 46. Mutex is use to prevent race condition which generates this error.

{{< gist ashishtiwari1993 d494b71ac264184ba46ced1bf2114c30 >}}

## References:

https://blog.golang.org/go-maps-in-action  
https://golang.org/doc/faq#atomic_maps  
https://gobyexample.com/mutexes  
