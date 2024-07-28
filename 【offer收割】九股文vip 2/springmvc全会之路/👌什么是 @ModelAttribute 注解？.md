`@ModelAttribute` 注解是 Spring MVC 中用于绑定请求参数到模型对象的注解。它可以用于方法参数、方法和控制器类中，以便将请求中的数据绑定到模型对象，并将该对象添加到模型中，以便在视图中使用。
### `@ModelAttribute` 的使用场景

1. **方法参数**：用于绑定请求参数到方法参数，并将该参数添加到模型中。
2. **方法**：用于在处理请求之前准备模型数据。通常用于在处理请求之前初始化一些公共数据。
3. **控制器类**：用于在所有请求处理方法之前初始化模型数据。
### 1. 用于方法参数
当 `@ModelAttribute` 注解用于控制器方法参数时，它会自动将请求参数绑定到该参数对象中，并将该对象添加到模型中。
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class UserController {

    @PostMapping("/register")
    public String registerUser(@ModelAttribute User user) {
        // user 对象已经绑定了请求参数
        // 可以在这里处理业务逻辑
        return "result";
    }
}
```
在`@ModelAttribute` 注解用于 `User` 对象的参数。这意味着 Spring MVC 会自动将请求参数绑定到 `User` 对象的属性中，并将该对象添加到模型中。
### 2. 用于方法
当 `@ModelAttribute` 注解用于控制器方法时，该方法会在每个处理请求的方法之前执行，用于准备模型数据
```
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class UserController {

    @ModelAttribute
    public void addAttributes(Model model) {
        model.addAttribute("commonAttribute", "This is a common attribute");
    }

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }
}
```
`addAttributes` 方法会在 `showForm` 方法之前执行，并将一个公共属性添加到模型中。这样，`commonAttribute` 可以在视图中使用。
### 3. 用于控制器类
当 `@ModelAttribute` 注解用于控制器类时，它会在所有请求处理方法之前执行，用于初始化模型数据。
```
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/users")
public class UserController {

    @ModelAttribute
    public void addCommonAttributes(Model model) {
        model.addAttribute("appName", "User Management System");
    }

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }

    @PostMapping("/register")
    public String registerUser(@ModelAttribute User user) {
        // user 对象已经绑定了请求参数
        // 可以在这里处理业务逻辑
        return "result";
    }
}
```
`addCommonAttributes` 方法会在所有请求处理方法之前执行，并将一个公共属性添加到模型中。这样，`appName` 可以在所有视图中使用。
### 总结
`@ModelAttribute` 注解在 Spring MVC 中用于将请求参数绑定到模型对象，并将该对象添加到模型中。它可以用于方法参数、方法和控制器类中，用于不同的场景：

1. **方法参数**：自动将请求参数绑定到方法参数对象中。
2. **方法**：在处理请求之前准备模型数据。
3. **控制器类**：在所有请求处理方法之前初始化模型数据。

使用 `@ModelAttribute` 注解，可以简化请求参数的绑定过程，并使得模型数据在视图中更容易访问。
