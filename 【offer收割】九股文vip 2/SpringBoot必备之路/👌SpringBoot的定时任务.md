在 Spring Boot 中，定时任务可以使用`@Scheduled`注解来实现。<br />首先，你需要在主应用类或配置类上添加`@EnableScheduling`注解，以启用 Spring 的定时任务调度功能。
```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;

@SpringBootApplication
@EnableScheduling
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```
### 步骤 2：创建定时任务
可以在任何 Spring 管理的 Bean 中使用`@Scheduled`注解来定义定时任务。
#### 示例：
```
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class ScheduledTasks {

    // 每隔5秒执行一次
    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        System.out.println("Current Time: " + System.currentTimeMillis());
    }

    // 每隔5秒执行一次，上一次任务结束后再等待5秒
    @Scheduled(fixedDelay = 5000)
    public void reportCurrentTimeWithFixedDelay() {
        System.out.println("Current Time with Fixed Delay: " + System.currentTimeMillis());
    }

    // 第一次延迟1秒后执行，然后每隔5秒执行一次
    @Scheduled(initialDelay = 1000, fixedRate = 5000)
    public void reportCurrentTimeWithInitialDelay() {
        System.out.println("Current Time with Initial Delay: " + System.currentTimeMillis());
    }

    // 使用Cron表达式来定义任务执行时间
    @Scheduled(cron = "0 0 * * * ?")
    public void reportCurrentTimeWithCron() {
        System.out.println("Cron Scheduled Task: " + System.currentTimeMillis());
    }
}
```
### `@Scheduled`参数说明

- `fixedRate`: 以固定频率执行任务，单位为毫秒。任务开始后，间隔指定时间再次执行，不论上一次任务是否完成。
- `fixedDelay`: 以固定延迟执行任务，单位为毫秒。任务完成后，间隔指定时间再次执行。
- `initialDelay`: 第一次执行任务前的延迟时间，单位为毫秒。
- `cron`: 使用 Cron 表达式来定义任务执行时间。
### Cron 表达式
Cron 表达式是一种特殊的字符串格式，用于指定任务的执行时间。它由六个或七个空格分隔的字段组成，每个字段代表一个时间单位。
#### Cron 表达式字段：
```
秒 分 时 日 月 星期 [年]
```
#### 示例：

- `"0 0 * * * ?"`: 每小时执行一次。
- `"0 0 12 * * ?"`: 每天中午12点执行一次。
- `"0 0 12 * * MON-FRI"`: 每周一到周五中午12点执行一次。
- `"0 0/5 * * * ?"`: 每5分钟执行一次。
### 完整示例

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@SpringBootApplication
@EnableScheduling
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}

@Component
class ScheduledTasks {

    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        System.out.println("Current Time: " + System.currentTimeMillis());
    }

    @Scheduled(fixedDelay = 5000)
    public void reportCurrentTimeWithFixedDelay() {
        System.out.println("Current Time with Fixed Delay: " + System.currentTimeMillis());
    }

    @Scheduled(initialDelay = 1000, fixedRate = 5000)
    public void reportCurrentTimeWithInitialDelay() {
        System.out.println("Current Time with Initial Delay: " + System.currentTimeMillis());
    }

    @Scheduled(cron = "0 0 * * * ?")
    public void reportCurrentTimeWithCron() {
        System.out.println("Cron Scheduled Task: " + System.currentTimeMillis());
    }
}
```
