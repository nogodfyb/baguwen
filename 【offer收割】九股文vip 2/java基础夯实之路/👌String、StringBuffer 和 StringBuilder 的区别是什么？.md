### 1.String
**特性**：

- **不可变性**：String对象是不可变的。一旦创建，字符串的内容就不能被改变。任何对字符串的修改都会生成一个新的String对象。
- **线程安全**：由于String对象是不可变的，它们是线程安全的，可以在多个线程中安全地共享。

**使用场景**：

- 适用于字符串内容不会发生变化的场景，例如字符串常量、少量的字符串操作等。

**示例**：
```
String str = "Hello";
str = str + " World"; // 生成一个新的字符串对象
```
### 2.StringBuffer
**特性**：

- **可变性**：StringBuffer对象是可变的，可以对字符串内容进行修改，而不会生成新的对象。
- **线程安全**：StringBuffer是线程安全的，它的方法是同步的，可以在多线程环境中安全使用。

**使用场景**：

- 适用于在多线程环境中需要频繁修改字符串内容的场景。

**示例**：
```
StringBuffer sb = new StringBuffer("Hello");
sb.append(" World"); // 修改同一个 StringBuffer 对象
```
### 3.StringBuilder
**特性**：

- **可变性**：StringBuilder对象也是可变的，可以对字符串内容进行修改，而不会生成新的对象。
- **非线程安全**：StringBuilder不是线程安全的，它的方法没有同步，因此在多线程环境中使用时需要额外注意。

**使用场景**：

- 适用于在单线程环境中需要频繁修改字符串内容的场景，性能比StringBuffer更高。

**示例**：
```
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World"); // 修改同一个 StringBuilder 对象
```
### 总结
| 特性 | String | StringBuffer | StringBuilder |
| --- | --- | --- | --- |
| 可变性 | 不可变 | 可变 | 可变 |
| 线程安全性 | 线程安全 | 线程安全 | 非线程安全 |
| 性能 | 低（频繁修改时） | 中（线程安全开销） | 高（无线程安全开销） |
| 适用场景 | 字符串内容不变 | 多线程环境中频繁修改 | 单线程环境中频繁修改 |

选择哪个类取决于具体的使用场景。如果字符串内容不变，使用String；如果需要在多线程环境中修改字符串，使用StringBuffer；如果在单线程环境中修改字符串，使用StringBuilder。
