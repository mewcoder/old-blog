---
layout:     post
title:      Spring Boot入门
subtitle:   
date:       2019-06-22
author:     ctrlcoder
header-img: 
catalog: true
tags:
    - Spring Boot
typora-root-url: ..
---

## 使用idea快速创建Spring Boot项目

![1562031738702](/img/assets_2019/1562031738702.png)

![1562031752252](/img/assets_2019/1562031752252.png)

![1562031761527](/img/assets_2019/1562031761527.png)

![1562031769621](/img/assets_2019/1562031769621.png)

![1562031779265](/img/assets_2019/1562031779265.png)

**通过idea快速创建的SpringBoot项目的pom.xml中已经导入了我们选择的web的起步依赖的坐标**

## SpringBoot工程热部署

我们在开发中反复修改类、页面等资源，每次修改后都是需要重新启动才生效，这样每次启动都很麻烦，浪费了大量的时间，我们可以在修改代码后不重启就能生效，在 pom.xml 中添加如下配置就可以实现这样的功能，我们称之为热部署。

```xml
<!--热部署配置-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

注意：IDEA进行SpringBoot热部署失败原因

出现这种情况，并不是热部署配置问题，其根本原因是因为Intellij IEDA默认情况下不会自动编译，需要对IDEA进行自动编译的设置，如下：

![](/I:/Java%2057%E6%9C%9F%20IDEA%E7%89%88/4.%E6%B5%81%E8%A1%8C%E6%A1%86%E6%9E%B6/%E9%98%B6%E6%AE%B54%20%E8%B5%84%E6%96%99/63.%E4%BC%9A%E5%91%98%E7%89%88(2.0)-%E5%B0%B1%E4%B8%9A%E8%AF%BE(2.0)-Spring%20Boot/89.SpringBoot/SpringBoot%E5%9F%BA%E7%A1%80/%E8%AE%B2%E4%B9%89(md,pdf)/img/19.png)

然后 Shift+Ctrl+Alt+/，选择Registry

![](/I:/Java%2057%E6%9C%9F%20IDEA%E7%89%88/4.%E6%B5%81%E8%A1%8C%E6%A1%86%E6%9E%B6/%E9%98%B6%E6%AE%B54%20%E8%B5%84%E6%96%99/63.%E4%BC%9A%E5%91%98%E7%89%88(2.0)-%E5%B0%B1%E4%B8%9A%E8%AF%BE(2.0)-Spring%20Boot/89.SpringBoot/SpringBoot%E5%9F%BA%E7%A1%80/%E8%AE%B2%E4%B9%89(md,pdf)/img/20.png)