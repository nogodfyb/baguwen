在Java中，Set是一种不包含重复元素的集合，它继承自Collection接口。用于存储无序(存入和取出的顺序不一定相同)元素，值不能重复。对象的相等性本质是对象 hashCode 值（java 是依据对象的内存地址计算出的此序号）判断的，如果想要让两个不同的对象视为相等的，就必须覆盖 Object 的 hashCode 方法和 equals 方法。Java中常见的Set实现类主要有三个：HashSet、LinkedHashSet和TreeSet。
# HashSet
HashSet是Set接口的一个实现类，它基于哈希表实现，具有快速的插入、删除和查找操作。<br />HashSet不保证元素的迭代顺序，允许null元素的存在，但只能有一个null元素，不是线程安全的，如果多个线程同时访问并修改HashSet，则需要外部同步。<br />当存储自定义对象时，需要重写对象的`hashCode()`和`equals()`方法，以确保对象的唯一性。
# LinkedHashSet
LinkedHashSet是HashSet的子类，它基于哈希表和双向链表实现，继承与 HashSet、又基于 LinkedHashMap 来实现的。可以维护元素的插入顺序。相比于 HashSet 增加了顺序性。<br />其主要是将元素按照插入的顺序进行迭代，同时继承了HashSet的所有特性。因此当需要保持元素插入顺序时，可以选择使用LinkedHashSet。
# TreeSet
TreeSet是Set接口的另一个实现类，它基于红黑树实现，可以对元素进行自然排序或自定义排序。<br />可以实现元素按照升序进行排序（自然排序或自定义排序）。不过它不允许null元素的存在。treeset 同样是线程不安全的。<br />当存储自定义对象时，如果想要进行排序，需要实现`Comparable`接口并重写`compareTo()`方法，或提供自定义的`Comparator`对象来进行排序。
# 适用场景

- `HashSet`：适用于需要快速查找的场景，不保证元素的顺序。
- `LinkedHashSet`：适用于需要保持元素插入顺序的场景。
- `TreeSet`：适用于需要元素排序的场景。
