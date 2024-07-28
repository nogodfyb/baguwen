`@ResponseBody` 注解用于标记方法的返回值将被写入到 HTTP 响应体中。这个注解通常与 `@Controller` 或 `@RestController` 结合使用，用于处理 AJAX 请求或返回 JSON、XML 等数据格式的 API 响应。
### 1. 添加必要的依赖
确保你的项目中已经添加了 Spring MVC 的依赖。如果你使用 Maven，可以在 `pom.xml` 文件中添加以下依赖：
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
如果你不使用 Spring Boot，也可以单独添加 Spring MVC 的依赖。
### 2. 创建一个控制器
创建一个带有 `@Controller` 或 `@RestController` 注解的类，表示这是一个 Spring MVC 控制器。
```
@RestController
public class MyController {
    
}
```
`@RestController` 是一个组合注解，相当于同时使用 `@Controller` 和 `@ResponseBody`。
### 3. 在方法上使用 @ResponseBody
在控制器的方法上使用 `@ResponseBody` 注解，指示 Spring MVC 将该方法的返回值作为 HTTP 响应体的内容。
```
@RestController
public class MyController {

    @GetMapping("/api/data")
    @ResponseBody
    public String getData() {
        return "Hello, World!";
    }
}
```
`getData()` 方法的返回值 `"Hello, World!"` 将被写入到 HTTP 响应体中。
### 4. 使用对象作为返回值
如果你想要返回一个对象，例如一个 JSON 对象，你可以使用 Jackson 或 Gson 等库来将对象序列化为 JSON 格式。
```
@RestController
public class MyController {

    @GetMapping("/api/user")
    @ResponseBody
    public User getUser() {
        User user = new User("John Doe", 30);
        return user;
    }
}

class User {
    private String name;
    private int age;

    // getters and setters
}
```
`getUser()` 方法返回一个 `User` 对象。Spring MVC 会自动将其序列化为 JSON 格式并写入到 HTTP 响应体中。
### 5. 处理异常
如果你的方法抛出一个异常，你可以使用 `@ExceptionHandler` 注解来捕获并处理该异常。
```
@RestController
public class MyController {

    @GetMapping("/api/data")
    @ResponseBody
    public String getData() {
        throw new RuntimeException("Something went wrong");
    }

    @ExceptionHandler(RuntimeException.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    @ResponseBody
    public String handleException(RuntimeException ex) {
        return "An error occurred: " + ex.getMessage();
    }
}
```
`getData()` 方法抛出一个 `RuntimeException`。`handleException()` 方法被用来捕获并处理该异常，并返回一个错误消息作为 HTTP 响应体的内容。<br />总结来说，使用 `@ResponseBody` 注解可以让你的控制器方法的返回值直接写入到 HTTP 响应体中，非常适合用于处理 AJAX 请求或提供 RESTful API。
