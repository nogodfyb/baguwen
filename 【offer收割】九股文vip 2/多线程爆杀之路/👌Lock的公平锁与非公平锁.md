### 公平锁与非公平锁的概念
- **公平锁（Fair Lock）**：公平锁按照线程请求锁的顺序（FIFO）分配锁。这意味着等待时间最长的线程最先获得锁，确保锁的分配是公平的。
- **非公平锁（Non-Fair Lock）**：非公平锁不保证锁的分配顺序，可能会让某些线程“插队”，从而可能导致某些线程长时间等待锁。
### ReentrantLock的公平性
ReentrantLock是Lock接口的一个常用实现，它可以配置为公平锁或非公平锁。默认情况下，ReentrantLock是非公平锁，但可以通过构造函数来指定锁的公平性。
```
import java.util.concurrent.locks.ReentrantLock;

public class FairNonFairLockExample {
    private final ReentrantLock fairLock = new ReentrantLock(true); // 公平锁
    private final ReentrantLock nonFairLock = new ReentrantLock(false); // 非公平锁

    public void testLock(ReentrantLock lock) {
        try {
            lock.lock();
            System.out.println(Thread.currentThread().getName() + " acquired the lock.");
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
            System.out.println(Thread.currentThread().getName() + " released the lock.");
        }
    }

    public static void main(String[] args) {
        FairNonFairLockExample example = new FairNonFairLockExample();

        Runnable task = () -> {
            for (int i = 0; i < 5; i++) {
                example.testLock(example.fairLock); // 测试公平锁
                // example.testLock(example.nonFairLock); // 测试非公平锁
            }
        };

        Thread t1 = new Thread(task, "Thread-1");
        Thread t2 = new Thread(task, "Thread-2");
        Thread t3 = new Thread(task, "Thread-3");

        t1.start();
        t2.start();
        t3.start();
    }
}
```
### 公平锁与非公平锁的优缺点
#### 公平锁的优点

1. **避免线程饥饿**：公平锁按照请求顺序分配锁，避免了某些线程长时间等待锁的问题。
2. **可预测性**：锁的分配是有序的，容易预测和调试。
#### 公平锁的缺点

1. **性能开销**：公平锁需要维护一个队列来管理线程的请求顺序，这会带来额外的性能开销。
2. **吞吐量较低**：由于公平锁严格按照顺序分配锁，可能会导致上下文切换频繁，降低系统的整体吞吐量。
#### 非公平锁的优点

1. **高性能**：非公平锁不需要维护请求队列，减少了性能开销。
2. **高吞吐量**：由于允许线程插队，非公平锁在某些情况下可以提高系统的整体吞吐量。
#### 非公平锁的缺点

1. **线程饥饿**：某些线程可能长时间无法获得锁，导致线程饥饿。
2. **不可预测性**：锁的分配是无序的，可能会导致调试和预测变得困难。
### 总结
公平锁和非公平锁在锁的分配策略上有所不同，各有优缺点。公平锁保证了锁的分配是公平的，避免了线程饥饿，但可能会带来性能开销和较低的吞吐量。非公平锁则通过允许线程插队来提高性能和吞吐量，但可能会导致线程饥饿。<br />在实际应用中，选择公平锁还是非公平锁需要根据具体的场景和需求来决定。如果需要严格的顺序和公平性，可以选择公平锁；如果更关注性能和吞吐量，可以选择非公平锁。
