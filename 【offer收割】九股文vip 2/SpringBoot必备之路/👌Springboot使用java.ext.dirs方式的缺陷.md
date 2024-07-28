### 1. 已被弃用和移除
- **弃用和移除**：`java.ext.dirs`选项已经在 Java 9 中被弃用，并在后续版本中被移除。因此，依赖于`java.ext.dirs`的解决方案在现代 Java 版本中将无法工作。
- **兼容性问题**：如果你的应用程序依赖于`java.ext.dirs`，那么在升级到更高版本的 Java 时可能会遇到兼容性问题。
### 2. 安全性问题

- **全局类加载器**：使用`java.ext.dirs`方式会将 JAR 包添加到扩展类加载器中，这意味着这些 JAR 包将对所有应用程序可见。这会导致潜在的安全风险，因为不受信任的代码可能会被加载并执行。
- **类冲突**：在扩展目录中添加 JAR 包可能会与其他应用程序使用的 JAR 包发生冲突，从而导致类加载问题和难以调试的错误。
### 3. 难以管理和维护

- **全局配置**：`java.ext.dirs`是一个全局配置，影响所有运行在同一 JVM 上的应用程序。这使得管理和维护变得复杂，因为你需要确保所有应用程序都兼容这些扩展 JAR 包。
- **不可预测的行为**：由于扩展目录中的 JAR 包对所有应用程序可见，可能会导致不可预测的行为，特别是在不同应用程序之间存在依赖冲突的情况下。
### 4. 不符合现代开发实践

- **模块化系统**：现代 Java 版本（从 Java 9 开始）引入了模块化系统（Project Jigsaw），提供了更好的依赖管理和模块隔离机制。`java.ext.dirs`方式与这种模块化系统不兼容，不符合现代 Java 开发的最佳实践。
- **Spring Boot 的依赖管理**：Spring Boot 提供了强大的依赖管理机制，通过`pom.xml`或`build.gradle`文件来管理依赖。使用`java.ext.dirs`方式绕过了这种依赖管理机制，降低了应用程序的可维护性和可移植性。
### 5. Spring Boot 的替代方案

- **Spring Boot 的 ClassLoader**：Spring Boot 使用自定义的类加载器来加载嵌入式 JAR 包，这种方式更加灵活和可控。你可以通过配置 Spring Boot 的类加载器来实现类似的功能，而无需依赖`java.ext.dirs`。
- **外部配置**：Spring Boot 提供了多种方式来配置和加载外部 JAR 包，例如使用`spring-boot-loader`模块或通过配置文件来指定额外的类路径。
### 示例：使用 Spring Boot 的 ClassLoader
如果需要加载外部 JAR 包，可以在 Spring Boot 应用程序中使用自定义的类加载器。使用`URLClassLoader`来加载外部 JAR 包：
```
import java.net.URL;
import java.net.URLClassLoader;

public class CustomClassLoader {
    public static void main(String[] args) throws Exception {
        URL[] urls = {new URL("file:///path/to/external.jar")};
        URLClassLoader urlClassLoader = new URLClassLoader(urls, Thread.currentThread().getContextClassLoader());
        Thread.currentThread().setContextClassLoader(urlClassLoader);

        // 启动 Spring Boot 应用程序
        SpringApplication.run(MyApplication.class, args);
    }
}
```
