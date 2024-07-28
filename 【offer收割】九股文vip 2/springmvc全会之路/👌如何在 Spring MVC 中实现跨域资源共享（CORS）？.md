### 1. 使用 `@CrossOrigin` 注解
`@CrossOrigin` 是 Spring Framework 提供的一个注解，用于在控制器方法或整个控制器上指定允许的跨域请求。这个注解可以在方法级别或类级别使用。
```
@RestController
public class MyController {

    @GetMapping("/api/data")
    @CrossOrigin(origins = "http://localhost:3000", maxAge = 3600)
    public List<Data> getData() {
        // 返回数据
    }
}
```
`@CrossOrigin` 注解指定了来自 `http://localhost:3000` 的跨域请求是被允许的，并且设置了预检请求的缓存时间为 3600 秒（1 小时）。
```
@RestController
@CrossOrigin(origins = "http://localhost:3000", maxAge = 3600)
public class MyController {

    @GetMapping("/api/data")
    public List<Data> getData() {
        // 返回数据
    }
}
```
所有在 `MyController` 中的方法都将允许来自 `http://localhost:3000` 的跨域请求。
### 2. 使用 `WebMvcConfigurer` 接口
如果你需要在全局范围内配置 CORS 策略，可以实现 `WebMvcConfigurer` 接口并重写 `addCorsMappings` 方法。
```
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
               .allowedOrigins("http://localhost:3000")
               .maxAge(3600);
    }
}
```
创建一个 `CorsConfig` 类，实现了 `WebMvcConfigurer` 接口，并在 `addCorsMappings` 方法中添加了一个全局的 CORS 配置，允许来自 `http://localhost:3000` 的跨域请求。




2024/7/16 19:58:55<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/45754509/1721308809642-8403bab8-f368-4268-9c52-9bd0a6093ac8.png#averageHue=%23997422&clientId=ue617351c-2fc5-4&from=paste&id=uf9139558&originHeight=64&originWidth=64&originalType=url&ratio=2&rotation=0&showTitle=false&size=7107&status=done&style=none&taskId=u9de559ae-1e29-499a-9cd3-bcbc0c364ac&title=)<br />重试<br />删除<br />固定<br />复制<br />如何在 Spring MVC 中使用模板引擎（如 Thymeleaf）？<br />2024/7/16 19:59:32<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/45754509/1721308809631-7d1e933e-3671-48da-a39d-64d92731c549.png#averageHue=%23e64941&clientId=ue617351c-2fc5-4&from=paste&id=u984c3997&originHeight=500&originWidth=500&originalType=url&ratio=2&rotation=0&showTitle=false&size=19317&status=done&style=none&taskId=u1321aded-42d1-4725-a9bf-69d30e45cdd&title=)<br />重试<br />删除<br />固定<br />复制<br />在 Spring MVC 中使用模板引擎（如 Thymeleaf）可以大大简化视图层的开发工作。下面是详细的步骤：
### 

2024/7/16 19:59:32<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/45754509/1721308809632-9bb16c9d-fe3c-4803-92ce-3fc43a6b604a.png#averageHue=%23997422&clientId=ue617351c-2fc5-4&from=paste&id=u01a0aa45&originHeight=64&originWidth=64&originalType=url&ratio=2&rotation=0&showTitle=false&size=7107&status=done&style=none&taskId=u76cb57cd-9f86-4061-bb30-cd8cf161a90&title=)<br />重试<br />删除<br />固定<br />复制<br />Spring MVC 与 Spring Boot 有什么区别？<br />2024/7/16 20:05:41<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/45754509/1721308809657-1f45f96e-b6ba-4eba-bb56-d4f2586da4e4.png#averageHue=%23e64941&clientId=ue617351c-2fc5-4&from=paste&id=u5209174c&originHeight=500&originWidth=500&originalType=url&ratio=2&rotation=0&showTitle=false&size=19317&status=done&style=none&taskId=ud15f2f14-4959-412f-9f85-96e923a76a9&title=)<br />重试<br />删除<br />固定<br />复制<br />Spring MVC 和 Spring Boot 都是由 Spring Framework 提供的重要组件，但它们各自有不同的目的和用途。
### 



2024/7/16 20:05:41<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/45754509/1721308809932-94784641-a26d-4c9d-b726-d92415e5f5a7.png#averageHue=%23997422&clientId=ue617351c-2fc5-4&from=paste&id=u9ad07200&originHeight=64&originWidth=64&originalType=url&ratio=2&rotation=0&showTitle=false&size=7107&status=done&style=none&taskId=u6cc599d3-0117-447d-99b6-80bd6c6ee3f&title=)<br />重试<br />删除<br />固定<br />复制<br />如何在 Spring MVC 中配置静态资源？<br />2024/7/16 20:07:31<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/45754509/1721308810062-267b47ad-b6bb-4f4f-a938-c769b00a5c05.png#averageHue=%23e64941&clientId=ue617351c-2fc5-4&from=paste&id=ub947849c&originHeight=500&originWidth=500&originalType=url&ratio=2&rotation=0&showTitle=false&size=19317&status=done&style=none&taskId=u809e3b22-f60f-4d1d-b11f-164c449543b&title=)<br />重试<br />删除<br />固定<br />复制


2024/7/16 20:07:31<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/45754509/1721308810069-ec97d142-7696-4f7c-9e9b-0a39f05c3735.png#averageHue=%23997422&clientId=ue617351c-2fc5-4&from=paste&id=ub4eb3d7b&originHeight=64&originWidth=64&originalType=url&ratio=2&rotation=0&showTitle=false&size=7107&status=done&style=none&taskId=u13f99b6d-66a7-44c4-951c-64209eb411b&title=)<br />重试<br />删除<br />固定<br />复制<br />如何在 Spring MVC 中使用 @ResponseBody 注解？<br />2024/7/16 20:09:15<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/45754509/1721308810061-6fb68f4f-5fa1-4bf2-8b0c-894ae89ff27c.png#averageHue=%23e64941&clientId=ue617351c-2fc5-4&from=paste&id=u800122fc&originHeight=500&originWidth=500&originalType=url&ratio=2&rotation=0&showTitle=false&size=19317&status=done&style=none&taskId=u11f69ad1-de9d-40ad-996d-eb73b8e864a&title=)<br />重试<br />删除<br />固定<br />复制







如何在 Spring MVC 中使用 @ExceptionHandler 注解？<br />2024/7/16 20:10:51<br />![image.png](https://cdn.nlark.com/yuque/0/2024/png/45754509/1721308841575-ed11cecb-6af8-4536-9295-39a0aab5c715.png#averageHue=%23e64941&clientId=ue617351c-2fc5-4&from=paste&id=u81998d08&originHeight=500&originWidth=500&originalType=url&ratio=2&rotation=0&showTitle=false&size=19317&status=done&style=none&taskId=uac496de4-60c4-4cca-847c-e8d9639c694&title=)<br />重试
