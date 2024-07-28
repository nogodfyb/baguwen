`@Controller` 注解用于标记一个类作为控制器组件。控制器是处理 HTTP 请求的核心组件，它负责接收请求、处理业务逻辑并返回视图或数据响应。
### 主要作用

1. **标识控制器类**：`@Controller` 注解告诉 Spring 该类是一个控制器，应该由 Spring 容器管理。
2. **处理请求**：控制器类中的方法通过映射注解（如 `@RequestMapping`、`@GetMapping`、`@PostMapping` 等）处理 HTTP 请求。
### 相关注解
在 Spring MVC 中，除了 `@Controller`，还有一些常用的注解用于处理请求：

1. `**@RequestMapping**`：
   - 用于定义请求 URL 和 HTTP 方法的映射。
   - 可以应用于类级别和方法级别。
2. `**@GetMapping**`**、**`**@PostMapping**`**、**`**@PutMapping**`**、**`**@DeleteMapping**`：
   - 分别用于处理 `GET`、`POST`、`PUT`、`DELETE` 请求。
   - 是 `@RequestMapping` 的快捷方式。
3. `**@RequestParam**`：
   - 用于绑定请求参数到方法参数。
   - 可以指定参数名称、是否必需以及默认值。
4. `**@PathVariable**`：
   - 用于绑定 URL 路径中的变量到方法参数。
5. `**@ModelAttribute**`：
   - 用于将请求参数绑定到模型对象，并将模型对象添加到模型中。
6. `**@ResponseBody**`：
   - 用于将方法的返回值直接作为 HTTP 响应体。
   - 常用于返回 JSON 或 XML 数据。
### 总结
`@Controller` 注解在 Spring MVC 中用于标记一个类为控制器组件，控制器负责处理 HTTP 请求并返回视图或数据响应。通过结合使用各种映射注解（如 `@RequestMapping`、`@GetMapping` 等）和参数绑定注解（如 `@RequestParam`、`@PathVariable` 等），可以灵活地处理不同类型的请求和参数。
