![[Quartz--1.png]]
# 概述
---
## 简介
---
Quartz 是一个开源的任务调度框架，用于在 Java 中执行定时任务，支持任务持久化，并且可以处理复杂的调度逻辑。它是 Java 最常用的调度框架之一，广泛用于定时任务、批量处理、数据同步等场景。
## 需求
---
在项目开发中，可能需要在特定时间、周期执行任务，最简单的做法是使用 Java 自带的 **`Timer`** 和 **`ScheduledExecutorService`** 来实现定时任务，但它们有很大的局限性，无法满足复杂的业务需求：
- **无法灵活控制任务的执行时间**（比如，每月 1 号 12:00 执行）。
- **应用重启后，任务会丢失**（如服务器重启，定时任务需要手动重新启动）。
- **无法动态增删任务**（比如，想在运行时修改任务时间，只能重启应用）。
- **不支持分布式调度**（多个节点可能会重复执行任务）。
而 Quartz 解决了这些问题：
- **任务不会丢失**：任务可以存入数据库，应用重启后仍然会继续执行。
- **支持复杂调度**：支持 **Cron 表达式**，可以精确控制任务的执行时间。
- **任务可动态管理**：可以 **在运行时增删任务**，无需重启应用。
- **支持集群模式**：多个服务器可以 **共享任务调度**，不会重复执行。
- **支持并发执行**：可以同时运行多个定时任务，互不影响。
## 对比
---
- ***uartz***
	- **优点**：
		- **支持持久化**（任务存入数据库，应用重启后任务不会丢失）。
		- **支持集群**（多个节点不会重复执行任务）。
		- **支持 Cron 表达式**（支持复杂的时间调度）。
		- **可以动态修改任务**（无需重启应用）。
	- **缺点**：
		- **相对较重**（配置复杂，使用门槛较高）。
		- **不适合海量任务调度**（如百万级任务）。
- ***Spring Task***
	- **优点**：
		- **集成简单**，Spring 项目直接使用 `@Scheduled` 注解即可。
		- **轻量级**，无需额外依赖。
	- **缺点**：
		- **不支持任务持久化**，应用重启后任务会丢失。
		- **不支持集群**，适用于单机调度。
		- **不能动态修改任务**，只能改代码后重启应用。
- ***ScheduledExecutorService***
	- **优点**：
		- **JDK 自带**，零配置，开箱即用。
		- **支持并发执行**（比 `Timer` 更强大）。
	- **缺点**：
		- **不支持持久化**，任务不会记录到数据库中。
		- - **不支持 Cron 表达式**（只能按固定间隔执行）。
		- **不支持动态修改任务**。
- ***XXL-JOB***
	- **优点**：
		- **轻量级**，基于 REST API 调度，使用简单。
		- **支持分布式任务调度**，适合大规模任务调度。
		- **任务可视化管理**，支持动态修改任务。
	- **缺点**：
		- **依赖 XXL-JOB 组件**，需要部署调度中心。
		- **需要额外的运维工作**（需要监控调度中心的稳定性）。
- ***Elastic-Job***
	- **优点**：
		- **基于 Zookeeper 进行任务调度**，高可用，适用于分布式架构。
		- **任务分片**，适合大规模任务调度。
		- **支持动态修改任务**，无需重启应用。
	- **缺点**：
		- **依赖 Zookeeper**，需要额外的配置和维护成本。
		- **学习成本较高**，需要理解 Zookeeper + 分布式任务调度。
- ***TBSchedule***
	- **优点**：
		- **支持大规模并发任务调度**（淘宝开源）。
		- **支持任务分片**，适合高并发任务调度场景。
		- **支持任务动态调整**，可在线修改任务执行时间。
	- **缺点**：
		- **依赖 ZooKeeper**，需要额外配置。
		- **学习曲线较陡**，上手成本高。
- ***Kubernetes CronJob***
	- **优点**：
		- **云原生**，基于 Kubernetes，适用于微服务架构。
		- **高可用**，任务 Pod 运行在 Kubernetes 集群中，可自动调度。
		- **支持 Cron 表达式**，灵活设置任务执行时间。
		- **支持任务持久化**，任务定义存储在 Kubernetes API Server 中。
		- **支持集群调度**，任务可以分布到不同节点运行，避免单点故障。
		- **任务隔离**，每次执行都会创建新的 Pod，避免影响主应用。
	- **缺点**：
		- **启动开销大**，每次执行任务都会创建一个新的 Pod，适合 **低频任务**，不适合 **高频调度**（如每秒执行一次）。
		- **依赖 Kubernetes**，仅适用于 **Kubernetes 生态**，如果是单机应用，不适合使用。
		- **日志管理复杂**，任务执行完后 Pod 会被删除，默认情况下无法保留历史日志（但可以配置 `successfulJobsHistoryLimit`）。
		- **需要额外的监控**，如果任务失败，需要借助 Kubernetes 的 `Job` 机制进行重试和错误恢复。

| **框架**                       | **特点**                | **适用场景**          | **持久化** | **集群支持** | **Cron 表达式** | **动态修改任务** |
| ---------------------------- | --------------------- | ----------------- | ------- | -------- | ------------ | ---------- |
| **Quartz**                   | 功能强大，支持数据库存储任务        | 任务管理复杂，持久化调度      | ✅       | ✅        | ✅            | ✅          |
| **Spring Task**              | 轻量级，Spring 集成简单       | 适用于单机定时任务         | ❌       | ❌        | ✅            | ❌          |
| **ScheduledExecutorService** | JDK 自带，使用简单           | 适用于简单定时任务         | ❌       | ❌        | ❌            | ❌          |
| **XXL-JOB**                  | 支持 REST 接口调度，轻量级分布式任务 | 大型分布式任务调度         | ✅       | ✅        | ✅            | ✅          |
| **Elastic-Job**              | 基于 Zookeeper，分布式任务调度  | 适用于微服务架构          | ✅       | ✅        | ✅            | ✅          |
| **TBSchedule**               | 淘宝开源，适用于大规模任务调度       | 互联网业务大规模任务        | ✅       | ✅        | ✅            | ✅          |
| **Kubernetes CronJob**       | 云原生调度，任务以 Pod 形式运行    | 适用于 Kubernetes 集群 | ✅       | ✅        | ✅            | ✅          |

# 开始
---
## 安装
---
1. 在 Maven 项目中引入 Quartz
	```xml
	<dependency>
		<groupId>org.quartz-scheduler</groupId>
		<artifactId>quartz</artifactId>
		<version>2.3.2</version>
	</dependency>		
	```
2. 在 SpringBoot 中引入 Quartz
	```xml
	<dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter-quartz</artifactId>
	</dependency>
	```
## 使用
---
1. 在 Maven 项目中使用
	```java
	// 1. 定义 Job
	public class MyJob implements Job {
	    @Override
	    public void execute(JobExecutionContext context) {
	        System.out.println("Quartz 定时任务执行：" + new Date());
	    }
	}
	
	// 2. 配置调度器
	public class QuartzExample {
	    public static void main(String[] args) throws SchedulerException {
	    
	        // 2.1 创建调度器
	        Scheduler scheduler = StdSchedulerFactory.getDefaultScheduler();
	
	        // 2.2 定义 Job 任务详情
	        JobDetail job = JobBuilder.newJob(MyJob.class)
	                .withIdentity("myJob", "group1") // 设置名称和分组
	                .build();
	
	        // 2.3 创建触发器（每 5 秒执行一次）
	        Trigger trigger = TriggerBuilder.newTrigger()
	                .withIdentity("myTrigger", "group1") // 设置名称和分组
	                .startNow() // 立即执行
	                .withSchedule(SimpleScheduleBuilder.simpleSchedule()
	                        .withIntervalInSeconds(5) // 每 5 秒执行一次
	                        .repeatForever()) // 重复执行
	                .build();
	
	        // 2.4 绑定 Job 和 Trigger，并启动调度器
	        scheduler.scheduleJob(job, trigger);
	        scheduler.start();
	    }
	}	
	```
2. 在 SpringBoot 中使用




# 概念
---
## 核心组件
---
1. **Scheduler（调度器）**：
    - Quartz 的 **核心组件**，用于调度 Job 的执行。
    - 它可以管理多个 `JobDetail` 和 `Trigger`。
    - 通过 `SchedulerFactory` 创建。
2. **Job（任务）**：
    - 具体执行的任务，必须实现 `org.quartz.Job` 接口。
    - 任务执行时 `Job.execute(JobExecutionContext context)` 方法会被调用。
3. **JobDetail（任务详情）**：
    - 描述 `Job` 的详细信息，如 `Job` 的 **名称、分组、存储方式** 等。
4. **Trigger（触发器）**：
    - 触发任务执行的 **调度策略**，控制 `Job` 何时执行。
    - **常见类型**：
        - **SimpleTrigger**（简单触发器）：按照固定时间间隔执行。
        - **CronTrigger**（Cron 表达式触发器）：使用 `Cron` 表达式指定执行规则。
5. **JobStore（任务存储）**：
    - 用于保存 `JobDetail` 和 `Trigger` 的存储方式：
        - **RAMJobStore**：存储在内存，任务信息会在应用重启后丢失（默认）。
        - **JDBCJobStore**：存储在数据库，支持持久化，任务不会因应用重启而丢失。
6. **Listener（监听器）**：
    - 监听 `Job` 或 `Trigger` 的执行状态变化，如任务执行前、执行后等。




# 配置
---






# 附录
---
> [!website]
    > https://www.quartz-scheduler.org/

> [!code]
    > https://github.com/quartz-scheduler/quartz

> [!video]
    > 

> [!doc]
    > [官方文档](https://www.quartz-scheduler.org/documentation/)
    > 

