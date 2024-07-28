在Java中，每个线程都有一个优先级，优先级决定了线程调度器对线程的调度顺序。线程的优先级是一个整数值，范围在1到10之间。具体来说：

- **最低优先级**：Thread.MIN_PRIORITY(值为1)
- **默认优先级**：Thread.NORM_PRIORITY(值为5)
- **最高优先级**：Thread.MAX_PRIORITY(值为10)
### 线程优先级的作用
线程优先级是对线程调度器的一种建议，调度器会根据优先级来决定哪个线程应该优先执行。然而，线程优先级并不能保证线程一定会按照优先级顺序执行，具体的调度行为依赖于操作系统的线程调度策略。
### 设置线程优先级
可以通过setPriority(int newPriority)方法来设置线程的优先级。需要注意的是，设置的优先级必须在1到10之间，否则会抛出IllegalArgumentException。
### 示例
```
public class ThreadPriorityExample {
    public static void main(String[] args) {
        Thread lowPriorityThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Low priority thread running");
            }
        });
        lowPriorityThread.setPriority(Thread.MIN_PRIORITY);

        Thread highPriorityThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("High priority thread running");
            }
        });
        highPriorityThread.setPriority(Thread.MAX_PRIORITY);

        lowPriorityThread.start();
        highPriorityThread.start();
    }
}
```
在这个示例中，我们创建了两个线程，一个设置为最低优先级，一个设置为最高优先级。通常情况下，系统会优先调度高优先级的线程执行，但这并不是绝对的，具体行为依赖于操作系统的调度策略。
### 注意事项

1. **优先级并不保证执行顺序**：线程优先级只是对线程调度器的一个建议，不能保证线程会按照优先级顺序执行。
2. **避免滥用优先级**：不建议过度依赖线程优先级来控制线程的执行顺序，应该更多地通过设计合理的并发控制机制（如锁、信号量、条件变量等）来管理线程。
3. **平台依赖性**：不同操作系统对线程优先级的支持和实现方式可能不同，因此在跨平台应用中，优先级的效果可能不一致。

应该谨慎使用，并且不能完全依赖它来控制线程的执行顺序。
