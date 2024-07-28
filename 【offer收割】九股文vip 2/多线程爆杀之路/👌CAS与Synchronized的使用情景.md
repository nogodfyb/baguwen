CAS（Compare-And-Swap）和synchronized是两种不同的并发控制机制，适用于不同的使用情景。
### CAS（Compare-And-Swap）
#### 特点：

1. **无锁操作**：CAS 是一种无锁的并发控制机制，不需要显式地获取锁。
2. **高性能**：由于不需要锁定资源，CAS 的性能通常比锁机制更高，尤其在高并发场景下。
3. **乐观锁**：CAS 基于乐观锁的思想，假设竞争不频繁，只有在检测到冲突时才会重试。
4. **原子操作**：CAS 操作是原子的，通常由硬件指令支持（如 x86 架构的cmpxchg指令）。
#### 使用场景：

1. **高并发场景**：适用于需要高并发访问的场景，如计数器、自旋锁、无锁队列等。
2. **轻量级操作**：适用于操作简单且执行时间短的场景，因为 CAS 操作本身是原子的，但如果操作复杂，可能会导致频繁重试。
3. **避免锁竞争**：在锁竞争激烈的场景下，CAS 可以避免线程阻塞，提高系统的吞吐量。
#### 示例：
使用AtomicInteger类实现 CAS 的示例：
```
import java.util.concurrent.atomic.AtomicInteger;

public class CASExample {
    private AtomicInteger counter = new AtomicInteger(0);

    public void increment() {
        int oldValue, newValue;
        do {
            oldValue = counter.get();
            newValue = oldValue + 1;
        } while (!counter.compareAndSet(oldValue, newValue));
    }

    public int getCounter() {
        return counter.get();
    }

    public static void main(String[] args) {
        CASExample example = new CASExample();
        example.increment();
        System.out.println(example.getCounter()); // 输出: 1
    }
}
```
### synchronized
#### 特点：

1. **互斥锁**：synchronized是一种互斥锁机制，确保同一时刻只有一个线程可以执行被锁定的代码块。
2. **简单易用**：synchronized是 Java 语言内置的关键字，使用简单，易于理解和维护。
3. **阻塞操作**：被锁定的线程会进入阻塞状态，直到获取到锁。
4. **内存可见性**：synchronized确保锁释放后，修改的变量对其他线程可见。
#### 使用场景：

1. **复杂操作**：适用于需要对共享资源进行复杂操作的场景，确保操作的原子性和一致性。
2. **低并发场景**：适用于并发度不高的场景，因为synchronized会导致线程阻塞，进而影响性能。
3. **需要内存可见性**：适用于需要确保变量修改对其他线程立即可见的场景。
#### 示例：
使用synchronized关键字实现线程安全的示例：
```
public class SynchronizedExample {
    private int counter = 0;

    public synchronized void increment() {
        counter++;
    }

    public synchronized int getCounter() {
        return counter;
    }

    public static void main(String[] args) {
        SynchronizedExample example = new SynchronizedExample();
        example.increment();
        System.out.println(example.getCounter()); // 输出: 1
    }
}
```
### 总结

- **CAS**适用于高并发、轻量级操作和避免锁竞争的场景，具有高性能的优势，但可能会导致重试。
- **synchronized**适用于复杂操作、低并发场景和需要内存可见性的场景，使用简单，但会导致线程阻塞和性能下降。
