1. **解析**`**@Transactional**`**注解**：
   - Spring在启动时会扫描所有带有`@Transactional`注解的类和方法，并解析注解中的属性，生成`TransactionAttribute`对象。
2. **创建代理对象**：
   - Spring使用AOP创建代理对象，代理对象会拦截对目标方法的调用。
3. **事务拦截器拦截方法调用**：
   - 当代理对象的方法被调用时，`TransactionInterceptor`会拦截该调用。
4. **获取事务属性**：
   - `TransactionInterceptor`从`TransactionAttributeSource`获取当前方法的事务属性。
5. **事务管理器处理事务**：
   - 根据事务属性，`TransactionInterceptor`会通过`TransactionManager`开启一个新事务或加入一个现有事务。
6. **执行目标方法**：
   - 事务开始后，`TransactionInterceptor`会调用目标方法。
7. **提交或回滚事务**：
   - 如果目标方法执行成功，`TransactionInterceptor`会通过`TransactionManager`提交事务。
   - 如果目标方法抛出异常，根据事务属性中的回滚规则，`TransactionInterceptor`会决定是否回滚事务。
8. **清理事务上下文**：
   - 事务提交或回滚后，`TransactionSynchronizationManager`会清理事务上下文，确保线程的事务状态一致。
## 相关概念
### 1. AOP（面向切面编程）
Spring的声明式事务管理主要依靠AOP来实现。AOP允许在方法执行之前和之后添加额外的行为（如事务管理）。
### 2. 事务管理器（Transaction Manager）
Spring提供了多种事务管理器实现，如：

- `DataSourceTransactionManager`：用于JDBC数据源的事务管理。
- `JpaTransactionManager`：用于JPA的事务管理。
- `HibernateTransactionManager`：用于Hibernate的事务管理。

这些事务管理器负责具体的事务处理逻辑。
### 3. 事务拦截器（Transaction Interceptor）
`TransactionInterceptor`是AOP的一个拦截器，用于拦截带有`@Transactional`注解的方法。它会在方法执行之前和之后执行事务管理逻辑。
### 4. 事务属性（Transaction Attributes）
`@Transactional`注解的属性（如传播行为、隔离级别、超时、只读等）会被解析为`TransactionAttribute`对象。这些属性定义了事务的具体行为。
### 5. 事务同步管理器（Transaction Synchronization Manager）
`TransactionSynchronizationManager`用于管理事务的同步状态。它维护了当前线程的事务状态，并负责在事务开始、提交或回滚时调用相应的回调。
### 6. 事务代理（Transaction Proxy）
Spring使用代理模式来实现声明式事务管理。代理对象会拦截对目标方法的调用，并在调用目标方法之前和之后执行事务管理逻辑。
### 7. 事务上下文（Transaction Context）
事务上下文包含了当前事务的状态信息，如事务是否已经开始、是否需要回滚等。Spring会在事务开始时创建事务上下文，并在事务结束时清理它。
