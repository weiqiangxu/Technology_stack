# Redis面试题

### Redis为什么这么快
```
单机的 redis 就可以支撑每秒 10 几万的并发

纯内存KV操作
单线程操作[无多线程上下文切换开销\没有锁的问题]
非阻塞的 IO 多路复用机制

高效数据结构
```

### 对Redis的使用熟悉吗
