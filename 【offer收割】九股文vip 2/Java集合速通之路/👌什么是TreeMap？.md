### 基本特点
1. **有序性**：TreeMap保证了键的自然顺序（通过Comparable接口）或通过提供的比较器（Comparator）的顺序。
2. **红黑树**：TreeMap内部使用红黑树数据结构来存储键值对，保证了插入、删除、查找等操作的时间复杂度为 O(log n)。
3. **不允许null键**：TreeMap不允许键为null，但允许值为null。
4. **线程不安全**：TreeMap不是线程安全的，如果需要在多线程环境中使用，需要通过外部同步机制来保证线程安全。
### 与其他集合类的比较

- **TreeMap vs. HashMap**：
   - TreeMap保证键的有序性，而HashMap不保证顺序。
   - TreeMap基于红黑树实现，操作的时间复杂度为 O(log n)，而HashMap基于哈希表实现，操作的平均时间复杂度为 O(1)。
   - TreeMap不允许null键，而HashMap允许一个null键。
- **TreeMap vs. LinkedHashMap**：
   - TreeMap保证键的自然顺序或比较器的顺序，而LinkedHashMap保证插入顺序或访问顺序。
   - TreeMap的操作时间复杂度为 O(log n)，而LinkedHashMap的操作时间复杂度为 O(1)。
### 适用场景
TreeMap适用于需要按键排序存储键值对的场景，例如：

- 实现基于范围的查询。
- 需要按顺序遍历键值对。
- 需要快速查找最小或最大键值对。
