循环依赖是指两个或多个Bean相互依赖，形成一个闭环。例如，Bean A 依赖于 Bean B，而 Bean B 又依赖于 Bean A。
### 1. 构造器注入的循环依赖
Spring 无法直接解决通过构造器注入（constructor injection）引起的循环依赖，因为在这种情况下，Spring 无法创建任何一个 Bean 实例而不先创建另一个 Bean 实例。这会导致一个无限循环。因此，通常建议避免在构造器注入中引入循环依赖。
### 2. Setter 注入的循环依赖
对于通过 setter 方法注入（setter injection）引起的循环依赖，Spring 采用三级缓存机制来解决问题。以下是具体的步骤：
#### 2.1 三级缓存机制
Spring 使用三级缓存来处理循环依赖：

1. **一级缓存（singletonObjects）**：
   - 用于存储完全初始化好的单例 Bean。类型：Map<String, Object>
2. **二级缓存（earlySingletonObjects）**：
   - 用于存储早期暴露的 Bean 实例，部分初始化的 Bean。Map<String, Object>
3. **三级缓存（singletonFactories）**：
   - 用于存储 Bean 工厂，主要用于创建 Bean 的代理对象。Map<String, ObjectFactory<?>>
#### 2.2 解决循环依赖的过程

1. **创建 Bean 实例**：
   - Spring 首先会尝试从一级缓存中获取 Bean 实例。如果没有找到，则会创建一个新的 Bean 实例（未初始化）。
2. **提前暴露 Bean 实例**：
   - 在 Bean 实例创建后，Spring 会将这个未初始化的 Bean 实例提前暴露到三级缓存中，以便其他 Bean 可以引用它。
3. **依赖注入**：
   - 对于每个依赖注入的属性，Spring 会检查是否已经有对应的 Bean 实例。如果依赖的 Bean 实例还没有完全初始化，Spring 会从三级缓存中获取早期暴露的 Bean 实例。
4. **初始化 Bean**：
   - 在所有依赖注入完成后，Spring 会对 Bean 进行初始化，包括调用afterPropertiesSet方法和自定义的初始化方法。
5. **放入一级缓存**：
   - 最后，Spring 会将完全初始化好的 Bean 实例放入一级缓存中，并从二级缓存和三级缓存中移除。
### 示例
假设有两个互相依赖的 Bean：A 和 B。
```
public class A {
    private B b;

    public void setB(B b) {
        this.b = b;
    }
}

public class B {
    private A a;

    public void setA(A a) {
        this.a = a;
    }
}
```
Spring 在处理这两个 Bean 的循环依赖时，会按照以下步骤进行：

1. 创建 A 的实例，并将其放入三级缓存。
2. 创建 B 的实例，并将其放入三级缓存。
3. 注入 B 到 A 中，此时从三级缓存中获取 B 的实例。
4. 注入 A 到 B 中，此时从三级缓存中获取 A 的实例。
5. 完成 A 和 B 的初始化。
6. 将 A 和 B 的完全初始化实例放入一级缓存。
### 总结
Spring 通过三级缓存机制解决了 setter 注入引起的循环依赖问题。对于构造器注入引起的循环依赖，建议重新设计代码结构以避免这种情况的发生。
