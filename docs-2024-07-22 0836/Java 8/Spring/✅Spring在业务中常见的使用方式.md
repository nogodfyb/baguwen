# 典型回答
## 通过IOC实现策略模式
很多时候，我们需要对不同的场景进行不同的业务逻辑处理，举个例子，譬如不同的场景需要不同支付方式，普通的逻辑是使用if-else，如下所示：
```java
public void use(Scene scene) {
	if(scene == TENCENT) {
    	doWeiXinPay();
	} else if (scene == ALIBABA) {
    	doAlipay();
	} else {
    	doDothing();
	}
}
```
如果sence越来越多，这种if-else显然非常不合适，就需要我们借助Spring来完成策略模式
```java
interface PayFacade extends InitializingBean {
    void pay();
    Scene getSupportScene();
    @Override
    default void afterPropertiesSet() throws Exception {
        PayFactory.register(getSupportScene(), this);
    }
}
@Component
class WeiXinPay implements PayFacade {

    @Override
    public void pay() {
        // by weixin
    }
    @Override
    public Scene getSupportScene() {
        return Scene.TENCENT;
    }
}
@Component
class AliPay implements PayFacade {

    @Override
    public void pay() {
        // by alipay
    }
    @Override
    public Scene getSupportScene() {
        return Scene.ALIBABA;
    }
}
class PayFactory {
    private static final Map<Scene, PayFacade> PAY_FACADE = Maps.newHashMap();
    public static void register(Scene scene, PayFacade payFacade) {
        PAY_FACADE.put(scene, payFacade);
    }
    public static PayFacade get(Scene scene) {
        return PAY_FACADE.get(scene);
    }
}
public void use(Scene scene) {
    PayFactory.get(scene);
}
```

这样子，调用方只需要调用PayFactory#get即可，不需要感知内部的实现细节和逻辑。

需要说明的是，这里使用了InitializingBean只是实现方式之一，还有其他的实现方式，如通过Autowired注解，BeanPostProcess等，这里不做过多赘述。
## 通过AOP实现拦截
很多时候，我们一般是通过注解和AOP相结合。大概的实现思路就是先定义一个注解，然后通过AOP去发现使用过该注解的类，对该类的方法进行代理处理，增加额外的逻辑，譬如参数校验，缓存，日志打印等等，如下代码所示：
### 参数校验
```java
@Target({ ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface ParamsCheck {
    boolean ignore() default false;
}

@Aspect
@Component
public class ValidateAspect {

    @Around("@annotation(com.hollis.annotation.ParamCheck)")
    public void ParamCheckAround(JoinPoint joinPoint) throws Throwable {
        // 判断是否需要校验
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        Method method = signature.getMethod();
        ParamsCheck paramsCheckAnnotation = method.getAnnotation(ParamsCheck.class);
        if (paramsCheckAnnotation != null && paramsCheckAnnotation.ignore()) {
            return joinPoint.proceed();
        }
        Object[] objects = joinPoint.getArgs();
        for (Object arg : objects) {
            if (arg == null) {
                break;
            }
           // 校验参数，失败抛出异常
        }
    }
}
```
### 缓存逻辑
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Cacheable {

    /**
     * 策略名称，需要保证唯一
     *
     * @return
     */
    public String keyName();

    /**
     * 超时时长，单位：秒
     *
     * @return
     */
    public int expireTime();

}
@Aspect
@Component
public class CacheableAspect {
    private static final Logger LOGGER = LoggerFactory.getLogger(FacadeAspect.class);
    @Around("@annotation(com.hollis.cache.Cacheable)")
    public Object cache(ProceedingJoinPoint pjp) throws Throwable {
        // 先查缓存，如果缓存中有值，直接返回。如果缓存中没有，先执行方法，再将返回值存储到缓存中。
    }
}
```
### 日志打印
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface OpLog {}
@Aspect
@Component
public class OpLogAspect {

    private static final Logger LOGGER = LoggerFactory.getLogger(OpLogAspect.class);

    @Autowired
    HttpServletRequest request;

    @Around("@annotation(com.hollis.annotation.OpLog)")
    public Object log(ProceedingJoinPoint pjp) throws Exception {

        Method method = ((MethodSignature)pjp.getSignature()).getMethod();
        OpLog opLog = method.getAnnotation(OpLog.class);

        Object response = null;

        try {
            // 目标方法执行
            response = pjp.proceed();
        } catch (Throwable throwable) {
            throw new Exception(throwable);
        } 

        LOGGER.info("log");
		return response;
    }
    
}
```
## 通过Event异步解耦
很多时候，可以一个单据状态的改变，要触发很多下游的行为，举个例子：

订单从确认订单变为支付成功，就要触发物流的发货，财务的记账，edm触达等等。但是如果订单状态改变同步触发下游的动作，这样对订单业务非常不友好，下游的每次变动都需要上游感知。所以，对于这种情况，我们就需要Event异步解藕。

具体说就是订单状态改变后，可以发出来一个Event事件，下游只感知这个Event事件，如果监听到这个事件，就去做自己对应的业务处理。如下代码所示：

```java
// 调用
@Component
public class OrderService {
    
    @Autowired
    private ApplicationEventPublisher publisher;

    public void payFinished() {
        PayFinishedEvent springEvent = new PayFinishedEvent();
        publisher.publishEvent(springEvent);
    }

}
// 监听
@Component
public class BillListener {

    @EventListener
    public void onListenPayFinished(PayFinishedEvent event) {
        // 记账
    }
}
@Component
public class EdmListener {

    @EventListener
    public void onListenPayFinished(PayFinishedEvent event) {
        // 发送站内信
    }
}
```
需要注意的是，SpringEvent有同步模式和异步模式，这里可以根据具体的业务进行配置
## 通过Spring管理事务
Spring的事务抽象了下游不同DataSource的实现（如，JDBC，Mybatis，Hibernate等），让我们不用再关心下游的事务提供方究竟是谁，直接启动事务即可。如下代码所示：
```java
@Component
public class TransactBean {

    @Autowired
    private TransactionTemplate transactionTemplate;

    public void test() {
        transactionTemplate.execute((status) -> {
            // doTransaction
            return null;
        });
    }
}
```
但是，我们在使用事务的时候，一定要注意，不能在事务中处理分布式缓存，RPC等操作，这样做有两个坏处，一个是RPC的RT很长，有可能引起长事务的问题，另一方面是如果事务执行失败进行回滚，RPC操作的调用时无法回滚的<br />当然对于声明式的事务也不能滥用，它有可能会导致一些问题：<br />[事务失效可能是哪些原因？](https://www.yuque.com/hollis666/fo22bm/bz0tulziboigw24b)
