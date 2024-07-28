Feign 的拦截器（Interceptor）用于在请求发送之前或响应接收之后对请求和响应进行处理或修改。拦截器可以用于各种场景，例如添加认证信息、记录日志、修改请求头或响应体等。
### Feign 拦截器的作用

1. **添加请求头**：
   - 可以在请求发送之前添加必要的请求头，例如认证信息、用户代理等。
2. **记录日志**：
   - 可以记录请求和响应的详细信息，以便调试和监控。
3. **修改请求参数**：
   - 可以在请求发送之前修改请求参数，例如对参数进行加密或签名。
4. **处理响应**：
   - 可以在接收到响应之后对响应进行处理，例如检查响应状态码、解析响应体等。
### 实现 Feign 拦截器
要实现一个 Feign 拦截器，需要实现`feign.RequestInterceptor`接口，并重写其`apply`方法。以下是一个简单的示例：
```
import feign.RequestInterceptor;
import feign.RequestTemplate;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeignConfig {

    @Bean
    public RequestInterceptor requestInterceptor() {
        return new RequestInterceptor() {
            @Override
            public void apply(RequestTemplate template) {
                // 添加请求头
                template.header("Authorization", "Bearer " + getAccessToken());

                // 记录请求日志
                System.out.println("Request: " + template.request());
            }

            private String getAccessToken() {
                // 获取访问令牌的逻辑
                return "your-access-token";
            }
        };
    }
}
```
在这个示例中，我们定义了一个 Feign 配置类`FeignConfig`，并在其中注册了一个`RequestInterceptor`。这个拦截器在每次请求发送之前都会添加一个`Authorization`请求头，并记录请求日志。
### 使用 Feign 拦截器
要使用自定义的 Feign 拦截器，只需要在 Feign 客户端配置中引用这个配置类即可。例如：
```
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@FeignClient(name = "example-client", url = "https://api.example.com", configuration = FeignConfig.class)
public interface ExampleClient {

    @GetMapping("/data")
    String getData(@RequestParam("param") String param);
}
```
在这个示例中，我们定义了一个 Feign 客户端`ExampleClient`，并在其配置中引用了`FeignConfig`类，这样每次通过`ExampleClient`发送请求时，都会应用我们定义的拦截器。
