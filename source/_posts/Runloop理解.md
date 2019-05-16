RunLoop类似一个do-while循环,但是需要处理消息事件,没有消息时休眠,有消息时唤醒.

1. 用来处理子线程
子线程在创建完,run一次之后立即被销毁,因此需要将子线程加入到RunLoop中,并且RunLoop的运行条件是有需要处理的项目.

RunLoop能处理的源有,输入源和时间源.
输入源包括 NSPort/performSelector/自定义源

CFRunLoopModeRef

每次启动 RunLoop 时，只能指定其中一个 Mode，这个就是 CurrentMode。要切换 Mode，只能退出 Loop，再重新指定一个 Mode 进入。
1. scrollview和timer的区分


RunLoop 有5个类
CFRunLoopRef
CFRunLoopModeRef

CFRunLoopObserverRef
CFRunLoopSourceRef
CFRunLoopObserverRef

1. CFRunLoopModeRef
RunLoop管理Mode,mode管理各个事件,mode中包含source0,source1,tiemr,observer和port.

NSDefaultRunLoopMode：App的默认Mode，通常主线程是在这个Mode下运行；
NSRunLoopCommonModes

暴露出来的就只有前两个
NSConnectionReplyMode
NSModalPanelRunLoopMode
NSEventTrackingRunLoopMode



CFRunLoopObserverRef
定时器

CFRunLoopSourceRef
Source0,处理App内部时间,App自己负责管理
Source1,由RUnLoop和内核管理
应用 1. 常驻线程  2. 崩溃弹窗

CFRunLoopObserverRef
观察者,状态发生变化时,通过回调接受到变化.
1. 利用RunLoop空闲时间储存数据
2. 检测UI卡顿


7.相关面试题
1.谈谈runloop的理解；
2.runloop有哪些状态；
3.RunLoop的作用是什么？它的内部工作机制了解么？（最好结合线程来说） 
4.TableView/ScrollView/CollectionView滚动时为什么NSTimer会停止？
5.RunLoop和线程有什么关系？

