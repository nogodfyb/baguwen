synchronized可以应用于实例方法、静态方法和代码块。
### 1. 同步实例方法
当synchronized用于实例方法时，锁定的是当前对象实例（this）。这意味着同一个对象实例的所有同步实例方法在同一时间只能被一个线程访问。
```
public class Example {
    public synchronized void instanceMethod() {
        // 代码块
    }
}
```

- **锁定范围**：当前对象实例。
- **影响**：同一个对象实例的所有同步实例方法在同一时间只能被一个线程访问，不同对象实例之间的同步方法不互相影响。
### 2. 同步静态方法
当synchronized用于静态方法时，锁定的是该类的Class对象。这意味着同一个类的所有同步静态方法在同一时间只能被一个线程访问。
```
public class Example {
    public static synchronized void staticMethod() {
        // 代码块
    }
}
```

- **锁定范围**：当前类的Class对象。
- **影响**：同一个类的所有同步静态方法在同一时间只能被一个线程访问，不同类之间的静态方法不互相影响。
### 3. 同步代码块
当synchronized用于代码块时，可以指定锁定的对象。这提供了更细粒度的控制，可以锁定任意对象。
```
public class Example {
    private final Object lock = new Object();

    public void method() {
        synchronized (lock) {
            // 代码块
        }
    }
}
```

- **锁定范围**：指定的锁对象。
- **影响**：只有在访问相同锁对象的同步代码块时，线程才会互相阻塞。可以更灵活地控制锁的粒度。
### 示例比较
```
public class Example {
    private static final Object classLock = new Object();
    private final Object instanceLock = new Object();

    public synchronized void synchronizedInstanceMethod() {
        // 锁定当前对象实例
    }

    public static synchronized void synchronizedStaticMethod() {
        // 锁定当前类的 Class 对象
    }

    public void methodWithSynchronizedBlock() {
        synchronized (instanceLock) {
            // 锁定 instanceLock 对象
        }

        synchronized (classLock) {
            // 锁定 classLock 对象
        }
    }
}
```
### 总结

- **同步实例方法**：锁定当前对象实例，确保同一时间只有一个线程可以访问该对象的同步实例方法。
- **同步静态方法**：锁定当前类的Class对象，确保同一时间只有一个线程可以访问该类的同步静态方法。
- **同步代码块**：锁定指定的对象，可以更灵活地控制锁的粒度，适用于需要更细粒度同步控制的场景。
