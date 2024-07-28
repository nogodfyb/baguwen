### 使用 XML 映射文件进行模糊查询
假设我们有一个UserMapper接口和对应的 XML 映射文件。我们希望通过用户名进行模糊查询。
#### Mapper 接口
```
public interface UserMapper {
    List<User> selectUsersByUsername(String username);
}
```
#### XML 映射文件
```
<mapper namespace="com.example.mapper.UserMapper">

  <!-- 定义 resultMap -->
  <resultMap id="userResultMap" type="com.example.model.User">
    <id property="id" column="user_id"/>
    <result property="username" column="user_name"/>
    <result property="password" column="user_password"/>
  </resultMap>

  <!-- 模糊查询 -->
  <select id="selectUsersByUsername" parameterType="String" resultMap="userResultMap">
    SELECT * FROM users WHERE user_name LIKE CONCAT('%', #{username}, '%')
  </select>

</mapper>
```
### 使用注解进行模糊查询
如果你更喜欢使用注解方式，也可以通过在 Mapper 接口中使用注解来实现模糊查询。
#### Mapper 接口
```
import org.apache.ibatis.annotations.*;

import java.util.List;

public interface UserMapper {

    @Results(id = "userResultMap", value = {
        @Result(property = "id", column = "user_id", id = true),
        @Result(property = "username", column = "user_name"),
        @Result(property = "password", column = "user_password")
    })
    @Select("SELECT * FROM users WHERE user_name LIKE CONCAT('%', #{username}, '%')")
    List<User> selectUsersByUsername(String username);
}
```

- **LIKE语句**：用于模糊查询，CONCAT('%', #{username}, '%')用于在输入的用户名前后添加百分号（%），以实现模糊匹配。
- **#{username}**：表示参数占位符，MyBatis 会自动将传入的参数值替换到这个位置。
