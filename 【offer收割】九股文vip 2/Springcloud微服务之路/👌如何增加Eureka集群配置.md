配置 Eureka 集群可以提高服务注册中心的可用性和容错能力。Eureka 集群通常由多个 Eureka Server 组成，这些服务器相互注册和同步服务实例信息。
### 1. 配置 Eureka Server
假设我们要配置一个包含三个 Eureka Server 的集群。首先，为每个 Eureka Server 创建独立的 `application.yml` 文件，并进行相应的配置。
#### Eureka Server 1 (`application-server1.yml`)
```
server:
  port: 8761

eureka:
  instance:
    hostname: eureka-server-1
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka-server-2:8762/eureka/,http://eureka-server-3:8763/eureka/
  server:
    enable-self-preservation: true

spring:
  application:
    name: eureka-server
```
#### Eureka Server 2 (`application-server2.yml`)
```
server:
  port: 8762

eureka:
  instance:
    hostname: eureka-server-2
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka-server-1:8761/eureka/,http://eureka-server-3:8763/eureka/
  server:
    enable-self-preservation: true

spring:
  application:
    name: eureka-server
```
#### Eureka Server 3 (`application-server3.yml`)
```
server:
  port: 8763

eureka:
  instance:
    hostname: eureka-server-3
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka-server-1:8761/eureka/,http://eureka-server-2:8762/eureka/
  server:
    enable-self-preservation: true

spring:
  application:
    name: eureka-server
```
### 2. 启动 Eureka Server
确保每个 Eureka Server 的主机名（或 IP 地址）和端口正确配置，并且它们能够相互访问。启动每个 Eureka Server：
```
java -jar eureka-server.jar --spring.config.name=application-server1
java -jar eureka-server.jar --spring.config.name=application-server2
java -jar eureka-server.jar --spring.config.name=application-server3
```
### 3. 配置 Eureka Client
在您的服务应用中，配置 Eureka Client 以连接到 Eureka 集群。假设我们有一个服务应用，其 `application.yml` 
```
server:
  port: 8080

eureka:
  client:
    service-url:
      defaultZone: http://eureka-server-1:8761/eureka/,http://eureka-server-2:8762/eureka/,http://eureka-server-3:8763/eureka/
    register-with-eureka: true
    fetch-registry: true
  instance:
    instance-id: ${spring.application.name}:${spring.application.instance_id:${random.value}}
    prefer-ip-address: true

spring:
  application:
    name: my-service

logging:
  level:
    com.netflix.eureka: INFO
```
### 4. 启动 Eureka Client
```
java -jar my-service.jar
```
### 5. 验证集群配置
访问任意一个 Eureka Server 的管理界面（例如 `http://eureka-server-1:8761`），应该能够看到所有三个 Eureka Server 以及注册的服务实例。
