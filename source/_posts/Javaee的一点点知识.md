---
layout: titile
title: SSM(Spring+SpringMVC+MyBatis)
date: 2018-12-02 16:45:19
tags: Javaee
categories: Java
---
# SSM
## Spring和bean
<beans…/>元素是Spring配置文件的根元素，<beans…/>元素可以包含多个<bean…/>子元素，每个<bean…/>元素可以定义一个Bean实例，每一个Bean对应Spring容器里的一个Java实例定义Bean时通常需要指定两个属性。
Id：确定该Bean的唯一标识符，容器对Bean管理、访问、以及该Bean的依赖关系，都通过该属性完成。Bean的id属性在Spring容器中是唯一的。
 <!--more--> 
Class：指定该Bean的具体实现类。注意这里不能使接口。通常情况下，Spring会直接使用new关键字创建该Bean的实例，因此，这里必须提供Bean实现类的类名。
Spring容器集中管理Bean的实例化，Bean实例可以通过BeanFactory的getBean(String  beanid)方法得到。BeanFactory是一个工厂，程序只需要获取BeanFactory引用，即可获得Spring容器管理全部实例的引用。程序不需要与具体实例的实现过程耦合。大部分Java EE应用里，应用在启动时，会自动创建Spring容器，组件之间直接以依赖注入的方式耦合，甚至无须主动访问Spring容器本身。
当我们在配置文件中通过<bean id=”xxxx” class=”xx.XxClass”/>方法配置一个Bean时，这样就需要该Bean实现类中必须有一个无参构造器
![sd](/Javaee的一点点知识/p1.png "bean和spring容器的关系")
## SpringMvc
Spring MVC属于SpringFrameWork的后续产品，已经融合在Spring Web Flow里面。Spring MVC 分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。
## MyBatis
MyBatis 本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。MyBatis是一个基于Java的持久层框架。iBATIS提供的持久层框架包括SQL Maps和Data Access Objects（DAO）MyBatis 消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis 使用简单的 XML或注解用于配置和原始映射，将接口和 Java 的POJOs（Plain Old Java Objects，普通的 Java对象）映射成数据库中的记录。
# Src
里面有五个主要的包，action，mapper,model,service,service.impl
## action包
其实就是servlet，servlet就是小服务程序或服务连接器，交互式地浏览和修改数据，生成动态web。前端点击后通过href连接传入到这个包中的文件
里面有很多个注解：
1.@Controller：注明这个类为控制器
2.@RequestMapping：有很多属性，**value**：String[]类型，将URL映射到方法上或者类上。**method**：用来支出该方法只处理哪些类的请求。method=RequestMethod.POST，仅仅处理post请求。**consumes**：指定处理请求的提交类型（Content-Type），String[]类型。**produces**：指定返回的内容类型，返回的内容类型必须是request请求头（Accept）中所包含的类型。String[] 类型。**params**：指明了request请求中包含了某些参数值时，才让该方法处理。String[] 类型。**headers**：指明了request请求中包含了某些指定的header值，才让该方法处理。String[] 类型。
3.@RequestParam：用于将request请求参数中的值，赋给方法中的形参。以下是属性说明**value**：请求参数的名称。是默认属性，当注解只有一个属性时，可以省略。String类型。**required**：参数是否必须。boolean类型。**defaultValue**：如果没有传参而使用的默认值。String类型。
4.@Autowired：自动装配，Spring会自动将我们标记为@Autowired的元素装配好，注入Bean的方法。
HttpServletRequest：当客户端通过HTTP协议访问服务器时，HTTP请求头中的所有信息都封装在这个对象中，通过这个对象提供的方法，可以获得客户端请求的所有信息。
HttpSession：一个session就是一系列某用户和服务器间的通讯。服务器有能力分辨出不同的用户。一个session的建立是从一个用户向服务器发第一个请求开始，而以用户显式结束或session超时为结束。void setAttribute(String name, Object value)：用来存储一个对象，也可以称之为存储一个域属性，例如：session.setAttribute(“xxx”, “XXX”)，在session中保存了一个域属性，域属性名称为xxx，域属性的值为XXX
## mapper包
包含接口类和对应的接口类方法的数据库实现操作(xml)
## model包
实体类的定义在这
## service包
接口类
## service.impl包
实现service包中的接口类，具体的方法还是调用Mapper类中的对数据库中数据进行操作
## config包
里卖包含三个文件：spring-mvc.xml,mabatis-config.xml,spring.common.xml
**spring.common.xml**：主要是封装类和默认对象的属性，1.有数据源的配置，即连接数据库。2.mybatis的SqlSession的工(value="classpath:config/mybatis-config.xml")。3. mybatis自动扫描加载Sql映射文件/接口 : MapperScannerConfigurer sqlSessionFactory basePackage:指定sql映射文件/接口所在的包（自动扫描）(value="com.dhee.mapper")。4.事务管理。5.使用声明式事务
**spring-mvc.xml**：1.springMVC扫描的带有@Controller包不能被spring监听器所扫描到，否则会将springMVC中的请求路径给过滤掉，所以(context:component-scan base-package="com.dhee.action")。2.视图解析器。3.防止处理静态请求
**mabatis-config.xml**：***与Spring相关的配置不在这个文件中***，它里面包含1.为实体类设置别名。2.实体接口映射资源，映射到具体的xml文件进行数据库的操作
**web.xml**（不在包中但是配置文件）：1.spring的监视器ContextLoaderListener2.springMVC的分发器DispatcherServlet，注意需要写入spring-mvc.xml配置文件所在路径和控制分发的请求路径3.其他配置，配置字符编码等