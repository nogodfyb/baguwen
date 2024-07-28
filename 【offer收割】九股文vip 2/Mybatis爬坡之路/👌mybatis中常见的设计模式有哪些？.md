### 1. 工厂模式（Factory Pattern）
**应用场景**：创建复杂对象时使用。

- **SqlSessionFactory**：MyBatis 使用SqlSessionFactory来创建SqlSession实例。SqlSessionFactory本身是由SqlSessionFactoryBuilder创建的。
- **示例**：
```
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```
### 2. 单例模式（Singleton Pattern）
**应用场景**：确保某个类只有一个实例，并提供一个全局访问点。

- **Configuration**：MyBatis 的Configuration类使用了单例模式，确保在一个SqlSessionFactory中只有一个Configuration实例。
- **示例**：
```
Configuration configuration = sqlSessionFactory.getConfiguration();
```
### 3. 代理模式（Proxy Pattern）
**应用场景**：为其他对象提供一种代理以控制对这个对象的访问。

- **Mapper 动态代理**：MyBatis 使用动态代理为 Mapper 接口生成实现类，从而在调用 Mapper 方法时执行 SQL 语句。
- **示例**：
```
UserMapper mapper = session.getMapper(UserMapper.class);
User user = mapper.selectUserById(1);
```
### 4. 模板方法模式（Template Method Pattern）
**应用场景**：定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。

- **Executor**：MyBatis 的Executor接口及其实现类使用了模板方法模式，定义了执行 SQL 语句的基本流程，而具体的执行细节由子类实现。
- **示例**：
```
public class BaseExecutor implements Executor {
    public <E> List<E> query(...) {
        // 模板方法，定义查询的基本流程
        // 具体查询细节由子类实现
    }
}
```
### 5. 建造者模式（Builder Pattern）
**应用场景**：将一个复杂对象的构建与其表示分离，使得同样的构建过程可以创建不同的表示。

- **SqlSessionFactoryBuilder**：MyBatis 使用SqlSessionFactoryBuilder来构建SqlSessionFactory实例。
- **示例**：
```
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```
### 6. 装饰器模式（Decorator Pattern）
**应用场景**：动态地给一个对象添加一些额外的职责。

- **插件机制**：MyBatis 的插件机制使用了装饰器模式，可以在执行器、参数处理器等核心组件上添加额外的功能。
- **示例**：
```
@Intercepts({@Signature(type = Executor.class, method = "update", args = {MappedStatement.class, Object.class})})
public class ExamplePlugin implements Interceptor {
    @Override
    public Object intercept(Invocation invocation) throws Throwable {
        // 在执行器的 update 方法上添加额外的功能
    }
}
```
### 7. 策略模式（Strategy Pattern）
**应用场景**：定义一系列算法，将每一个算法封装起来，并且使它们可以互相替换。

- **缓存策略**：MyBatis 的缓存机制使用了策略模式，不同的缓存实现可以互相替换。
- **示例**：
```
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```
### 8. 责任链模式（Chain of Responsibility Pattern）
**应用场景**：为请求创建一个接收者对象的链。

- **拦截器链**：MyBatis 的拦截器机制使用了责任链模式，可以在执行 SQL 语句的过程中通过一系列拦截器进行处理。
- **示例**：
```
public Object pluginAll(Object target) {
    for (Interceptor interceptor : interceptors) {
        target = interceptor.plugin(target);
    }
    return target;
}
```
### 9. 外观模式（Facade Pattern）
**应用场景**：为子系统中的一组接口提供一个一致的界面。

- **SqlSession**：MyBatis 的SqlSession提供了一个简化的接口，隐藏了内部复杂的实现细节。
- **示例**：
```
try (SqlSessionsession= sqlSessionFactory.openSession()) {
    UserMappermapper= session.getMapper(UserMapper.class);
    Useruser= mapper.selectUserById(1);
}
```
