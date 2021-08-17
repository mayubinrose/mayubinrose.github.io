---
layout: title
title: SpringBoot
date: 2018-12-06 16:18:27
tags: Java
---
# 说明
云计算大作业目的是学习调用华为云的API，然后前后端的框架选择了SpringBoot
# Spring的简史
第一阶段：Spring 1.x时代，随着项目的扩大，xml配置文件放到不同的配置文件
第二阶段：注解配置，Spring提供声明Bean的注解，大大减少了配置量。应用的基本配置用xml，业务配置用注解
第三阶段：Java配置，Spring提供了Java配置，使用Java配置可以让你更理解你所配置的Bean。
SpringBoot：使用Spring Boot很容易创建一个独立运行、准生产级别的基于Spring框架的项目，使用Spring Boot你可以不用或者只需要很少的Spring配置
 <!--more--> 
# Controller
@RestController：注解相当于@ResponseBody ＋ @Controller。使用@Controller 注解，在对应的方法上，视图解析器可以解析return 的jsp,html页面，并且跳转到相应页。若返回json等内容到页面，则需要加@ResponseBody注解，但使用@RestController这个注解，就不能返回jsp,html页面，视图解析器无法解析jsp,html页面
@RequestMapping：配置web请求的映射
@PostMapping：@RequestMapping(method = RequestMethod.POST)的缩写
@GetMapping：@RequestMapping(method = RequestMethod.GET)的缩写
GET和POST最直观的区别就是GET把参数包含在URL中，POST通过request body传递参数
@RequestParam：GET和POST中请求传的参数会自动赋值到其注解的变量上
@RequestBody：用来处理content-type不是默认的application/x-www-form-urlcoded编码的内容，一般是来处理application/json类型。假如我有一个User类，拥有如下字段：String userName;String pwd;那么上述参数可以改为以下形式：@RequestBody User user 这种形式会将JSON字符串中的值赋予user中对应的属性上。需要注意的是，JSON字符串中的key必须对应user中的属性名，否则是请求不过去的。
HttpServletRequest：代表客户端的请求，当客户端通过HTTP协议访问服务器时，HTTP请求头中的所有信息都封装在这个对象中，通过这个对象提供的方法，可以获得客户端请求的所有信息
# Session
## 简单介绍
在WEB开发中，服务器可以为每个用户浏览器创建一个会话对象（session对象），注意：一个浏览器独占一个session对象(默认情况下)。因此，在需要保存用户数据时，服务器程序可以把用户数据写到用户浏览器独占的session中，当用户使用浏览器访问其它程序时，其它程序可以从用户的session中取出该用户的数据，为用户服务
## Session和Cookie的主要区别
Cookie是把用户的数据写给用户的浏览器
Session技术是把用户的数据写到独占的Session中
Session对象由服务器创建，开发人员可以调用request对象的getSession方法得到session对象
## Session实现原理
服务器创建sesssion出来后，会把session的id号，以cookie的形式回写给客户机，只要客户机的浏览器不关，再去访问服务器时，都会带着session的id号去，服务器发现客户机浏览器带session id过来了，就会使用内存中与之对应的session为之服务
# mapper
为一个接口，它的方法实现与数据库相关
在mapper映射文件.xml文件中的头写入<mapper namespace="com.example.demo.mapper.UserMapper">表示调用的相关mapper为UserMapper。id为映射的方法，parameterType为输入参数。resultType为返回参数
# service
包含一个接口类和接口实现类impl
impl类中包含注解@service和@Autowired的mapper接口对象，用该接口对象调用对数据库操作的方法，在controller中有@Autowired的service接口对象
# model
实体类
# application.properties
配置数据库：spring.datasource.username=root spring.datasource.password=xxx
spring.datasource.driver-class-name=com.mysql.jdbc.Driver 
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/数据库名?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC&allowMultiQueries=true
配置thymeleaf：spring.thymeleaf.cache=true
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
spring.thymeleaf.mode=HTML5
spring.thymeleaf.encoding=UTF-8
配置mybatis：
