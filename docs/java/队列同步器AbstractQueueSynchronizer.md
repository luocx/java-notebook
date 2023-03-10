# 队列同步器 AbstractQueueSynchronizer
## 核心方法
* `acquire`
* `compareAndSetState` 通过Unsafe类对state状态做CAS操作，在ReentreLock中用于获取锁
* `addWaiter` 通过Unsafe类对tail尾部节点做CAS操作，将当前线程加入等待队列尾部
* `acquireQueued`
* `release`

## 子类实现方法
* `tryAcquire`
* `tryRelease`