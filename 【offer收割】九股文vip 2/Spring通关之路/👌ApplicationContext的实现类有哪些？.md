### 基于XML配置的实现类
1. **ClassPathXmlApplicationContext**
   - 从类路径下加载XML配置文件。
```
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```

1. **FileSystemXmlApplicationContext**
   - 从文件系统路径加载XML配置文件。
```
ApplicationContext context = new FileSystemXmlApplicationContext("C:/path/to/applicationContext.xml");
```
### 基于注解配置的实现类

1. **AnnotationConfigApplicationContext**
   - 从Java配置类（使用@Configuration注解的类）加载配置。
```
@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}

ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
```
### 基于Web应用的实现类

1. **XmlWebApplicationContext**
   - 专门为Web应用设计的ApplicationContext实现类，从Web应用的上下文中加载XML配置文件。
   - 通常在web.xml中配置：
```
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/applicationContext.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

1. **AnnotationConfigWebApplicationContext**
   - 专门为Web应用设计的ApplicationContext实现类，从Java配置类加载配置
```
public class WebAppInitializer implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext container) {
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.register(AppConfig.class);
        container.addListener(new ContextLoaderListener(context));
    }
}
```
### 基于Groovy配置的实现类

1. **GenericGroovyApplicationContext**
   - 从Groovy脚本配置文件加载配置。
```
ApplicationContext context = new GenericGroovyApplicationContext("applicationContext.groovy");
```
### 其他实现类

1. **GenericApplicationContext**
   - 一个通用的ApplicationContext实现类，通常与其他配置方式结合使用。
```
GenericApplicationContext context = new GenericApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("applicationContext.xml");
context.refresh();
```

1. **StaticApplicationContext**
   - 一个静态的ApplicationContext实现类，主要用于测试或简单的应用场景。
```
StaticApplicationContext context = new StaticApplicationContext();
context.registerSingleton("myBean", MyBean.class);
context.refresh();
```
# 
