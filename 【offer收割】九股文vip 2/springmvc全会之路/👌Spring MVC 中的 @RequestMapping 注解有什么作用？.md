`@RequestMapping` 注解用于映射 HTTP 请求到处理器方法（控制器方法）上。它可以应用于类级别和方法级别，用于定义请求 URL 和 HTTP 方法的映射关系。
### 主要作用

1. **URL 映射**：将特定的 URL 映射到控制器类或方法上。
2. **HTTP 方法映射**：指定处理请求的 HTTP 方法（如 GET、POST、PUT、DELETE 等）。
3. **请求参数和头信息映射**：可以根据请求参数、头信息等进一步细化映射条件。
### 详细用法
#### 类级别和方法级别的结合
`@RequestMapping` 可以同时应用于类级别和方法级别，用于构建更复杂的 URL 映射结构。
```
@Controller
@RequestMapping("/api")
public class ApiController {

    @RequestMapping("/users")
    public String getUsers() {
        // 处理 /api/users 请求
        return "users";
    }

    @RequestMapping("/products")
    public String getProducts() {
        // 处理 /api/products 请求
        return "products";
    }
}
```
#### 指定 HTTP 方法
可以通过 `method` 属性指定处理请求的 HTTP 方法。
```
@RequestMapping(value = "/submit", method = RequestMethod.POST)
public String handleSubmit() {
    // 处理 POST 请求 /submit
    return "submitSuccess";
}
```
#### 请求参数和头信息
可以通过 `params` 和 `headers` 属性进一步细化映射条件。
```
@RequestMapping(value = "/filter", params = "type=admin")
public String filterAdmin() {
    // 处理包含参数 type=admin 的请求
    return "adminPage";
}

@RequestMapping(value = "/filter", headers = "User-Agent=Mozilla/5.0")
public String filterByUserAgent() {
    // 处理包含特定 User-Agent 头信息的请求
    return "mozillaPage";
}
```
#### 消息体和内容类型
可以通过 `consumes` 和 `produces` 属性指定请求和响应的内容类型。
```
@RequestMapping(value = "/json", method = RequestMethod.POST, consumes = "application/json", produces = "application/json")
@ResponseBody
public ResponseEntity<String> handleJsonRequest(@RequestBody String jsonData) {
    // 处理 JSON 请求并返回 JSON 响应
    return ResponseEntity.ok("{\"status\":\"success\"}");
}
```
### 总结
`@RequestMapping` 注解在 Spring MVC 中用于将 HTTP 请求映射到处理器方法。它可以应用于类级别和方法级别，用于定义请求 URL 和 HTTP 方法的映射关系。通过结合使用 `@RequestMapping` 的各种属性（如 `method`、`params`、`headers`、`consumes`、`produces` 等），可以灵活地处理各种类型的请求和条件。
