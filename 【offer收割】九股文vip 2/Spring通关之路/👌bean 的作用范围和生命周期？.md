Bean的作用范围（Scope）和生命周期（Lifecycle）决定了Bean的创建、使用和销毁方式。
### Bean的作用范围（Scope）

1. **singleton**
   - 默认作用范围。
   - 整个Spring容器中只有一个实例，所有对该Bean的引用都指向同一个实例。
   - 适用于无状态的Bean。
```
<bean id="myBean" class="com.example.MyBean" scope="singleton"/>
```

1. **prototype**
   - 每次请求该Bean时都会创建一个新的实例。
   - 适用于有状态的Bean。
```
<bean id="myBean" class="com.example.MyBean" scope="prototype"/>
```

1. **request**
   - 每次HTTP请求都会创建一个新的实例。
   - 仅适用于Web应用。
```
<bean id="myBean" class="com.example.MyBean" scope="request"/>
```

1. **session**
   - 每个HTTP会话都会创建一个新的实例。
   - 仅适用于Web应用。
```
<bean id="myBean" class="com.example.MyBean" scope="session"/>
```

1. **globalSession**
   - 每个全局HTTP会话都会创建一个新的实例。
   - 仅适用于Portlet应用。
```
<bean id="myBean" class="com.example.MyBean" scope="globalSession"/>
```

1. **application**
   - 每个ServletContext会创建一个新的实例。
   - 适用于Web应用。
```
<bean id="myBean" class="com.example.MyBean" scope="application"/>
```
### Bean的生命周期（Lifecycle

1. **实例化（Instantiation）**
   - Spring容器根据配置创建Bean实例。
2. **属性设置（Property Population）**
   - Spring容器进行依赖注入，设置Bean的属性。
3. **初始化（Initialization）**
   - 如果Bean实现了InitializingBean接口，Spring会调用其afterPropertiesSet()方法。
   - 如果在XML配置中指定了init-method属性，Spring会调用指定的初始化方法。
   - 如果Bean使用了@PostConstruct注解，Spring会调用标注的方法。
4. **使用（Usage）**
   - Bean处于就绪状态，可以被应用程序使用。
5. **销毁（Destruction）**
   - 如果Bean实现了DisposableBean接口，Spring会调用其destroy()方法。
   - 如果在XML配置中指定了destroy-method属性，Spring会调用指定的销毁方法。
   - 如果Bean使用了@PreDestroy注解，Spring会调用标注的方法。
### 生命周期回调接口和注解

1. **InitializingBean接口**
   - 方法：afterPropertiesSet()
```
public class MyBean implements InitializingBean {
    @Override
    public void afterPropertiesSet() throws Exception {
        // 初始化逻辑
    }
}
```

1. **DisposableBean接口**
   - 方法：destroy()
```
public class MyBean implements DisposableBean {
    @Override
    public void destroy() throws Exception {
        // 销毁逻辑
    }
}
```

1. **@PostConstruct注解**
   - 用于标注初始化方法。
```
public class MyBean {
    @PostConstruct
    public void init() {
        // 初始化逻辑
    }
}
```

1. **@PreDestroy注解**
   - 用于标注销毁方法。
```
public class MyBean {
    @PreDestroy
    public void cleanup() {
        // 销毁逻辑
    }
}
```
### 示例配
```
<bean id="myBean" class="com.example.MyBean" scope="singleton" init-method="customInit" destroy-method="customDestroy"/>
```
```
public class MyBean implements InitializingBean, DisposableBean {
    @Override
    public void afterPropertiesSet() throws Exception {
        // 初始化逻辑
    }

    @Override
    public void destroy() throws Exception {
        // 销毁逻辑
    }

    @PostConstruct
    public void init() {
        // 初始化逻辑
    }

    @PreDestroy
    public void cleanup() {
        // 销毁逻辑
    }

    public void customInit() {
        // 自定义初始化逻辑
    }

    public void customDestroy() {
        // 自定义销毁逻辑
    }
}
```
# 
