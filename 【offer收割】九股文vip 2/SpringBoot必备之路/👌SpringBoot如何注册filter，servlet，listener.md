在 Spring Boot 中，注册 Filter、Servlet 和 Listener 有多种方式。你可以通过代码配置、注解配置或在`application.properties`文件中进行配置。
### 1. 使用`@ServletComponentScan`和注解配置
你可以使用`@WebFilter`、`@WebServlet`和`@WebListener`注解来注册 Filter、Servlet 和 Listener。这种方式不需要额外的配置类。
#### 示例：
```
import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

@WebFilter(urlPatterns = "/*")
public class MyFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // 初始化代码
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        // 过滤器逻辑
        System.out.println("Filter is called");
        chain.doFilter(request, response);
    }

    @Override
    public void destroy() {
        // 销毁代码
    }
}
```

```
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/myServlet")
public class MyServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("Hello from MyServlet");
    }
}
```

```
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class MyListener implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("Context Initialized");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("Context Destroyed");
    }
}
```
**在主应用类中添加**`**@ServletComponentScan**`**注解：**
```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletComponentScan;

@SpringBootApplication
@ServletComponentScan
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```
### 2. 使用`@Bean`配置
也可以在配置类中通过`@Bean`注解来注册 Filter、Servlet 和 Listener。
#### 示例：
```
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyFilterConfig {

    @Bean
    public FilterRegistrationBean<MyFilter> myFilter() {
        FilterRegistrationBean<MyFilter> registrationBean = new FilterRegistrationBean<>();
        registrationBean.setFilter(new MyFilter());
        registrationBean.addUrlPatterns("/*");
        return registrationBean;
    }
}
```

```
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyServletConfig {

    @Bean
    public ServletRegistrationBean<MyServlet> myServlet() {
        ServletRegistrationBean<MyServlet> registrationBean = new ServletRegistrationBean<>(new MyServlet(), "/myServlet");
        return registrationBean;
    }
}
```

```
import org.springframework.boot.web.servlet.ServletListenerRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyListenerConfig {

    @Bean
    public ServletListenerRegistrationBean<MyListener> myListener() {
        return new ServletListenerRegistrationBean<>(new MyListener());
    }
}
```
### 3. 使用`application.properties`配置
对于某些简单的配置，可以在`application.properties`文件中进行配置。
#### 示例：
```
# 配置 Filter
spring.servlet.filter-order=1
spring.servlet.filter-url-patterns=/*

# 配置 Servlet
spring.servlet.servlet-name=myServlet
spring.servlet.servlet-url-patterns=/myServlet

# 配置 Listener
spring.listener.listener-class=com.example.MyListener
```
