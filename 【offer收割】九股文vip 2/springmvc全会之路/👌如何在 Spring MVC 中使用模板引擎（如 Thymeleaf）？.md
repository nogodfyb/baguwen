### 1. 添加依赖
在项目的 `pom.xml` 文件中添加 Thymeleaf 的依赖项：
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
如果你不是使用 Spring Boot，可以添加依赖项：
```
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring4</artifactId>
</dependency>
```
### 2. 配置 Thymeleaf
在 Spring Boot 中，Thymeleaf 的配置通常通过 `application.properties` 或 `application.yml` 文件进行。可以在 `application.properties` 中添加以下配置：
```
spring.thymeleaf.cache=false
spring.thymeleaf.mode=HTML5
```
这将禁用 Thymeleaf 的模板缓存，并将模板解析模式设置为 HTML5。<br />如果你不是使用 Spring Boot，可以在 Spring 配置文件中创建一个 `ThymeleafViewResolver` bean来配置 Thymeleaf：
```
<bean id="thymeleafViewResolver" class="org.thymeleaf.spring4.view.ThymeleafViewResolver">
    <property name="templateEngine" ref="templateEngine"/>
    <property name="characterEncoding" value="UTF-8"/>
</bean>

<bean id="templateEngine" class="org.thymeleaf.spring4.SpringTemplateEngine">
    <property name="templateResolver" ref="templateResolver"/>
</bean>

<bean id="templateResolver" class="org.thymeleaf.templateresolver.ClassLoaderTemplateResolver">
    <property name="prefix" value="classpath:templates/"/>
    <property name="suffix" value=".html"/>
    <property name="templateMode" value="HTML5"/>
    <property name="characterEncoding" value="UTF-8"/>
</bean>
```
配置指定了 Thymeleaf 的模板位置、解析模式和字符编码等信息。
### 3. 创建模板文件
在项目的 `src/main/resources/templates` 目录下创建一个名为 `index.html` 的模板文件。这个文件将作为我们的视图模板：
```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Thymeleaf Example</title>
</head>
<body>
    <h1 th:text="${message}">Hello World!</h1>
</body>
</html>
```
使用 Thymeleaf 的语法，`th:text` 属性将显示传入的 `message` 变量的值。
### 4. 在控制器中使用模板
在控制器中，使用 `@Controller` 注解和 `@GetMapping` 注解来处理 HTTP GET 请求，并返回一个 `String` 对象作为视图名：
```
@Controller
public class MyController {

    @GetMapping("/")
    public String index(Model model) {
        model.addAttribute("message", "Welcome to Thymeleaf!");
        return "index";
    }
}
```
我们将一个名为 `message` 的模型属性添加到模型中，并将其传递给 Thymeleaf 模板。然后，控制器返回 `index` 视图名，Spring MVC 会自动选择并渲染 `index.html` 模板文件。当访问应用程序的根路径时，应该能看到一个带有欢迎消息的网页。


