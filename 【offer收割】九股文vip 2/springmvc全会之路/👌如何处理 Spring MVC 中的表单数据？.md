1. **创建表单页面**：使用 HTML 表单元素。
2. **创建数据模型**：定义一个 Java 类来接收表单数据。
3. **创建控制器方法**：处理表单提交请求。
4. **数据绑定和验证**：使用 Spring 提供的验证机制来验证表单数据。
### 1. 创建表单页面
首先，创建一个 HTML 表单页面来收集用户输入。一个简单的用户注册表单：
```
<!DOCTYPE html>
<html>
<head>
    <title>Register</title>
</head>
<body>
    <h2>Register</h2>
    <form action="/register" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username"><br><br>
        <label for="password">Password:</label>
        <input type="password" id="password" name="password"><br><br>
        <input type="submit" value="Register">
    </form>
</body>
</html>
```
### 2. 创建数据模型
创建一个 Java 类来表示表单数据。一个用户类：
```
public class User {
    private String username;
    private String password;

    // Getters and Setters
    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```
### 3. 创建控制器方法
在控制器类中创建方法来处理表单提交请求。
```
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class UserController {

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }

    @PostMapping("/register")
    public String submitForm(@ModelAttribute("user") User user, Model model) {
        // 处理表单数据
        model.addAttribute("message", "User registered successfully");
        return "result";
    }
}
```

- `@GetMapping("/register")` 方法用于显示注册表单。
- `@PostMapping("/register")` 方法用于处理表单提交。通过 `@ModelAttribute` 注解，Spring MVC 会自动将表单数据绑定到 `User` 对象。
### 4. 数据绑定和验证
为了确保表单数据的有效性，可以使用 Spring 的验证机制。首先，在数据模型类上使用注解进行验证：
```
import javax.validation.constraints.NotEmpty;

public class User {
    @NotEmpty(message = "Username is required")
    private String username;

    @NotEmpty(message = "Password is required")
    private String password;

    // Getters and Setters
}
```
在控制器方法中添加验证逻辑：
```
import org.springframework.validation.BindingResult;
import javax.validation.Valid;

@Controller
public class UserController {

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }

    @PostMapping("/register")
    public String submitForm(@Valid @ModelAttribute("user") User user, BindingResult bindingResult, Model model) {
        if (bindingResult.hasErrors()) {
            return "register";
        }
        // 处理表单数据
        model.addAttribute("message", "User registered successfully");
        return "result";
    }
}
```

- `@Valid` 注解用于触发验证。
- `BindingResult` 参数用于检查验证结果。如果有验证错误，返回到表单页面并显示错误信息。
### 5. 显示验证错误信息
在表单页面上显示验证错误信息：
```
<!DOCTYPE html>
<html>
<head>
    <title>Register</title>
</head>
<body>
    <h2>Register</h2>
    <form action="/register" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" value="${user.username}"><br>
        <span style="color:red">${#fields.hasErrors('username')} ? ${#fields.errors('username')} : ''</span><br><br>
        
        <label for="password">Password:</label>
        <input type="password" id="password" name="password"><br>
        <span style="color:red">${#fields.hasErrors('password')} ? ${#fields.errors('password')} : ''</span><br><br>
        
        <input type="submit" value="Register">
    </form>
</body>
</html>
```
### 总结
以上可以在 Spring MVC 中处理表单数据，包括显示表单、接收和绑定表单数据、验证数据以及处理验证错误。这个流程可以帮助构建健壮的表单处理机制，确保数据的完整性和有效性。
