# 典型回答

[✅什么是负载均衡，有哪些常见算法？](https://www.yuque.com/hollis666/fo22bm/dw07di?view=doc_embed)

Dubbo 支持多种负载均衡策略，允许服务消费者根据不同的场景和需求选择适合的策略。在集群负载均衡时，Dubbo 提供了多种均衡策略，缺省为 random 随机调用。

**Dubbo 提供的是客户端负载均衡，即由 Consumer 通过负载均衡算法得出需要将请求提交到哪个 Provider 实例。**

目前支持的负载均衡算法如下：

![image.png](https://cdn.nlark.com/yuque/0/2024/png/5378072/1714196537849-3db807c1-96f0-4798-8d86-96fb0f5c2c65.png#averageHue=%23f4f3f2&clientId=u846e92d9-1080-4&from=paste&height=464&id=u513aafdf&originHeight=464&originWidth=1311&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88430&status=done&style=none&taskId=ub9c8fcdf-8866-42a7-a8cf-69f5f89499b&title=&width=1311)

### 加权随机负载均衡（Random Load Balance）

- 随机负载均衡策略会从所有可用的提供者中随机选择一个。权重设置高的提供者被选中的概率更大，可以通过权重来调整每个服务提供者的负载。
- 适用场景：适用于各个服务提供者之间处理能力大致相等的情况。
### 轮询负载均衡（Round Robin Load Balance）

- 轮询负载均衡策略会按顺序逐一选择每个服务提供者。类似于随机负载均衡，它也支持权重设置。
- 适用场景：适合处理能力均匀且相对稳定的服务环境。
### 最少活跃调用数负载均衡（Least Active Load Balance）

- 这种策略优先调用当前活跃调用数最少的提供者。如果有多个提供者具有相同的活跃调用数，会随机选择一个。活跃调用数指的是某个提供者当前正在处理的调用数量。
- 适用场景：适用于执行时间长短不一的服务调用，可以较好地处理突发请求，避免某一节点因长时间任务过多而响应缓慢。

### 最短响应优先负载均衡（ShortestResponseLoadBalance）

- 最短响应优先负载均衡会跟踪每个服务提供者的最近响应时间，并优先选择那些响应时间最短的服务提供者。
- 适用场景：适用于存在资源需求高且处理时间可能因负载而波动的应用

### 一致性哈希负载均衡（Consistent Hash Load Balance）

- 一致性哈希负载均衡通过哈希算法将调用请求映射到某个服务提供者上，同一请求总是映射到相同的提供者。这种策略非常适合有状态的服务。
- 适用场景：非常适合需要会话亲和性（Session Affinity）的场景，例如，在一个用户的会话期内需要由同一个服务实例来维护状态。

# 扩展知识

## 配置方法

在 Dubbo 中，负载均衡策略可以通过配置来指定，既可以在服务提供方配置，也可以在消费方进行配置。例如，在消费方配置随机负载均衡可以通过 XML 或注解如下：


```java
//服务端配置
<dubbo:service interface="com.example.SomeService" loadbalance="roundrobin"/>

//客户端配置
<dubbo:reference interface="com.example.SomeService" loadbalance="roundrobin" />
```

```java
@Service(loadbalance = "random")
public class SomeServiceImpl implements SomeService {
    // 实现方法
}
```

## 自定义负载均衡策略

Dubbo除了内置的负载均衡策略之外，还可以开发者自定义负载均衡策略。

具体方式直接参考官方文档就行了，写的很详细了：

[https://cn.dubbo.apache.org/zh-cn/docs/references/spis/load-balance/](https://cn.dubbo.apache.org/zh-cn/docs/references/spis/load-balance/)
