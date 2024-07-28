### 1. 使用参数化查询
参数化查询是防止 SQL 注入的主要手段。MyBatis 会自动将参数化查询中的参数进行预处理，避免直接拼接 SQL 字符串，从而防止 SQL 注入。
#### 示例：
```
<select id="selectUser" parameterType="int" resultType="com.example.model.User">
  SELECT * FROM users WHERE user_id = #{id}
</select>
```
在这个例子中，#{id}是一个占位符，MyBatis 会将其替换为一个安全的参数，而不是直接拼接到 SQL 字符串中。
### 2. 使用#{}占位符
MyBatis 提供了#{}占位符，用于安全地传递参数。与之相对的是${}，后者会直接将参数值嵌入到 SQL 语句中，容易导致 SQL 注入，因此应尽量避免使用${}。
#### 安全的使用方式：
```
<select id="selectUserByName" parameterType="string" resultType="com.example.model.User">
  SELECT * FROM users WHERE username = #{username}
</select>
```
#### 不安全的使用方式（应避免）：
```
<select id="selectUserByName" parameterType="string" resultType="com.example.model.User">
  SELECT * FROM users WHERE username = '${username}'
</select>
```
### 3. 使用 SQL 构建工具
MyBatis 提供了 SQL 构建工具（如SqlBuilder），可以帮助构建安全的 SQL 查询。
#### 示例：
```
import org.apache.ibatis.jdbc.SQL;

public String buildSelectUserById(final int id) {
    return new SQL() {{
        SELECT("*");
        FROM("users");
        WHERE("user_id = #{id}");
    }}.toString();
}
```
### 4. 使用 MyBatis 动态 SQL
MyBatis 动态 SQL 允许在 XML 文件中使用<if>、<choose>、<where>等标签来构建动态 SQL 查询。这些标签可以帮助你构建安全的动态 SQL 查询，而不必手动拼接字符串。
#### 示例：
```
<select id="findUsers" resultType="com.example.model.User">
  SELECT * FROM users
  <where>
    <if test="username != null">
      AND username = #{username}
    </if>
    <if test="email != null">
      AND email = #{email}
    </if>
  </where>
</select>
```
### 5. 使用 MyBatis 的@Param注解
在 Mapper 接口中，可以使用@Param注解来传递多个参数，这样可以更清晰地管理参数，并且确保参数化查询的安全性。
#### 示例：
```
public interface UserMapper {
    @Select("SELECT * FROM users WHERE username = #{username} AND email = #{email}")
    User findUserByUsernameAndEmail(@Param("username") String username, @Param("email") String email);
}
```
