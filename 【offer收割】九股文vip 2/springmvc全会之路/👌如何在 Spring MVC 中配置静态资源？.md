在 Spring MVC 中，配置静态资源可以让应用程序直接提供静态文件（如 HTML、CSS、JavaScript、图片等），而不必通过控制器来处理这些请求。
### 1. 将静态资源放置在正确的位置
首先，需要将静态资源文件放置在 Spring MVC 可以访问的目录下。通常，这些文件会被放置在 `src/main/resources/static` 或 `src/main/webapp` 目录中。
### 2. 在 Spring MVC 配置中启用静态资源处理
在 Spring MVC 的配置文件中（例如 `spring-mvc.xml`），添加以下配置来启用静态资源处理：
```
<mvc:resources mapping="/resources/**" location="/resources/" />
```
这段代码告诉 Spring MVC，将所有以 `/resources/` 开头的请求映射到 `src/main/resources/static` 目录下的文件。<br />如果你使用 Spring Boot，可以在 `application.properties` 或 `application.yml` 文件中添加以下配置：
```
spring.mvc.static-path-pattern=/resources/**
```
这将启用同样的静态资源处理功能。
### 3. 访问静态资源
现在可以通过在浏览器中输入 `http://localhost:8080/resources/your-static-file.html` 来访问静态资源文件。其中，`your-static-file.html` 是你放置在 `src/main/resources/static` 目录下的实际文件名。
### 4. 自定义静态资源路径
如果你想使用自定义的路径来访问静态资源，可以在配置中修改 `mapping` 和 `location` 属性。例如：
```
<mvc:resources mapping="/my-resources/**" location="/my-resources/" />
```
这将使得所有以 `/my-resources/` 开头的请求都被映射到 `src/main/resources/static/my-resources` 目录下的文件。






