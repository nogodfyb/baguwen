### 1. 初始化阶段
#### 1.1 加载全局配置文件 (mybatis-config.xml)

- **步骤**：
   - 使用Resources.getResourceAsStream方法读取 MyBatis 全局配置文件。
   - 解析配置文件中的<environments>、<mappers>等节点，加载数据源、事务管理器和 Mapper 文件。
- **示例代码**：
```
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
```
#### 1.2 创建 SqlSessionFactory

- **步骤**：
   - 使用SqlSessionFactoryBuilder构建SqlSessionFactory对象。
   - SqlSessionFactory会根据配置文件初始化 MyBatis 环境，包括数据源、事务管理器和 Mapper 文件。
- **示例代码**：
```
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```
### 2. 获取 SqlSession
#### 2.1 打开 SqlSession

- **步骤**：
   - 从SqlSessionFactory获取一个新的SqlSession实例。
   - SqlSession是 MyBatis 与数据库交互的核心对象，负责执行 SQL 语句、获取 Mapper 接口实例、管理事务等。
- **示例代码**：
```
try (SqlSession session = sqlSessionFactory.openSession()) {
```
### 3. Mapper 接口操作
#### 3.1 获取 Mapper 接口实例

- **步骤**：
   - 通过SqlSession获取 Mapper 接口的实现类实例。
   - MyBatis 会在运行时生成 Mapper 接口的实现类，并通过SqlSession获取其实例。
- **示例代码**：
```
UserMapper mapper = session.getMapper(UserMapper.class);
```
### 4. 执行 SQL 语句
#### 4.1 调用 Mapper 方法

- **步骤**：
   - 调用 Mapper 接口的方法，例如mapper.selectUserById(1);。
   - MyBatis 根据方法名称和参数查找对应的 SQL 语句。
- **示例代码**：
```
User user = mapper.selectUserById(1);
```
#### 4.2 参数处理

- **步骤**：
   - 将方法参数转换为 SQL 语句中的占位符（如#{id}）所需的值。
   - MyBatis 使用TypeHandler将 Java 类型转换为 JDBC 类型。
#### 4.3 执行 SQL 语句

- **步骤**：
   - MyBatis 使用 JDBC 通过数据源执行 SQL 语句。
   - 数据库返回结果集。
#### 4.4 结果映射

- **步骤**：
   - 将 SQL 查询结果集映射为 Java 对象。
   - 根据映射文件或注解中的配置，将结果集中的数据映射到 Java 对象的属性中。
### 5. 事务管理
#### 5.1 提交事务

- **步骤**：
   - 如果执行的数据操作需要提交事务，调用session.commit();提交事务。
   - 提交事务会将所有未提交的更改保存到数据库。
- **示例代码**：
```
session.commit();
```
#### 5.2 回滚事务

- **步骤**：
   - 如果操作过程中发生异常，需要回滚事务，调用session.rollback();回滚事务。
   - 回滚事务会撤销所有未提交的更改。
- **示例代码**：
```
session.rollback();
```
### 6. 关闭资源
#### 6.1 关闭 SqlSession

- **步骤**：
   - 操作完成后，调用session.close();关闭SqlSession，释放数据库连接。
   - 关闭SqlSession是确保资源释放的重要步骤。
- **示例代码**：
```
session.close();
```
### 流程示例代码
```
// 1. 加载配置文件并创建 SqlSessionFactory
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

// 2. 获取 SqlSession
try (SqlSession session = sqlSessionFactory.openSession()) {
    // 3. 获取 Mapper 接口实例
    UserMapper mapper = session.getMapper(UserMapper.class);

    // 4. 执行 SQL 语句
    User user = mapper.selectUserById(1);
    System.out.println(user);

    // 5. 提交事务（如果有数据修改操作）
    session.commit();
} catch (Exception e) {
    // 5. 回滚事务
    session.rollback();
    e.printStackTrace();
} finally {
    // 6. 关闭 SqlSession
    session.close();
}
```
### 总结
MyBatis 实现了从配置初始化到 SQL 执行再到事务管理和资源释放的完整过程。每一步都通过明确的接口和配置文件进行定义和执行，确保了与数据库交互的高效和可靠性。这些步骤包括加载配置文件、创建SqlSessionFactory、获取SqlSession、执行 SQL 语句、处理参数和结果映射、管理事务以及关闭资源。
