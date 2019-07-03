---
layout:     post
title:      SpringMVC
subtitle:  
date:       2019-06-20
author:     ctrlcoder
header-img: 
catalog: true
tags:
    - Java
typora-root-url: ..
---

## 三层架构和MVC

- B/S三层架构
  - 表现层：web层，一般使用MVC模型
  - 业务层：service层
  - 持久层：dao层
- MVC模型
  - Model：数据模型，JavaBean的类，用来进行数据封装
  - View：指JSP、HTML用来显示数据给用户
  - Controler：用来接收用户的请求，整个流程的控制器

## 1 SpringMVC概述

1. 是一种基于Java实现的MVC设计模型的请求驱动类型的轻量级WEB框架。

2. Spring MVC属于SpringFrameWork的后续产品，已经融合在Spring Web Flow里面。Spring 框架提供了构建 Web 应用程序的全功能 MVC 模块。

3. 使用 Spring 可插入的 MVC 架构，从而在使用Spring进行WEB开发时，可以选择使用Spring的SpringMVC框架或集成其他MVC开发框架，如Struts1(现在一般不用)，Struts2等。
   ![img](/img/assets_2019/1173674-20190412084707014-2051006696.png)

   ### SpringMVC 和 Struts2 对比

- 共同点：
  - 它们都是表现层框架，都是基于 MVC 模型编写的。
  - 它们的底层都离不开原始 ServletAPI。
  - 它们处理请求的机制都是一个核心控制器。
- 区别：
  - Spring MVC 的入口是 Servlet, 而 Struts2 是 Filter
  - Spring MVC 是基于方法设计的，而 Struts2 是基于类，Struts2 每次执行都会创建一个动作类。所以 Spring MVC 会稍微比 Struts2 快些。
  - Spring MVC 使用更加简洁,同时还支持 JSR303, 处理 ajax 的请求更方便(JSR303 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注解加在我们 JavaBean 的属性上面，就可以在需要校验的时候进行校验了。)
  - Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些，但执行效率并没有比 JSTL 提升，尤其是 struts2 的表单标签，远没有 html 执行效率高。

## SpringMVC的入门程序

1. 创建WEB项目，引入坐标（jar包）

2. 在web.xml中配置前端控制器DispatcherServlet

   ```xml
       <!-- 前端控制器（加载classpath:springmvc.xml 服务器启动创建servlet） -->
       <servlet>
           <servlet-name>dispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!-- 配置初始化参数，创建完DispatcherServlet对象，加载springmvc.xml配置文件 -->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:spring-mvc.xml</param-value>
           </init-param>
           <!-- 服务器启动的时候，让DispatcherServlet对象创建 -->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>dispatcherServlet</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   ```

3. 编写springmvc.xml的配置文件

   ```xml
    <!-- 配置视图解析器 -->
       <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <!-- JSP文件所在的目录 -->
           <property name="prefix" value="/pages/"/>
           <!-- 文件的后缀名 -->
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <!-- 设置静态资源不过滤 -->
       <mvc:resources location="/css/" mapping="/css/**"/>
       <mvc:resources location="/img/" mapping="/img/**"/>
       <mvc:resources location="/js/" mapping="/js/**"/>
       <mvc:resources location="/plugins/" mapping="/plugins/**"/>
   
       <!-- 开启对SpringMVC注解的支持 -->
       <mvc:annotation-driven/>
   
   ```

4. 编写index.jsp和HelloController控制器类

5. 在WEB-INF目录下创建pages文件夹，编写success.jsp的成功页面

6. 启动Tomcat服务器，进行测试

   

### ☆☆入门案例的执行流程

1. 当启动Tomcat服务器的时候，因为配置了load-on-startup标签，所以会创建DispatcherServlet对象，就会加载springmvc.xml配置文件

2. 开启了注解扫描，那么HelloController对象就会被创建

3. 从index.jsp发送请求，请求会先到达DispatcherServlet核心控制器，根据配置@RequestMapping注解找到执行的具体方法

4. 根据执行方法的返回值，再根据配置的视图解析器，去指定的目录下查找指定名称的JSP文件

5. Tomcat服务器渲染页面，做出响应

   ![img](/img/assets_2019/1173674-20190412092949541-139986443.png)

#### ☆☆☆SpringMVC的请求响应流程

![img](/img/assets_2019/1135193-20171005165210099-1015669941.png)

1. 客户端通过url发送请求

2. **核心控制器**Dispatcher Servlet接收到请求，通过**处理器映射器**HandlerMapping找到对应的handler，并将url映射的控制器controller返回给核心控制器。

3. 通过核心控制器找到系统或默认的**处理器适配器**HandlAdapter。

4. 由找到的适配器，调用实现对应接口的处理器，并将**ModelAndView**（数据和视图结合的对象）结果返回给**适配器**，再返回给**核心控制器**。

5. **核心控制器**将获取的ModelAndView对象传递给**View Resolver**视图解析器，解析后返回具体View（视图）。

6. 核心控制器对View进行渲染（即将模型数据填充至视图中），即将渲染结果返回给客户端。

**☆适配器作用**

　　　　SpringMVC涉及的映射器，视图解析器的作用不难理解，映射器负责将前端请求的url映射到配置的处理器，视图解析器将最终的结果进行解析，但中间为什么要经过一层适配器呢，为什么不经映射器找到controller后直接执行返回呢？

　　　　那是因为SpringMVC为业务处理器提供了多种接口实现（例如实现了Controller接口），而适配器就是用来根据处理器实现了什么接口，最终选择与已经注册好的不同类型的Handler Adapter进行匹配，并最终执行，例如，SimpleControllerHandlerAdapter是支持实现了controller接口的控制器，如果自己写的控制器实现了controller接口，那么SimpleControllerHandlerAdapter就会去执行自己写的控制器中的具体方法来完成请求。

Spring4.0之前需要配置处理器映射器、处理器适配器、视图解析器。



## 2 SpringMVC组件

- ### DispatcherServlet： 前端控制器

  - 用户请求到达前端控制器，它就相当于 mvc 模式中的 c，dispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet 的存在降低了组件之间的耦合性。

    

- ### HandlerMapping：处理器映射器

  - HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

- ### Handler：处理器

  - 它就是我们开发中要编写的具体业务控制器。由 DispatcherServlet 把用户请求转发到 Handler。由Handler 对具体的用户请求进行处理。

- ### HandlAdapter：处理器适配器

  - 通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行

    

- ### View Resolver：视图解析器

  - View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。

- ### View：视图

  - SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView等。我们最常用的视图就是 jsp。
  
  - 一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。
  
    

### @Controller注解

- 指示Spring类的实例是一个控制器类
- 需要在SpringMVC的配置文件中添加相应的扫描配置

### @RequestMapping注解

- 作用是建立请求URL和处理方法之间的对应关系
- 作用在类上：第一级的访问目录，不写的化相当于应用根目录，写的话需要以/开头
- 作用在方法上：第二级的访问目录
- 属性
  - value：默认属性，地址
  - method：指定请求的方式，RequestMethod.GET...
    - 组合注解：GetMapping，PostMapping，PutMapping，DeleteMapping，PatchMapping
  - params：指定限制请求参数的条件
  - headers：指定限制请求消息头的条件
  - 注意：以上条件出现2个以上时，是与的关系





## 3 参数绑定

### 绑定的机制

- 表单提交的数据都是k=v格式的 username=haha&password=123
- SpringMVC的参数绑定过程是把表单提交的请求参数，作为控制器中方法的参数进行绑定的
- 要求：提交表单的name和参数的名称是相同的

### 支持的数据类型

- SpringMVC 绑定请求参数是自动实现的，但是要想使用，必须遵循使用要求。指表单中的name属性

#### 1. 基本数据类型和String类型

- 要求我们的参数名称必须和控制器中方法的形参名称保持一致。(严格区分大小写)

2. 实体类型（POJO类或包装POJO类）

- 要求表单中参数名称和 POJO 类的属性名称保持一致。并且控制器方法的参数类型是 POJO 类型。
- 如果一个JavaBean类中包含其他的引用类型，那么表单的name属性需要编写成：对象.属性 例如：address.name

### 3. 复杂数据类型

1. 数组

- 表单中name属性相同，value不同；形参是数组，且名称相同

2. 集合

- 要求集合类型的请求参数必须在 POJO 中。在表单中请求参数名称要和 POJO 中集合属性名称相同
- 给 List 集合中的元素赋值，使用下标
- 给 Map 集合中的元素赋值，使用键值对

#### 4. 请求参数中文乱码的解决

- 在web.xml中配置Spring提供的过滤器类

```xml
<!-- 配置过滤器，解决中文乱码的问题 -->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!-- 指定字符集 -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <!-- 过滤所有请求 --> 
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

- 在 springmvc 的配置文件中可以配置，静态资源不过滤

```xml
<!-- location 表示路径，mapping 表示文件，**表示该目录下的文件以及子目录的文件 -->
<mvc:resources location="/css/" mapping="/css/**"/>
<mvc:resources location="/images/" mapping="/images/**"/>
<mvc:resources location="/scripts/" mapping="/javascript/**"/>
```

- get 请求方式：tomacat 对 GET 和 POST 请求处理方式是不同的，GET 请求的编码问题，要改 tomcat 的 server.xml配置文件

#### 5. 自定义类型转换器

1. 表单提交的任何数据类型全部都是字符串类型，但是后台定义Integer类型，数据也可以封装上，说明Spring框架内部会默认进行数据类型转换。
2. 如果想自定义数据类型转换，可以实现Converter的接口
3. 注册自定义类型转换器，在springmvc.xml配置文件中编写配置
4. 还是可以使用Formatter进行类型转换

#### 6. 使用 ServletAPI 对象作为方法参数

- HttpServletRequest

- HttpServletResponse

- HttpSession

- 等

  

## 4 常用的注解

### 1. RequestParam注解

把请求中的指定名称的参数传递给控制器中的形参赋值

- value：请求参数中的名称
- required：请求参数中是否必须提供此参数，默认值是true，必须提供

### 2. RequestBody注解

用于获取请求体的内容（注意：get方法不可以），直接使用得到是 key=value&key=value...结构的数据。

- required：是否必须有请求体，默认值是true

### 3. PathVariable注解

拥有绑定url中的占位符的。例如：url中有/delete/{id}，{id}就是占位符

- value：指定url中的占位符名称

- RESTful风格：**把请求参数变成请求路径的一种风格**

  

### 4. RequestHeader注解

用于获取请求消息头

- value：提供消息头名称
- required：是否必须有此消息头
- 在实际开发中一般不怎么用



### 5. CookieValue注解

用于把指定 cookie 名称的值传入控制器方法参数

- value：指定 cookie 的名称

- required：是否必须有此 cookie

  

### 6. ModelAttribute注解

该注解是 SpringMVC4.3 版本以后新加入的。它可以用于修饰方法和参数

- 出现在方法上，表示当前方法会在控制器的方法执行之前，先执行。它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法。 

- 出现在参数上，获取指定的数据给参数赋值。 

- value：用于获取数据的 key。key 可以是 POJO 的属性名称，也可以是 map 结构的 key
- 应用场景：当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据
  1. ModelAttribute 修饰方法带返回值
  2. ModelAttribute 修饰方法无返回值



### 7. SessionAttribute注解

用于多次执行控制器方法间的参数共享

- value：用于指定存入的属性名称

- type：用于指定存入的数据类型

- SpringMVC 将在Model中对应的属性暂存到 HttpSession 中

  

## 4 响应数据和结果视图

### 返回值分类

**参数类型**

- Model

  org.springframework.ui.Model是一个包含Map对象的SpringMVC类型，如果方法中添加了Model参数，则每次调用该请求处理方法时，SpringMVC都会创建Model对象，并将其作为参数传递给方法。

  - Model 是 spring 提供的一个接口，该接口有一个实现类 ExtendedModelMap，该类继承了 ModelMap，而 ModelMap 就是 LinkedHashMap 子类 

- HttpServletRequest、HttpServletResponse、HttpSession

返回类型

- ModelAndView

- Model

- Map

- View

- String

  - redirect 重定向
  - forward 请求转发

- Void

  

#### 1. 字符串

controller 方法返回字符串可以指定逻辑视图名，通过视图解析器解析为物理视图地址。

#### 2. void

在 controller 方法形参上可以定义 request 和 response，使用 request 或 response 指定响应结果
1、使用 request 转向页面

`request.getRequestDispatcher("/WEB-INF/pages/success.jsp").forward(request,  response);` 

2、也可以通过 response 页面重定向

`response.sendRedirect("testRetrunString")`

3、也可以通过 response 指定响应结果

`response.setCharacterEncoding("utf-8");` 

`response.setContentType("application/json;charset=utf-8");` 

`response.getWriter().write("**json 串"**);`

#### 3. ModelAndView

ModelAndView对象是Spring提供的一个对象，可以用来调整具体的JSP视图

- 方法
  - addObject(String attributeName,Object attributeValue)
    添加模型到该对象中，作用类似于request对象的setAttribute方法的作用、
    ${requestScope.attributeName}
  - setView(String viewName)
    设置逻辑视图名称，视图解析器会根据名称前往指定的视图
  
  `mv.addObject("username", "张三");` 
  
  `mv.setViewName("success");`

### 转发和重定向

#### forward转发

- 如果用了 formward：则路径必须写成实际视图 url，不能写逻辑视图。
- 它相当于“request.getRequestDispatcher("url").forward(request,response)”
- 使用请求转发，既可以转发到 jsp，也可以转发到其他的控制器方法

#### redirect重定向

- 它相当于“response.sendRedirect(url)”
- 如果是重定向到 jsp 页面，则 jsp 页面不能写在 WEB-INF 目录中，否则无法找到

### ResponseBody 响应 JSON 数据

- 该注解用于将 Controller 的方法返回的对象，通过 HttpMessageConverter 接口转换为指定格式的数据如：json,xml 等，通过 Response 响应给客户端
- Springmvc 默认用 MappingJacksonHttpMessageConverter 对 json 数据进行转换，需要加入jackson 的包（3个）
- POJO对象和JSON数据互相转换



## 5 文件上传

### SpringMVC传统方式文件上传

1. 导入文件上传的jar包
   - 使用 Commons-fileupload 组件实现文件上传，需要导入该组件相应的支撑 jar 包：Commons-fileupload 和commons-io
2. 编写文件上传的JSP页面
3. 编写文件上传的Controller控制器
   - SpringMVC框架提供了MultipartFile对象，该对象表示上传的文件，要求变量名称必须和表单file标签的name属性名称相同
4. 配置文件解析器对象
   - 配置文件解析器对象，要求id名称必须是multipartResolver

### SpringMVC跨服务器方式文件上传

1. 导入开发需要的jar包
   - jersey-core
   - jersey-client
2. 编写文件上传的JSP页面
3. 编写控制器
   定义图片服务器的请求路径
4. 配置文件解析器对象



## 6  异常处理

- Controller调用service，service调用dao，异常都是向上抛出的，最终有DispatcherServlet找异常处理器进行异常的处理

1. 自定义异常类
2. 自定义异常处理器
   - 实现HandlerExceptionResolver接口
3. 配置异常处理器

```xml
<bean id="sysExceptionResolver" class="cn.itcast.exception.SysExceptionResolver"/>
```



## 拦截器

### 拦截器的概述

1. SpringMVC框架中的拦截器用于对处理器进行预处理和后处理的技术。
2. 可以定义拦截器链，连接器链就是将拦截器按着一定的顺序结成一条链，在访问被拦截的方法时，拦截器链中的拦截器会按着定义的顺序执行。
3. 拦截器和过滤器的功能比较类似，有区别
   1. 过滤器是Servlet规范的一部分，任何框架都可以使用过滤器技术。
   2. 拦截器是SpringMVC框架独有的。
   3. **过滤器配置了/*，可以拦截任何资源。**
   4. **拦截器只会对控制器中的方法进行拦截**。
4. 拦截器也是AOP思想的一种实现方式
5. 想要自定义拦截器，需要实现HandlerInterceptor接口。

### 自定义拦截器

**实现HandlerInterceptor接口**

- preHandle方法是controller方法执行前拦截的方法
   可以使用request或者response跳转到指定的页面

- return true放行，执行下一个拦截器，如果没有拦截器，执行controller中的方法。
- return false不放行，不会执行controller中的方法

1. postHandle是controller方法执行后执行的方法，在JSP视图执行前。
   1. 可以使用request或者response跳转到指定的页面
   2. 如果指定了跳转的页面，那么controller方法跳转的页面将不会显示。
2. afterCompletion方法是在JSP执行后执行
   request或者response不能再跳转页面了



### 在springmvc.xml中配置拦截器类

```xml
<!-- 配置拦截器 -->
<mvc:interceptors>
    <mvc:interceptor>
    <!-- 哪些方法进行拦截 -->
    <mvc:mapping path="/user/*"/>
    <!-- 哪些方法不进行拦截
    <mvc:exclude-mapping path=""/>
    <!-- 注册拦截器对象 -->
    <bean class="cn.itcast.demo1.MyInterceptor1"/>
    </mvc:interceptor>
</mvc:interceptors>
```

- 单个拦截器的执行流程
  - pre->HandlerAdapter->post->DispacherSevlet->after
- 多个拦截器执行顺序
  ![img](/img/assets_2019/1173674-20190412212035443-2023243854.png)
  
  - pre顺序执行，post和after反序执行

