### 1. 初始容量（Initial Capacity）
初始容量是HashMap在创建时分配的桶（bucket）数组的大小。默认初始容量是 16。可以在创建HashMap时通过构造函数指定初始容量。
```
HashMap<K, V> map = newHashMap<>(initialCapacity);
```
### 2. 负载因子（Load Factor）
负载因子是一个衡量HashMap何时需要调整大小（即扩容）的参数。默认负载因子是 0.75，这意味着当HashMap中的条目数达到当前容量的 75% 时，HashMap会进行扩容。负载因子越低，哈希表中的空闲空间越多，冲突越少，但空间利用率也越低。
```
HashMap<K, V> map = newHashMap<>(initialCapacity, loadFactor);
```
### 3. 阈值（Threshold）
阈值是HashMap需要扩容的临界点，计算方式为初始容量 * 负载因子。当实际存储的键值对数量超过这个阈值时，HashMap会进行扩容。
### 4. 桶（Bucket）
HashMap内部使用一个数组来存储链表或树（在 Java 8 及之后的版本中，当链表长度超过一定阈值时，会转化为树）。每个数组元素称为一个桶（bucket）。哈希值经过计算后决定了键值对存储在哪个桶中。
### 5. 哈希函数（Hash Function）
HashMap使用哈希函数将键的哈希码转换为数组索引。Java 的HashMap使用了扰动函数（perturbation function）来减少哈希冲突：
```
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
### 6. 链表和树（Linked List and Tree）
在桶中的键值对存储方式上，HashMap使用链表来处理哈希冲突。在 Java 8 及之后的版本中，当链表的长度超过阈值（默认是 8）时，链表会转换为红黑树，以提高查找效率。
### 7. 红黑树转换阈值（Treeify Threshold）
这是一个阈值，当单个桶中的链表长度超过这个值时，链表会转换为红黑树。默认值是 8。
### 8. 最小树化容量（Minimum Treeify Capacity）
这是一个阈值，当HashMap的容量小于这个值时，即使链表长度超过Treeify Threshold，也不会将链表转换为红黑树，而是会先进行扩容。默认值是 64。
### 9. 扩容因子（Resize Factor）
当HashMap的大小超过阈值时，容量会加倍。即新的容量是旧容量的两倍。
### 10. 迭代器（Iterators）
HashMap提供了键、值和条目的迭代器，用于遍历HashMap中的元素。迭代器是快速失败的（fail-fast），即在迭代过程中，如果HashMap结构被修改（除了通过迭代器自身的remove方法），迭代器会抛出ConcurrentModificationException。
### 11. 版本（ModCount）
HashMap维护了一个内部版本号modCount，用于跟踪HashMap的结构修改次数。这在迭代器中用于检测并发修改。<br />这些参数和属性共同决定了HashMap的性能和行为。理解这些参数可以帮助开发者更好地使用HashMap，并在需要时进行适当的调整以满足特定的性能需求。
