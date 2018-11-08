# spring-boot
spring-boot，Spring家族成员之一，现在比较流行，我们公司的项目要从原来的Struts转为spring-boot，一开始我还是比较慌的，在4天的摸索和学习之后，渐渐的有了一点底。

spring-boot使用maven构建，比起spring-mvc的常规的套路而言，配置少了很多，不用去写一大堆的xml文件，内嵌tomcat（也支持netty），用maven打成一个jar包直接就能运行。

学习最重要的还是实践，配合好的教程，就比较容易上手，在此推荐[Spring Boot系列教程](https://blog.lqdev.cn/2018/07/11/springboot/chapter-zero/)。

## 基本项目构建

使用IDEA构建，新建项目选择"Spring Initializr"（当然随着日子的变迁以后IDEA可能没有这个选项，但是如果官网还活着的话，可以去[https://start.spring.io]按照提示来操作）然后选择maven构建，填写相关内容即可。

## 常用注解
无论是Spring还是Spring boot，注解都是核心成员，它们扮演了很重要的角色

* @Controller
   作用对象：类
   作用效果：声明此类为Controller

* @RequestMapping
  作用对象：类，方法
  作用效果：被作用对象将负责对应URL的解析

* @WebFilter
  作用对象：类
  作用效果：被作用对象将在实现javax.servlet.Filter接口后成为一个过滤器（需重写对应方法）
  作用条件：Spring boot启动类添加@ServletComponentScan注解

* @WebListener
  作用对象：类
  作用效果：被作用对象在实现javax.servlet.ServletRequestListener接口后成为一个监听器（需重写对应方法）
  作用条件：同WebFilter注解

* @Value("${variable_name_defined_in_properties_file}")
  作用对象：类字段，方法，方法参数，以及注解类型
  作用效果：被注解者将获取{}内对应变量在被Spring boot管理起来的properties文件中的值
  作用条件：存在properties文件且该变量也存在于其中

* @Bean

## 最简单的HelloWorld项目
* 确保pom文件中有以下依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

* 自定义controller类，注意注解
```java

@RestController //RestController只返回json数据
public class SimpleController {
    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public String hello(){
        return "Hello World!";
    }
}
```

* 配置文件定义上下文和端口
```
# application.properties
server.port=8080
server.servlet.context-path=demo
# 这样就可以用localhost:8080/demo/就是本项目的基础URL了
```

* 访问
`http://localhost:8080/demo/hello`

## 自定义过滤器

## 自定义拦截器

## 自定义监听器

## 自定义配置文件

## 自定义日志


