`@PathVariable` 注解是 Spring MVC 中用于将 URL 路径中的变量绑定到处理方法的参数上的注解。它允许你从 URL 路径中提取参数，并将这些参数传递给控制器方法，从而实现更加动态和灵活的 URL 路由。
### `@PathVariable` 的基本用法
#### 1. 绑定单个路径变量
可以将 URL 路径中的变量绑定到方法参数上：
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class UserController {

    @GetMapping("/users/{id}")
    @ResponseBody
    public String getUserById(@PathVariable String id) {
        return "User ID: " + id;
    }
}
```
`@PathVariable` 注解用于将 URL 路径中的 `id` 变量绑定到方法参数 `id` 上。如果请求 URL 是 `/users/123`，那么方法参数 `id` 的值将是 `123`。
#### 2. 指定路径变量名称
可以通过 `value` 属性指定路径变量的名称：
```
@GetMapping("/users/{userId}")
@ResponseBody
public String getUserById(@PathVariable("userId") String id) {
    return "User ID: " + id;
}
```
`@PathVariable("userId")` 表示将 URL 路径中的 `userId` 变量绑定到方法参数 `id` 上。如果请求 URL 是 `/users/123`，那么方法参数 `id` 的值将是 `123`。
### 高级用法
#### 绑定多个路径变量
可以绑定多个路径变量到方法参数上：
```
@GetMapping("/users/{userId}/orders/{orderId}")
@ResponseBody
public String getUserOrder(@PathVariable String userId, @PathVariable String orderId) {
    return "User ID: " + userId + ", Order ID: " + orderId;
}
```
`userId` 和 `orderId` 路径变量将分别绑定到方法参数 `userId` 和 `orderId` 上。如果请求 URL 是 `/users/123/orders/456`，那么方法参数 `userId` 的值将是 `123`，`orderId` 的值将是 `456`。
#### 绑定到特定类型
可以将路径变量绑定到特定类型的参数上：
```
@GetMapping("/products/{productId}")
@ResponseBody
public String getProductById(@PathVariable int productId) {
    return "Product ID: " + productId;
}
```
`productId` 路径变量将绑定到方法参数 `productId` 上，并自动转换为 `int` 类型。如果请求 URL 是 `/products/789`，那么方法参数 `productId` 的值将是 `789`。
