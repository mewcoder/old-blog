---

layout:     post
title:     Spring Boot的过滤器
subtitle:   
date:       2019-08-03
author:     ctrlcoder
header-img: 
catalog: true
tags:
    - Spring Boot
typora-root-url: ..
---

### 定义Filter

```java
public class CsrfFilter implements Filter {

 
    @Override
    public void destroy() {
  
    }
 
    @Override
    public void doFilter(ServletRequest arg0, ServletResponse arg1, FilterChain arg2) throws IOException, ServletException {
	// 过滤掉不合法的请求，只允许get或post
	// 校验Referer头，运管所管理的服务器过来才合法
	// 校验Host头，防止http host头攻击
 
    }
 
    @Override
    public void init(FilterConfig arg0) throws ServletException {
        l 
    }

```



### 通过Bean配置

```java
Spring 提供了FilterRegistrationBean类，此类提供setOrder方法，可以为filter设置排序值，
让spring在注册web filter之前排序后再依次注册。

@Bean
public FilterRegistrationBean csrfFilterRegistration() {
  FilterRegistrationBean registration = new FilterRegistrationBean();
  registration.setFilter(csrfFilter());
  registration.addUrlPatterns("/*");
  registration.setName("csrfFilter");
  registration.setOrder(-200);//指定执行的顺序
  return registration;
}

```



### 通过注解配置

直接用@WebFilter就可以进行配置，同样，可以设置url匹配模式，过滤器名称等。这里需要注意一点的是@WebFilter这个注解是Servlet3.0的规范，并不是Spring boot提供的。除了这个注解以外，我们还需在配置类中加另外一个注解：@ServletComponetScan，指定扫描的包。

```java
@Component
@WebFilter(urlPatterns = "/webapi/*", filterName = "authFilter")
public class AuthFilter implements Filter {
    ......
}
```

@WebFilter这个注解并没有指定执行顺序的属性，其执行顺序依赖于Filter的名称，是根据Filter类名（注意不是配置的filter的名字）的字母顺序倒序排列，并且@WebFilter指定的过滤器优先级都高于FilterRegistrationBean配置的过滤器。