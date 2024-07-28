Spring 事务传播行为（Transaction Propagation Behavior）定义了在事务方法被调用时，事务如何传播。Spring 提供了多种传播行为，允许开发者定义事务的边界和行为。这些传播行为通过@Transactional注解的propagation属性来配置。
### 传播行为类型
以下是 Spring 提供的主要传播行为类型：

1. **REQUIRED**（默认值）
   - 如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
2. **REQUIRES_NEW**
   - 总是创建一个新的事务。如果当前存在事务，则挂起当前事务。
3. **SUPPORTS**
   - 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务方式继续执行。
4. **NOT_SUPPORTED**
   - 总是以非事务方式执行，如果当前存在事务，则挂起当前事务。
5. **MANDATORY**
   - 如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。
6. **NEVER**
   - 总是以非事务方式执行，如果当前存在事务，则抛出异常。
7. **NESTED**
   - 如果当前存在事务，则在嵌套事务内执行；如果当前没有事务，则创建一个新的事务。
   - 注意：嵌套事务依赖于底层的事务管理器支持，如 JDBC 的Savepoints。
### 示例
假设有两个服务类ServiceA和ServiceB，可以通过不同的传播行为来控制事务的传播。
```
@Service
public class ServiceA {
    
    @Autowired
    private ServiceB serviceB;

    @Transactional(propagation = Propagation.REQUIRED)
    public void methodA() {
        // Some database operations
        serviceB.methodB();
        // Some more database operations
    }
}

@Service
public class ServiceB {

    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void methodB() {
        // Some database operations
    }
}
```
ServiceA的methodA方法使用REQUIRED传播行为，这意味着如果methodA被调用时没有事务存在，它将创建一个新的事务。如果methodA被调用时已经有一个事务存在，它将加入该事务。<br />ServiceB的methodB方法使用REQUIRES_NEW传播行为，这意味着它总是会创建一个新的事务。如果methodB被调用时已经有一个事务存在，该事务将被挂起，直到methodB的事务完成。
### 事务传播行为的选择

- **REQUIRED**：大多数情况下使用的默认传播行为，适用于大多数需要事务管理的方法。
- **REQUIRES_NEW**：适用于需要独立事务的情况，例如记录日志、审计等操作，即使外层事务回滚，这些操作也应该提交。
- **SUPPORTS**：适用于可选事务的情况，例如读取操作，可以在事务内或事务外执行。
- **NOT_SUPPORTED**：适用于不需要事务的情况，例如调用外部服务。
- **MANDATORY**：适用于必须在事务内执行的方法，例如严格依赖事务上下文的操作。
- **NEVER**：适用于必须在非事务上下文中执行的方法。
- **NESTED**：适用于需要嵌套事务的情况，例如需要在一个事务内执行多个子事务，并且可以单独回滚子事务。
