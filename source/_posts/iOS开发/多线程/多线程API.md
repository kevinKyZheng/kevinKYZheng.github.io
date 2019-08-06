---
title: 多线程常用API
date: 2019-06-01 09:22:05
tags: 多线程 面试
categories: iOS
---

## Thread

* 休眠当前线程

    ```swift
    open class func sleep(until date: Date)
    open class func sleep(forTimeInterval ti: TimeInterval)
    ```

* 获取当前线程和主线程

    ```swift
    Thread.main
    Thread.current
    ```

* 取消线程(cancel只是标记,并不会真正取消,线程会继续会执行完,exit会真正关闭线程)

    ```swift
    open class func exit()
    open func cancel()
    ```

## GCD

### 队列

* 串行队列
  + Swift

    ```swift
    //
    DispatchQueue.main//主队列
    DispatchQueue.init(label: "")//自定义队列
    ```

  + OC

    ```objc
    //OC
    dispatch_get_main_queue()//主队列
    dispatch_queue_create("queue_name", DISPATCH_QUEUE_SERIAL)
    //自定义队列
    ```

* 并行队列

  + Swift

    ```swift
    DispatchQueue.global()//全局队列
    DispatchQueue.init(label: "", attributes: .concurrent)//自定义队列
    ```

  + OC

    ```objc
    //OC
    dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0)//全局队列
    dispatch_queue_create("ss_queue", DISPATCH_QUEUE_CONCURRENT)
    //自定义队列
    ```

### 队列组`DispacthGroup`

* 普通加入方式

    ```swift
    let group = DispatchGroup()
    DispatchQueue.global().async(group: group) {

    }
    group.notify(queue: DispatchQueue.main) {
        //此方法中的queue是通知回调的队列
    }
    ```

* 系统api加入group的方式(因为获取不到系统api执行时的队列)

    ```swift
    let session = URLSession()
    group.enter()
    let task = session.dataTask(with: URL.init(string: "")!) { (data, response, error) in
        group.leave()
    }
    task.resume()
    ```

### 信号量

```swift
let sem = DispatchSemaphore(value: 0)
sem.wait()
sem.signal()
```

### 栅栏函数

```swift
let task = DispatchWorkItem(flags: .barrier) {
    // do something
}
queue.async(execute: tas)
```

### 延迟函数

```swift
let t = DispatchTime.now() + time

DispatchQueue.main.asyncAfter(deadline: t, execute: block)
```

## NSOperation

```swift
OperationQueue.current
OperationQueue.main

//创建队列
let queue = OperationQueue()
//创建operation
let o = BlockOperation(){

}
//添加依赖
o.addDependency(b)
//操作加入队列
queue.addOperation(b)
```
