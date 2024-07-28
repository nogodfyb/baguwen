JDK 动态代理主要依赖于java.lang.reflect.Proxy类和java.lang.reflect.InvocationHandler接口来实现。
### 实现步骤

1. **定义接口**：定义需要代理的接口。
2. **实现接口**：创建接口的实现类。
3. **创建调用处理器**：实现InvocationHandler接口，并在invoke方法中定义代理逻辑。
4. **创建代理对象**：通过Proxy.newProxyInstance方法创建代理对象。
### 示例代码
假设我们有一个简单的服务接口MyService和它的实现类MyServiceImpl，我们将通过 JDK 动态代理为MyService创建一个代理对象，并在方法调用前后添加日志。
#### 1. 定义接口
```
public interface MyService {
    void performTask();
}
```
#### 2. 实现接口
```
public class MyServiceImpl implements MyService {
    @Override
    public void performTask() {
        System.out.println("Performing task");
    }
}
```
#### 3. 创建调用处理器
```
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class LoggingInvocationHandler implements InvocationHandler {
    private final Object target;

    public LoggingInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Logging before method execution: " + method.getName());
        Object result = method.invoke(target, args);
        System.out.println("Logging after method execution: " + method.getName());
        return result;
    }
}
```
#### 4. 创建代理对象并使用
```
import java.lang.reflect.Proxy;

public class MainApp {
    public static void main(String[] args) {
        // 创建目标对象
        MyService myService = new MyServiceImpl();

        // 创建调用处理器
        LoggingInvocationHandler handler = new LoggingInvocationHandler(myService);

        // 创建代理对象
        MyService proxyInstance = (MyService) Proxy.newProxyInstance(
                myService.getClass().getClassLoader(),
                myService.getClass().getInterfaces(),
                handler
        );

        // 调用代理对象的方法
        proxyInstance.performTask();
    }
}
```
### 详细解释

1. **接口定义和实现**：
   - MyService是一个简单的接口，定义了一个方法performTask。
   - MyServiceImpl是MyService的实现类，实现了performTask方法。
2. **调用处理器**：
   - LoggingInvocationHandler实现了InvocationHandler接口。它的invoke方法在代理对象的方法调用时被调用。
   - invoke方法接收三个参数：
      - proxy：代理对象。
      - method：被调用的方法。
      - args：方法参数。
   - 在invoke方法中，我们在方法调用前后添加了日志打印。
3. **创建代理对象**：
   - 使用Proxy.newProxyInstance方法创建代理对象。
   - newProxyInstance方法接收三个参数：
      - 类加载器：通常使用目标对象的类加载器。
      - 接口数组：目标对象实现的所有接口。
      - 调用处理器：实现了InvocationHandler接口的实例。
4. **使用代理对象**：
   - 通过代理对象调用方法时，实际调用的是LoggingInvocationHandler的invoke方法。
   - 在invoke方法中，首先打印日志，然后通过反射调用目标对象的方法，最后再打印日志。
### 总结
JDK 动态代理通过接口和反射机制实现代理功能，可以在运行时动态生成代理类，从而增强或修改现有类的行为。它适用于有接口的情况，通过实现InvocationHandler接口和使用Proxy.newProxyInstance方法，可以创建代理对象并在方法调用前后添加自定义逻辑。
