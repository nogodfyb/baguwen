依赖注入（Dependency Injection，简称DI）是Spring框架实现控制反转（IoC）的主要手段。DI的核心思想是将对象的依赖关系从对象内部抽离出来，通过外部注入的方式提供给对象。这样，依赖对象的创建和管理由Spring容器负责，而不是由对象自身负责，使得代码更加模块化、松耦合和易于测试。
### 依赖注入的基本概念
在传统编程中，一个对象通常会自己创建它所依赖的其他对象。这种方式使得代码紧密耦合，不利于维护和测试。依赖注入通过将依赖关系从代码中移除，转而由外部容器（如Spring容器）来注入，从而实现了对象之间的松耦合。
### 依赖注入的类型
Spring框架主要提供了三种依赖注入的方式：

1. **构造函数注入**：
   - 通过构造函数将依赖对象传递给被依赖对象。
   - 优点：依赖关系在对象创建时就被确定，适合依赖关系不可变的情况。
```
public class Service {
    private final Repository repository;

    public Service(Repository repository) {
        this.repository = repository;
    }
}
```

1. **Setter方法注入**：
   - 通过Setter方法将依赖对象注入到被依赖对象中。
   - 优点：依赖关系可以在对象创建后动态设置，适合依赖关系可变的情况。
```
public class Service {
    private Repository repository;

    public void setRepository(Repository repository) {
        this.repository = repository;
    }
}
```

1. **字段注入**：
   - 直接在字段上使用注解进行注入。
   - 优点：代码简洁，适合快速开发。
   - 缺点：不利于单元测试，因为无法通过构造函数或Setter方法来模拟依赖对象。
```
public class Service {
    @Autowired
    private Repository repository;
}
```
### 依赖注入的配置方式
Spring支持多种方式来配置依赖注入：

1. **XML配置**：
   - 通过XML文件定义Bean及其依赖关系。
```
<beans>
    <bean id="repository" class="com.example.Repository"/>
    <bean id="service" class="com.example.Service">
        <constructor-arg ref="repository"/>
    </bean>
</beans>
```

1. **Java配置**：
   - 通过Java类和注解定义Bean及其依赖关系。
```
@Configuration
public class AppConfig {
    @Bean
    public Repository repository() {
        return new Repository();
    }

    @Bean
    public Service service() {
        return new Service(repository());
    }
}
```

1. **注解配置**：
   - 通过注解（如@Component,@Autowired）自动扫描和注入Bean。
```
@Component
public class Repository {
}

@Component
public class Service {
    @Autowired
    private Repository repository;
}
```
### 总结
依赖注入（DI）是Spring框架实现控制反转（IoC）的核心机制。通过DI，Spring容器负责管理对象的创建和依赖关系的注入，使得代码更加模块化、松耦合和易于测试。Spring提供了多种DI方式，包括构造函数注入、Setter方法注入和字段注入，并支持XML配置、Java配置和注解配置等多种配置方式。
