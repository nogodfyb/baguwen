`@Value`和`@ConfigurationProperties`是 Spring Boot 中两种常用的注入配置属性的方法。它们各自有不同的使用场景和优缺点。
### 1.`@Value`注解
#### 优点

1. **简单直接**：`@Value`注解非常简单，适合注入单个属性。
2. **灵活性高**：可以直接在字段、方法参数或构造函数参数上使用。
3. **支持 SpEL**：`@Value`注解支持 Spring 表达式语言（SpEL），可以进行复杂的表达式计算。
#### 缺点

1. **不支持批量注入**：对于一组相关的配置属性，需要逐个使用`@Value`注解，代码会显得冗长。
2. **类型安全性差**：`@Value`注解注入的属性是字符串，需要手动进行类型转换，容易出错。
#### 示例
```
app.name=MyApp
app.version=1.0.0
```
```
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class AppConfig {

    @Value("${app.name}")
    private String appName;

    @Value("${app.version}")
    private String appVersion;

    // Getters and setters
}
```
### 2.`@ConfigurationProperties`注解
#### 优点

1. **支持批量注入**：可以将一组相关的配置属性映射到一个 Java Bean 中，代码更加整洁。
2. **类型安全**：`@ConfigurationProperties`注解会自动进行类型转换，提供更好的类型安全性。
3. **易于测试**：配置类是普通的 POJO，更加容易进行单元测试。
4. **支持嵌套属性**：可以将复杂的配置结构映射到嵌套的 Java Bean 中。
#### 缺点

1. **需要更多的配置**：相比`@Value`注解，`@ConfigurationProperties`需要更多的配置和初始化步骤。
2. **不支持 SpEL**：不能直接使用 Spring 表达式语言进行复杂的表达式计算。
#### 示例
```
app:
  name: MyApp
  version: 1.0.0
```
```
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "app")
public class AppProperties {

    private String name;
    private String version;

    // Getters and setters
}
```
```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;

@SpringBootApplication
@EnableConfigurationProperties(AppProperties.class)
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```
### 3. 选择指南
#### 适用场景

- **使用**`**@Value**`：
   - 需要注入单个或少量的配置属性。
   - 需要使用 Spring 表达式语言进行复杂的计算或操作。
   - 快速简单的配置注入。
- **使用**`**@ConfigurationProperties**`：
   - 需要注入一组相关的配置属性。
   - 需要类型安全性和自动类型转换。
   - 需要使用嵌套属性和复杂的配置结构。
   - 需要更好的可测试性。
#### 性能考虑
在大多数情况下，性能差异是微不足道的，选择应主要基于代码的可维护性和清晰度。
### 总结
`@Value`和`@ConfigurationProperties`各有优缺点，适用于不同的场景。`@Value`注解简单直接，适合注入单个属性，但缺乏类型安全性和批量注入的能力。而`@ConfigurationProperties`注解支持批量注入、类型安全和嵌套属性，适合处理复杂的配置结构。根据具体需求选择合适的注入方式，可以提高代码的可维护性和可读性。
