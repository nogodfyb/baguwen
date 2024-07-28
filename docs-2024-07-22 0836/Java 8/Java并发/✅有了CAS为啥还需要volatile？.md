# 典型回答

在AQS的实现中，维护了一个volatile的int类型的state变量。state变量的值修改的动作通过CAS来完成的。

CAS大家都知道，底层其实是在总线上面加了锁的，那为啥这里还需要这个volatile呢？没有回有什么问题吗？

[✅CAS在操作系统层面是如何保证原子性的？](https://www.yuque.com/hollis666/fo22bm/ed72dt8guaf4fvn8?view=doc_embed)

> 这个问题有的时候也这么问：解决并发问题，用CAS就够了么？


其实，CAS只能保证原子性，即一个修改命令，以原子性的操作完成，中间不会被中断。但是并发编程中，我们要解决的问题还包括有序性和可见性。

[✅什么是Java内存模型？](https://www.yuque.com/hollis666/fo22bm/hmi3m1?view=doc_embed&inner=dIOas)

因为，**CAS虽然提供了一种无锁的方式来原子地更新值，但它本身并不能保证变量修改的可见性**。也就是说，一个线程可能已经通过CAS成功地改变了一个变量的值，但这个新值可能还没有被其他线程所看到，因为这个值可能仅仅存储在当前线程的本地内存中。

> 这里可能有人还有疑惑，在[https://www.yuque.com/hollis666/fo22bm/ed72dt8guaf4fvn8](https://www.yuque.com/hollis666/fo22bm/ed72dt8guaf4fvn8) 中明明说"cmpxchg是基于 CPU 缓存一致性协议实现的，在多核 CPU 中，所有核心的缓存都是一致的。"这里咋又不保证可见性了呢？  主要是因为这里把多核CPU的缓存之间的一致性和Java的多线程本地内存之间的一致性搞混了。即没有正确的理解MESI和JMM的作用。可以看下：[https://www.yuque.com/hollis666/fo22bm/yx29gk7wsw26ec4r](https://www.yuque.com/hollis666/fo22bm/yx29gk7wsw26ec4r)


而想要实现一个变量的可见性，就需要使用 volatile了。通过将AQS的状态变量声明为 volatile，我们可以确保这个变量的读写都是直接对主内存，这保证了一个线程对这个变量的修改对其他线程立即可见。这对于避免同步问题和内存一致性错误是至关重要的。

volatile除了可以保证数据的可见性之外，还有一个强大的功能，那就是他可以禁止指令重排优化，这就保证了代码的程序会严格按照代码的先后顺序执行。

[✅volatile是如何保证可见性和有序性的？](https://www.yuque.com/hollis666/fo22bm/iscdbslmh7h7qp6p?view=doc_embed)

所以，CAS和 volatile 在AQS中是互补的：**CAS提供原子性操作以避免锁的使用，而 volatile 确保修改的可见性和内存操作的有序性。**两者结合，使得AQS能够以一种高效且线程安全的方式管理同步状态。
