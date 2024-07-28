### put方法的实现
```
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```
put方法调用了putVal方法。这里的hash(key)是计算键的哈希值。
### 计算哈希值
hash方法用于计算键的哈希值并进行扰动处理，以减少冲突。
```
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
### putVal方法的实现
putVal方法是HashMap中实际执行插入操作的核心方法。
```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```
### 详细步骤解析

1. **初始化表**：如果哈希表还没有初始化或长度为0，则进行初始化（扩容）。
```
if ((tab = table) == null || (n = tab.length) == 0)
    n = (tab = resize()).length;
```

1. **计算索引**：通过哈希值和数组长度计算出索引位置。
```
if ((p = tab[i = (n - 1) & hash]) == null)
    tab[i] = newNode(hash, key, value, null);
```

1. **插入新节点**：如果索引位置为空，直接插入新节点。
2. **处理哈希冲突**：如果索引位置不为空，需要处理冲突。
   - **检查是否存在相同的键**：如果找到相同的键，替换其值。
   - **红黑树处理**：如果当前节点是红黑树节点，则调用putTreeVal方法插入。
   - **链表处理**：如果当前节点是链表节点，遍历链表插入新节点。
3. **转换为红黑树**：如果链表长度超过阈值（8），则将链表转换为红黑树。
```
if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
    treeifyBin(tab, hash);
```

1. **更新节点值**：如果存在相同的键，更新其值。
```
if (e != null) { // existing mapping for key
    VoldValue= e.value;
    if (!onlyIfAbsent || oldValue == null)
        e.value = value;
    afterNodeAccess(e);
    return oldValue;
}
```

1. **调整大小**：插入新节点后，增加元素数量。如果超过阈值，则进行扩容。
```
++modCount;if (++size > threshold)
    resize();
```

1. **插入后的处理**：进行一些插入后的处理操作。
```
afterNodeInsertion(evict);
```
