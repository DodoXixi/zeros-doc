# ThreadLocal的缺陷

## 这篇文章讲解ThreadLocal中的一些知识和坑，占用大家5分钟时间


- 依托于线程的生命周期而存在，贯穿于整个线程，解决了线程前后值传递的问题。
特点：
- 一次存入，只要线程不结束都可以获取到
- 不具有多线程之间共用数值的特性，只存在于单个线程内，主子线程之间不会出现值传递。(除非进行特殊的代码操作)，但是多线程对象却共同存在于
ThreadLocalMap的Entry中，这也是多线程处理并发的一种能力
- ThreadLocal被ThreadLocalMap中的entry的key弱引用，如果出现GC的情况时，没有被其他对象引用，
会被回收，但是ThreadLocal对应的value却不会回收，容易造成内存泄漏，这也间接导致了内存溢出以及数据假丢失。

在前面的总结中我为啥说数据会假丢失呢，大家可以看如下代码：

Entry中的key在GC的时候会被回收，但是对应的Value却还存在，这样就会造成key(null)的情况，
对应的value也会取不到。这就是内存泄漏的原因。

同时也会造成数据丢失。。如下图中的代码：

留坑必须要填：既然发现问题，就要解决问题
如果我们要使用ThreadLocal的作为线程前后的数据传输，又不想在遇到GC的时候数据被丢失，可以如下操作：




