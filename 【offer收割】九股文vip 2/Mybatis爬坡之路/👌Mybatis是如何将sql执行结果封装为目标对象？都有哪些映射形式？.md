### 1. 自动映射
MyBatis 可以根据结果集的列名自动映射到目标对象的属性上。自动映射通常依赖于数据库列名和 Java 对象属性名之间的一致性。<br />假设有一个User类：
```
public class User {
    private int id;
    private String username;
    private String email;

    // Getters and Setters
}
```
对应的 SQL 查询：
```
<select id="selectUserById" parameterType="int" resultType="com.example.model.User">
    SELECT id, username, email FROM users WHERE id = #{id}
</select>
```
在这个例子中，MyBatis 会自动将结果集中的id列映射到User对象的id属性，username列映射到username属性，email列映射到email属性。
### 2. 手动映射
手动映射允许你明确指定数据库列和 Java 对象属性之间的映射关系。这可以通过<resultMap>元素来实现。
#### 示例：
定义一个resultMap：
```
<resultMap id="UserResultMap" type="com.example.model.User">
    <id property="id" column="id"/>
    <result property="username" column="username"/>
    <result property="email" column="email"/>
</resultMap>
```
使用resultMap：
```
<select id="selectUserById" parameterType="int" resultMap="UserResultMap">
    SELECT id, username, email FROM users WHERE id = #{id}
</select>
```
在这个例子中，UserResultMap明确指定了数据库列和User对象属性之间的映射关系。
### 3. 嵌套映射
嵌套映射用于处理复杂对象的映射，例如一个对象包含另一个对象。
#### 示例：
假设有一个Order类，其中包含一个User对象：
```
public class Order {
    private int id;
    private User user;
    private String orderNumber;

    // Getters and Setters
}
```
定义一个嵌套的resultMap：
```
<resultMap id="OrderResultMap" type="com.example.model.Order">
    <id property="id" column="id"/>
    <result property="orderNumber" column="order_number"/>
    <association property="user" javaType="com.example.model.User">
        <id property="id" column="user_id"/>
        <result property="username" column="username"/>
        <result property="email" column="email"/>
    </association>
</resultMap>
```
使用嵌套的resultMap：
```
<select id="selectOrderById" parameterType="int" resultMap="OrderResultMap">
    SELECT o.id, o.order_number, u.id AS user_id, u.username, u.email
    FROM orders o
    JOIN users u ON o.user_id = u.id
    WHERE o.id = #{id}
</select>
```
在这个例子中，OrderResultMap包含了一个嵌套的association元素，用于将User对象的属性映射到Order对象的user属性。
### 4. 嵌套查询
嵌套查询用于处理复杂对象的映射，类似于嵌套映射，但它通过执行额外的查询来获取嵌套对象的数据。
#### 示例：
定义一个嵌套查询的resultMap：
```
<resultMap id="OrderResultMap" type="com.example.model.Order">
    <id property="id" column="id"/>
    <result property="orderNumber" column="order_number"/>
    <association property="user" javaType="com.example.model.User" select="selectUserById" column="user_id"/>
</resultMap>
```
定义嵌套查询：
```
<select id="selectUserById" parameterType="int" resultType="com.example.model.User">
    SELECT id, username, email FROM users WHERE id = #{id}
</select>
```
使用嵌套查询的resultMap：
```
<select id="selectOrderById" parameterType="int" resultMap="OrderResultMap">
    SELECT id, order_number, user_id FROM orders WHERE id = #{id}
</select>
```
在这个例子中，OrderResultMap中的association元素使用select属性指定了一个额外的查询selectUserById，用于获取User对象的数据。
### 5. 集合映射
集合映射用于处理一对多的关系，例如一个对象包含一个集合。
#### 示例：
假设有一个Department类，其中包含一个List<User>：
```
public class Department {
    private int id;
    private String name;
    private List<User> users;

    // Getters and Setters
}
```
定义一个集合的resultMap：
```
<resultMap id="DepartmentResultMap" type="com.example.model.Department">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <collection property="users" ofType="com.example.model.User">
        <id property="id" column="user_id"/>
        <result property="username" column="username"/>
        <result property="email" column="email"/>
    </collection>
</resultMap>
```
使用集合的resultMap：
```
<select id="selectDepartmentById" parameterType="int" resultMap="DepartmentResultMap">
    SELECT d.id, d.name, u.id AS user_id, u.username, u.email
    FROM departments d
    LEFT JOIN users u ON d.id = u.department_id
    WHERE d.id = #{id}
</select>
```
在这个例子中，DepartmentResultMap中的collection元素用于将User对象的集合映射到Department对象的users属性。
