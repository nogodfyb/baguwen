主要目标是减少应用程序的启动时间、内存占用和打包大小。
### 1. 减少依赖

- **移除不必要的依赖**：检查`pom.xml`或`build.gradle`文件，移除那些不必要的依赖。
- **精简依赖**：使用`spring-boot-starter`提供的精简依赖，比如`spring-boot-starter-web`可能包含了很多你不需要的内容，可以考虑使用更具体的依赖。
### 2. 使用 ProGuard 或类似工具

- **ProGuard**：可以通过混淆和优化来减小 JAR 包的大小。
- **Maven ProGuard 插件**：
```
<build>
    <plugins>
        <plugin>
            <groupId>com.github.wvengen</groupId>
            <artifactId>proguard-maven-plugin</artifactId>
            <version>2.0.16</version>
            <executions>
                <execution>
                    <goals>
                        <goal>proguard</goal>
                    </goals>
                </execution>
            </executions>
            <configuration>
                <options>
                    <option>-dontwarn</option>
                    <option>-dontnote</option>
                    <option>-dontoptimize</option>
                    <option>-dontshrink</option>
                    <option>-dontobfuscate</option>
                </options>
            </configuration>
        </plugin>
    </plugins>
</build>
```
### 3. 使用 Spring Boot 2.x 提供的 Layered JAR

- **Layered JAR**：Spring Boot 2.3 引入了分层 JAR，可以将应用程序分成多个层，便于缓存和优化 Docker 镜像。
- **配置 Layered JAR**：
```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <layers>
                    <enabled>true</enabled>
                </layers>
            </configuration>
        </plugin>
    </plugins>
</build>
```
### 4. 禁用不必要的自动配置

- **排除自动配置**：通过`@SpringBootApplication`注解的`exclude`属性排除不需要的自动配置类。
```
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class, HibernateJpaAutoConfiguration.class})
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```
### 5. 使用 GraalVM Native Image

- **GraalVM Native Image**：可以将 Spring Boot 应用程序编译成原生二进制文件，显著减少内存占用和启动时间。
- **配置 GraalVM**：
```
mvn -Pnative native:compile
```
### 6. 优化 Spring Boot 配置

- **调整 JVM 参数**：根据应用程序的需求调整 JVM 参数，以优化内存使用和性能。
```
java -Xms256m -Xmx512m -jar myapp.jar
```

- **使用 Spring Boot 的资源优化配置**：
```
spring.main.lazy-initialization=true
```
### 7. 使用 Spring Boot 3.x 的 AOT 编译

- **AOT 编译**：Spring Boot 3.x 引入了 Ahead-of-Time (AOT) 编译，可以在构建时进行更多的优化。
- **配置 AOT 编译**：
```
mvn -Pnative native:compile
```
### 8. 使用 JLink 创建精简的 JRE

- **JLink**：可以创建一个包含应用程序所需模块的精简 JRE。
- **使用 JLink**：
```
jlink --module-path $JAVA_HOME/jmods --add-modules java.base,java.logging --output custom-jre
```
### 9. 使用 Docker 多阶段构建

- **多阶段构建**：减少 Docker 镜像的大小。
```
FROM maven:3.8.1-openjdk-11 AS build
COPY src /home/app/src
COPY pom.xml /home/app
RUN mvn -f /home/app/pom.xml clean package

FROM openjdk:11-jre-slim
COPY --from=build /home/app/target/myapp.jar /usr/local/lib/myapp.jar
ENTRYPOINT ["java","-jar","/usr/local/lib/myapp.jar"]
```
### 10. 使用 Spring Native

- **Spring Native**：提供了将 Spring 应用程序编译成原生镜像的支持。
- **配置 Spring Native**：
```
<dependency>
    <groupId>org.springframework.experimental</groupId>
    <artifactId>spring-native</artifactId>
    <version>0.11.0</version>
</dependency>
```
