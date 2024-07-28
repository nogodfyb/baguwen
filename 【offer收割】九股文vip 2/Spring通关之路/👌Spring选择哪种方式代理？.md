Spring AOP 主要通过两种方式创建代理：JDK 动态代理和 CGLIB 代理。
### 1. JDK 动态代理

- **适用范围**：JDK 动态代理仅适用于实现了一个或多个接口的类。
- **实现原理**：JDK 动态代理使用java.lang.reflect.Proxy类和相关的InvocationHandler接口来创建代理对象。
- **特点**：代理对象是目标对象实现的接口类型的实例。
### 2. CGLIB 代理

- **适用范围**：CGLIB 代理适用于没有实现接口的类，或者需要代理类中的所有方法（包括那些没有在接口中定义的方法）。
- **实现原理**：CGLIB 代理使用字节码生成技术，在运行时生成目标类的子类，并在子类中拦截方法调用。
- **特点**：代理对象是目标类的子类。
### Spring 代理选择策略
Spring AOP 默认的代理选择策略如下：

1. **如果目标对象实现了至少一个接口**，Spring AOP 会优先选择使用**JDK 动态代理**。
2. **如果目标对象没有实现任何接口**，Spring AOP 会使用**CGLIB 代理**。
### 配置代理方式
在 Spring 配置中，可以通过@EnableAspectJAutoProxy注解的proxyTargetClass属性来强制使用 CGLIB 代理。
#### 使用 JDK 动态代理（默认行为）
如果目标对象实现了接口，Spring 默认会使用 JDK 动态代理。
```
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@EnableAspectJAutoProxy
public class AppConfig {
    // 配置 Bean
}
```
#### 强制使用 CGLIB 代理
无论目标对象是否实现了接口，都可以通过设置proxyTargetClass属性为true来强制使用 CGLIB 代理：
```
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class AppConfig {
    // 配置 Bean
}
```
### 总结
Spring AOP 可以根据目标对象的类型自动选择使用 JDK 动态代理或 CGLIB 代理。默认情况下，如果目标对象实现了接口，Spring 会使用 JDK 动态代理；否则，会使用 CGLIB 代理。通过配置@EnableAspectJAutoProxy注解的proxyTargetClass属性，可以强制使用 CGLIB 代理。
