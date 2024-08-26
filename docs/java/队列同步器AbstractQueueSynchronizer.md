# 队列同步器 AbstractQueueSynchronizer
## 核心方法
* `acquire`
* `compareAndSetState` 通过Unsafe类对state状态做CAS操作，在ReentreLock中用于获取锁
* `addWaiter` 通过Unsafe类对tail尾部节点做CAS操作，将当前线程加入等待队列尾部
* `acquireQueued` 询问当前节点上一个 node 是否为队列头部节点，否则park当前线程；是则开启自旋，成功则将当前node设置为头节点并结束自旋
* `release` 

## 子类实现方法
* `tryAcquire`
* `tryRelease`


## 原理
多个线程争夺资源时，将按照执行先后顺序将多个线程维护成一个双向链表，如果当前线程不是 head 节点，则将其 `park` 休眠等待，直至被其他线程唤醒，锁的持有者在 `release` 释放锁之后会唤醒队列中后续的线程节点来争夺锁资源。

## ReentrantLock
可重入锁，同一个持有锁的线程重复申请锁时是允许的，在state执行+1操作，释放时执行-1
* 非公平锁
	- 每次申请锁时直接尝试修改state，成功则不加入队列，直接获取锁，失败则加入队列尾部，这样一定程度能减少线程唤醒的开销，但同时会导致队列线程长时间无法处理任务
* 公平锁
	- 每次申请锁时先加入队列

## ReentrantReadWriteLock
可重入读写锁