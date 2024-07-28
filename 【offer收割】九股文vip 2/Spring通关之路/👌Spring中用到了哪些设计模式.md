### 1. 工厂模式（Factory Pattern）
Spring 使用工厂模式来创建对象实例。BeanFactory和ApplicationContext是 Spring 框架中实现工厂模式的核心接口。

- **应用场景**：Spring 容器负责创建和管理 bean 的实例。
```
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
MyBean myBean = (MyBean) context.getBean("myBean");
```
### 2. 单例模式（Singleton Pattern）
Spring 默认以单例模式管理 bean，这意味着每个 bean 在 Spring 容器中只有一个实例。

- **应用场景**：默认情况下，Spring 容器中的每个 bean 都是单例的。
```
@Component
public class MySingletonBean {
    // This bean will be a singleton
}
```
### 3. 代理模式（Proxy Pattern）
Spring AOP（面向切面编程）使用代理模式来创建代理对象，以实现方法拦截和增强功能。

- **应用场景**：AOP 实现事务管理、日志记录、安全检查等。
```
@Aspect
public class LoggingAspect {
    @Before("execution(* com.example.service.*.*(..))")
    public void logBeforeMethod(JoinPoint joinPoint) {
        System.out.println("Executing method: " + joinPoint.getSignature().getName());
    }
}
```
### 4. 模板方法模式（Template Method Pattern）
Spring 提供了多种模板类（如JdbcTemplate、RestTemplate），这些类封装了常见的操作步骤，允许用户只需实现特定的步骤。

- **应用场景**：简化数据库操作、RESTful 服务调用等。
```
@Autowired
private JdbcTemplate jdbcTemplate;

public void saveData(String data) {
    String sql = "INSERT INTO my_table (data) VALUES (?)";
    jdbcTemplate.update(sql, data);
}
```
### 5. 观察者模式（Observer Pattern）
Spring 的事件处理机制使用观察者模式。ApplicationEventPublisher和ApplicationListener是实现观察者模式的核心接口。

- **应用场景**：实现事件驱动的编程模型。
```
public class MyEvent extends ApplicationEvent {
    public MyEvent(Object source) {
        super(source);
    }
}

@Component
public class MyEventListener implements ApplicationListener<MyEvent> {
    @Override
    public void onApplicationEvent(MyEvent event) {
        System.out.println("Received event: " + event.getSource());
    }
}

@Component
public class MyEventPublisher {
    @Autowired
    private ApplicationEventPublisher applicationEventPublisher;

    public void publishEvent() {
        MyEvent event = new MyEvent(this);
        applicationEventPublisher.publishEvent(event);
    }
}
```
### 6. 依赖注入模式（Dependency Injection Pattern）
Spring 的核心功能之一就是依赖注入，通过构造函数注入、setter 注入或字段注入，将对象的依赖关系注入到对象中。

- **应用场景**：解耦对象之间的依赖关系，便于测试和维护。
```
@Component
public class MyService {
    private final MyRepository myRepository;

    @Autowired
    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }
}
```
### 7. 装饰器模式（Decorator Pattern）
Spring 使用装饰器模式来增强 bean 的功能，特别是在 AOP 中，通过将增强逻辑应用到目标对象上。

- **应用场景**：动态地为对象添加职责，而不影响其他对象。
```
@Aspect
public class PerformanceAspect {
    @Around("execution(* com.example.service.*.*(..))")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        System.out.println("Method executed in: " + executionTime + "ms");
        return result;
    }
}
```
### 8. 策略模式（Strategy Pattern）
Spring 中的TransactionManager使用策略模式来定义事务管理的策略，允许在运行时选择不同的事务管理器。

- **应用场景**：定义一系列算法，允许在运行时选择具体的算法。
```
@Configuration
public class AppConfig {
    @Bean
    public PlatformTransactionManager transactionManager() {
        return new DataSourceTransactionManager(dataSource());
    }
}
```
**单例模式**<br />Spring默认创建的Bean就是单例的。进一步的，依赖注入Bean实例默认是double check的单例模式。并且用ConcurrentHashMap保证线程安全。<br />**代理模式**<br />AOP底层就是动态代理模式的实现。分JDK动态代理和CGLIB两种默认使用jdk动态代理，如果开启了cglib则优先使用cglib。如果没有接口，jdk动态代理会失效。cglib则没有这个问题<br />**工厂方法**Spring中的BeanFactory就是工厂模式的体现，将对象的创建交给工厂来做。<br />原来依赖方和被依赖方耦合在一起，现在beanFactory将bean之间的耦合打开了。依赖方到工厂里拿依赖，不直接和被依赖方接触耦合了，<br />**模版模式**<br />模板方法模式是一种行为设计模式，模版模式类似继承，父类定义规则，子类具体实现<br />Spring 中 jdbcTemplate、hibernateTemplate 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。<br />**装饰者模式**<br />Spring 在配置 DataSource 的时候，DataSource 可能是不同的数据库和数据源。这个时候就要用到装饰者模式。Spring 中用到的包装器模式在类名上含有 Wrapper或者 Decorator。这些类基本上都是动态地给一个对象添加一些额外的职责。<br />**适配器模式**<br />实现方式：SpringMVC中的适配器HandlerAdatper。<br />实现原理：HandlerAdatper根据Handler规则执行不同的Handler。<br />实现过程：DispatcherServlet根据HandlerMapping返回的handler，向HandlerAdatper发起请求，处理Handler。HandlerAdapter根据规则找到对应的Handler并让其执行，执行完毕后Handler会向HandlerAdapter返回一个ModelAndView，最后由HandlerAdapter向DispatchServelet返回一个ModelAndView。<br />实现意义：HandlerAdatper使得Handler的扩展变得容易，只需要增加一个新的Handler和一个对应的HandlerAdapter即可。因此Spring定义了一个适配接口，使得每一种Controller有一种对应的适配器实现类，让适配器代替controller执行相应的方法。这样在扩展Controller时，只需要增加一个适配器类就完成了SpringMVC的扩展了。<br />**观察者模式**<br />观察者模式是一种对象行为型模式。a观察b的行为，b发生时a相应的做出反应、<br />Spring 事件驱动模型就是观察者模式很经典的一个应用。主要包括三个组成部分：事件，事件发布者，事件订阅者，当事件发生时，事件订阅者相应的做出改变。
