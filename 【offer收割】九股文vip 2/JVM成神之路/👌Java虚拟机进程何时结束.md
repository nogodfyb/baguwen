Java 虚拟机进程的结束通常有以下几种情况：
### 1. 所有非守护线程（Non-Daemon Threads）结束
JVM 进程会在所有非守护线程结束后自动退出。非守护线程是默认的线程类型，通常用于执行主要任务。守护线程（Daemon Thread）则是辅助线程，通常用于执行后台任务，例如垃圾回收。

- **非守护线程**：主要任务线程，JVM 会等待其执行完毕。
- **守护线程**：辅助任务线程，JVM 不会等待其执行完毕。

当所有非守护线程都结束时，JVM 会自动退出，即使还有守护线程在运行。
### 2. 调用System.exit(int status)
可以通过调用System.exit(int status)方法来显式终止 JVM 进程。status参数是一个整数，通常用于表示退出状态码。
```
public class ExitExample {
    public static void main(String[] args) {
        System.out.println("Program is exiting");
        System.exit(0); // 正常退出
    }
}
```

- System.exit(0)：表示正常退出。
- 非零状态码：表示异常退出。
### 3. JVM 遇到未捕获的异常或错误
如果主线程或其他非守护线程中出现未捕获的异常或错误，且没有相应的异常处理机制，JVM 进程会终止。
```
public class UncaughtExceptionExample {
    public static void main(String[] args) {
        throw new RuntimeException("Uncaught exception");
    }
}
```
### 4. 通过外部命令强制终止
可以使用操作系统的命令或工具强制终止 JVM 进程，例如使用kill命令（在 Unix/Linux 系统上）或任务管理器（在 Windows 系统上）。
```
# 查找 JVM 进程 ID
ps -ef | grep java

# 强制终止 JVM 进程
kill -9 <pid>
```
### 5. 主线程结束且没有其他非守护线程
如果主线程结束且没有其他非守护线程在运行，JVM 进程也会结束。
```
public class MainThreadExample {
    public static void main(String[] args) {
        System.out.println("Main thread is ending");
    }
}
```
### 6. 调用Runtime.halt(int status)
Runtime.halt(int status)方法会立即终止 JVM 进程，不执行任何关闭钩子（Shutdown Hook）或finalize方法。
```
public class HaltExample {
    public static void main(String[] args) {
        Runtime.getRuntime().halt(0); // 立即终止 JVM
    }
}
```
### 7. 关闭钩子（Shutdown Hook）
在 JVM 进程结束前，可以注册关闭钩子来执行一些清理操作。关闭钩子是在 JVM 关闭前执行的线程。
```
public class ShutdownHookExample {
    public static void main(String[] args) {
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            System.out.println("Shutdown hook is running");
        }));

        System.out.println("Main thread is ending");
    }
}
```
### 总结
JVM 进程的结束通常是由于所有非守护线程结束、显式调用System.exit(int status)、未捕获的异常或错误、外部命令强制终止、主线程结束且没有其他非守护线程、调用Runtime.halt(int status)等原因。
