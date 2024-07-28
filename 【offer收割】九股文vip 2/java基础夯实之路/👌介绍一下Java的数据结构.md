### 基本数据结构
1. **数组 (Array)**：
   - 固定大小的容器，用于存储相同类型的元素。数组在内存中是连续存储的，支持通过索引快速访问元素。
```
int[] numbers = new int[10];
numbers[0] = 1;
```
### Java Collections Framework (JCF)
JCF 提供了一组接口和类，用于管理和操作集合（如列表、集合、映射等）。以下是一些主要的接口和它们的实现类：

1. **List 接口**：
   - 有序集合，允许重复元素。常用实现类包括`ArrayList`、`LinkedList`和`Vector`。
```
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
```

- **ArrayList**：
   - 基于动态数组实现，支持快速随机访问和遍历。
- **LinkedList**：
   - 基于双向链表实现，适合频繁的插入和删除操作。
- **Vector**：
   - 类似于`ArrayList`，但线程安全。
1. **Set 接口**：
   - 无序集合，不允许重复元素。常用实现类包括`HashSet`、`LinkedHashSet`和`TreeSet`。
```
Set<String> set = new HashSet<>();
set.add("A");
set.add("B");
```

- **HashSet**：
   - 基于哈希表实现，提供快速的插入、删除和查找操作。
- **LinkedHashSet**：
   - 保持插入顺序的`HashSet`。
- **TreeSet**：
   - 基于红黑树实现，元素按自然顺序排序。
1. **Map 接口**：
   - 键值对映射，不允许重复键。常用实现类包括`HashMap`、`LinkedHashMap`和`TreeMap`。
```
Map<String, Integer> map = new HashMap<>();
map.put("A", 1);
map.put("B", 2);
```

- **HashMap**：
   - 基于哈希表实现，提供快速的键值对存取。
- **LinkedHashMap**：
   - 保持插入顺序的`HashMap`。
- **TreeMap**：
   - 基于红黑树实现，键按自然顺序排序。
1. **Queue 接口**：
   - 先进先出（FIFO）集合。常用实现类包括`LinkedList`和`PriorityQueue`。
```
Queue<String> queue = new LinkedList<>();
queue.add("A");
queue.add("B");
```

- **LinkedList**：
   - 作为队列使用时，支持快速的插入和删除操作。
- **PriorityQueue**：
   - 基于堆实现，元素按自然顺序或指定的比较器顺序进行优先级排序。
1. **Deque 接口**：
   - 双端队列，支持在两端进行插入和删除操作。常用实现类包括`LinkedList`和`ArrayDeque`。
```
Deque<String> deque = new LinkedList<>();
deque.addFirst("A");
deque.addLast("B");
```

- **LinkedList**：
   - 作为双端队列使用时，支持在两端进行快速的插入和删除操作。
- **ArrayDeque**：
   - 基于动态数组实现的双端队列，性能优于`LinkedList`。
