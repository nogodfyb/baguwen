`Handler Adapter`负责将处理器（Handler）适配为具体的处理方法。`Handler Adapter` 的主要作用是根据处理器的类型和具体实现，执行相应的处理逻辑。`Handler Adapter` 是 `DispatcherServlet` 和具体处理器之间的桥梁。
### 主要职责

1. **处理器执行**：调用处理器的方法来处理请求。
2. **返回模型和视图**：处理完请求后，返回一个 `ModelAndView` 对象，包含视图名称和模型数据。
### 工作流程

1. **请求到达 **`**DispatcherServlet**`：当一个 HTTP 请求到达 `DispatcherServlet` 时，它会先通过 `Handler Mapping` 找到对应的处理器。
2. **选择 **`**Handler Adapter**`：`DispatcherServlet` 根据处理器的类型选择合适的 `Handler Adapter`。
3. **执行处理器**：`Handler Adapter` 调用处理器的方法来处理请求。
4. **返回结果**：处理完请求后，`Handler Adapter` 返回一个 `ModelAndView` 对象，`DispatcherServlet` 再根据这个对象生成最终的响应。
### 常见的 `Handler Adapter` 实现

1. `**HttpRequestHandlerAdapter**`：
   - 用于处理实现 `HttpRequestHandler` 接口的处理器。
   - 例如实现了 `HttpRequestHandler` 接口的处理器。
2. `**SimpleControllerHandlerAdapter**`：
   - 用于处理实现 `Controller` 接口的处理器。
   - 例如实现了 `Controller` 接口的处理器。
3. `**RequestMappingHandlerAdapter**`：
   - 最常用的 `Handler Adapter` 实现。
   - 用于处理使用 `@RequestMapping` 注解的控制器方法。
   - 支持复杂的请求映射规则和数据绑定。
### 示例代码
使用 `RequestMappingHandlerAdapter` ：
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Controller
@RequestMapping("/hello")
public class HelloController {

    @GetMapping
    public ModelAndView helloWorld() {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("hello");
        modelAndView.addObject("message", "Hello, World!");
        return modelAndView;
    }
}

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.example")
public class WebConfig implements WebMvcConfigurer {
    // 可以在这里添加其他配置
}
```

- `HelloController` 类使用 `@RequestMapping` 和 `@GetMapping` 注解来定义请求映射。
- `RequestMappingHandlerAdapter` 会根据注解找到 `helloWorld` 方法并执行它。
- `helloWorld` 方法返回一个 `ModelAndView` 对象，包含视图名称和模型数据。
### 总结
`Handler Adapter` 负责将处理器适配为具体的处理方法。通过使用不同的 `Handler Adapter` 实现，Spring MVC 可以灵活地支持多种类型的处理器。最常用的 `Handler Adapter` 实现是 `RequestMappingHandlerAdapter`，它支持通过注解定义的控制器方法，并提供了强大的请求处理功能。
