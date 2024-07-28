# 典型回答

在 Spring 应用中，根据运行时的配置（比如数据库配置、配置文件、配置中心等）动态生成 Spring Bean 是一种常见需求，特别是在面对多环境配置或者需要根据不同条件创建不同实例时。

Spring 提供了几种方式来实现这一需求。

### 基于条件的 Bean 注册

Spring提供了@Conditional（或者@CondationOnProperties）注解，允许你在满足特定条件时才创建Bean。你可以定义自己的条件，这些条件实现了Condition接口。

```java
public class MyCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        return context.getEnvironment().getProperty("my.condition.enabled", Boolean.class, false);
    }
}

@Configuration
public class MyConfiguration {
    @Bean
    @Conditional(MyCondition.class)
    public MyBean myBean() {
        return new MyBean();
    }
}

```

`my.condition.enabled`可以通过配置文件或者配置中心进行配置，然后当`my.condition.enabled`属性为true时，MyBean才会被创建。

### 使用@ConfigurationProperties

`@ConfigurationProperties`注解可以将配置文件中的属性绑定到Bean的属性上，然后就可以基于他做动态配置了。

```java
@ConfigurationProperties(prefix = "app.datasource")
public class DataSourceProperties {
    private String url;
    private String username;
    private String password;
    // 省略getters和setters
}

@Configuration
@EnableConfigurationProperties(DataSourceProperties.class)
public class DataSourceConfig {
    @Autowired
    private DataSourceProperties properties;

    @Bean
    public DataSource dataSource() {
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setUrl(properties.getUrl());
        dataSource.setUsername(properties.getUsername());
        dataSource.setPassword(properties.getPassword());
        return dataSource;
    }
}
```

### 编程式Bean注册
我们还可以通过实现BeanDefinitionRegistryPostProcessor接口来编程式地注册Bean。

```java
public class DynamicBeanRegistrar implements BeanDefinitionRegistryPostProcessor {
    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException {
        if (满足某个条件) {
            GenericBeanDefinition beanDefinition = new GenericBeanDefinition();
            beanDefinition.setBeanClass(MyDynamicBean.class);
            registry.registerBeanDefinition("myDynamicBean", beanDefinition);
        }
    }

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        // 通常不需要在这个方法中实现任何逻辑
    }
}

```

### 使用@Profile

**@**Profile注解允许根据活动的Spring Profiles来创建Bean。**一般是用于区分开发、测试和生产环境的。**

```java
@Configuration
public class ProfileConfig {
    @Bean
    @Profile("dev")
    public DataSource devDataSource() {
        return new EmbeddedDatabaseBuilder().setType(EmbeddedDatabaseType.H2).build();
    }

    @Bean
    @Profile("prod")
    public DataSource prodDataSource() {
        // 返回生产环境的DataSource实例
    }
}
```

