## SpringScheduled笔记

### 一、基本使用

#### 1.确保导入了Spring的Scheduled包

#### 2.在启动类添加@EnableScheduling

```
@SpringBootApplication
@EnableScheduling
public class Application {

    public static void main(String[] args) throws JsonProcessingException {
        SpringApplication.run(Application.class);
    }
}
```

#### 3.在需要定时的方法前添加@Scheduled（）

- 内部可写Cron表达式
- 或者如fixedDelay，fixedRate等spring封装的时间格式

1.    **fixDelay:  自上一次调度后的时间间隔，需要等上一次任务完全执行完毕**
2.    **fixRate:  自上一次进入调度方法后的时间间隔**
3.    其他还有如**initialDelay（指定初始化延迟）**等属性

### 二、单线程与多线程的阻塞问题

SpringScheduled 调度时默认使用的是**单线程**。在多任务执行下会出现**阻塞**。

如果前一个任务一直未运行完毕，后一个任务到触发点时会**一直等待**前一个任务完成。

解决方案如下：重写配置文件，指定使用线程池的任务调度队列，替换默认的单线程池

```java
@Configuration
public class ScheduledBlockConfig implements SchedulingConfigurer {
    @Override
    public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {
        taskRegistrar.setScheduler(Executors.newScheduledThreadPool(10));
    }
}
```

### 三、Cron表达式

大多数调度都会使用到Cron表达式。

Cron表达式通常有两种书写方式： 常规的七值和缺省的六值

| 表达式        | 意义                                                         |
| ------------- | ------------------------------------------------------------ |
| * * * * * * * | 秒 分 时 日 月 星期 年                                       |
| * * * * * *   | 秒 分 时 日 月 星期 （年缺省）                               |
| /             | 指定增量    例如 秒域 的   2/4  则代表第2秒开始，间隔4秒执行 |
| *             | 配置任意时间段                                               |
| ？            | 仅在日期和星期域使用，代表不确定，或者缺省                   |
| -             | 出现在可以表示为时间段的域上 例如秒域 的 2-10 代表第2 至 10秒 |
| #             | 仅仅能在星期域上面使用    a#b 代表   a周的星期b              |
| ，            | 相当于集合  例如 秒域 的    2,10,11,13 代表第 2 10 11 10秒都会被触发 |

### 四、手动触发

此处不过多涉及，类型与quartz的触发方式，同样有context，job，scheduler等概念，相当于spring封装了一层。如果需要用到复杂的业务或者集群时，采用quartz更好。