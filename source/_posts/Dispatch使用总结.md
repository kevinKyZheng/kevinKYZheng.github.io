1. 建立队列

```
DispatchQueue(label: "com.appcoda.queue2", qos: DispatchQoS.utility)
DispatchQueue.global()
DispatchQueue.main

```

2. 同步执行和异步执行

```
queue.async{

}
queue.sync{

}
///代码块
let workItem = DispatchWorkItem{

}
queue.async(execute: workItem)
workItem.notify(queue: DispatchQueue.main) {

}
```

3. 延迟执行

```
queue.asyncAfter(deadline: .now() + DispatchTimeInterval.seconds(2)) {

}
```