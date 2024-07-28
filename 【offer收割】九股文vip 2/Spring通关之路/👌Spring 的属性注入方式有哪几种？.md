属性注入（Dependency Injection, DI）是将依赖对象注入到目标对象中的过程。Spring 支持多种属性注入方式，主要包括构造器注入、Setter 方法注入和字段注入。下面详细介绍这些注入方式。
### 1. 构造器注入
构造器注入通过类的构造器来注入依赖对象。这种方式在对象创建时就完成了依赖注入，确保了依赖对象的不可变性。
```
public class MyBean {
    private final Dependency dependency;

    public MyBean(Dependency dependency) {
        this.dependency = dependency;
    }
}
```
#### XML 配置
```
<bean id="myBean" class="com.example.MyBean">
    <constructor-arg ref="dependency"/>
</bean>
<bean id="dependency" class="com.example.Dependency"/>
```
#### 注解配置
```
@Component
public class MyBean {
    private final Dependency dependency;

    @Autowired
    public MyBean(Dependency dependency) {
        this.dependency = dependency;
    }
}
```
### 2. Setter 方法注入
Setter 方法注入通过类的 Setter 方法来注入依赖对象。这种方式允许在对象创建后进行依赖注入，并且可以在运行时更改依赖对象。
```
public class MyBean {
    private Dependency dependency;

    public void setDependency(Dependency dependency) {
        this.dependency = dependency;
    }
}
```
#### XML 配置
```
<bean id="myBean" class="com.example.MyBean">
    <property name="dependency" ref="dependency"/>
</bean>
<bean id="dependency" class="com.example.Dependency"/>
```
#### 注解配置
```
@Component
public class MyBean {
    private Dependency dependency;

    @Autowired
    public void setDependency(Dependency dependency) {
        this.dependency = dependency;
    }
}
```
### 3. 字段注入
字段注入（也称为属性注入）直接通过在类的字段上使用注解来注入依赖对象。这种方式不需要 Setter 方法，但不推荐使用，因为它违反了面向对象设计原则，使得依赖关系不够明确。
```
@Component
public class MyBean {
    @Autowired
    private Dependency dependency;
}
```
### 4. 通过@Value注解注入基本类型和字符串
除了注入 Bean 对象，Spring 还支持通过@Value注解注入基本类型、字符串和其他值，如从配置文件中读取的值。
```
@Component
public class MyBean {
    @Value("${my.property}")
    private String myProperty;
}
```
### 5. 通过环境变量或系统属性注入
Spring 允许通过环境变量或系统属性来注入值，这通常与@Value注解结合使用。
```
@Component
public class MyBean {
    @Value("#{systemProperties['user.name']}")
    private String userName;
}
```
### 6. 通过@Resource注解注入
@Resource通过名称或类型进行注入。
```
@Component
public class MyBean {
    @Resource(name = "dependency")
    private Dependency dependency;
}
```
### 7. 通过@Inject注解注入
@Inject它的使用方式类似于@Autowired。
```
@Component
public class MyBean {
    @Inject
    private Dependency dependency;
}
```
### 总结
Spring 的属性注入方式，包括构造器注入、Setter 方法注入、字段注入、通过@Value注解注入基本类型和字符串、通过环境变量或系统属性注入、通过@Resource注解注入以及通过@Inject注解注入。开发者可以根据具体需求选择合适的注入方式来实现依赖注入。构造器注入和 Setter 方法注入是最常用的方式，而字段注入虽然方便，但不推荐在生产代码中广泛使用。
