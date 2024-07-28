@ComponentScan是用于自动扫描指定的包及其子包中的组件类，并将它们注册为 Spring 容器管理的 Bean。这个过程被称为组件扫描（Component Scanning）。
### @ComponentScan注解
@ComponentScan注解通常与@Configuration注解一起使用，用于定义 Spring 应用的配置类。在这个配置类中，@ComponentScan指定了要扫描的包路径，Spring 会自动扫描这些包及其子包中的所有类，并将带有特定注解的类（如@Component、@Service、@Repository、@Controller等）注册为 Bean。
#### 基本用法
```
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
    // 配置类内容
}
```
在上面的例子中，Spring 会扫描com.example包及其子包中的所有类，并将带有@Component、@Service、@Repository和@Controller注解的类注册为 Spring Bean。
### 详细说明
#### 1.basePackages属性
basePackages属性用于指定要扫描的包。可以指定一个或多个包路径。
```
@ComponentScan(basePackages = {"com.example", "com.another"})
```
#### 2.basePackageClasses属性
basePackageClasses属性用于指定一个或多个类，Spring 会扫描这些类所在的包。
```
@ComponentScan(basePackageClasses = {MyClass1.class, MyClass2.class})
```
#### 3.includeFilters和excludeFilters属性
includeFilters和excludeFilters属性用于指定包含或排除的过滤器。可以根据注解、类型、正则表达式等进行过滤。
```
@ComponentScan(
    basePackages = "com.example",
    includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = CustomAnnotation.class),
    excludeFilters = @ComponentScan.Filter(type = FilterType.REGEX, pattern = "com\\.example\\.exclude\\..*")
)
```
#### 4.lazyInit属性
lazyInit属性用于指定是否延迟初始化扫描到的 Bean。默认值为false，表示立即初始化。
```
@ComponentScan(basePackages = "com.example", lazyInit = true)
```
### 示例
假设我们有以下包结构：
```
com.example
    ├── service
    │   └── MyService.java
    ├── repository
    │   └── MyRepository.java
    └── controller
        └── MyController.java
```
其中，MyService.java、MyRepository.java和MyController.java分别带有@Service、@Repository和@Controller注解。
#### 定义配置类
```
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
    // 配置类内容
}
```
#### 启动 Spring 容器
```
public class Application {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        
        MyService myService = context.getBean(MyService.class);
        MyRepository myRepository = context.getBean(MyRepository.class);
        MyController myController = context.getBean(MyController.class);
        
        // 使用 Bean
        context.close();
    }
}
```
Spring 会扫描com.example包及其子包中的所有类，并将带有@Service、@Repository和@Controller注解的类注册为 Spring Bean。
### 总结
@ComponentScan注解是 Spring 框架中用于自动扫描和注册组件类的关键注解。通过指定要扫描的包路径或类，Spring 可以自动发现并注册带有特定注解的类为 Spring Bean。
