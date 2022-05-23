# Quartz学习中需要注意的地方

### Key

将 Job 和 Trigger 注册到 Scheduler 时，可以为它们设置 key，配置其身份属性。 Job 和 Trigger 的 key（JobKey 和 TriggerKey）可以用于将 Job 和 Trigger 放到不同的分组（group）里，然后基于分组进行操作。**同一个分组下的 Job 或 Trigger 的名称必须唯一，即一个 Job 或 Trigger 的 key 由名称（name）和分组（group）组成。**

### JobDataMap

在Job执行时，JobExecutionContext中的JobDataMap为我们提供了很多的便利。它是JobDetail中的JobDataMap和Trigger中的JobDataMap的并集，但是如果存在相同的数据，则后者会覆盖前者的值。

### Trigger 

**1. Trigger 的公共属性：**

- **jobKey**属性：当trigger触发时被执行的job的身份；
- **startTime**属性：设置trigger第一次触发的时间；该属性的值是java.util.Date类型，表示某个指定的时间点；有些类型的trigger，会在设置的startTime时立即触发，有些类型的trigger，表示其触发是在startTime之后开始生效。比如，现在是1月份，你设置了一个trigger–“在每个月的第5天执行”，然后你将startTime属性设置为4月1号，则该trigger第一次触发会是在几个月以后了(即4月5号)。
- **endTime**属性：表示trigger失效的时间点。比如，”每月第5天执行”的trigger，如果其endTime是7月1

**2. 优先级(priority)**

如果你的**trigger很多(或者Quartz线程池的工作线程太少)**，Quartz可能没有足够的资源同时触发所有的trigger；这种情况下，你可能希望控制哪些trigger优先使用Quartz的工作线程，要达到该目的，可以在trigger上设置priority属性。比如，你有N个trigger需要同时触发，但只有**Z个工作线程**，优先级最高的**Z个trigger会被首先触发**。**如果没有为trigger设置优先级，trigger使用默认优先级，值为5；**priority属性的值可以是任意**整数，正数、负数**都可以。

注意：只有同时触发的trigger之间才会比较优先级。10:59触发的trigger总是在11:00触发的trigger之前执行。

注意：如果trigger是可恢复的，在恢复后再调度时，优先级与原trigger是一样的。

**3. 错过触发(misfire Instructions)**

 不同类型的trigger，有不同的misfire机制。它们**默认都使用“智能机制(smart policy)”**，即根据trigger的类型和配置动态调整行为。当**scheduler启动的时候**，查询所有错过触发(misfire)的持久性trigger。然后根据它们各自的**misfire机制**更新trigger的信息。

**3. 日历示例(calendar)**

Quartz的Calendar对象(不是java.util.Calendar对象)可以在定义和存储trigger的时候与trigger进行关联。Calendar用于从trigger的调度计划中排除时间段。比如，可以创建一个trigger，每个工作日的上午9:30执行，然后增加一个Calendar，排除掉所有的商业节日。