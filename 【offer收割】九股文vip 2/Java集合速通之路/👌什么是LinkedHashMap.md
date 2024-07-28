LinkedHashMap是 Java 集合框架中的一个类，它继承自HashMap，并且具有哈希表和链表的双重特性。LinkedHashMap通过维护一个双向链表来记录键值对的插入顺序或访问顺序，从而提供了一种有序的映射。
### 基本特点

1. **有序性**：LinkedHashMap保证了键值对的顺序，可以是插入顺序（默认）或访问顺序（可选）。
2. **哈希表和链表结合**：LinkedHashMap通过哈希表实现快速的键值对查找，同时通过双向链表维护顺序。
3. **允许null键和值**：LinkedHashMap允许一个null键和多个null值。
4. **线程不安全**：LinkedHashMap不是线程安全的，如果需要在多线程环境中使用，需要通过外部同步机制来保证线程安全。
### 与其他集合类的比较

- **LinkedHashMap vs. HashMap**：
   - LinkedHashMap保证键值对的顺序（插入顺序或访问顺序），而HashMap不保证顺序。
   - LinkedHashMap通过维护链表来记录顺序，因此在插入和删除操作上可能略慢于HashMap。
- **LinkedHashMap vs. TreeMap**：
   - LinkedHashMap保证插入顺序或访问顺序，而TreeMap保证键的自然顺序或比较器的顺序。
   - LinkedHashMap基于哈希表实现，操作的平均时间复杂度为 O(1)，而TreeMap基于红黑树实现，操作的时间复杂度为 O(log n)。
### 适用场景
LinkedHashMap适用于需要保持键值对的插入顺序或访问顺序的场景，例如：

- 实现 LRU（最近最少使用）缓存。
- 需要按插入顺序遍历键值对。
- 需要在保持顺序的同时快速查找键值对。
