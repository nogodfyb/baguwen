Spring Boot 中的日志系统是基于 SLF4J（Simple Logging Facade for Java）和 Logback 的组合。SLF4J 提供了一个简单的日志记录 API，而 Logback 是一个强大的日志实现。
### 日志框架关系

1. **SLF4J**：一个日志记录抽象层，允许你在代码中使用统一的日志 API，而不依赖于具体的日志实现。它可以与多种日志实现（如 Logback、Log4j、Java Util Logging 等）配合使用。
2. **Logback**：Spring Boot 默认使用的日志实现。它是由 Log4j 的作者开发的，具有更好的性能和更灵活的配置。
### 默认日志配置
Spring Boot 默认使用 Logback 作为日志实现，并且提供了一个默认的配置文件`logback-spring.xml`。如果你没有提供自定义的配置文件，Spring Boot 会使用默认的配置。<br />默认情况下，Spring Boot 的日志配置如下：

- 日志输出到控制台。
- 日志级别为`INFO`。
- 可以通过`application.properties`文件进行简单的日志配置。
### 日志配置
你可以通过`application.properties`或`application.yml`文件来配置日志级别和日志输出格式。
#### 在`application.properties`中配置日志
```
# 设置根日志级别为 DEBUG
logging.level.root=DEBUG

# 设置特定包的日志级别为 INFO
logging.level.com.example.myapp=INFO

# 设置日志输出到文件
logging.file.name=logs/myapp.log

# 设置日志文件的最大大小和滚动策略
logging.file.max-size=10MB
logging.file.max-history=30

# 设置日志模式（console/file）
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} - %msg%n
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss} - %msg%n
```
### 自定义 Logback 配置
如果你需要更复杂的日志配置，可以创建一个`logback-spring.xml`文件，并将其放在`src/main/resources`目录下。Spring Boot 会自动加载该配置文件。
#### 示例`logback-spring.xml`
```
<configuration>
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <include resource="org/springframework/boot/logging/logback/console-appender.xml"/>

    <property name="LOG_FILE" value="logs/myapp.log"/>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_FILE}</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logs/myapp-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>10MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="DEBUG">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```
### 日志级别
Spring Boot 支持以下日志级别，按优先级从低到高排列：

1. `TRACE`
2. `DEBUG`
3. `INFO`
4. `WARN`
5. `ERROR`
6. `FATAL`（Logback 不支持）
7. `OFF`

你可以在`application.properties`或`application.yml`文件中设置日志级别。
### 使用 SLF4J 记录日志
在 Spring Boot 应用程序中，你可以使用 SLF4J 记录日志。
```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyApplication {

    private static final Logger logger = LoggerFactory.getLogger(MyApplication.class);

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);

        logger.debug("This is a DEBUG message");
        logger.info("This is an INFO message");
        logger.warn("This is a WARN message");
        logger.error("This is an ERROR message");
    }
}
```
### 结论
Spring Boot 的日志系统基于 SLF4J 和 Logback，提供了灵活且强大的日志功能。你可以通过`application.properties`或`application.yml`文件进行简单配置，或者通过`logback-spring.xml`文件进行复杂配置。
