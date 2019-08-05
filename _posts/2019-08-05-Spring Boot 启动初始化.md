---
layout:     post
title:      Spring Boot 启动初始化
subtitle:   
date:       2019-08-05
author:     ctrlcoder
header-img: 
catalog: true
tags:
    - Spring Boot
typora-root-url: ..
---

### ApplicationRunner与CommandLineRunner

如果需要在`SpringApplication`启动时执行一些特殊的代码 ，你可以实现`ApplicationRunner`或`CommandLineRunner`接口， 这两个接口工作方式相同，都只提供单一的`run`方法，而且该方法仅在`SpringApplication.run(…)`完成之前调用，更准确的说是在构造`SpringApplication`实例完成之后调用`run()`的时候，具体分析见后文 ，所以这里将他们分为一类。
#### ApplicationRunner
构造一个类实现`ApplicationRunner`接口
```java
@Component
@Order(value = 10)
public class AgentApplicationRun2 implements ApplicationRunner {
	@Override
	public void run(ApplicationArguments applicationArguments) throws Exception {
	}
```

#### CommandLineRunner
对于这两个接口而言，我们可以通过`Order`注解来指定调用顺序， `@Order()` 中的值越小，优先级越高
```java
@Component
@Order(value = 11)
public class AgentApplicationRun implements CommandLineRunner {

	@Override
	public void run(String... strings) throws Exception {

	}
}
```
#### 两者区别联系

- 当然我们也可以同时使用`ApplicationRunner`和 `CommandLineRunner`，默认情况下前者比后者先执行，但是这没有必要，使用一个就好了

- `ApplicationRunner`中`run`方法的参数为`ApplicationArguments`，`而CommandLineRunner`接口中`run`方法的参数为`String`数组。想要更详细地获取命令行参数，那就使用`ApplicationRunner`接口。