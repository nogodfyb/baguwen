`@RestController` 是 用于标记一个类作为 RESTful 控制器。这个注解实际上是 `@Controller` 和 `@ResponseBody` 注解的组合。
### `@Controller` 注解
`@Controller` 是 Spring MVC 中的一个基本注解，用于标记一个类作为控制器。控制器负责处理 HTTP 请求，并将结果返回给客户端。通常，控制器中的方法会返回一个视图名或一个 View 对象，用于渲染 HTML 页面。
### `@ResponseBody` 注解
`@ResponseBody` 是另一个重要的注解，用于标记方法的返回值将被写入到 HTTP 响应体中，而不是用来选择一个视图进行渲染。如果一个方法没有使用 `@ResponseBody` 注解，则其返回值通常会被解释为视图名。
### `@RestController` 注解
`@RestController` 注解是这两个注解的结合体。它不仅标记了一个类作为控制器，还指示了该控制器的所有方法都应该将其返回值写入到 HTTP 响应体中，而不是用来选择一个视图进行渲染。这意味着，使用 `@RestController` 注解的控制器非常适合用于构建 RESTful API。
```
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        // 从数据库中获取用户
        return user;
    }
}
```
`UserController` 被标记为一个 RESTful 控制器。`getUser()` 方法返回一个 `User` 对象，Spring MVC 会自动将其序列化为 JSON 或 XML 等格式，并写入到 HTTP 响应体中。<br />总的来说，`@RestController` 注解使得开发者可以更方便地构建 RESTful API，简化了代码并提高了开发效率。
