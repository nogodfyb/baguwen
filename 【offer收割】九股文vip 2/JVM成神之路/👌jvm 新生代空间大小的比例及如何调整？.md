在 JVM 中，堆内存通常被划分为新生代（Young Generation）和老年代（Old Generation）。新生代又进一步划分为 Eden 区和两个 Survivor 区（S0 和 S1）。
### 新生代空间的默认比例
默认情况下，HotSpot JVM 使用的比例大致如下：

- **新生代（Young Generation）**：占整个堆内存的 1/3 到 1/4 左右。
- **老年代（Old Generation）**：占整个堆内存的 2/3 到 3/4 左右。

在新生代内部，默认的比例是：

- **Eden 区**：占新生代的 8/10（即 80%）。
- **每个 Survivor 区（S0 和 S1）**：各占新生代的 1/10（即 10%）。
### 新生代空间大小的调整
调整新生代空间大小的主要目的是优化垃圾收集性能，减少应用程序的停顿时间。以下是一些常用的 JVM 参数和调整方法：
#### 1. 调整新生代和老年代的比例

- **-Xms和-Xmx**：设置堆内存的初始大小和最大大小。
- **-XX:NewSize和-XX:MaxNewSize**：设置新生代的初始大小和最大大小。例如：
```
-XX:NewSize=512m
-XX:MaxNewSize=512m
```

- **-XX:NewRatio**：设置新生代和老年代的比例。例如，-XX:NewRatio=3表示新生代占整个堆的 3/4，老年代占 1/4。
#### 2. 调整 Eden 区和 Survivor 区的比例

- **-XX:SurvivorRatio**：设置 Eden 区和 Survivor 区的比例。例如，-XX:SurvivorRatio=8表示 Eden 区占新生代的 8/10，每个 Survivor 区占 1/10。
#### 3. 调整 Survivor 区的数量

- **-XX:SurvivorRatio**：默认情况下，JVM 使用两个 Survivor 区（S0 和 S1）。你可以通过调整 Survivor 区的比例来优化内存使用和垃圾收集性能。
#### 4. 动态调整新生代大小

- **-XX:+UseAdaptiveSizePolicy**：启用自适应大小策略，JVM 会根据应用程序的运行情况动态调整新生代和老年代的大小。
### 调整策略
在调整新生代空间大小时，需要考虑以下因素：

1. **应用程序的对象生命周期**：
   - 如果应用程序创建了大量短生命周期对象（例如 Web 应用中的请求对象），则需要较大的新生代空间，以减少 Minor GC 的频率。
   - 如果应用程序有较多长生命周期对象，则需要较大的老年代空间，以减少 Major GC 的频率。
2. **GC 日志分析**：
   - 启用 GC 日志（例如-Xlog:gc*或-XX:+PrintGCDetails），分析垃圾收集的频率和停顿时间，调整新生代和老年代的大小以优化性能。
3. **性能测试**：
   - 在调整 JVM 参数后，进行性能测试，观察 GC 行为和应用程序的响应时间，进一步调整参数以达到最佳性能。
### 示例配置
假设你有一个堆内存大小为 4GB 的 JVM 实例，你希望新生代占 1GB，老年代占 3GB，并且 Eden 区占新生代的 80%，每个 Survivor 区占 10%。可以使用如下参数：
```
-Xms4g -Xmx4g
-XX:NewSize=1g 
-XX:MaxNewSize=1g
-XX:NewRatio=3
-XX:SurvivorRatio=8
```
通过合理调整新生代空间大小和比例，可以优化垃圾收集性能，减少应用程序的停顿时间，从而提高整体性能。
