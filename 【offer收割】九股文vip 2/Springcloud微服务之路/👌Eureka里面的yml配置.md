在 Spring Boot 应用中，Eureka 的配置通常通过 `application.yml` 文件进行。下面是一个典型的 `application.yml` 文件示例，其中包含了 Eureka Server 和 Eureka Client 的配置。
### Eureka Server 配置
```
server:
  port: 8761 # Eureka Server 运行的端口

eureka:
  instance:
    hostname: localhost # Eureka Server 的主机名
  client:
    register-with-eureka: false # Eureka Server 本身不需要向 Eureka 注册
    fetch-registry: false # Eureka Server 不需要从 Eureka 获取注册表
  server:
    enable-self-preservation: true # 启用自我保护模式

spring:
  application:
    name: eureka-server # 应用名称
```
### Eureka Client 配置
```
server:
  port: 8080 # 应用服务运行的端口

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/ # Eureka Server 的地址
    register-with-eureka: true # 是否向 Eureka Server 注册
    fetch-registry: true # 是否从 Eureka Server 获取注册表
  instance:
    instance-id: ${spring.application.name}:${spring.application.instance_id:${random.value}} # 实例 ID
    prefer-ip-address: true # 是否使用 IP 地址进行注册

spring:
  application:
    name: my-service # 应用名称

logging:
  level:
    com.netflix.eureka: INFO # 设置 Eureka 的日志级别
```
### 详细配置说明
#### Eureka Server 相关配置

- `server.port`: 指定 Eureka Server 运行的端口。
- `eureka.instance.hostname`: 指定 Eureka Server 的主机名。
- `eureka.client.register-with-eureka`: 指定 Eureka Server 是否向自己注册（通常为 `false`）。
- `eureka.client.fetch-registry`: 指定 Eureka Server 是否从自己获取注册表（通常为 `false`）。
- `eureka.server.enable-self-preservation`: 启用自我保护模式，防止网络故障时错误剔除服务实例。
#### Eureka Client 相关配置

- `server.port`: 指定应用服务运行的端口。
- `eureka.client.service-url.defaultZone`: 指定 Eureka Server 的地址，客户端将向此地址注册和获取服务实例信息。
- `eureka.client.register-with-eureka`: 指定客户端是否向 Eureka Server 注册。
- `eureka.client.fetch-registry`: 指定客户端是否从 Eureka Server 获取注册表信息。
- `eureka.instance.instance-id`: 指定服务实例的唯一标识，通常使用应用名和随机值组合。
- `eureka.instance.prefer-ip-address`: 指定是否使用 IP 地址进行注册，默认为 `false`，使用主机名。
#### Spring 应用相关配置

- `spring.application.name`: 指定应用的名称，在 Eureka 中注册时使用。
- `logging.level.com.netflix.eureka`: 设置 Eureka 相关日志的级别，便于调试和监控。
