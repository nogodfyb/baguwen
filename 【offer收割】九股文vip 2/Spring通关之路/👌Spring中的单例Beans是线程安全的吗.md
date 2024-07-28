在 Spring 中，单例（Singleton）Beans 是默认的 Bean 范围（Scope）。这意味着 Spring 容器在整个应用程序生命周期内只会创建一个 Bean 实例，并在需要时重复使用该实例。
### 线程安全性
单例 Beans 本身并不是线程安全的。Spring 容器不会自动为你处理线程安全问题。如果你的单例 Bean 被多个线程同时访问，并且这些线程执行的操作会修改 Bean 的状态，那么你需要自己确保线程安全。
### 例子
```
public class Counter {
    private int count = 0;

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```
在多线程环境中，如果多个线程同时调用increment()方法，可能会导致竞争条件，从而产生错误的结果。
### 解决方法
#### 1. 使用同步（synchronization）
可以在方法级别使用synchronized关键字，确保同一时间只有一个线程可以执行该方法：
```
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```
#### 2. 使用java.util.concurrent包中的并发工具类
```
import java.util.concurrent.atomic.AtomicInteger;

public class Counter {
    private AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();
    }

    public int getCount() {
        return count.get();
    }
}
```
#### 3. 使用无状态（Stateless）设计
如果可能，设计无状态的 Beans，这样就不需要担心线程安全问题。无状态的 Bean 不会保存任何会被多个线程共享的状态。
### 总结
虽然 Spring 中的单例 Beans 是默认的 Bean 范围，但它们本身并不是线程安全的。如果你的单例 Bean 在多线程环境中被使用，并且会修改其内部状态，那么你需要自己采取措施来确保线程安全。可以通过同步、使用并发工具类或者设计无状态的 Bean 来解决线程安全问题。
