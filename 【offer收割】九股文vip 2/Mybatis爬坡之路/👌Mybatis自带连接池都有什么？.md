MyBatis 自带的连接池实现主要有两种：`PooledDataSource`和`UnpooledDataSource`。这两种连接池实现提供了不同的特性和使用场景。
### 1.`UnpooledDataSource`
`UnpooledDataSource`是 MyBatis 提供的一个简单的、未池化的连接池实现。每次请求数据库连接时，`UnpooledDataSource`都会创建一个新的连接，而不是从连接池中获取连接。这种实现适用于对性能要求不高的小型应用或测试环境。<br />**配置示例：**
```
<environment id="development">
    <transactionManager type="JDBC"/>
    <dataSource type="UNPOOLED">
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mydatabase"/>
        <property name="username" value="root"/>
        <property name="password" value="password"/>
    </dataSource>
</environment>
```
### 2.`PooledDataSource`
`PooledDataSource`是 MyBatis 提供的一个简单的连接池实现。它维护了一组数据库连接，以便在需要时快速提供连接，而不需要每次都创建新的连接。`PooledDataSource`提供了基本的连接池功能，如连接池大小、连接超时等。<br />**配置示例：**
```
<environment id="development">
    <transactionManager type="JDBC"/>
    <dataSource type="POOLED">
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mydatabase"/>
        <property name="username" value="root"/>
        <property name="password" value="password"/>
        <property name="poolMaximumActiveConnections" value="10"/>
        <property name="poolMaximumIdleConnections" value="5"/>
        <property name="poolMaximumCheckoutTime" value="20000"/>
        <property name="poolTimeToWait" value="20000"/>
    </dataSource>
</environment>
```
#### `PooledDataSource`配置属性：

- `poolMaximumActiveConnections`: 连接池中最大活跃连接数。
- `poolMaximumIdleConnections`: 连接池中最大空闲连接数。
- `poolMaximumCheckoutTime`: 连接被检查出并返回之前的最长时间（以毫秒为单位）。
- `poolTimeToWait`: 在无法获取连接时，线程在重新尝试之前等待的时间（以毫秒为单位）。
### 选择连接池实现
虽然 MyBatis 提供了内置的连接池实现，但在生产环境中，通常推荐使用功能更强大、性能更好的第三方连接池，例如：

- **HikariCP**：以高性能和低延迟著称。
- **Apache DBCP**：一个成熟且广泛使用的连接池实现。
- **C3P0**：一个功能丰富的连接池实现，适用于需要高级功能的场景。

使用第三方连接池时，可以通过配置 MyBatis 使用外部的数据源来替代内置的连接池。<br />**使用 HikariCP 的配置示例：**

1. 添加 HikariCP 依赖（以 Maven 为例）：
```
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>5.0.1</version>
</dependency>
```

1. 配置 MyBatis 使用 HikariCP：
```
<environment id="development">
    <transactionManager type="JDBC"/>
    <dataSource type="POOLED">
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mydatabase"/>
        <property name="username" value="root"/>
        <property name="password" value="password"/>
        <property name="dataSource.type" value="com.zaxxer.hikari.HikariDataSource"/>
        <property name="dataSource.maximumPoolSize" value="10"/>
        <property name="dataSource.minimumIdle" value="5"/>
        <property name="dataSource.connectionTimeout" value="30000"/>
        <property name="dataSource.idleTimeout" value="600000"/>
        <property name="dataSource.maxLifetime" value="1800000"/>
    </dataSource>
</environment>
```
### 总结
MyBatis 提供了两种内置的连接池实现：`UnpooledDataSource`和`PooledDataSource`。`UnpooledDataSource`是一个简单的未池化实现，适用于小型应用或测试环境；`PooledDataSource`提供了基本的连接池功能，适用于对性能有一定要求的场景。在生产环境中，通常推荐使用功能更强大的第三方连接池，如 HikariCP、Apache DBCP 或 C3P0。通过配置 MyBatis 使用外部的数据源，可以轻松集成这些第三方连接池。
