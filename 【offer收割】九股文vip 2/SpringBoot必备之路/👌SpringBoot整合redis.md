### 步骤 1：添加依赖
首先，在你的`pom.xml`文件（如果你使用的是 Maven）或`build.gradle`文件（如果你使用的是 Gradle）中添加 Spring Data Redis 和 Jedis 或 Lettuce 依赖。
#### Maven:
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```
#### Gradle:
```
implementation 'org.springframework.boot:spring-boot-starter-data-redis'
implementation 'redis.clients:jedis'
```
### 步骤 2：配置 Redis 连接
在`src/main/resources/application.properties`文件中配置 Redis 连接属性。
```
spring.redis.host=localhost
spring.redis.port=6379
spring.redis.password=yourpassword  # 如果 Redis 没有设置密码，可以省略这一行
```
### 步骤 3：创建 Redis 配置类
创建一个配置类来自定义 RedisTemplate 和连接工厂。
```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new JedisConnectionFactory();
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        return template;
    }
}
```
### 步骤 4：使用 RedisTemplate 进行操作
可以在你的服务类或控制器中注入`RedisTemplate`并使用它进行 Redis 操作。
#### 示例服务类：
```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;

@Service
public class RedisService {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    public void save(String key, Object value) {
        redisTemplate.opsForValue().set(key, value);
    }

    public Object find(String key) {
        return redisTemplate.opsForValue().get(key);
    }

    public void delete(String key) {
        redisTemplate.delete(key);
    }
}
```
#### 示例控制器：
```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/redis")
public class RedisController {

    @Autowired
    private RedisService redisService;

    @PostMapping("/save")
    public String save(@RequestParam String key, @RequestParam String value) {
        redisService.save(key, value);
        return "Saved!";
    }

    @GetMapping("/find")
    public Object find(@RequestParam String key) {
        return redisService.find(key);
    }

    @DeleteMapping("/delete")
    public String delete(@RequestParam String key) {
        redisService.delete(key);
        return "Deleted!";
    }
}
```
