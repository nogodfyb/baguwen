ExecutorService接口中，shutdown()和shutdownNow()是用于关闭线程池的方法。
### 1.shutdown()

- **行为**：shutdown()方法会启动线程池的关闭过程。它会停止接受新的任务提交，但会继续执行已经提交的任务（包括正在执行的和已提交但尚未开始执行的任务）。
- **调用后状态**：调用shutdown()后，线程池会进入一个平滑的关闭过程，等待所有已提交的任务完成后才会完全终止。
```
ExecutorServiceexecutorService= Executors.newFixedThreadPool(5);
// 提交一些任务for (inti=0; i < 10; i++) {
    executorService.submit(() -> {
        System.out.println("Task executed by " + Thread.currentThread().getName());
    });
}
// 调用shutdown()
executorService.shutdown();
```
### 2.shutdownNow()

- **行为**：shutdownNow()方法会尝试停止所有正在执行的任务，并返回一个包含尚未开始执行的任务的列表。它会立即停止接收新的任务，并试图中断正在执行的任务。
- **调用后状态**：调用shutdownNow()后，线程池会尽快停止所有正在执行的任务，并返回尚未开始执行的任务列表。需要注意的是，无法保证所有正在执行的任务都能被中断。
```
ExecutorServiceexecutorService= Executors.newFixedThreadPool(5);
// 提交一些任务for (inti=0; i < 10; i++) {
    executorService.submit(() -> {
        System.out.println("Task executed by " + Thread.currentThread().getName());
        try {
            Thread.sleep(1000); // 模拟长时间运行的任务
        } catch (InterruptedException e) {
            System.out.println("Task interrupted");
        }
    });
}
// 调用shutdownNow()
List<Runnable> notExecutedTasks = executorService.shutdownNow();
System.out.println("Tasks not executed: " + notExecutedTasks.size());
```
### 3. 总结

- **shutdown()**：
   - 停止接受新的任务。
   - 等待所有已经提交的任务完成（包括正在执行的和已提交但尚未开始执行的任务）。
   - 平滑关闭线程池。
- **shutdownNow()**：
   - 停止接受新的任务。
   - 尝试中断所有正在执行的任务。
   - 返回尚未开始执行的任务列表。
   - 尽快关闭线程池。
