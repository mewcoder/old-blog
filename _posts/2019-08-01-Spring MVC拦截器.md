---

layout:     post
title:     Spring MVC拦截器
subtitle:   
date:       2019-08-01
author:     ctrlcoder
header-img: 
catalog: true
tags:
    - Spring
typora-root-url: ..
---

### 拦截器的概述

1. SpringMVC框架中的拦截器用于对处理器进行预处理和后处理的技术。
2. 可以定义拦截器链，连接器链就是将拦截器按着一定的顺序结成一条链，在访问被拦截的方法时，拦截器链中的拦截器会按着定义的顺序执行。
3. 拦截器和过滤器的功能比较类似，有**区别**
   1. 过滤器是Servlet规范的一部分，任何框架都可以使用过滤器技术。
   2. 拦截器是SpringMVC框架独有的。
   3. 过滤器配置了/\*，可以拦截任何资源。
   4. **拦截器只会对控制器中的方法进行拦截。**
4. 拦截器也是AOP思想的一种实现方式
5. 想要自定义拦截器，需要实现HandlerInterceptor接口。



### 定义拦截器

创建类，**实现HandlerInterceptor接口，重写需要的方法**

```java
public interface HandlerInterceptor {
   /**
		preHandle方法是controller方法执行前拦截的方法 可以使用request或者response跳转到指定的页面
		return true放行，执行下一个拦截器，如果没有拦截器，执行controller中的方法。
		return false不放行，不会执行controller中的方法
   */
    boolean preHandle(HttpServletRequest request,
                      HttpServletResponse response,
                      Object handler)throws Exception;

   /**
	 	postHandle是controller方法执行后执行的方法，在视图执行前。
		可以使用request或者response跳转到指定的页面
		如果指定了跳转的页面，那么controller方法跳转的页面将不会显示。
   */

    void postHandle(HttpServletRequest request, 
                    HttpServletResponse response, 
                    Object handler, ModelAndView modelAndView) throws Exception;

   /**
  	postHandle方法是在JSP执行后执行，request或者response不能再跳转页面了
	整个请求处理完毕回调方法，即在视图渲染完毕时回调，如性能监控中我们可以在此记录结束时间并输出消耗时间，还可以进行一些资源清理，类似于try-catch-finally中的finally，但仅调用处理器执行链中
   */

    void afterCompletion( HttpServletRequest request,
                         HttpServletResponse response,
                         Object handler, Exception ex) throws Exception;
    
}
```



### 拦截器的配置

 要使自定义的拦截器生效，还需要在**Spring MVC的配置文件**中进行配置。

```xml
<!-- 配置拦截器 -->
<mvc:interceptors>
	<mvc:interceptor>
		<!-- 哪些方法进行拦截 -->
		<mvc:mapping path="/user/*"/>
		<!-- 哪些方法不进行拦截
		<mvc:exclude-mapping path=""/>
		-->
		<!-- 注册拦截器对象 -->
		<bean class="cn.itcast.demo1.MyInterceptor1"/>
	</mvc:interceptor>
</mvc:interceptors>
```

**可配置多个拦截器**

```xml
<!-- 配置拦截器 -->
<mvc:interceptors>
	<mvc:interceptor>
		<!-- 哪些方法进行拦截 -->
		<mvc:mapping path="/user/*"/>
		<!-- 哪些方法不进行拦截
		<mvc:exclude-mapping path=""/>
		-->
		<!-- 注册拦截器对象 -->
		<bean class="cn.itcast.demo1.MyInterceptor1"/>
		</mvc:interceptor>
	<mvc:interceptor>
		<!-- 哪些方法进行拦截 -->
		<mvc:mapping path="/**"/>
		<!-- 注册拦截器对象 -->
		<bean class="cn.itcast.demo1.MyInterceptor2"/>
	</mvc:interceptor>
</mvc:interceptors>
```



### 拦截器的执行流程

- **单个拦截器的执行流程**

  - preHandler

  - Handler

  - postHandler

  - DispatcherServlet

  - afterCompletion

    

- **多拦截器的执行流程**

![img](/img/assets_2019/1173674-20190412212035443-2023243854-1564906511127.png)

​	**preHandler顺序执行，postHandler和afterCompletion逆序执行**