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

### ipc方式，共享存储区原理

### 线程的栈在哪里分配

### 多个线程读，一个线程写一个int32会不会有问题，int64呢（这里面试官后来说了要看数据总线的位数，32位的话写int32没问题，int64就有问题）

### 判断二叉树是否为满二叉树

### lru实现

119.一个大整数（字符串形式表示的），移动字符求比它大的数中最小的

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








