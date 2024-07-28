`CountDownLatch`是 Java 的并发工具类，位于`java.util.concurrent`包中。它允许一个或多个线程等待，直到在其他线程中执行的一组操作完成。`CountDownLatch`通常用于在并发编程中协调多个线程的执行顺序。
### 基本用法
`CountDownLatch`的基本用法包括以下几个步骤：

1. **初始化**：创建一个`CountDownLatch`实例，指定计数值。
2. **等待**：一个或多个线程调用`await()`方法等待计数值变为零。
3. **计数减一**：其他线程在完成某个任务后调用`countDown()`方法将计数值减一。
4. **继续执行**：当计数值变为零时，所有等待的线程将继续执行。
### 示例代码
下面是一个简单的示例，展示了`CountDownLatch`的基本用法。<br />假设我们有一个任务需要多个子任务完成后才能继续执行：
```
import java.util.concurrent.CountDownLatch;
publicclassCountDownLatchExample {

    publicstaticvoidmain(String[] args) {
        intnumberOfTasks=3;
        CountDownLatchlatch=newCountDownLatch(numberOfTasks);

        // 创建并启动多个子任务线程
        for (inti=0; i < numberOfTasks; i++) {
            newThread(newTask(latch)).start();
        }

        try {
            // 主线程等待所有子任务完成
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 所有子任务完成后，主线程继续执行
        System.out.println("All tasks are completed. Main thread continues.");
    }
}
classTaskimplementsRunnable {
    private CountDownLatch latch;

    publicTask(CountDownLatch latch) {
        this.latch = latch;
    }

    @Override
    publicvoidrun() {
        try {
            // 模拟任务执行
            System.out.println(Thread.currentThread().getName() + " is executing task.");
            Thread.sleep((long) (Math.random() * 1000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            // 子任务完成后，计数减一
            latch.countDown();
            System.out.println(Thread.currentThread().getName() + " has finished task.");
        }
    }
}
```
### 解释

1. **初始化**`**CountDownLatch**`：
```
CountDownLatchlatch=newCountDownLatch(numberOfTasks);
```

1. 这里我们创建了一个`CountDownLatch`实例，计数值为`numberOfTasks`（3）。
2. **子任务线程**：
```
newThread(newTask(latch)).start();
```

1. 我们启动了多个子任务线程，每个线程都会执行`Task`类中的`run`方法。
2. **等待**：
```
latch.await();
```

1. 主线程调用`await()`方法，等待计数值变为零。
2. **任务执行和计数减一**：
```
latch.countDown();
```

1. 每个子任务完成后调用`countDown()`方法将计数值减一。
2. **继续执行**： 当所有子任务完成后，计数值变为零，`await()`方法返回，主线程继续执行。
### 其他方法

- `long getCount()`：返回当前计数值。
- `boolean await(long timeout, TimeUnit unit)`：等待指定时间，如果计数值在超时前变为零则返回`true`，否则返回`false`。
### 使用场景

- **主线程等待子线程完成**：例如，主线程需要等待多个子线程完成初始化或加载数据。
- **多个线程起点同步**：例如，多个线程需要在同一时间点开始执行任务。

`CountDownLatch`是一个非常有用的并发工具类，能够简化线程间的协调和同步。通过合理使用`CountDownLatch`，可以有效地控制线程的执行顺序，避免竞态条件和其他并发问题。
