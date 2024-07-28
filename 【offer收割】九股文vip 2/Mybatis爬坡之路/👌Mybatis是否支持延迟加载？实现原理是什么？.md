MyBatis 支持延迟加载（Lazy Loading）。延迟加载是一种优化技术，只有在真正需要使用数据的时候才进行加载，从而提高系统性能和响应速度。MyBatis 的延迟加载主要用于关联对象和集合。
### 延迟加载的实现原理
MyBatis 的延迟加载主要通过动态代理和 CGLIB 代理来实现。

1. **动态代理**：
   - MyBatis 使用 Java 的动态代理技术来创建对象的代理。
   - 当调用代理对象的方法时，代理对象会拦截调用，并在需要时触发真实对象的加载。
2. **CGLIB 代理**：
   - 对于没有接口的类，MyBatis 使用 CGLIB 生成子类代理。
   - 代理类会在调用方法时拦截调用，并在需要时触发真实对象的加载。
### 延迟加载的配置
延迟加载可以在 MyBatis 的全局配置文件（mybatis-config.xml）中进行配置：
```
<configuration>
    <settings>
        <!-- 启用延迟加载 -->
        <setting name="lazyLoadingEnabled" value="true"/>
        <!-- 启用代理延迟加载 -->
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>
</configuration>
```

- lazyLoadingEnabled：是否启用延迟加载。默认值是false。
- aggressiveLazyLoading：是否启用积极的延迟加载。默认值是false。如果设置为true，则只要调用任何延迟加载的属性，所有延迟加载的属性都会被加载。
### 延迟加载的使用
在 Mapper 文件中，可以通过fetchType属性来配置延迟加载：
```
<resultMap id="userResultMap" type="User">
    <id property="id" column="id"/>
    <result property="username" column="username"/>
    <result property="email" column="email"/>
    <association property="address" column="address_id" javaType="Address" fetchType="lazy">
        <id property="id" column="id"/>
        <result property="street" column="street"/>
        <result property="city" column="city"/>
    </association>
</resultMap>
```

- fetchType：可以设置为lazy或eager。lazy表示延迟加载，eager表示立即加载。
### 延迟加载的实现示例
以下是一个使用延迟加载的示例：
#### 配置文件（mybatis-config.xml）
```
<configuration>
    <settings>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>
</configuration>
```
#### Mapper 文件（UserMapper.xml）
```
<mapper namespace="com.example.UserMapper">
    <resultMap id="userResultMap" type="User">
        <id property="id" column="id"/>
        <result property="username" column="username"/>
        <result property="email" column="email"/>
        <association property="address" column="address_id" javaType="Address" fetchType="lazy">
            <id property="id" column="id"/>
            <result property="street" column="street"/>
            <result property="city" column="city"/>
        </association>
    </resultMap>

    <select id="selectUserById" resultMap="userResultMap">
        SELECT * FROM users WHERE id = #{id}
    </select>
</mapper>
```
#### Java 代码
```
try (SqlSession session = sqlSessionFactory.openSession()) {
    UserMapper mapper = session.getMapper(UserMapper.class);
    User user = mapper.selectUserById(1);

    // 此时 Address 还未加载
    System.out.println(user.getUsername());

    // 当访问 Address 属性时，触发延迟加载
    System.out.println(user.getAddress().getStreet());
}
```
### 总结
MyBatis 通过动态代理和 CGLIB 代理实现延迟加载。延迟加载可以通过全局配置文件进行启用，并在 Mapper 文件中通过fetchType属性进行具体配置。延迟加载可以提高系统性能，减少不必要的数据加载。
