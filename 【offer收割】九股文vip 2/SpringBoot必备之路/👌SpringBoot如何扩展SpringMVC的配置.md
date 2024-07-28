包括使用配置类、实现特定接口、或通过注解来进行配置。
### 方式一：使用`@Configuration`类配置
你可以通过创建一个带有`@Configuration`注解的类并继承`WebMvcConfigurer`接口来扩展 Spring MVC 的配置。`WebMvcConfigurer`接口包含许多方法，可以用来定制 Spring MVC 的行为。
#### 示例：
```
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;

@Configuration
public class MyWebMvcConfig implements WebMvcConfigurer {

    // 配置跨域请求
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://example.com")
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("*")
                .allowCredentials(true)
                .maxAge(3600);
    }

    // 配置静态资源处理
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**")
                .addResourceLocations("classpath:/static/");
    }

    // 配置视图控制器
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/login").setViewName("login");
    }

    // 配置拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyCustomInterceptor())
                .addPathPatterns("/**")
                .excludePathPatterns("/login", "/static/**");
    }
}
```
### 方式二：实现特定接口
除了`WebMvcConfigurer`接口，Spring MVC 还提供了其他一些接口和类，可以用来定制特定的功能。可以实现`HandlerInterceptor`接口来自定义拦截器。
#### 示例：自定义拦截器
```
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class MyCustomInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 在请求处理之前执行的逻辑
        System.out.println("Pre Handle method is Calling");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        // 在请求处理之后但在视图渲染之前执行的逻辑
        System.out.println("Post Handle method is Calling");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception exception) throws Exception {
        // 在整个请求完成之后，即视图渲染之后执行的逻辑
        System.out.println("Request and Response is completed");
    }
}
```
然后在配置类中注册这个拦截器：
```
import org.springframework.context.annotation.Configuration;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class MyWebMvcConfig implements WebMvcConfigurer {

    @Autowired
    private MyCustomInterceptor myCustomInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myCustomInterceptor)
                .addPathPatterns("/**")
                .excludePathPatterns("/login", "/static/**");
    }
}
```
### 方式三：使用注解
你还可以通过使用注解来定制 Spring MVC 的一些功能。使用`@ControllerAdvice`和`@ExceptionHandler`来全局处理异常。
#### 示例：全局异常处理
```
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.ModelAndView;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ModelAndView handleException(Exception ex) {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("message", ex.getMessage());
        modelAndView.setViewName("error");
        return modelAndView;
    }
}
```
### 方式四：使用`@Bean`注解
你可以在配置类中使用`@Bean`注解来注册一些 Spring MVC 的组件，例如`ViewResolver`、`MessageConverter`等。
#### 示例：自定义`ViewResolver`
```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

@Configuration
public class MyViewResolverConfig {

    @Bean
    public ViewResolver viewResolver() {
        InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        viewResolver.setPrefix("/WEB-INF/views/");
        viewResolver.setSuffix(".jsp");
        return viewResolver;
    }
}
```
### 总结
Spring Boot 提供了多种方式来扩展和定制 Spring MVC 的配置，包括通过配置类、实现特定接口、使用注解以及注册`@Bean`组件。
