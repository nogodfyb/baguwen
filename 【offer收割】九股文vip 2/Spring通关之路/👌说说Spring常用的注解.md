### 1. 核心注解
#### @Component

- **用途**：将一个类标记为 Spring 组件类，表示这个类的实例将由 Spring 容器管理。
#### @Autowired

- **用途**：自动注入依赖对象。可以用于构造器、字段、Setter 方法或其他任意方法。
#### @Qualifier

- **用途**：在有多个候选 Bean 时，指定要注入的 Bean。通常与@Autowired一起使用。
```
@Component
public class MyComponent {
    @Autowired
    @Qualifier("specificBean")
    private Dependency dependency;
}
```
#### @Value

- **用途**：注入外部化的值，例如配置文件中的属性值、系统属性或环境变量。
```
@Component
public class MyComponent {
    @Value("${my.property}")
    private String myProperty;
}
```
#### @Configuration

- **用途**：标记一个类为 Spring 配置类，该类可以包含一个或多个@Bean方法。
```
@Configuration
public class AppConfig {
    @Bean
    public MyComponent myComponent() {
        return new MyComponent();
    }
}
```
#### @Bean

- **用途**：定义一个方法，返回一个要注册为 Spring 容器管理的 Bean。
```
@Bean
public MyComponent myComponent() {
    return new MyComponent();
}
```
#### @ComponentScan

- **用途**：指定要扫描的包，以查找带有@Component、@Service、@Repository和@Controller注解的类，并将它们注册为 Spring Bean。
```
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
    // 配置类内容
}
```
#### @Primary

- **用途**：标记一个 Bean 为首选 Bean，当有多个候选 Bean 时，Spring 会优先注入带有@Primary注解的 Bean。
```
@Component
@Primary
public class PrimaryBean implements Dependency {
    // 实现
}
```
#### @Scope

- **用途**：指定 Bean 的作用域，例如单例（singleton）、原型（prototype）等。
```
@Component
@Scope("prototype")
public class PrototypeBean {
    // 实现
}
```
### 2. 特定用途注解
#### @Service

- **用途**：标记一个类为服务层组件，实际上是@Component的特殊化。
```
@Service
public class MyService {
    // 服务逻辑
}
```
#### @Repository

- **用途**：标记一个类为数据访问层组件，实际上是@Component的特殊化，并且支持持久化异常转换。
```
@Repository
public class MyRepository {
    // 数据访问逻辑
}
```
#### @Controller

- **用途**：标记一个类为 Spring MVC 控制器，实际上是@Component的特殊化
```
@Controller
public class MyController {
    // 控制器逻辑
}
```
#### @RestController

- **用途**：标记一个类为 RESTful Web 服务的控制器，等同于@Controller和@ResponseBody的组合。
```
@RestController
public class MyRestController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```
### 3. AOP（面向切面编程）相关注解
#### @Aspect

- **用途**：标记一个类为切面类。
```
@Aspect
@Component
public class MyAspect {
    // 切面逻辑
}
```
#### @Before

- **用途**：定义一个方法，在目标方法执行之前执行。
```
@Aspect
@Component
public class MyAspect {
    @Before("execution(* com.example.service.*.*(..))")
    public void beforeMethod(JoinPoint joinPoint) {
        // 前置逻辑
    }
}
```
#### @After

- **用途**：定义一个方法，在目标方法执行之后执行。
```
@Aspect
@Component
public class MyAspect {
    @After("execution(* com.example.service.*.*(..))")
    public void afterMethod(JoinPoint joinPoint) {
        // 后置逻辑
    }
}
```
#### @Around

- **用途**：定义一个方法，包围目标方法的执行
```
@Aspect
@Component
public class MyAspect {
    @Around("execution(* com.example.service.*.*(..))")
    public Object aroundMethod(ProceedingJoinPoint joinPoint) throws Throwable {
        // 环绕逻辑
        return joinPoint.proceed();
    }
}
```
### 4. 事务管理相关注解
#### @Transactional

- **用途**：标记一个方法或类，表示该方法或类中的所有方法都需要事务管理。
```
@Service
public class MyService {
    @Transactional
    public void performTransaction() {
        // 事务逻辑
    }
}
```
### 总结
Spring 提供了丰富的注解来简化配置和依赖注入过程。常用的注解包括@Component、@Autowired、@Qualifier、@Value、@Configuration、@Bean、@ComponentScan、@Primary、@Scope以及特定用途的注解如@Service、@Repository、@Controller、@RestController，还有 AOP 相关的@Aspect、@Before、@After、@Around和事务管理相关的@Transactional。
