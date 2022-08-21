# GO笔试题

### go的调度模型
```
GPM
```

### go struct能不能比较
```
成员属性都是基本数据类型时候可以直接比较
如果有引用类型则不可以
reflect.DeepEqual深度比较
```

### go defer（for defer）特性
```
1.先进后出，后进先出
2.panic了也会执行
3.在return后面执行
4.可以更改函数声明里面的返回值比如 func add() (result int)
```

### select可以用于什么
```
监听channel
多个case随机
default有且case无匹配就会执行
case无匹配且default没有会阻塞
```

### context包的用途
```
链路追踪ID传递context.WithValue
超时信号传递contxt.WithTimeout\context.WithDeadline
手动取消context.WithCancle

https://zhuanlan.zhihu.com/p/445382956
```

### HTTP请求如何实现长连接复用TCP连接
```
http.Client{Transport: net.http.Transport}
```

### 主协程如何等其余协程完再操作
```
sync.WaitGroup(Add\Done\Wait)
channel阻塞
```

### slice扩容机制
```
1024以前2倍速\以后1.25倍速
```

### map如何顺序读取
```
获取所有map.key然后转slice后遍历访问
```

### 实现线程安全的map(Set,Get)
```
sync.Mutex.Lock/Unlock
sync.RWMutex.Lock/Unlock/RLock/RUnlock
```

### 实现消息队列（多生产者，多消费者）


### http跟head有什么区别
```
HEAD和GET差别在于不返回消息体
常用于测试检查资源是否有效
```

### http的keep-alive
```
复用TCP连接减少TCP连接次数
https://www.jianshu.com/p/142b35998947
```

### 解释golang的channel死锁条件，如何避免

### linux命令，查看端口占用，cpu负载，内存占用，如何发送信号给一个进程
```
ps -ef | grep xxx
netstat -nlp
```


### 项目里的微信支付这块，在支付完微信通知这里，收到两次微信相同的支付通知，怎么防止重复消费（类似接口的幂等性），说了借助Redis或者数据库的事务


### 对Go的反射包了解吗，主要用来做什么了
```
struct的tag获取
reflect.DeepEqual深度比较
```

### 退出程序时怎么防止channel没有消费完
```
context.WithCancle
```


40.写的循环队列是不是线程安全，不是，怎么保证线程安全，加锁，效率有点低啊，然后面试官就提醒Go推崇原子操作和channel
```
counter := int32(12)
old := int32(12)
ok := atomic.CompareAndSwapInt32(&counter, old, old+1)
if ok {
	// 更新成功
	log.Println(ok)
} else {
	// 更新失败
}
```


### sync.Pool用过吗，为什么使用，
```
对象池，避免频繁分配对象（GC有关）
```

### 唯一索引和主键索引的区别
```
唯一索引允许null主键不允许
```


### io模型，同步阻塞，同步非阻塞，异步

### redis排行榜数据结构（跳跃表），查询时间复杂度

### 使用过redis分布式吗，有的话如何减少同步延迟

### MySQL能实现redis的功能吗

### go使用踩过什么坑

### go线程自己独享什么

### Nginx的select.epoll了解吗

### 排行榜怎么实现

### go的锁如何实现，用了什么cpu指令

### 怎么实现协程完美退出
```
context.WithCancle
```

### 用channel实现定时器？（实际上是两个协程同步）

### go为什么高并发好？讲了go的调度模型

### 怎么理解go的interface

### 100亿个数选top5，小根堆

### redis容灾，备份，扩容

### 跳跃表，zset为什么使用跳跃表而不使用红黑树
```
数据变更后，实现更简单不用旋转节点
范围查找性能更优秀
```

### 输入url后涉及什么(http请求到web应用后台程序生命周期)

### IPC（Inter-Process Communication，进程间通信）进程之间通信的方式
```
管道
消息队列
共享内存
信号量
信号
socket
```

### 线程的栈在哪里分配

### 多个线程读，一个线程写一个int32会不会有问题，int64呢

### 判断二叉树是否为满二叉树

### LRU算法实现
```
最近最少使用算法，长期不用的数据，未来被用概率也不大，优先将这些数据替换掉
双向链表
每次访问都将数据位置提升至链表最右侧
每次删除是删除链表最左侧
（假定是定长阶）
```

### 一个大整数（字符串形式表示的），移动字符求比它大的数中最小的
```
遍历即可
```

### GO笔试题 - 程序员客栈

https://jishuin.proginn.com/p/763bfbd4cfd5

120.golang的内存逃逸分析 - 什么时候会有内存逃逸

121.golang内存泄漏场景

(time.Ticker定时器需要手动释、goroutine由于死锁导致、)

https://zhuanlan.zhihu.com/p/469817707

https://blog.csdn.net/m0_37290103/article/details/116493163

手动搞一个包变量Map类型不断追加不释放也是内存泄漏

### Go的GC原理

1. Mark and Sweep算法（标记清除算法） ，遍历存活的内存单元，被引用的对象会被标记为“被引用”，否则标记“不可达”状态，达到某一个阙值或时间间隔后，进行垃圾回收；

2. 三色标记法（上面改良版本） - 白色灰色黑色
内存达到阙值活着时间每隔2min做GC

https://www.jianshu.com/p/cfc669f83eaa

1. 定时调用2min至少一次
2. 手动调用
3. 内存增长达到阙值会触发

创建的对象全部标记为白色，可达对象标记为灰色，
灰色对象之中找到其引用的对象，将自身标记为黑色

然后重复的对灰色对象（可达）找到引用的对象，再将自身标记为黑色，循环往复，直至没有灰色对象

只有黑色（最终引用的对象）和白色对象

将白色对象清除


### Go的GMP模型详解
```
全局队列 [存放等待运行的G]
P的本地队列 [存放等待运行的G]


每个 M 都代表了 1 个内核线程
P的数量 runtime.GOMAXPROCS() 决定
runtime/debug.SetMaxThreads 设定 M 的数量 (默认最大1w)

P何时创建 : 程序启动时候
M何时创建 : M都阻塞了,P还有很多就绪任务时候,M可以新建

调度器的设计策略 : 复用线程M 避免线程被频繁地创建和销毁

work stealing 机制 : 当前M没有可运行的G的时候，可以去偷取其他线程的P的G
hand off 机制 : 当前M的G阻塞时候，可以将P释放转移给其他M执行

```

### Go的并发为什么效率这么高
```
线程复用
GMP调度模型(work stealing\hand off)
传统并发基于多线程,大小>2M,上下文切换 [内核态] 涉及IO,性能杀手
协程 上下文切换[用户态] 大小>2kb

https://zhuanlan.zhihu.com/p/502740833
```

### GMP之中P和M的对应关系是什么

### Goroutine的状态有哪些
```
Grunnable Grunning Gwaiting GDead Gsyscall
```


### Redis滑动窗口限频有什么弊端

### 令牌算法实现限制频率是什么

### 什么是抢占式调度

https://www.cnblogs.com/Leo_wl/p/10993731.html

```
抢占式调度（Preemptive Scheduling）是一种CPU调度技术,通过将CPU的时隙划分给给定的进程来工作

给定的时间间隔执行进程。当进程的区间时间（burst time）大于CPU周期时
它将被放回到就绪队列（ready queue）中
并在下一个时机（chance）执行。

当进程切换到就绪状态时，会使用这种调度方式


非抢占调度（Non-preemptive Scheduling）一种CPU调度技术
进程获取资源(CPU时间)并持有它
直到进程终止或推送到等待状态。进程不会被中断
直到它完成，然后处理器切换到另一个进程
```
```
所以抢占式就是CPU有中途打断交给其他线程动作
```

### 什么是IO多路复用


参考文章

[深度解析go](https://tiancaiamao.gitbooks.io/go-internals/content/zh/05.2.html)
[深入学习Go的GMP](https://zhuanlan.zhihu.com/p/502740833)

