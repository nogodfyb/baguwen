可以映射。
### 1. 使用 MyBatis 内置的EnumTypeHandler
MyBatis 提供了内置的EnumTypeHandler，可以直接将枚举类型映射到数据库中的字符串或整数类型。
#### 映射到字符串
假设有一个UserStatus枚举类：
```
public enum UserStatus {
    ACTIVE,
    INACTIVE,
    DELETED
}
```
在数据库中，status字段是字符串类型。可以在 MyBatis 的 XML 配置文件中指定使用EnumTypeHandler：
```
<resultMap id="userResultMap" type="com.example.model.User">
    <id property="id" column="id"/>
    <result property="status" column="status" typeHandler="org.apache.ibatis.type.EnumTypeHandler"/>
</resultMap>
```
或者在注解中使用：
```
@Results({
    @Result(property = "id", column = "id"),
    @Result(property = "status", column = "status", typeHandler = EnumTypeHandler.class)
})
@Select("SELECT id, status FROM users WHERE id = #{id}")
User selectUserById(int id);
```
#### 映射到整数
假设UserStatus枚举类有一个整数值：
```
public enum UserStatus {
    ACTIVE(1),
    INACTIVE(2),
    DELETED(3);

    private final int value;

    UserStatus(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}
```
可以创建一个自定义的TypeHandler来处理枚举和整数之间的转换：
```
public class UserStatusTypeHandler extends BaseTypeHandler<UserStatus> {
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, UserStatus parameter, JdbcType jdbcType) throws SQLException {
        ps.setInt(i, parameter.getValue());
    }

    @Override
    public UserStatus getNullableResult(ResultSet rs, String columnName) throws SQLException {
        int value = rs.getInt(columnName);
        return UserStatus.fromValue(value);
    }

    @Override
    public UserStatus getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        int value = rs.getInt(columnIndex);
        return UserStatus.fromValue(value);
    }

    @Override
    public UserStatus getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        int value = cs.getInt(columnIndex);
        return UserStatus.fromValue(value);
    }
}
```
在枚举类中添加一个静态方法来根据整数值获取枚举实例：
```
public static UserStatus fromValue(int value) {
    for (UserStatus status : UserStatus.values()) {
        if (status.getValue() == value) {
            return status;
        }
    }
    throw new IllegalArgumentException("Unknown enum value: " + value);
}
```
然后在 MyBatis 配置文件中使用自定义的TypeHandler：
```
<resultMap id="userResultMap" type="com.example.model.User">
    <id property="id" column="id"/>
    <result property="status" column="status" typeHandler="com.example.typehandler.UserStatusTypeHandler"/>
</resultMap>
```
### 2. 使用 MyBatis 3.4.5+ 提供的EnumOrdinalTypeHandler和EnumTypeHandler
MyBatis 3.4.5 及以上版本提供了两个新的TypeHandler：EnumOrdinalTypeHandler和EnumTypeHandler。

- **EnumOrdinalTypeHandler**：将枚举的序数（ordinal）存储到数据库中。
- **EnumTypeHandler**：将枚举的名称（name）存储到数据库中。

可以在配置文件中指定使用这些TypeHandler：
```
<resultMap id="userResultMap" type="com.example.model.User">
    <id property="id" column="id"/>
    <result property="status" column="status" typeHandler="org.apache.ibatis.type.EnumOrdinalTypeHandler"/>
</resultMap>
```
3. 全局配置枚举处理器<br />可以在 MyBatis 全局配置文件中指定默认的枚举处理器：
```
<typeHandlers>
    <typeHandler javaType="com.example.model.UserStatus" handler="org.apache.ibatis.type.EnumTypeHandler"/>
</typeHandlers>
```
这样就不需要在每个resultMap或注解中重复指定TypeHandler。
