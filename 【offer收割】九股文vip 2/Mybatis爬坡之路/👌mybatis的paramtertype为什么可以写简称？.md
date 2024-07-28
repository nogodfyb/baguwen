在 MyBatis 配置文件中，`parameterType`属性用于指定传递给 SQL 语句的参数类型。MyBatis 允许开发者使用全限定类名（例如`com.example.User`）或简称（例如`User`）来指定参数类型。这种灵活性源于 MyBatis 的类型别名机制。
### 类型别名机制
MyBatis 提供了一种类型别名机制，允许开发者为常用的 Java 类型定义简短的别名，从而简化配置文件中的类型声明。这种机制不仅可以减少冗长的类名，还可以提高配置文件的可读性。
#### 默认类型别名
MyBatis 内置了一些常用类型的默认别名，例如：

- `java.lang.Integer`->`int`
- `java.lang.String`->`string`
- `java.util.Date`->`date`
- `java.util.Map`->`map`
#### 自定义类型别名
开发者还可以在 MyBatis 配置文件中定义自定义类型别名。以下是一些定义和使用类型别名的示例：

1. **在 MyBatis 配置文件中定义类型别名**：
```
<typeAliases>
    <typeAlias alias="User" type="com.example.User"/>
    <typeAlias alias="Order" type="com.example.Order"/>
</typeAliases>
```

1. **在映射文件中使用类型别名**：
```
<select id="selectUserById" parameterType="User" resultType="User">
    SELECT * FROM users WHERE id = #{id}
</select>
```
在上述示例中，`User`被定义为`com.example.User`类的别名，因此在`parameterType`和`resultType`属性中可以直接使用`User`。
### 类型别名解析过程
MyBatis 在解析`parameterType`和`resultType`等属性时，会首先检查是否存在与之对应的类型别名。如果存在，则使用别名对应的类；如果不存在，则尝试直接使用属性值作为类名进行加载。<br />以下是 MyBatis 解析类型别名的简化流程：

1. **初始化类型别名**： 在 MyBatis 配置文件中，`<typeAliases>`元素用于定义类型别名。MyBatis 会在启动时解析这些配置，并将别名与对应的类映射存储在一个内部映射表中。
2. **解析参数类型**： 当 MyBatis 解析`parameterType`等属性时，会首先检查属性值是否在别名映射表中。如果找到匹配的别名，则使用对应的类；如果未找到匹配的别名，则尝试将属性值作为类的全限定名进行加载。
3. **加载类**： 如果属性值不是别名，MyBatis 会尝试使用`Class.forName`方法加载类。如果加载成功，则使用该类；如果加载失败，则抛出异常。
### 代码示例
```
public class TypeAliasRegistry {
    private final Map<String, Class<?>> typeAliases = new HashMap<>();

    public void registerAlias(String alias, Class<?> type) {
        typeAliases.put(alias.toLowerCase(Locale.ENGLISH), type);
    }

    public Class<?> resolveAlias(String alias) {
        String key = alias.toLowerCase(Locale.ENGLISH);
        Class<?> type = typeAliases.get(key);
        if (type == null) {
            try {
                type = Class.forName(alias);
            } catch (ClassNotFoundException e) {
                throw new RuntimeException("Could not resolve type alias: " + alias, e);
            }
        }
        return type;
    }
}
```
在这个示例中，`TypeAliasRegistry`类维护了一个别名到类的映射表，并提供了`registerAlias`方法来注册别名。`resolveAlias`方法用于解析别名或直接加载类。
### 总结
MyBatis 允许在`parameterType`等属性中使用简称，是因为它提供了类型别名机制。通过定义类型别名，开发者可以简化配置文件中的类型声明，提高可读性和可维护性。MyBatis 在解析这些属性时，会首先检查是否存在对应的类型别名，如果存在则使用别名对应的类，否则尝试直接加载类。
