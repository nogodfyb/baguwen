配置 `DispatcherServlet` 通常有两种方式：基于 XML 配置和基于 Java 配置（也称为 Java Config 或 Java-based Configuration）。
### 基于 XML 配置

1. **web.xml 配置**

在传统的 Spring MVC 应用中，`DispatcherServlet` 通常是在 `web.xml` 文件中配置的。
```
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
                             http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" 
         version="3.1">
    
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
</web-app>
```

- `<servlet>` 元素定义了一个名为 `dispatcher` 的 `DispatcherServlet` 实例。
- `<load-on-startup>` 元素指定了 `DispatcherServlet` 应该在应用启动时加载。
- `<servlet-mapping>` 元素将所有请求（`/`）映射到 `DispatcherServlet`。
1. **Spring 配置文件（如 spring-servlet.xml）**

`DispatcherServlet` 会加载一个 Spring 配置文件，该文件的名称通常是 `[servlet-name]-servlet.xml`，例如 `dispatcher-servlet.xml`。这个文件包含 Spring MVC 的具体配置，如视图解析器、控制器扫描等：
```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- 启用注解驱动的控制器 -->
    <context:component-scan base-package="com.example" />
    <mvc:annotation-driven />
    
    <!-- 配置视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/" />
        <property name="suffix" value=".jsp" />
    </bean>
    
</beans>
```
### 基于 Java 配置
Spring 提供了基于 Java 配置的方式来配置 `DispatcherServlet`，这通常是在不使用 `web.xml` 的情况下进行的（例如，使用 Spring Boot 或者 Spring 的 Java Config）。

1. **Web 应用初始化类**

创建一个类来替代 `web.xml`，实现 `WebApplicationInitializer` 接口：
```
import org.springframework.web.WebApplicationInitializer;
import org.springframework.web.context.support.AnnotationConfigWebApplicationContext;
import org.springframework.web.servlet.DispatcherServlet;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletRegistration;

public class MyWebAppInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.register(WebConfig.class);
        
        DispatcherServlet dispatcherServlet = new DispatcherServlet(context);
        ServletRegistration.Dynamic registration = servletContext.addServlet("dispatcher", dispatcherServlet);
        registration.setLoadOnStartup(1);
        registration.addMapping("/");
    }
}
```

1. **Spring 配置类**

创建一个 Java 配置类来替代 XML 配置文件：
```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.example")
public class WebConfig implements WebMvcConfigurer {

    @Bean
    public InternalResourceViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }
}
```
### 总结

- **基于 XML 配置**：使用 `web.xml` 配置 `DispatcherServlet`，并在 Spring 配置文件（如 `dispatcher-servlet.xml`）中进行具体的 Spring MVC 配置。
- **基于 Java 配置**：使用 `WebApplicationInitializer` 接口和 Java 配置类（如 `WebConfig`）来配置 `DispatcherServlet` 和 Spring MVC。
