join是Thread类中的一个方法，它允许一个线程等待另一个线程的完成。调用join方法的线程将暂停执行，直到被调用join方法的线程完成其执行。使用join方法，可以管理多线程程序的执行流程。
### 工作原理
当一个线程调用另一个线程的join方法时，当前线程会进入等待状态，直到目标线程完成或指定的等待时间到期。join方法内部是通过wait机制实现的，当目标线程完成时，会调用notifyAll方法唤醒所有等待的线程。
### 适用场景

- **线程同步**：确保一个线程在另一个线程完成之后再执行。例如，在多线程计算中，主线程需要等待所有子线程完成计算后再汇总结果。
- **顺序执行**：强制线程按特定顺序执行。例如，必须确保某些初始化任务在线程执行之前完成。
### 示例代码
主线程中等待多个子线程完成
```
publicclassJoinExample {
    publicstaticvoidmain(String[] args) {
        Threadt1=newThread(() -> {
            try {
                Thread.sleep(2000); // 模拟任务
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Thread t1 completed");
        });

        Threadt2=newThread(() -> {
            try {
                Thread.sleep(3000); // 模拟任务
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Thread t2 completed");
        });

        t1.start();
        t2.start();

        try {
            t1.join(); // 等待t1完成
            t2.join(); // 等待t2完成
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //主线程将等待t1和t2都完成后，才会继续执行并打印最终的消息
        System.out.println("Main thread completed after t1 and t2");
    }
}
```
