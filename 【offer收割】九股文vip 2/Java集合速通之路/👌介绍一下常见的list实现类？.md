List 是平常使用非常多的一个集合类型，是一个有序的 Collection。<br />常见的有以下几种：
# ArrayList
ArrayList 是最常用的 List 实现类，线程不安全，内部是通过数组实现的，继承了AbstractList，实现了List。它允许对元素进行快速随机访问。数组的缺点是每个元素之间不能有间隔，当数组大小不满足时需要增加存储能力，就要将已经有数组的数据复制到新的存储空间中。当从 ArrayList 的中间位置插入或者删除元素时，需要对数组进行复制、移动、代价比较高。因此，它适合随机查找和遍历，不适合插入和删除。排列有序，可重复，容量不够的时候，新容量的计算公式为：<br />newCapacity = oldCapacity + (oldCapacity >> 1)，这实际上是将原容量增加50%（即乘以1.5）。<br />ArrayList实现了RandomAccess接口，即提供了随机访问功能。RandomAccess是java中用来被List实现，为List提供快速访问功能的。在ArrayList中，我们即可以通过元素的序号快速获取元素对象，这就是快速随机访问。<br />ArrayList实现java.io.Serializable接口，这意味着ArrayList支持序列化，能通过序列化去传输。
# LinkedList（链表）
LinkedList 是用链表结构存储数据的，**线程不安全。**很适合数据的动态插入和删除，随机访问和遍历速度比较慢，增删快。另外，他还提供了 List 接口中没有定义的方法，专门用于操作表头和表尾元素，可以当作堆栈、队列和双向队列使用。底层使用双向链表数据结构。<br />LinkedList是一个继承于AbstractSequentialList的双向链表。**它也可以被当作堆栈、队列或双端队列进行操作。**
# Vector（数组实现、线程同步）
Vector 与 ArrayList 一样，也是通过数组实现的，Vector和ArrayList用法上几乎相同，但Vector比较古老，一般不用。Vector是线程同步的，效率低。即某一时刻只有一个线程能够写 Vector，避免多线程同时写而引起的不一致性，但实现同步需要很高的花费，因此，访问它比访问 ArrayList 慢。默认扩展一倍容量。
