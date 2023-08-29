---
title: "PhoenixCore 业务并发框架"
date: 2023-06-29T20:45:48+08:00
draft: true
author: "Meng小羽"
authorLink: "https://debuginn.cn"
authorEmail: "debuginn@icloud.com"
keywords: "PhoenixCore,java"
comments: true
weight: 0
tags: ["PhoenixCore","java"]
categories: ["debuginn"]
image: ""
---

```bash
	____  _                      _         ____
	|  _ \| |__   ___   ___ _ __ (_)_  __  / ___|___  _ __ ___
	| |_) | '_ \ / _ \ / _ \ '_ \| \ \/ / | |   / _ \| '__/ _ \
	|  __/| | | | (_) |  __/ | | | |>  <  | |__| (_) | | |  __/
	|_|   |_| |_|\___/ \___|_| |_|_/_/\_\  \____\___/|_|  \__
```

近期，在业务系统上自研了一套并发框架，本篇文章主要讲一下原理，讲一下我们是如何设计及解决问题的，希望大家阅读后可以提出自己宝贵的意见 ♥️

## PhoenixCore 业务并发框架

不死鸟并发框架，自动构建有向图按照深度进行构建并发组并进行并发调用结果的框架。

产品站业务静态接口与动态接口都需要调用大量的后台服务进行获取数据进行业务编排，而各个并发调用之间又相互存在依赖，采用并发组设计拆解依赖，同时并发控制调用，BO
to DTO 采用统一的 Transfer 层进行设计，开发人员只需要关系定义每次调用事件的 Task 和 Transfer 代码逻辑的书写，直接返回业务数据。

**名词解释：**

- **PhoenixCore** 不死鸟核心（凤凰核心），此业务并发框架的名称；
- **Task** 在业务并发中定义一次调用，可以是 HTTP、DUBBO、或者是 Redis 获取、MySQL 读库操作；
- **Transfer** 在业务定义中是一个子业务模块的转换逻辑将 BO 数据转换为 DTO 数据；

## 设计原理

### 解耦逻辑 拆分调用

在程序中定义了若干个 Task 任务（这里每一个 Task 都可以是一个 HTTP、DUBBO、或者是各种中间件的读取操作）：

![拆分 Task](https://image.debuginn.cn/202306292003014.png)

这里我们将每一块业务的调用都进行了解耦拆封，将一些耗时且一次调用的逻辑封装成各个 Task，每个 Task 之间都做了数据资源的隔离，同时各个 Task 的采用统一的并发上下文 PhoenixContext；

### 单向依赖 划分成组

![单向依赖](https://image.debuginn.cn/202306292007655.png)

> 在框架设计中，**严格约束相互依赖与循环依赖**，这样的目的是划分调用并发组，最后将并发组依次调用。

- 若是采用串行调用的话，假设每个 Task 耗时在 50ms 左右，那么整个调用耗时会变的特别长，特别是在产品站业务中，若一个产品站需要调用 20 个服务来拼接数据返回给用户，那么相信用户会跳起来的；
- 若是将上图的依赖关系写死在代码中，随着业务的逐步复杂，代码的可维护性也会变得越来越差，最后不得不到不能维护的阶段，但是我们看到，依赖关系是存在的，我们怎么让依赖存在并且还将逻辑代码进行解除代码的耦合呢？ 答案就是上面所说的并发调用组设计。

![并发调用组](https://image.debuginn.cn/202306292017666.png)


### 怎么定义 Task

在框架设计之初，我们内部有两种方案，一种是继承抽象类实现的方式，Task 通过继承实现 PhoenixTask 类实现定义 Task，另外一种是采用注解的方式，将每个 Task 都定义成具有强约束的 Task ，并且把详细的描述信息在注解中定义，给开发人员一目了然的设计思路。

经过内部讨论，我们选择了 Java 优秀的语言特性，注解的方式声明定义 Task ，这样的定义使得代码简洁明了，也有利于通过 Spring Bean 收集工具来收集我们的定义。

```java
/**
 * PhoenixTaskAnnotation
 * 任务注解
 *
 * @author debuginn
 */
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface PhoenixTaskAnnotation {
    TaskEnum taskName();          // 任务名称
    TaskEnum[] beforeTaskName();  // 前置任务
    TaskBoEnum[] taskBoName();    // 任务生成的 BO 数据 用来校验冲突
}
```

PhoenixCore Task 注解定义非常简单：

- `taskName`用来标识任务名称，采用枚举，强制约束命名的唯一；
- `beforeTaskName`前置任务，前面讲到每个任务都是一次事件，区分前置任务是需要在并发调用的时候等待结果的返回，之后用来作为此 Task 调用的前置参数；
- `taskBoName`任务转化为 BO 的数据，通过接口调用或者中间件获取数据，转化为 Transfer 层使用的数据，在框架层做数据参数校验；

### 怎么定义 Trans

Trans 是 Transfer 的简称，同样和 Task 设计一样，也是采用注解的方式定义：

```java
/**
 * PhoenixTransAnnotation
 * 业务编排注解
 *
 * @author debuginn
 */
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface PhoenixTransAnnotation {

    TransEnum transName();     // 业务编排块名称
    PhoenixApi apiName();      // 执行 使用并发 API
    TaskEnum[] tasks();        // 依赖的 Task 任务
}
```

PhoenixCore Trans 注解定义同样非常简单：

- `transName`用来标识业务编排块名称；
- `apiName`用来区分这个 Transfer 业务编排是隶属于那个并发 API 所属的；
- `tasks`就是定义依赖的 Task 任务有哪些，一个业务编排可能会利用 n 个 Task 返回的 BO 数据进行编排；

> 大家在这里可能会比较疑惑，为啥 Task 中没有定义 apiName 而是 Transfer 中定义的，是因为在设计中，为了便于后续 Task 可以被 n 个并发 API 公用，这样在 Transfer 定义了 apiName，之后通过 tasks 定义依赖的 Task 就可以推断出这个 Task 目前是被哪一个并发 API 使用的。

