## Java基础部分
## 目录  

[string]: <https://github.com/luocx/java-notebook/blob/main/docs/java-basis.md#stringstringbuilderstringbuffer>
[hashmap]: <https://github.com/luocx/java-notebook/blob/main/docs/java-basis.md#map接口的实现hashmaptreemap>  
[collection]: <https://github.com/luocx/java-notebook/blob/main/docs/java-basis.md#collection接口的实现-arraylistlinkedlist>  
[thread]: <https://github.com/luocx/java-notebook/blob/main/docs/java-basis.md#>  
[theadPool]: <https://github.com/luocx/java-notebook/blob/main/docs/java-basis.md#>  
[io]: <https://github.com/luocx/java-notebook/blob/main/docs/java-basis.md#>  
[aqs]: <https://github.com/luocx/java-notebook/blob/main/docs/java-basis.md#>  
[reflection]: <https://github.com/luocx/java-notebook/blob/main/docs/java-basis.md#>  

1. [String、StringBuilder、StringBuffer][string]
2. [Map接口的实现：HashMap、TreeMap][hashmap]
3. [Collection接口的实现: ArrayList、LinkedList][collection]
4. [进程和线程的区别及线程的生命周期][thread] 
5. [Java里的线程池][theadPool]
5. [输入输出流，IO与NIO][io]
6. [AbstractQueuedSynchroizer原理及常用的锁][aqs]
7. [反射机制(reflect)][reflection]


## String、StringBuilder、StringBuffer
1. `String`实现了CharSequence、Serializable、Comparable接口，可以被序列化，内置了比较方法用于排序。  
比较规则：从左至右遍历两个字符串的char，按字符编码中的顺序作为比较结果，即两个字符char做减法运算，当一个字符串完全匹配时返回两个字符串的长度差。  
由于使用final修饰了存放字符的数组`final char[] value`，所以它是**不可变的**字符串对象。

2. `StringBuilder`实现了CharSequence、Serializable接口，表明它是一个可读的字符序列以及可被序列化。内部用char数组存放字符，并提供了操作字符的方法。默认容量16，容量不足以存储新字符串时扩容。

3. `StringBuffer`用法基本与`StringBuilder`一致，不过它是线程安全的，因为其对做读写操作的方法上加了synchronized关键字修饰。

> Note: 因为String的不可变，每次对其做+运算时等同于new String() ，如果在for循环中做大字符串的拼接操作会产生大量的垃圾对象，加重GC工作负担，此时可使用StringBuilder或StringBuffer。

 
## Map接口的实现：HashMap、TreeMap
  
| 对比项 | HashMap | TreeMap |
| ----------- | ----------- |----------- |
| 数据结构 | 链表 + 数组 |二叉树 |
| 查找时间复杂度 | ≈Θ(1) |Θ(log n) |
| 有序性 | 无序 |有序 |
| 多线程操作 | 不安全 |不安全 |
  
### HashMap
HashMap是单链表数组或红黑树数组的存储结构，数据结构会根据hash冲突的情况改变，默认capacity容量是16，loadFactor负载因数是0.75。当数组大小超过临界值threshold时，将会触发扩容机制`resize()`，新容量为原来的2倍，临界值 = 容量 * 负载因数。  

HashMap.Node类实现了Map.Entry接口，有属性hash、key、value、next分别用于存储哈希值、键、值和下一节点。    
写入key-value时，**key可以是null**，如果不存在相同的key，则会创建一个Node对象，写入到Node[]数组的某一位置。位置是由key的hash值与数组容量求模运算得到数组的下标，如果数组位置为空，直接写入新Node节点；由于hash值会存在冲突，导致不同key计算的数组下标相同，此时会将新节点追加到该链的尾部，如果链表长度超过6个会转换为红黑树。  


>思考：链表长度超过6时为什么要用红黑树？<br>
见解：链表查找的时间复杂度为Θ(n)，红黑树为Θ(㏒ n)，由于红黑树需要自旋以保持平衡，在节点数量超过6时红黑树的综合性能要优于单链表。

>思考：为什么要用HashMap，什么情况下用HashMap？<br>
见解：HashMap作为键值对形式的存储结构，它根据key查找value的时间复杂度近似于Θ(1)，但是遍历时数据是无序的且非线程安全，适用于单线程环境且对排序无要求存储和查找键值对数据的情况。

### TreeMap
TreeMap 是红黑树存储结构，写入map的key需要实现`java.util.Comparator`接口或者通过构造方法传入key对象类型的比较器。  
根据key的比较结果确定数据存放的节点位置，**key不允许为null**，结果最小的在最左边的叶子节点，最大的在最右边的叶子节点。查找时从根节点开始遍历，依次比较节点的key, 直至找到相等的key或遍历完为止。  
  
>思考: TreeMap的使用场景是什么？<br>
>见解: TreeMap作为自平衡的二叉查找树，增删改查最好和最坏情况下时间复杂度都是θ(log n)，性能相对稳定，对于需按自定义比较规则排序、有序查找、有序遍历的情况可以使用TreeMap。
  
  
## Collection接口的实现: ArrayList、LinkedList
| 对比项 | ArrayList | LinkedList |
| ----------- | ----------- |----------- |
| 数据结构 | 数组 | 双链表 |
| 查找时间复杂度 | Θ(n) |Θ(n) |
| 适合场景 | 读多写少，数据量确定 | 写多，不随机访问，数据量不确定 |
| 多线程操作 | 不安全 |不安全 |

### ArrayList
`ArrayList`是通过数组实现能自动扩容的有序列表，根据写入的先后顺序排序，默认capacity为10， 当容量不够存储当前新增的元素时，将新建一个容量为`oldCapacity + (oldCapacity >> 1)`且最少不低于最小需要容量、最大不超过`Integer.MAX_VALUE - 8`的新数组，通过数组复制将数据复制到新的数组中完成扩容。  

插入的时间复杂度为Θ(1)（不考虑扩容时发生数组复制的情况），根据下标查找的时间复杂度为Θ(1)，根据元素值查找为Θ(n)，删除时，被删除元素之后的元素都将向前移位，因此时间复杂度为Θ(n)。 

>由于ArrayList默认初始容量较小，在数据个数明确的前提下，最好通过构造方法指定容量，避免发生过多的数组复制。 

### LinkedList
`LinkedList`是双链表结构，除了首尾节点，其余的每个节点都有指向其上一个节点和下一个节点，所有的节点组成双链，因此被称作双链表。  
  
`ListedList` 实现了`Deque`接口，通过利用`offer(E e)`、`poll()`方法可实现FIFO队列。`offer(E e)` + `pollLast()`实现LIFO队列。  
   
![双链表](https://github.com/luocx/java-notebook/blob/main/docs/image/linkedlist.png)  


## 进程和线程的区别及线程的生命周期
进程(Process)对于操作系统来说就是一个任务的执行过程，线程(Thread)是进程的子任务，一个进程可以有多个线程，但至少要有一个线程。一个应用程序可以理解为一个进程（不一定只有一个), 进程执行时可以开启由多个线程同一时间处理不同的任务。  

**创建线程的方式有**:   
- 实现Runnable接口，new 一个Thread对象通过构造方法传入实现了Runnable接口的对象，调用start()方法
- new 一个Thread对象的子类，子类中重写run方法，调用start()方法。
- 使用线程池提交任务开启线程  
  
`Thread` 类本身实现了Runnable接口的run方法，但是实际调用的是构造方法传入的Runnable接口实现类，类似于设计模式中的`装饰者模式(Decorator pattern)`

### 线程的6种状态及含义
**NEW**:  线程对象已创建，还未调用start方法，该状态只会在start()调用前出现  
**RUNNABLE**: 线程在Java虚拟机中已是执行状态 (对于操作系统来说可能处于等待阶段，比如等待处理器资源)  
**BLOCKED**: 线程阻塞，等待其它线程释放锁，如被synchronized修饰的对象同一时间只能由一个线程获取，其它访问的线程处于blocked状态  
**WAITING**: 线程等待状态，等待被其他线程唤醒，如调用Object.wait()时则会进入waiting状态，需要另外的线程调用Object.notify/notifyAll()唤醒  
**TIMED_WAITING**: 具有超时时间的等待状态  
**TERMINATED**: 线程终止，线程执行完毕  

> 要通过开启线程执行任务，必须调用start()方法，而不是run()，前者是通过native 方法启用线程执行run(), 后者是当前线程调用一个普通的方法。

## Java里的线程池


## 输入输出流，IO与NIO  
IO在计算机中指Input/Output，分别是输入和输出。Java中输入和输出被称为抽象的流，流可以看做一组有序的字节集合，即两个设备之间数据的传输。
  
**BIO**: 阻塞IO（Blocking IO）, 传统的读写流实现，。  
**NIO**: 非阻塞IO （Nonblocking IO）, J2SE1.4引入的技术特性。  
**AIO**: 异步非阻塞的 IO (Asynchronous I/O) 也就是 NIO 2，在 Java 7 中引入了 NIO 的改进版 NIO 2。  