### 1. 使用TCC（Try-Confirm/Cancel）
TCC模式需要自己实现Try、Confirm和Cancel逻辑，通常使用Spring AOP来管理事务。

1. 定义TCC接口：
```
public interface TccService {
    void tryMethod();
    void confirmMethod();
    void cancelMethod();
}
```

1. 实现TCC逻辑：
```
@Service
public class TccServiceImpl implements TccService {
    @Override
    public void tryMethod() {
        // 预留资源
    }

    @Override
    public void confirmMethod() {
        // 提交资源
    }

    @Override
    public void cancelMethod() {
        // 释放资源
    }
}
```

1. 使用AOP管理TCC事务：
```
@Aspect
@Component
public class TccAspect {
    @Around("@annotation(Tcc)")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        // 执行Try方法
        Object result = pjp.proceed();
        // 根据执行结果决定执行Confirm或Cancel方法
        return result;
    }
}
```

1. 使用TCC注解：
```
@Service
public class MyService {
    @Autowired
    private TccService tccService;

    @Tcc
    public void performDistributedTransaction() {
        tccService.tryMethod();
        // 其他业务逻辑
    }
}
```
