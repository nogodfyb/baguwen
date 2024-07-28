### 1. 继承Thread类
通过继承java.lang.Thread类并重写其run方法来创建线程。
#### 示例
```
public class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running");
    }

    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start();
    }
}
```
### 2. 实现Runnable接口
通过实现java.lang.Runnable接口并将其传递给Thread对象来创建线程。
#### 示例
```
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable is running");
    }

    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread thread = new Thread(myRunnable);
        thread.start();
    }
}
```
### 3. 实现Callable接口和使用FutureTask
通过实现java.util.concurrent.Callable接口来创建线程，并使用FutureTask来管理返回结果。
#### 示例
```
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class MyCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        return "Callable result";
    }

    public static void main(String[] args) {
        MyCallable myCallable = new MyCallable();
        FutureTask<String> futureTask = new FutureTask<>(myCallable);
        Thread thread = new Thread(futureTask);
        thread.start();

        try {
            System.out.println("Result: " + futureTask.get());
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```
### 4. 使用线程池
通过java.util.concurrent.ExecutorService创建和管理线程池，避免手动创建和管理线程。
#### 示例
```
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(3);

        for (int i = 0; i < 5; i++) {
            executorService.execute(() -> {
                System.out.println("Thread pool task is running");
            });
        }

        executorService.shutdown();
    }
}
```
### 5. 使用Lambda表达式 (Java 8及以上)
通过Lambda表达式简化Runnable接口的实现。
#### 示例
```
public class LambdaExample {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> System.out.println("Lambda thread is running"));
        thread.start();
    }
}
```
### 总结

- **继承Thread类**：适用于需要直接操作线程对象的场景，但由于Java不支持多继承，灵活性较低。
- **实现Runnable接口**：更灵活，适用于需要共享资源或任务的场景。
- **实现Callable接口和使用FutureTask**：适用于需要返回结果的并发任务。
- **使用线程池**：适用于需要管理大量线程的场景，提供了更高的性能和资源管理能力。
- **使用Lambda表达式**：简化代码，适用于Java 8及以上版本。
