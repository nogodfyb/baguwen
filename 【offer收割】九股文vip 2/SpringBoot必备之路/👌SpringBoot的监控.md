在 Spring Boot 应用程序中，监控是确保应用程序健康运行和性能优化的重要部分。Spring Boot 提供了多种监控和管理功能，其中最常用的是 Spring Boot Actuator 和集成外部监控工具（如 Prometheus 和 Grafana）。
### 方法 1：使用 Spring Boot Actuator
Spring Boot Actuator 提供了一组内置的端点，用于监控和管理应用程序。它可以帮助你查看应用程序的健康状况、度量指标、环境信息等。
#### 步骤 1：添加依赖
在你的`pom.xml`文件（如果你使用的是 Maven）或`build.gradle`文件（如果你使用的是 Gradle）中添加 Spring Boot Actuator 依赖。
##### Maven:
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```
##### Gradle:
```
implementation 'org.springframework.boot:spring-boot-starter-actuator'
```
#### 步骤 2：配置 Actuator 端点
你可以在`src/main/resources/application.properties`文件中配置 Actuator 端点。
```
# 启用所有 Actuator 端点
management.endpoints.web.exposure.include=*
```
#### 常用 Actuator 端点

- `/actuator/health`: 显示应用程序的健康状况。
- `/actuator/info`: 显示应用程序的基本信息。
- `/actuator/metrics`: 显示应用程序的度量指标。
- `/actuator/loggers`: 显示和配置日志记录级别。
- `/actuator/env`: 显示当前环境属性。
#### 示例：
启动应用程序后，你可以通过以下 URL 访问 Actuator 端点：
```
http://localhost:8080/actuator/health
http://localhost:8080/actuator/info
http://localhost:8080/actuator/metrics
```
### 方法 2：使用 Prometheus 和 Grafana 进行监控
Prometheus 是一个开源的系统监控和报警工具，Grafana 是一个开源的分析和监控平台。它们常常一起使用来监控 Spring Boot 应用程序。
#### 步骤 1：添加依赖
在你的`pom.xml`文件中添加 Micrometer Prometheus 依赖：
```
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```
#### 步骤 2：配置 Prometheus 端点
在`src/main/resources/application.properties`文件中配置 Prometheus 端点：
```
management.endpoints.web.exposure.include=prometheus
```
#### 步骤 3：配置 Prometheus
在 Prometheus 配置文件中添加你的 Spring Boot 应用程序作为一个抓取目标。
```
scrape_configs:
  - job_name: 'spring-boot'
    static_configs:
      - targets: ['localhost:8080']
```
#### 步骤 4：配置 Grafana
在 Grafana 中添加 Prometheus 数据源，并创建仪表盘来可视化你的应用程序指标。
### 完整示例

#### `pom.xml`
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo</name>
    <description>Demo project for Spring Boot</description>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.4</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <java.version>11</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>io.micrometer</groupId>
            <artifactId>micrometer-registry-prometheus</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```
#### `MyApplication.java`
```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```
#### `application.properties`
```
management.endpoints.web.exposure.include=*
management.endpoint.prometheus.enabled=true
```
