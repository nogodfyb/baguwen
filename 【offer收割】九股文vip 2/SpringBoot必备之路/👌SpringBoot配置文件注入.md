在 Spring Boot 配置文件用于定义应用程序的各种配置参数。Spring Boot 支持多种配置文件格式，包括`application.properties`和`application.yml`。通过配置文件注入，可以方便地将配置参数注入到 Spring Bean 中，从而使应用程序更加灵活和易于管理。
### 1. 使用`@Value`注解注入配置
`@Value`注解可以直接将配置文件中的值注入到 Spring Bean 的字段中。
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

    public String getAppName() {
        return appName;
    }

    public String getAppVersion() {
        return appVersion;
    }
}
```
### 2. 使用`@ConfigurationProperties`注解注入配置
`@ConfigurationProperties`注解可以将配置文件中的属性映射到一个 Java Bean 中。通常与`@EnableConfigurationProperties`注解配合使用。
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

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getVersion() {
        return version;
    }

    public void setVersion(String version) {
        this.version = version;
    }
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
### 3. 使用`Environment`接口获取配置
Spring 的`Environment`接口可以用来访问配置文件中的属性。
```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.stereotype.Component;

@Component
public class AppConfig {

    @Autowired
    private Environment env;

    public String getAppName() {
        return env.getProperty("app.name");
    }

    public String getAppVersion() {
        return env.getProperty("app.version");
    }
}
```
### 4. 使用`@PropertySource`注解加载外部配置文件
如果需要加载外部的配置文件，可以使用`@PropertySource`注解。
```
external.property=value
```

```
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.beans.factory.annotation.Value;

@Configuration
@PropertySource("classpath:external.properties")
public class ExternalConfig {

    @Value("${external.property}")
    private String externalProperty;

    // Getter

    public String getExternalProperty() {
        return externalProperty;
    }
}
```
### 总结
Spring Boot 提供了多种方式来将配置文件中的属性注入到应用程序中，包括使用`@Value`注解、`@ConfigurationProperties`注解、`Environment`接口和`@PropertySource`注解等。此外，Spring Boot 还支持多环境配置，使得应用程序可以根据不同的环境加载不同的配置文件。通过这些机制，可以方便地管理和使用配置文件中的属性，从而提高应用程序的灵活性和可维护性。
