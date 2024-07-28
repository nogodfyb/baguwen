# 典型回答

[✅什么是ThreadLocal，如何实现的？](https://www.yuque.com/hollis666/fo22bm/ihoye3?view=doc_embed)

ThreadLocal 提供了一种线程级别的数据存储机制。每个线程都拥有自己独立的 ThreadLocal 副本，这意味着每个线程都可以独立地、安全地操作这些变量，而不会影响其他线程。

ThreadLocal其实在工作中应该是非常常见的，以下是一些比较典型的使用场景：

**用户身份信息存储**： 在很多应用中，都需要做登录鉴权，一旦鉴权通过之后，就可以把用户信息存储在ThreadLocal中，这样在后续的所有流程中，需要获取用户信息的，直接取ThreadLocal中获取就行了。非常的方便。

**线程安全**：ThreadLocal可以用来定义一些需要并发安全处理的成员变量，比如SimpleDateFormat，由于 SimpleDateFormat 不是线程安全的，可以使用 ThreadLocal 为每个线程创建一个独立的 SimpleDateFormat 实例，从而避免线程安全问题。

**日志上下文存储**：在Log4j等日志框架中，经常使用ThreadLocal来存储与当前线程相关的日志上下文。这允许开发者在日志消息中包含特定于线程的信息，如用户ID或事务ID，这对于调试和监控是非常有用的。

**traceId存储：**和上面存储日志上下文类似，在分布式链路追中中，需要存储本次请求的traceId，通常也都是基于ThreadLocal存储的。

**数据库Session：**很多ORM框架，如Hibernate、Mybatis，都是使用ThreadLocal来存储和管理数据库会话的。这样可以确保每个线程都有自己的会话实例，避免了在多线程环境中出现的线程安全问题。

**PageHelper分页**：PageHelper是MyBatis中提供的分页插件，主要是用来做物理分页的。我们在代码中设置的分页参数信息，页码和页大小等信息都会存储在ThreadLocal中，方便在执行分页时读取这些数据。


其实，看了这么多，主要就是俩作用：

**1、解决并发问题**，这个不需要多说了。<br />**2、在线程中传递数据**，在同一个线程执行过程中，ThreadLocal的数据一直在，所以我们可以在前面把数据放到ThreadLocal中，然后再后面的时候再取出来用，就可以避免要把这些数据一直通过参数传递。
