MyBatis 是一个优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis消除了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的工作。MyBatis可以使用简单的XML或注解来配置和映射原生类型、接口和Java POJO（Plain Old Java Objects）到数据库中的记录。
### MyBatis 特性

1. **SQL映射**：MyBatis 允许你直接编写 SQL 语句，并将这些 SQL 语句与 Java 方法进行映射。你可以在 XML 文件中定义 SQL 语句，也可以使用注解直接在 Java 类中定义。
2. **动态 SQL**：MyBatis 提供了一种强大的动态 SQL 生成功能，可以根据条件动态生成 SQL 语句，避免了拼接 SQL 字符串的麻烦。
3. **高级映射**：MyBatis 支持复杂的结果集映射，可以将查询结果映射到复杂的 Java 对象结构中。
4. **缓存支持**：MyBatis 内置了一级缓存和二级缓存，能够显著提高查询性能。
5. **插件机制**：MyBatis 提供了灵活的插件机制，可以在执行 SQL 语句之前或之后进行拦截和处理。
