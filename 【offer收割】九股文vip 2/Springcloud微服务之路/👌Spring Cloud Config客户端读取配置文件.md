Spring Cloud Config 客户端读取配置文件的过程涉及到从 Spring Cloud Config Server 获取配置，并将这些配置应用到客户端应用程序中。
### 配置步骤
#### 1. 创建 Spring Cloud Config Server
假设你已经有一个 Spring Cloud Config Server，它从 GitHub 仓库中读取配置文件。配置步骤如下：

1. 创建一个新的 Spring Boot 应用程序，并添加 Spring Cloud Config Server 的依赖：
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

2. 在主应用类上添加 `@EnableConfigServer` 注解：
```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

3. 在 `application.yml` 文件中配置 GitHub 仓库的 URI 和分支：
```
server:
  port: 8888

spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/your-username/config-repo
          searchPaths: config
          clone-on-start: true
          username: your-github-username
          password: your-github-token
```
#### 2. 创建 Spring Cloud Config 客户端

1. 创建一个新的 Spring Boot 应用程序，并添加 Spring Cloud Config Client 的依赖：
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

2. 在 `bootstrap.yml` 文件中配置 Config Server 的 URI 和应用名称：
```
spring:
  application:
    name: my-app
  cloud:
    config:
      uri: http://localhost:8888
```

   - `spring.application.name`：指定应用程序的名称，这个名称将用于从 Config Server 获取对应的配置文件。
   - `spring.cloud.config.uri`：指定 Config Server 的 URI。
3. 在 GitHub 仓库中，创建一个名为 `my-app.yml` 的配置文件：
```
my:
  property: value-from-config-server
```

4. 在客户端应用程序中，可以通过 `@Value` 注解获取配置属性：
```
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @Value("${my.property}")
    private String myProperty;

    @GetMapping("/property")
    public String getProperty() {
        return myProperty;
    }
}
```
#### 3. 动态刷新配置
为了支持动态刷新配置，可以添加 Spring Boot Actuator 和 Spring Cloud Bus 的依赖：
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```
在 `application.yml` 文件中启用 Actuator 的 `/refresh` 端点：
```
management:
  endpoints:
    web:
      exposure:
        include: refresh
```
在需要动态刷新的类上添加 `@RefreshScope` 注解：
```
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RefreshScope
@RestController
public class MyController {

    @Value("${my.property}")
    private String myProperty;

    @GetMapping("/property")
    public String getProperty() {
        return myProperty;
    }
}
```
