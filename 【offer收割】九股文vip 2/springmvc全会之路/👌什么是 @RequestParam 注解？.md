`@RequestParam` 注解是 Spring MVC 中用于将请求参数绑定到处理方法的参数上的注解。它可以用于从 URL 查询参数、表单数据或其他请求参数中提取值，并将这些值传递给控制器方法的参数。
### `@RequestParam` 的基本用法
#### 1. 绑定单个请求参数
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class UserController {

    @GetMapping("/greet")
    @ResponseBody
    public String greetUser(@RequestParam String name) {
        return "Hello, " + name;
    }
}
```
`@RequestParam` 注解用于将请求参数 `name` 的值绑定到方法参数 `name` 上。如果请求 URL 是 `/greet?name=John`，那么方法参数 `name` 的值将是 `John`。
#### 2. 指定请求参数名称
可以通过 `value` 属性指定请求参数的名称：
```
@GetMapping("/greet")
@ResponseBody
public String greetUser(@RequestParam("username") String name) {
    return "Hello, " + name;
}
```
`@RequestParam("username")` 表示将请求参数 `username` 的值绑定到方法参数 `name` 上。如果请求 URL 是 `/greet?username=John`，那么方法参数 `name` 的值将是 `John`。
#### 3. 设置请求参数的默认值
可以通过 `defaultValue` 属性设置请求参数的默认值：
```
@GetMapping("/greet")
@ResponseBody
public String greetUser(@RequestParam(value = "name", defaultValue = "Guest") String name) {
    return "Hello, " + name;
}
```
如果请求 URL 中没有 `name` 参数，那么 `name` 参数的默认值将是 `Guest`。
#### 4. 请求参数为可选
可以通过 `required` 属性指定请求参数是否是必需的：
```
@GetMapping("/greet")
@ResponseBody
public String greetUser(@RequestParam(value = "name", required = false) String name) {
    if (name == null) {
        name = "Guest";
    }
    return "Hello, " + name;
}
```
`@RequestParam(value = "name", required = false)` 表示 `name` 参数是可选的。如果请求 URL 中没有 `name` 参数，那么 `name` 参数的值将是 `null`。
### 高级用法
#### 绑定多个请求参数
```
@GetMapping("/user")
@ResponseBody
public String getUserInfo(@RequestParam String name, @RequestParam int age) {
    return "User: " + name + ", Age: " + age;
}
```
`name` 和 `age` 请求参数将分别绑定到方法参数 `name` 和 `age` 上。如果请求 URL 是 `/user?name=John&age=30`，那么方法参数 `name` 的值将是 `John`，`age` 的值将是 `30`。
#### 绑定到集合类型
```
@GetMapping("/numbers")
@ResponseBody
public String getNumbers(@RequestParam List<Integer> nums) {
    return "Numbers: " + nums;
}
```
`nums` 请求参数将绑定到方法参数 `nums` 上。如果请求 URL 是 `/numbers?nums=1&nums=2&nums=3`，那么方法参数 `nums` 的值将是 `[1, 2, 3]`。
### 总结
`@RequestParam` 注解在 Spring MVC 中用于将请求参数绑定到控制器方法的参数上。它提供了多种配置选项，如：

1. 指定请求参数名称。
2. 设置请求参数的默认值。
3. 指定请求参数是否是必需的。
4. 支持绑定单个值、多个值以及集合类型的参数。

使用 `@RequestParam` 注解，可从请求中提取参数，并将其传递给控制器方法进行处理。
