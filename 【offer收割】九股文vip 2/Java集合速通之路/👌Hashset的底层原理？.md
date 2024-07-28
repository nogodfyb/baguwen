HashSet是 Java 中一个常用的集合类，它用于存储不重复的元素。HashSet的底层实现依赖于HashMap。以下是HashSet的底层实现原理：<br />1. 基本结构<br />HashSet底层使用HashMap来存储元素。具体来说，每当你向HashSet中添加一个元素时，这个元素实际上是作为HashMap的键来存储的，而HashMap的值是一个固定的常量对象。
```
public class HashSet<E> extends AbstractSet<E> implements Set<E>, Cloneable, java.io.Serializable {
    private transient HashMap<E,Object> map;

    private static final Object PRESENT = new Object();

    public HashSet() {
        map = new HashMap<>();
    }

    // 其他构造函数和方法
}
```
2. 添加元素 (add方法)<br />当你调用HashSet的add方法时，HashSet会将元素作为键插入到HashMap中，值为一个常量对象PRESENT。
```
public boolean add(E e) {
    return map.put(e, PRESENT) == null;
}
```
HashMap的put方法会检查键是否已经存在，如果不存在则插入新键值对。如果键已经存在，则更新键值对并返回旧值。因此，HashSet能够保证元素唯一性。<br />3. 元素查找 (contains方法)<br />HashSet的contains方法实际上是调用HashMap的containsKey方法。
```
public boolean contains(Object o) {
    return map.containsKey(o);
}
```
HashMap的containsKey方法通过计算键的hashCode值来快速定位元素的位置，然后进行比较。<br />4. 删除元素 (remove方法)<br />HashSet的remove方法调用HashMap的remove方法来删除元素。
```
public boolean remove(Object o) {
    return map.remove(o) == PRESENT;
}
```
