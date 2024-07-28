Spring MVC 的生命周期主要分为这几个阶段：
### 1. 初始化阶段
Spring 容器初始化，包括加载和解析配置文件、创建 Bean 对象等。对于 Spring MVC 应用，通常需要配置 `DispatcherServlet`，它是 Spring MVC 的核心组件，负责接收请求、处理请求和生成响应。
### 2. 处理阶段
当一个请求到达服务器时，`DispatcherServlet` 会根据请求的 URL 找到对应的处理器（Controller）。涉及：

- **URL匹配**：`DispatcherServlet` 会遍历所有的 `HandlerMapping`，找到能够处理当前请求的 `Handler`（即 Controller）。
- **解析请求参数**：如果请求中包含参数，Spring MVC 会自动将其绑定到 `@RequestParam` 或 `@PathVariable` 注解的方法参数中。
- **执行业务逻辑**：`DispatcherServlet` 会调用 Controller 的方法，执行相应的业务逻辑。
- **视图解析**：如果 Controller 方法返回一个视图名，`DispatcherServlet` 会根据视图名找到对应的视图解析器，并将模型数据传递给视图。
### 3. 渲染阶段
视图会根据模型数据生成最终的响应内容。Spring MVC 支持多种视图技术，包括 JSP、Thymeleaf、Freemarker 等。
### 4. 错误处理阶段
如果在处理阶段或渲染阶段发生了错误，Spring MVC 会跳转到错误处理阶段。在这个阶段，`DispatcherServlet` 会查找并调用 `@ExceptionHandler` 注解的方法来处理异常。
### 5. 销毁阶段
当应用程序关闭或者 Spring 容器被销毁时，所有的 Bean 对象也会被销毁。对于 Spring MVC 应用，这意味着 `DispatcherServlet` 和所有的 Controller 对象都会被销毁。

Spring MVC 的生命周期是一个从初始化到销毁的完整过程，涵盖了请求的接收、处理、渲染和错误处理等各个方面
