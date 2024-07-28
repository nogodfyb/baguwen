### 1. 通过构造器实例化
这是最常见的方式之一，Spring 可以通过调用类的构造器来实例化 Bean。构造器可以是无参构造器，也可以是有参构造器。
#### 无参构造器
```
public class MyBean {
    public MyBean() {
        // 无参构造器
    }
}
```
配置方式：
```
<bean id="myBean" class="com.example.MyBean"/>
```
#### 有参构造器
```
public class MyBean {
    private String name;

    public MyBean(String name) {
        this.name = name;
    }
}
```
配置方式：
```
<bean id="myBean" class="com.example.MyBean">
    <constructor-arg value="exampleName"/>
</bean>
```
### 2. 通过静态工厂方法实例化
Spring 可以通过调用静态工厂方法来实例化 Bean。这种方式适用于需要复杂初始化逻辑的情况。
```
public class MyBeanFactory {
    public static MyBean createInstance(String name) {
        return new MyBean(name);
    }
}
```
配置方式：
```
<bean id="myBean" class="com.example.MyBeanFactory" factory-method="createInstance">
    <constructor-arg value="exampleName"/>
</bean>
```
### 3. 通过实例工厂方法实例化
这种方式类似于静态工厂方法，不同之处在于实例工厂方法需要先实例化工厂类，然后调用工厂类的实例方法来创建 Bean。
```
public class MyBeanFactory {
    public MyBean createInstance(String name) {
        return new MyBean(name);
    }
}
```
配置方式：
```
<bean id="myBeanFactory" class="com.example.MyBeanFactory"/>
<bean id="myBean" factory-bean="myBeanFactory" factory-method="createInstance">
    <constructor-arg value="exampleName"/>
</bean>
```
### 4. 通过 FactoryBean 接口实例化
Spring 提供了一个FactoryBean接口，允许开发者定制 Bean 的创建逻辑。实现FactoryBean接口的类可以被用作工厂来生成其他 Bean。
```
public class MyFactoryBean implements FactoryBean<MyBean> {
    private String name;

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public MyBean getObject() throws Exception {
        return new MyBean(name);
    }

    @Override
    public Class<?> getObjectType() {
        return MyBean.class;
    }

    @Override
    public boolean isSingleton() {
        return true;
    }
}
```
配置方式：
```
<bean id="myFactoryBean" class="com.example.MyFactoryBean">
    <property name="name" value="exampleName"/>
</bean>
<bean id="myBean" factory-bean="myFactoryBean" factory-method="getObject"/>
```
### 5. 通过 @Bean 注解实例化
使用@Bean注解的方法可以用来实例化和配置 Bean。这种方式更加直观和灵活，适合基于 Java 配置的 Spring 应用。
```
@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean("exampleName");
    }
}
```
### 6. 通过 @Component 注解实例化
使用@Component注解可以将类标记为 Spring 管理的 Bean。结合@ComponentScan注解，Spring 会自动扫描并实例化这些类。
```
@Component
public class MyBean {
    // Bean 定义
}
```
配置方式：
```
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
    // 配置类
}
```

