### 配置步骤
#### 1. 创建 GitHub 仓库
首先，在 GitHub 上创建一个新的仓库，用于存储配置文件。例如，可以创建一个名为 `config-repo` 的仓库。<br />在这个仓库中，创建不同的配置文件，例如 `application.yml`、`application-dev.yml`、`application-prod.yml` 等，用于不同环境的配置。
#### 2. 配置 Spring Cloud Config Server
创建一个新的 Spring Boot 应用程序，并添加 Spring Cloud Config Server 的依赖：
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```
在主应用类上添加 `@EnableConfigServer` 注解：
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
在 `application.yml` 文件中配置 GitHub 仓库的 URI 和分支：
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
其中：

- `uri`: 指定 GitHub 仓库的 URI。
- `searchPaths`: 指定配置文件所在的路径（可选）。
- `clone-on-start`: 在启动时克隆仓库（可选）。
- `username` 和 `password`: 用于访问私有仓库的 GitHub 凭据（可以使用 GitHub 个人访问令牌）。
#### 3. 配置 Spring Cloud Config Client
创建一个新的 Spring Boot 应用程序，并添加 Spring Cloud Config Client 的依赖：
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```
在 `bootstrap.yml` 文件中配置 Config Server 的 URI：
```
spring:
  application:
    name: my-app
  cloud:
    config:
      uri: http://localhost:8888
```
在 GitHub 仓库中，创建一个名为 `my-app.yml` 的配置文件：
```
my:
  property: value-from-config-server
```
在客户端应用程序中，可以通过 `@Value` 注解获取配置属性：
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
#### 4. 动态刷新配置
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
