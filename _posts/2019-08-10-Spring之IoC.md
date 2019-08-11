---
layout:     post
title:      Spring之IoC
subtitle:   
date:       2019-08-10
author:     ctrlcoder
header-img: 
catalog: true
tags:
    - Spring
typora-root-url: ..
---

## 概念

**IoC（控制反转）**：在使用 Spring 框架之后，对象的实例不再由调用者来创建，而是由Spring容器来创建，Spring 容器负责控制程序之间的关系，而不是由调用者的程序代码直接控制。这样，控制权由应用代码转移到了Spring 容器，控制权发生了反转，这就是Spring 的控制反转。

**DI（依赖注入）**：从Spring 容器的角度来看，Spring 容器负责将被依赖对象复制给调用者的成员变量，这相当于为调用者注入了它依赖的实例，这就是Spring 的依赖注入。

## 核心容器

![1565488224315](/img/assets_2019/1565488224315.png)



![1565488249270](/img/assets_2019/1565488249270.png)



### 对比

**BeanFactory接口**

BeanFactory 才是 Spring 容器中的顶层接口，

**ApplicationContext 接口**

ApplicationContext 是BeanFactory 的子接口，也被称为**应用上下文**，是另一种常用的 Spring核心容器。 不仅包含了BeanFactory的所有功能，还添加了对国际化、资源访问、事件传播等方面的支持。

**BeanFactory 和 ApplicationContext 的区别**

- 创建对象的时间点不一样。 

- ApplicationContext：只要一读取配置文件，默认情况下就会创建对象。 

- BeanFactory：什么使用什么时候创建对象。 

**ApplicationContext接口**

- FileSystemXmlApplicationContext 它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。

- **ClassPathXmlApplicationContext** 它是从类的根路径下加载配置文件 推荐使用这种

- **AnnotationConfigApplicationContext** 当我们使用注解配置容器对象时，需要使用此类来创建 Spring 容器。它用来读取注解。

   

## 入门程序

1. 创建接口和实现类

2. 配置文件
   
```xml
<bean id="userDao" class="com.zen.dao.impl.UserDaoImpl"></bean>
```

3. 测试程序
   
```java
     ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
     UserDao userDao = (UserDao) ac.getBean("userDao")
```



## Bean

### 常用标签

- id：给对象在容器中提供一个唯一标识。用于获取对象。 

- class：指定类的全限定类名。用于反射创建对象。默认情况下调用无参构造函数。 

- scope：指定对象的作用范围。 
  - singleton :默认值，单例的. 

  - prototype :多例的. 

  - request :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 request 域中. 

  - session :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 session 域中. 

  - global session :WEB 项目中,应用在 Portlet 环境.如果没有 Portlet 环境那么 globalSession 相当于 session. 

- init-method：指定类中的初始化方法名称。 

- destroy-method：指定类中销毁方法名称。 

![1562136501698](/img/assets_2019/1562136501698.png)

### 	基于XML的依赖注入（过时）

- **构造函数注入**
  - 类中需要提供一个对应参数列表的构造函数
  - 涉及的标签：`<constructor-arg>`

- **属性setter()方法注入**
  - 类中提供默认无参构造函数和setter方法
  - 涉及的标签：`<property>`

### **实例化** **Bean** **的三种方式** 

- **使用默认无参构造函数**
- 静态工厂方法（了解）
- 实例工厂方法（了解）

### 基于注解的装配

#### 		常用注解

- **用于创建对象的**
  - 相当于：`<bean id="" class="">`
  - **@Component**：把资源让 Spring 来管理。相当于在 xml 中配置一个 bean
  - **@Controller**
  - **@Service** 
  - **@Repository**
  - 如果不指定 value 属性，默认 bean 的 id 是当前类的类名，首字母小写
- **用于注入数据的**
  
  - 相当于：`<property name="" ref=""> |<property name="" value="">`
  - **@Autowired **自动按照类型注入
  - **@Qualifier**
    - 在自动按照类型注入的基础之上，再按照 Bean 的 id 注入
    - 必须和 @Autowired 一起使用；但是给方法参数注入时，可以独立使用
  - **@Resource**：直接按照 Bean 的 id 注入
  - **@Value**：注入基本数据类型和 String 类型数据的
- **用于改变作用范围的**

  - 相当于：`<bean id="" class="" scope="">`
  - **@Scope**：指定 bean 的作用范围
- **和生命周期相关的**
  - **相当于：`<init-method="" destroy-method="" />`
  - **@PostConstruct**：用于指定初始化方法
  - **@PreDestroy**：用于指定销毁方法

#### 注解配置 

- applicationContext.xml

```xml
<!-- 告知Spring框架在，读取配置文件，创建容器时，扫描注解，依据注解创建对象，并存入容器中 -->
<context:component-scan base-package="com.zen"></context:component-scan>
```

#### 纯注解方法

- **@Configuration**
  - 用于指定当前类是一个 spring 配置类，当创建容器时会从该类上加载注解。获取容器时需要使用AnnotationApplicationContext(有@Configuration 注解的类.class)。 

- **@ComponentScan**

  > 使用<context:component-scan>隐式地启用了<context:annotation-config>的功能。<context:annotation-config>的作用是让Spring具备解析@Component等注解的功能。当使用<context:component-scan>时，通常不需要包含<context:annotation-config>元素。

  - 用于指定 spring 在初始化容器时要扫描的包。作用和在 spring 的 xml 配置文件中的：

    `<context:component-scan base-package="com.itheima"/>`是一样的。 

    - 有注解（开启扫描）

    - - 有路径：扫描指定路径
      - 没路径：默认扫描当前配置类所在包及其子包下组件

    - 没有注解（不开启扫描）

- **@Bean**
  - 该注解只能写在方法上，表明使用此方法创建一个对象，并且放入 spring 容器。
  - name：给当前@Bean 注解方法创建的对象指定一个名称(即 bean 的 id）。

- **@PropertySource**
  - 用于加载.properties 文件中的配置

- **@Import**
  - 用于导入其他配置类，在引入其他配置类时，可以不用再写@Configuration 注解。当然，写上也没问 

  题。 

- **通过注解获取容器**

  ```java
  ApplicationContext ac =  new AnnotationConfigApplicationContext(SpringConfiguration.class); 
  ```

  

## 提高部分

###  BeanDefinition

 Class只是描述了一个类有哪些字段、方法，但是无法描述如何实例化这个bean！**如果说，Class类描述了一块猪肉，那么BeanDefinition就是描述如何做红烧肉。

在容器内部，这些bean定义被表示为BeanDefinition对象，包含以下元数据：

1. 包限定的类名：通常，定义bean的实际实现类。
2. Bean行为配置：它声明Bean在容器中的行为(范围、生命周期回调，等等)。
3. Bean依赖：对其他Bean的引用。
4. 对当前Bean的一些设置：例如，池的大小限制或在管理连接池的bean中使用的连接数。  



- Spring首先会扫描解析指定位置的所有的类得到Resources（可以理解为读取.Class文件）
- 然后依照TypeFilter和@Conditional注解决定是否将这个类解析为BeanDefinition
- 稍后再把一个个BeanDefinition取出实例化成Bean

### 后置处理器

![img](/img/assets_2019/v2-819a67daa7540d1a7bc5828b0c8e5dc9_hd.jpg)

#### 后置处理器分类

![img](/img/assets_2019/v2-9bd6efe7c86130553896c3744c338778_hd.jpg)

BeanFactoryPostProcessor是用来干预BeanFactory创建的，而BeanPostProcessor是用来干预Bean的实例化。

### 容器配置

Spring 的容器配置方式可以分为 3 种：

- Schema-based Container Configuration（XML配置）
- Annotation-based Container Configuration（注解）
- Java-based Container Configuration（@Configuration配置类）

Spring 支持的 2 种注入方式：

- 构造方法注入
- setter方法注入

所谓的3种编程风格其实指的是“将Bean交给Spring管理的3种方式”，可以理解为IOC，而2种注入方式即DI，是建立在IOC的基础上的。也就是说Spring的DI（依赖注入）其实是以IOC容器为前提。

![img](/img/assets_2019/v2-d1894656e55d9db98345b7f75c1c4260_hd.jpg)

### 自动装配

XML 实现自动装配可以分为两种：全局、局部。

所谓全局，就是在 XML 根标签末尾再加一个配置`default-autowire="byName"`，那么在此 XML 中配置的每一个`<bean>`都遵守这个自动装配模式，可选值有4个：

- byName
- byType
- constructor
- no

Spring 支持自动装配（全局/局部），把原先`<bean>`标签的职责单一化，只定义 bean，而依赖关系交给类本身维护

自动装配共 4 种，除了 no，其他 3 种各自对应两种注入方式：`byName/byType`对应 setter 方法注入，constructor 对应构造方法注入 

**@Autowired默认采用byType模式自动装配，如果找到多个同类型的，会根据名字匹配。都不匹配，则会报错**

为了弥补@Autowired不能指定名字的缺憾，Spring提供了@Qualifier注解

@Resource：和@Autowired几乎一样，但不能配合@Qualifier，因为它本身就可以指定beanName

### JavaConfig方式：@Configuration+@Bean

