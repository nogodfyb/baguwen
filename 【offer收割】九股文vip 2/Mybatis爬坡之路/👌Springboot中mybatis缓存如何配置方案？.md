在 Spring Boot 中配置 MyBatis 缓存可以通过以下步骤来实现。MyBatis 提供了两级缓存机制：一级缓存（本地缓存）和二级缓存（全局缓存）。一级缓存是默认开启的，而二级缓存需要手动配置。
### 1. 一级缓存
一级缓存是默认开启的，无需额外配置。它是基于`SqlSession`的缓存，在同一个`SqlSession`中执行的相同 SQL 会被缓存。
### 2. 二级缓存
二级缓存是基于命名空间的缓存，多个`SqlSession`可以共享这些缓存。要启用 MyBatis 的二级缓存，需要进行以下配置：
#### 2.1 添加依赖
如果你使用的是 MyBatis 官方的缓存实现（如`mybatis-ehcache`），需要在`pom.xml`中添加相应的依赖：
```
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.2.1</version>
</dependency>
```
#### 2.2 配置 MyBatis 全局设置
在`application.yml`或`application.properties`中启用二级缓存：
```
mybatis:
  configuration:
    cache-enabled:true
```
或者在`application.properties`中：
```
mybatis.configuration.cache-enabled=true
```
#### 2.3 配置 Mapper 文件
在每个需要使用二级缓存的 Mapper 文件中，添加`<cache>`标签。例如：
```
<mapper namespace="com.example.mapper.UserMapper">
    <cache/>

    <select id="selectUserById" resultType="User">
        SELECT * FROM users WHERE id = #{id}
    </select>
</mapper>
```
#### 2.4 配置缓存实现
如果使用的是`mybatis-ehcache`，需要创建一个`ehcache.xml`文件来配置 Ehcache：
```
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd">
    <cache name="com.example.mapper.UserMapper"
           maxEntriesLocalHeap="1000"
           timeToLiveSeconds="3600"/>
</ehcache>
```
将`ehcache.xml`文件放在`src/main/resources`目录下。
#### 2.5 配置缓存策略
你也可以在`<cache>`标签中配置缓存策略，例如：
```
<cache eviction="LRU" flushInterval="60000" size="512" readOnly="true"/>
```

- `eviction`：缓存淘汰策略（如 LRU、FIFO 等）。
- `flushInterval`：刷新间隔时间（以毫秒为单位）。
- `size`：缓存对象的数量。
- `readOnly`：只读属性，设置为`true`时，缓存中的对象不能被修改。
### 3. 使用第三方缓存框架
除了 MyBatis 自带的缓存实现，你也可以使用其他缓存框架，如 Redis、Caffeine 等。以下是使用 Redis 作为二级缓存的示例。
#### 3.1 添加依赖
在`pom.xml`中添加`mybatis-redis`依赖：
```
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-redis</artifactId>
    <version>1.0.0</version>
</dependency>
```
#### 3.2 配置 Redis 连接
在`application.yml`中配置 Redis 连接：
```
spring:
  redis:
    host:localhost
    port:6379
    password:yourpassword
```
#### 3.3 配置 MyBatis 全局设置
启用二级缓存：
```
mybatis:
  configuration:
    cache-enabled:true
```
#### 3.4 配置 Mapper 文件
在每个需要使用二级缓存的 Mapper 文件中，添加`<cache>`标签，并指定使用 Redis 缓存：
```
<mapper namespace="com.example.mapper.UserMapper">
    <cache type="org.mybatis.caches.redis.RedisCache"/>

    <select id="selectUserById" resultType="User">
        SELECT * FROM users WHERE id = #{id}
    </select>
</mapper>
```
### 总结
在 Spring Boot 中配置 MyBatis 缓存主要涉及以下步骤：

1. 默认启用一级缓存。
2. 启用二级缓存并配置全局设置。
3. 在 Mapper 文件中配置二级缓存。
4. 选择并配置具体的缓存实现（如 Ehcache、Redis 等）。
