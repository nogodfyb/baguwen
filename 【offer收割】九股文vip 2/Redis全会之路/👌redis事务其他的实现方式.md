### 1. 乐观锁WATCH
用WATCH命令可以实现乐观锁机制。WATCH命令监视一个或多个键，如果在事务执行前这些键被修改，事务会被中止。
```
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

public class RedisOptimisticLockingExample {
    public static void main(String[] args) {
        try (Jedis jedis = new Jedis("localhost")) {
            String key = "mykey";
            String newValue = "newvalue";

            jedis.watch(key);
            String currentValue = jedis.get(key);

            Transaction transaction = jedis.multi();
            transaction.set(key, newValue);

            // 提交事务
            if (transaction.exec() == null) {
                System.out.println("Transaction aborted due to changes in the watched key.");
            } else {
                System.out.println("Transaction executed successfully.");
            }
        }
    }
}
```
### 2. Lua脚本
Redis支持用Lua脚本来实现多个命令的操作。Lua脚本在Redis中是原子的，整个脚本作为一个整体执行，不会被其他命令打断。
```
import redis.clients.jedis.Jedis;

public class RedisLuaScriptExample {
    public static void main(String[] args) {
        try (Jedis jedis = new Jedis("localhost")) {
            String script = "local current = redis.call('GET', KEYS[1]); " +
                            "if current == ARGV[1] then " +
                            "return redis.call('SET', KEYS[1], ARGV[2]) " +
                            "else return nil end";
            String key = "mykey";
            String oldValue = "oldvalue";
            String newValue = "newvalue";

            Object result = jedis.eval(script, 1, key, oldValue, newValue);
            if (result == null) {
                System.out.println("Script execution failed due to value mismatch.");
            } else {
                System.out.println("Script executed successfully.");
            }
        }
    }
}
```
### 3. 分布式锁
setNx确保只有一个客户端可以在同一时间内执行某些关键操作。
```
import redis.clients.jedis.Jedis;

public class RedisDistributedLockExample {
    public static void main(String[] args) {
        try (Jedis jedis = new Jedis("localhost")) {
            String lockKey = "lock_key";
            String lockValue = "unique_value";
            int lockExpireTime = 10000; // 10 seconds

            String result = jedis.set(lockKey, lockValue, "NX", "PX", lockExpireTime);
            if ("OK".equals(result)) {
                try {
                    // 执行关键操作
                    System.out.println("Lock acquired, executing critical operation.");
                } finally {
                    // 释放锁
                    if (lockValue.equals(jedis.get(lockKey))) {
                        jedis.del(lockKey);
                    }
                }
            } else {
                System.out.println("Failed to acquire lock.");
            }
        }
    }
}
```
### 4. 使用Redlock算法实现分布式锁
Redlock是Redis官方推荐的分布式锁算法，适用于需要高可靠性的分布式锁场景
```
import org.redisson.Redisson;
import org.redisson.api.RLock;
import org.redisson.api.RedissonClient;
import org.redisson.config.Config;

public class RedisRedlockExample {
    public static void main(String[] args) {
        Config config = new Config();
        config.useSingleServer().setAddress("redis://localhost:6379");

        RedissonClient redisson = Redisson.create(config);
        RLock lock = redisson.getLock("lock_key");

        try {
            boolean isLocked = lock.tryLock();
            if (isLocked) {
                try {
                    // 执行关键操作
                    System.out.println("Lock acquired, executing critical operation.");
                } finally {
                    lock.unlock();
                }
            } else {
                System.out.println("Failed to acquire lock.");
            }
        } finally {
            redisson.shutdown();
        }
    }
}
```
