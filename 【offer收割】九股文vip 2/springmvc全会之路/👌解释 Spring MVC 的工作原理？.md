Spring MVC 的工作原理基于 Model-View-Controller（MVC）设计模式，旨在将应用程序的业务逻辑、用户界面和数据分离开来。
### 1. 用户请求
用户通过浏览器发送 HTTP 请求到服务器。例如，用户访问 `http://example.com/hello`。
### 2. 前端控制器（DispatcherServlet）
Spring MVC 的核心组件 `DispatcherServlet` 充当前端控制器，它拦截所有进入的 HTTP 请求。`DispatcherServlet` 在 `web.xml` 文件中配置，负责初始化 Spring MVC 的上下文环境。
```
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```
### 3. 处理器映射（Handler Mapping）
`DispatcherServlet` 接收到请求后，会根据请求 URL 通过处理器映射（Handler Mapping）找到相应的控制器（Controller）。处理器映射是由 `HandlerMapping` 接口实现的，常见的实现包括 `RequestMappingHandlerMapping`，它会扫描控制器中的 `@RequestMapping` 注解。
```
@Controller
public class HelloController {
    @RequestMapping("/hello")
    public ModelAndView helloWorld() {
        String message = "Hello, Spring MVC!";
        return new ModelAndView("hello", "message", message);
    }
}
```
### 4. 控制器处理
找到相应的控制器后，`DispatcherServlet` 调用控制器的方法处理请求。控制器执行业务逻辑，通常会调用服务层或数据访问层获取数据，并将数据封装到模型中。
```
@Controller
public class HelloController {
    @RequestMapping("/hello")
    public ModelAndView helloWorld() {
        String message = "Hello, Spring MVC!";
        return new ModelAndView("hello", "message", message);
    }
}
```
### 5. 视图解析器（View Resolver）
控制器处理完请求后，会返回一个 `ModelAndView` 对象，其中包含视图名称和模型数据。`DispatcherServlet` 使用视图解析器（View Resolver）将视图名称解析为实际的视图对象。常见的视图解析器包括 `InternalResourceViewResolver`、`ThymeleafViewResolver` 等。
```
@Bean
public InternalResourceViewResolver viewResolver() {
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/views/");
    resolver.setSuffix(".jsp");
    return resolver;
}
```
### 6. 视图渲染
视图解析器将视图名称解析为实际的视图对象后，视图对象负责将模型数据渲染为用户界面，通常是 HTML 页面。视图对象可以是 JSP、Thymeleaf 模板、FreeMarker 模板等。
```
<!-- hello.jsp -->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<body>
    <h2>${message}</h2>
</body>
</html>
```
### 7. 响应返回
渲染后的视图返回给 `DispatcherServlet`，`DispatcherServlet` 将最终的响应发送回用户浏览器。用户在浏览器中看到渲染后的页面。
### 总结

1. 用户发送请求到服务器。
2. `DispatcherServlet` 拦截请求。
3. `DispatcherServlet` 通过处理器映射找到相应的控制器。
4. 控制器处理请求，执行业务逻辑，并返回 `ModelAndView` 对象。
5. `DispatcherServlet` 使用视图解析器将视图名称解析为视图对象。
6. 视图对象渲染模型数据为用户界面。
7. `DispatcherServlet` 将响应返回给用户浏览器。

Spring MVC 实现了请求处理和视图渲染的分离，简化了 Web 应用程序的开发和维护。
