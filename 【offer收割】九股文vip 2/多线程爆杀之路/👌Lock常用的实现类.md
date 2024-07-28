### 1.ReentrantLock
**描述**：ReentrantLock是Lock接口的一个常用实现类，它是一个可重入的互斥锁，提供了比synchronized关键字更灵活的锁定机制。<br />**特点**：

- 可重入：同一个线程可以多次获取该锁，而不会导致死锁。
- 公平锁和非公平锁：可以选择公平锁（按请求顺序获取锁）或非公平锁（默认，可能会有线程饥饿）。
- 提供了tryLock方法，可以尝试获取锁而不阻塞。
- 提供了lockInterruptibly方法，可以在等待锁的过程中响应中断。

**示例**：
```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantLockExample {
    private final Lock lock = new ReentrantLock();

    public void method() {
        lock.lock();
        try {
            // 临界区代码
        } finally {
            lock.unlock();
        }
    }
}
```
### 2.ReentrantReadWriteLock
**描述**：ReentrantReadWriteLock提供了读写锁的实现，允许多个读线程同时访问，但写线程独占。<br />**特点**：

- 读锁和写锁分离：读锁是共享的，可以被多个读线程同时持有；写锁是独占的，只有一个线程可以持有写锁。
- 提高了读多写少场景下的并发性能。
- 提供了公平和非公平两种模式。

**示例**：
```
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReentrantReadWriteLockExample {
    private final ReadWriteLock rwLock = new ReentrantReadWriteLock();
    private final Lock readLock = rwLock.readLock();
    private final Lock writeLock = rwLock.writeLock();

    public void readMethod() {
        readLock.lock();
        try {
            // 读操作
        } finally {
            readLock.unlock();
        }
    }

    public void writeMethod() {
        writeLock.lock();
        try {
            // 写操作
        } finally {
            writeLock.unlock();
        }
    }
}
```
### 3.StampedLock
**描述**：StampedLock是一种改进的读写锁，提供了三种锁模式：写锁、悲观读锁和乐观读锁。<br />**特点**：

- 乐观读锁：在没有写操作时，乐观读锁几乎不需要加锁，大大提高了并发性能。
- 悲观读锁：类似于传统的读锁，适用于需要严格保护的读操作。
- 写锁：类似于传统的写锁，适用于需要独占访问的写操作。

**示例**：
```
import java.util.concurrent.locks.StampedLock;

public class StampedLockExample {
    private final StampedLock stampedLock = new StampedLock();

    public void readMethod() {
        long stamp = stampedLock.readLock();
        try {
            // 读操作
        } finally {
            stampedLock.unlockRead(stamp);
        }
    }

    public void writeMethod() {
        long stamp = stampedLock.writeLock();
        try {
            // 写操作
        } finally {
            stampedLock.unlockWrite(stamp);
        }
    }

    public void optimisticReadMethod() {
        long stamp = stampedLock.tryOptimisticRead();
        try {
            // 读操作
            if (!stampedLock.validate(stamp)) {
                // 如果乐观读失败，重新获取读锁
                stamp = stampedLock.readLock();
                try {
                    // 读操作
                } finally {
                    stampedLock.unlockRead(stamp);
                }
            }
        } finally {
            // 乐观读不需要显式解锁
        }
    }
}
```
### 4.Condition
**描述**：Condition是与Lock结合使用的条件变量，实现线程间的协调。<br />**特点**：

- 可以替代Object的wait、notify和notifyAll方法。
- 可以创建多个条件变量，提供更灵活的线程调度机制。

**示例**：
```
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ConditionExample {
    private final Lock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();

    public void awaitMethod() throws InterruptedException {
        lock.lock();
        try {
            condition.await(); // 当前线程等待
        } finally {
            lock.unlock();
        }
    }

    public void signalMethod() {
        lock.lock();
        try {
            condition.signal(); // 唤醒等待的线程
        } finally {
            lock.unlock();
        }
    }
}
```
### 总结
Lock接口及其实现类提供了丰富的并发控制工具，适用于不同的并发编程需求。选择合适的锁类型和使用模式，可以有效提高程序的并发性能和可维护性。
