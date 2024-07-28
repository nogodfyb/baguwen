`View Resolver` 负责将逻辑视图名称解析为具体的视图对象（如 JSP、Thymeleaf 模板等）。`View Resolver` 的主要作用是根据控制器返回的视图名称，找到相应的视图资源，并将其渲染成最终的 HTML 响应。
### 主要职责

1. **视图名称解析**：将控制器返回的逻辑视图名称解析为具体的视图对象。
2. **视图对象返回**：返回一个 `View` 对象，该对象可以用来渲染模型数据。
### 工作流程

1. **控制器处理请求**：当一个 HTTP 请求到达 `DispatcherServlet` 时，它会通过 `Handler Adapter` 调用控制器的方法来处理请求。
2. **返回视图名称**：控制器方法处理完请求后，会返回一个包含视图名称和模型数据的 `ModelAndView` 对象。
3. **视图名称解析**：`DispatcherServlet` 使用 `View Resolver` 将逻辑视图名称解析为具体的视图对象。
4. **渲染视图**：`View` 对象使用模型数据来渲染最终的 HTML 响应。
### 常见的 `View Resolver` 实现

1. `**InternalResourceViewResolver**`：
   - 最常用的 `View Resolver` 实现。
   - 用于解析 JSP 文件。
   - 通过配置前缀和后缀来确定视图的实际路径。
2. `**ThymeleafViewResolver**`：
   - 用于解析 Thymeleaf 模板文件。
   - 需要配合 Thymeleaf 模板引擎使用。
3. `**BeanNameViewResolver**`：
   - 通过视图名称作为 bean 名称来查找视图对象。
   - 适用于视图对象作为 Spring bean 定义的情况。
4. `**XmlViewResolver**`：
   - 通过 XML 文件配置视图名称和视图对象的映射关系。
### 示例代码
使用 `InternalResourceViewResolver` ：
```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Bean
    public ViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }
}
```

- `InternalResourceViewResolver` 被配置为视图解析器。
- 视图的前缀被设置为 `/WEB-INF/views/`，后缀被设置为 `.jsp`。
- 例如，当控制器返回视图名称 `home` 时，`InternalResourceViewResolver` 会将其解析为 `/WEB-INF/views/home.jsp`。
### 控制器示例
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class HomeController {

    @GetMapping("/home")
    public ModelAndView home() {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("home");
        modelAndView.addObject("message", "Welcome to the home page!");
        return modelAndView;
    }
}
```

- `HomeController` 类定义了一个处理 `/home` 请求的方法。
- 该方法返回一个 `ModelAndView` 对象，其中视图名称为 `home`。
- `InternalResourceViewResolver` 会将视图名称 `home` 解析为 `/WEB-INF/views/home.jsp`。
### 总结
`View Resolver` 负责将逻辑视图名称解析为具体的视图对象。通过使用不同的 `View Resolver` 实现，Spring MVC 可以灵活地支持多种类型的视图技术，如 JSP、Thymeleaf 等。最常用的 `View Resolver` 实现是 `InternalResourceViewResolver`，它通过配置前缀和后缀来解析 JSP 文件。
