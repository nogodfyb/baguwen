# 典型回答
## volatile和可见性

对于volatile变量，当对volatile变量进行写操作的时候，JVM会向处理器发送一条lock前缀的指令，将这个缓存中的变量回写到系统主存中。

所以，如果一个变量被volatile所修饰的话，在每次数据变化之后，其值都会被强制刷入主存。而其他处理器的缓存由于遵守了缓存一致性协议，也会把这个变量的值从主存加载到自己的缓存中。这就保证了一个volatile在并发编程中，其值在多个缓存中是可见的。

[✅什么是MESI缓存一致性协议](https://www.yuque.com/hollis666/fo22bm/gg2n5fqckk442ouf?view=doc_embed)

## volatile和有序性

volatile除了可以保证数据的可见性之外，还有一个强大的功能，那就是他可以禁止指令重排优化等。

普通的变量仅仅会保证在该方法的执行过程中所依赖的赋值结果的地方都能获得正确的结果，而不能保证变量的赋值操作的顺序与程序代码中的执行顺序一致。

**volatile是通过内存屏障来禁止指令重排的**，这就保证了代码的程序会严格按照代码的先后顺序执行。这就保证了有序性。被volatile修饰的变量的操作，会严格按照代码顺序执行，load->add->save 的执行顺序就是：load、add、save。

如经典的双重校验锁必须加volatile的问题，就是因为volatile加了内存屏障。

[✅有了synchronized为什么还需要volatile?](https://www.yuque.com/hollis666/fo22bm/nl3dfw?view=doc_embed&inner=wyvtu)

# 扩展知识
## 内存屏障

[✅到底啥是内存屏障？到底怎么加的？](https://www.yuque.com/hollis666/fo22bm/kozqs205honv8nso?view=doc_embed)
