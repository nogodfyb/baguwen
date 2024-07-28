`Handler Mapping` 负责将 HTTP 请求映射到相应的处理器（通常是控制器方法）。当 `DispatcherServlet` 接收到一个请求时，它会使用 `Handler Mapping` 来确定哪个处理器应该处理这个请求。
### 主要职责

1. **请求映射**：根据请求的 URL、HTTP 方法、请求参数等信息，查找并确定相应的处理器。
2. **处理器返回**：返回一个包含处理器对象和处理器拦截器链的 `HandlerExecutionChain` 对象。
### 工作流程

1. **请求到达 **`**DispatcherServlet**`：当一个 HTTP 请求到达 `DispatcherServlet` 时，它会首先交给 `Handler Mapping` 进行处理。
2. **查找处理器**：`Handler Mapping` 根据请求的 URL、HTTP 方法等信息查找匹配的处理器。
3. **返回处理器**：`Handler Mapping` 返回一个 `HandlerExecutionChain` 对象，其中包含处理器（通常是控制器方法）和处理器拦截器链。
4. **处理请求**：`DispatcherServlet` 使用找到的处理器来处理请求，并生成响应。
### 常见的 `Handler Mapping` 实现

1. `**BeanNameUrlHandlerMapping**`：
   - 通过 bean 的名称来映射处理器。
   - 例如，bean 名称为 `/hello` 的处理器会处理 `/hello` 请求。
2. `**SimpleUrlHandlerMapping**`：
   - 通过显式配置的 URL 路径来映射处理器。
   - 可以在 Spring 配置文件中指定 URL 到处理器的映射关系。
3. `**DefaultAnnotationHandlerMapping**`（过时）：
   - 通过注解（如 `@RequestMapping`）来映射处理器。
   - 在较新的 Spring 版本中被 `RequestMappingHandlerMapping` 取代。
4. `**RequestMappingHandlerMapping**`：
   - 这是最常用的 `Handler Mapping` 实现。
   - 通过注解（如 `@RequestMapping`、`@GetMapping`、`@PostMapping` 等）来映射处理器。
   - 支持复杂的请求映射规则，包括路径变量、请求参数、请求头等。
### 示例代码
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Controller
@RequestMapping("/hello")
public class HelloController {

    @GetMapping
    public String helloWorld() {
        return "hello";
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
- `/hello` URL 会被映射到 `helloWorld` 方法。
### 总结
`Handler Mapping` 负责将 HTTP 请求映射到相应的处理器。通过使用不同的 `Handler Mapping` 实现，开发者可以灵活地配置请求映射规则，以满足各种应用需求。最常用的 `Handler Mapping` 实现是 `RequestMappingHandlerMapping`，它通过注解提供了强大的请求映射功能。
