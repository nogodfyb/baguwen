[![13、动态代理导致的频繁fullgc.mp4 (11.82MB)](https://gw.alipayobjects.com/mdn/prod_resou/afts/img/A*NNs6TKOR3isAAAAAAAAAAABkARQnAQ)](https://www.yuque.com/docs/176645982?_lake_card=%7B%22status%22%3A%22done%22%2C%22name%22%3A%2213%E3%80%81%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86%E5%AF%BC%E8%87%B4%E7%9A%84%E9%A2%91%E7%B9%81fullgc.mp4%22%2C%22size%22%3A12390511%2C%22taskId%22%3A%22u8f19b632-76fd-4969-b415-3b6482d9f11%22%2C%22taskType%22%3A%22upload%22%2C%22url%22%3Anull%2C%22cover%22%3Anull%2C%22videoId%22%3A%22inputs%2Fprod%2Fyuque%2F2024%2F29413969%2Fmp4%2F1719845271434-4ed1ca0e-910b-4ae2-b2c6-8f4d0667e233.mp4%22%2C%22download%22%3Afalse%2C%22__spacing%22%3A%22both%22%2C%22id%22%3A%22azw3T%22%2C%22margin%22%3A%7B%22top%22%3Atrue%2C%22bottom%22%3Atrue%7D%2C%22card%22%3A%22video%22%7D#azw3T)这道题也是一样的，当你简历写了 jvm 调优。还是一样，jvm 调优案例！<br />本案例为：jvm动态代理导致的频繁full gc！<br />本性能优化案例是通用的！大家消化完成，直接套到自己项目即可！<br />本题配备了视频助于理解。
# 背景
在我们 618 的时候，需要对系统做压测，刚开始10并发进行压测，cpu压到了100%但是系统最大qps才200多。通过JVM监控查看JVM younggc很频繁，fullGC数量为零。
## **排查过程**
cpu 达到100% 则先看cpu使用率最高是哪个进程，可以直接通过linux命令 top查看，找到对应的进程ID，发现正是压测的java系统进程ID，找到进程ID后，然后在查找该进程下CPU使用率最高是哪个线程，可以通过top -p 进程ID -H 命令显示线程使用cpu信息，拿到一个pid。<br />PID列则为十进制显示的线程ID，然后转换为16进制通过jstack 系统进程ID | grep 15e 进制线程ID 可以找到对应的线程信息如下，也就是该线程占用了一半左右的cpu。
```
jstack 222 | grep 15e
```
执行完命令之后，可以看到一个 Finalizer 线程。<br />Finalizer线程是个单一职责的线程。这个线程会不停的循环等待java.lang.ref.Finalizer.ReferenceQueue中的新增对象。一旦Finalizer线程发现队列中出现了新的对象，它会弹出该对象，调用它的finalize()方法，将该引用从Finalizer类中移除，因此下次GC再执行的时候，这个Finalizer实例以及它引用的那个对象就可以回垃圾回收掉了。<br />说明Finalizer的队列中有许多的等待回收的垃圾对象，可以通过命令查看等待回收的对象都有哪些。
```
jmap -finalizerinfo 进程ID
```
执行命令后显示结果如下<br />Count Class description<br />-------------------------------------------------------<br />32221 com.XXXXXXX$$EnhancerByCGLIB$$200e6ee6<br />14908 com.XXXXXXXX$$EnhancerByCGLIB$$a59933de<br />11982 java.util.zip.Deflater<br />1 java.net.SocksSocketImpl<br />发现有好多的自定义对象，通过类名可以看到这些对象都是通过CGLIB动态代理创建的，而这些动态代理类都默认实现了finalize方法，导致这些对象在进行垃圾回收时必须先要执行finalize方法，所以都积压到了finalizer的队列中。
## **解决方案**
选择复用对象，不要每次都new一个 cglib 代理的对象。
