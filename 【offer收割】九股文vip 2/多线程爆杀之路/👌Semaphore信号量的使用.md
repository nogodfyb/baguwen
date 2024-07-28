Semaphore是 Java 中用于控制多个线程对共享资源访问的同步机制。它通过维护一个许可集来限制并发访问的数量。Semaphore可以用于实现资源池、限流等功能。
### 1. 基本概念
Semaphore主要有两个操作：

- **acquire()**：获取一个许可，如果没有许可可用，则阻塞直到有许可可用。
- **release()**：释放一个许可，释放后可以唤醒等待的线程。

Semaphore有两种模式：

- **公平模式**：线程按照请求许可的顺序获得许可。
- **非公平模式**：线程可能会抢占许可，不按照请求顺序。
### 2. 构造方法
```
public Semaphore(int permits)
public Semaphore(int permits, boolean fair)
```

- permits：初始的许可数量。
- fair：是否使用公平模式。
### 3. 主要方法

- void acquire() throws InterruptedException：获取一个许可，阻塞直到有许可可用。
- void acquire(int permits) throws InterruptedException：获取指定数量的许可。
- void release()：释放一个许可。
- void release(int permits): 释放指定数量的许可。
- boolean tryAcquire()：尝试获取一个许可，立即返回结果。
- boolean tryAcquire(long timeout, TimeUnit unit) throws InterruptedException：在指定时间内尝试获取一个许可。
### 4. 使用示例
#### 示例 1：限制访问的线程数量
假设我们有一个资源池，同时只能允许最多 3 个线程访问资源。
```
import java.util.concurrent.Semaphore;

public class SemaphoreExample {
    private final Semaphore semaphore = new Semaphore(3);

    public void accessResource() {
        try {
            semaphore.acquire(); // 获取许可
            System.out.println(Thread.currentThread().getName() + " is accessing the resource.");
            Thread.sleep(2000); // 模拟访问资源
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            System.out.println(Thread.currentThread().getName() + " is releasing the resource.");
            semaphore.release(); // 释放许可
        }
    }

    public static void main(String[] args) {
        SemaphoreExample example = new SemaphoreExample();

        for (int i = 0; i < 10; i++) {
            new Thread(() -> {
                example.accessResource();
            }).start();
        }
    }
}
```
在这个示例中，最多只能有 3 个线程同时访问资源。当一个线程完成访问后，它会释放许可，使其他等待的线程能够继续访问。
#### 示例 2：实现资源池
我们可以使用Semaphore来实现一个简单的资源池。例如，一个数据库连接池。
```
import java.util.concurrent.Semaphore;
import java.util.concurrent.LinkedBlockingQueue;

public class ConnectionPool {
    private final Semaphore semaphore;
    private final LinkedBlockingQueue<Connection> pool;

    public ConnectionPool(int poolSize) {
        semaphore = new Semaphore(poolSize);
        pool = new LinkedBlockingQueue<>(poolSize);
        for (int i = 0; i < poolSize; i++) {
            pool.offer(new Connection("Connection-" + i));
        }
    }

    public Connection acquireConnection() throws InterruptedException {
        semaphore.acquire();
        return pool.poll();
    }

    public void releaseConnection(Connection connection) {
        pool.offer(connection);
        semaphore.release();
    }

    public static void main(String[] args) {
        ConnectionPool connectionPool = new ConnectionPool(3);

        for (int i = 0; i < 10; i++) {
            new Thread(() -> {
                try {
                    Connection connection = connectionPool.acquireConnection();
                    System.out.println(Thread.currentThread().getName() + " acquired " + connection.getName());
                    Thread.sleep(2000); // 模拟使用连接
                    connectionPool.releaseConnection(connection);
                    System.out.println(Thread.currentThread().getName() + " released " + connection.getName());
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}

class Connection {
    private final String name;

    public Connection(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```
在这个示例中，我们创建了一个连接池，最多允许 3 个线程同时获取连接。每个线程获取连接后，模拟使用连接一段时间，然后释放连接。
### 5. 总结
Semaphore是一种用于控制并发访问的同步机制，通过维护一个许可集来限制并发线程的数量。它可以用于实现资源池、限流等功能。
