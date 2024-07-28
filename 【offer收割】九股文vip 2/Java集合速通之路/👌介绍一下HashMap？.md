HashMap主要是用于存储键值对。它是基于哈希表实现的，提供了快速的插入、删除和查找操作。从安全角度，HashMap不是线程安全的。如果多个线程同时访问一个HashMap并且至少有一个线程修改了它，则必须手动同步。<br />HashMap允许一个 null 键和多个 null 值。<br />HashMap不保证映射的顺序，特别是它不保证顺序会随着时间的推移保持不变。<br />HashMap提供了 O(1) 时间复杂度的基本操作（如 get 和 put），前提是哈希函数的分布良好且冲突较少。
### HashMap主要方法

- **put(K key, V value)**：将指定的值与该映射中的指定键关联。如果映射以前包含一个该键的映射，则旧值将被替换。
- **get(Object key)**：返回指定键所映射的值；如果此映射不包含该键的映射，则返回 null。
- **remove(Object key)**：如果存在一个键的映射，则将其从映射中移除。
- **containsKey(Object key)**：如果此映射包含指定键的映射，则返回 true。
- **containsValue(Object value)**：如果此映射将一个或多个键映射到指定值，则返回 true。
- **size()**：返回此映射中的键值映射关系的数量。
- **isEmpty()**：如果此映射不包含键值映射关系，则返回 true。
- **clear()**：从此映射中移除所有键值映射关系。
### 示例代码
```
import java.util.HashMap;
import java.util.Map;

public class HashMapExample {
    public static void main(String[] args) {
        // 创建一个 HashMap 实例
        Map<String, Integer> map = new HashMap<>();

        // 添加键值对
        map.put("apple", 1);
        map.put("banana", 2);
        map.put("orange", 3);

        // 访问元素
        System.out.println("Value for key 'apple': " + map.get("apple"));

        // 检查是否包含某个键或值
        System.out.println("Contains key 'banana': " + map.containsKey("banana"));
        System.out.println("Contains value 3: " + map.containsValue(3));

        // 移除元素
        map.remove("orange");

        // 遍历 HashMap
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
        }

        // 获取大小
        System.out.println("Size of map: " + map.size());

        // 清空 HashMap
        map.clear();
        System.out.println("Is map empty: " + map.isEmpty());
    }
}
```
### 内部工作原理
HashMap使用哈希表来存储数据。哈希表是基于数组和链表的组合结构。

1. **哈希函数**：HashMap使用键的hashCode()方法来计算哈希值，然后将哈希值映射到数组的索引位置。
2. **数组和链表**：HashMap使用一个数组来存储链表或树结构（Java 8 及以后）。每个数组位置被称为一个“桶”，每个桶存储链表或树。
3. **冲突处理**：当两个键的哈希值相同时，它们会被存储在同一个桶中，形成一个链表（或树）。这种情况称为哈希冲突。
4. **再哈希**：当HashMap中的元素数量超过容量的负载因子（默认 0.75）时，HashMap会进行再哈希，将所有元素重新分配到一个更大的数组中。
### 性能注意事项

- **初始容量和负载因子**：可以通过构造函数设置HashMap的初始容量和负载因子，以优化性能。初始容量越大，减少再哈希的次数；负载因子越小，减少冲突的概率，但会增加空间开销。
- **哈希函数的质量**：哈希函数的质量直接影响HashMap的性能。理想的哈希函数应尽可能均匀地分布键。
### 线程安全性
如前所述，HashMap不是线程安全的。如果需要线程安全的映射，可以使用Collections.synchronizedMap来包装HashMap，或者使用ConcurrentHashMap，后者在高并发环境下性能更好。
```
Map<String, Integer> synchronizedMap = Collections.synchronizedMap(newHashMap<>());
```
或者使用ConcurrentHashMap：
```
Map<String, Integer> concurrentMap = newConcurrentHashMap<>();
```
