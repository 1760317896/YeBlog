---
title: '详解SSM框架工作原理及流程'
date: 2021-12-28 18:49:20
tags: 
- 笔记 
- Spring
cover: https://s2.loli.net/2021/12/29/prBNgICwdx2Qtoc.png
top_img: /img/20.jpg
---
## 一、什么是SSM框架：

SSM框架是Spring、SpringMVC和Mybatis框架的整合，是标准的MVC模式，将整个系统划分为View层，Controller层，Service层，DAO层四层，使用Spring MVC负责请求的转发和视图管理，Spring实现业务对象管理，Mybatis作为数据对象的持久化引擎。

## 二、各框架介绍

1. Spring

Spring就像是整个项目中装配bean的大工厂，在配置文件中可以指定使用特定的参数去调用实体类的构造方法来实例化对象。也可以称之为项目中的粘合剂。
Spring的核心思想是IoC（控制反转），即不再需要程序员去显式地`new`一个对象，而是让Spring框架帮你来完成这一切。


2. SpringMVC

SpringMVC作用于web层，相当于controller，与struts中的action一样，都是用来处理用户请求的。同时，相比于struts2来说，更加细粒度，它是基于方法层面的，而struts是基于类层面的。Spring MVC 分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。

SpringMVC在项目中拦截用户请求，它的核心Servlet即DispatcherServlet承担中介或是前台这样的职责，将用户请求通过HandlerMapping去匹配Controller，Controller就是具体对应请求所执行的操作。SpringMVC相当于SSH框架中struts。

SpringMVC具体工作流程如下

![24.jpg](https://s2.loli.net/2021/12/28/MQKh3PsZGeUNty4.jpg)


3. mybatis

MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。

mybatis是对jdbc的封装，它让数据库底层操作变的透明。

mybatis的操作都是围绕一个sqlSessionFactory实例展开的。mybatis通过配置文件关联到各实体类的Mapper文件，Mapper文件中配置了每个类对数据库所需进行的sql语句映射。在每次与数据库交互时，通过sqlSessionFactory拿到一个sqlSession，再执行sql命令。

Mybatis层次图：

![27.png](https://s2.loli.net/2021/12/29/NVsUmGkYPcpXg4y.png)


## 三、SSM框架中各层介绍

### pojo层，Dao层，Mapper层，service层，controller层

### DAO层：

DAO层叫数据访问层，全称为data access object，某个DAO一定是和数据库的某一张表一一对应的，其中封装了CRUD（增加Create、检索Retrieve、更新Update和删除Delete）基本操作，DAO只做原子操作。无论多么复杂的查询，dao只是封装增删改查。至于增删查改如何去实现一个功能，dao是不管的。Mapper就是Mybatis操作数据库的那一层，就是DAO层

### Service层：

Service层叫服务层，被称为服务，粗略的理解就是对一个或多个DAO进行的再次封装，封装成一个服务，所以这里也就不会是一个原子操作了，需要事物控制。管理具体的功能的。service包含了serviceImpl（service接口的实现类） 是提供给controller 使用的，针对于某些业务将 dao 的对于某些表的crud进行组合，也就是说间接的和数据库打交道。

### Controller层：

Controler负责请求转发，接受页面过来的参数，传给Service处理，接到返回值，再传给页面。管理业务（Service）调度和管理跳转的。controller 通过调用service来完成业务逻辑。

### pojo层

实体类这一层，与数据库中的属性值基本保持一致。有的开发写成pojo，有的写成model，也有domain，也有dto（这里做参数验证，比如password不能为空等）。

### mapper层

Mapper就是Mybatis操作数据库的那一层，就是DAO层，对数据库进行数据持久化操作，他的方法语句是直接针对数据库操作的。

各个层之间的流程如下：

![26.jpg](https://s2.loli.net/2021/12/28/AWg2Q4Ze7lrGItC.jpg)

![25.jpg](https://s2.loli.net/2021/12/28/hkmle1TdL4g3yrx.jpg)


## 四、框架核心原理

1. AOP 面向切面编程（AOP）提供另外一种角度来思考程序结构，通过这种方式弥补了面向对象编程（OOP）的不足。除了类（classes）以外，AOP提供了切面。切面对关注点进行模块化，例如横切多个类型和对象的事务管理。Spring的一个关键的组件就是AOP框架，可以自由选择是否使用AOP。提供声明式企业服务，特别是为了替代EJB声明式服务。最重要的服务是声明性事务管理，这个服务建立在Spring的抽象事物管理之上。允许用户实现自定义切面，用AOP来完善OOP的使用可以把Spring AOP看作是对Spring的一种增强。AOP的实现乃至spring框架基本上核心代码都是基于Java语言的反射机制（所谓的反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。）。
AOP主要作用就是不通过修改源代码的方式、将非核心功能代码织入来实现对方法的增强。那么Spring AOP的底层如何实现对方法的增强？实现的关键在于使用了代理模式。代理模式的作用就是为其它对象提供一种代理，以控制对这个对象的访问，用于解决在直接访问对象时带来的各种问题，比如要访问的对象在远程的机器上。在面向对象系统中，由于其他某些原因（对象创建开销很大，或者某些操作需要安全控制，或者需要进程外的访问）等
代理主要分为两种方式：静态代理和动态代理

2. IOC IoC不是一种技术，只是一种思想，一个重要的面向对象编程的法则，它能指导我们如何设计出松耦合、更优良的程序。有了IoC容器后，把创建和查找依赖对象的控制权交给了容器，由容器进行注入组合对象，所以相较于传统的java servlet需要自己request.getParamiter等需要一系列取值，转换中文，转换值类型的繁琐，更重要的是使得程序的整个体系结构变得非常灵活。
自定义一个IOC容器的思路：
Map做一个容器，然后用解析xml文件的工具解析出需要扫描的包。利用Java反射机制拿到锁具有的方法，属性，注入到Map容器中

3. DI 依赖注入，是组件之间依赖关系由容器在运行期决定，形象的说，即由容器动态地将某个依赖关系注入到组件之中。依赖注入的目的并非为软件系统带来更多功能，而是为了提升组件重用的频率，并为系统搭建一个灵活、可扩展的平台。通过依赖注入机制，我们只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，完成自身的业务逻辑，而不需要关心具体的资源来自何处，由谁实现。

### AOP,IoC以及DI我单独写了两篇博客，比较详细的介绍了其思想，大家可以在"笔记"标签中找到。

## 五、SSM框架工作流程

1. 用户访问客户端发出请求，请求会被Spring MVC中的前端控制器拦截，前端控制器配置在web.xml中。
2. DispatcherServlet拦截到请求后，调用处理器映射器在dispatcher-servlet.xml文件中
3. 处理器映射器根据URL找到具体的处理器，生成具体的处理器对象及处理器拦截器（如果有生成）返回给前端控制器。
4. 前端控制器会选择合适的处理器适配器
5. HandlerAdapter会调用并执行Handler（Controller层）也被称之为后端控制器
6. 处理器对持久化对象进行增删改查
7. POJO将操作映射到ORM框架
8. ORM框架将操作映射到数据库
9. 关系数据库把操作的数据返回给ORM框架
10. ORM框架把数据返回给持久化对象
11. 持久化对象把数据返回给Handler
12. Handler返回一个ModelAndView对象，包含模型和视图名
13. 处理器适配器将这个模型返回给前端控制器
14. 前端控制器会根据ModelAndView选择一个合适的ViewResolver
15. 视图解析器解析后返回一个合适的视图View给前端控制器
16. 前端控制器对view进行渲染
17. 返回给客户端浏览器显示

### SSM框架全网最详细流程图如下：

![22.jpg](https://s2.loli.net/2021/12/28/pJsGW5PoaBcOrFZ.jpg)

## 六、SSM框架整合

要让几个框架相互配合，配置文件怎么写，项目的目录结构怎么设计对我这样一个新手来说实在很头疼。
目前我也只刚刚写过一个用户登录的demo，在此记录一下。

### (一)、项目目录结构
 －LoginDemo
      &emsp;&emsp; －src
         &emsp;&emsp; &emsp; －项目主包
       &emsp;  &emsp; &emsp; &emsp;     －controller
    &emsp;  &emsp; &emsp; &emsp;      －mapper
   &emsp;  &emsp; &emsp; &emsp;       －entity
  &emsp;  &emsp; &emsp; &emsp;        －service
   &emsp;&emsp; －web
  &emsp;&emsp; &emsp; －WEB-INF
   &emsp;  &emsp; &emsp; &emsp;    －log4j.properties
   &emsp;  &emsp; &emsp; &emsp;      －spring-mybatis.xml
   &emsp;  &emsp; &emsp; &emsp;    －springMVC-config.xml
  &emsp;  &emsp; &emsp; &emsp;      －web.xml
    &emsp;&emsp; &emsp;  －index.jsp
    &emsp;&emsp;  －pom.xml

### 二、需要引入的包 (pom.xml)

``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <name>LoginDemo</name>
    <groupId>com.cyan</groupId>
    <artifactId>ssm</artifactId>
    <packaging>war</packaging>
    <version>1.0-SNAPSHOT</version>
    <url>http://maven.apache.org</url>


    <build>
        <finalName>ssm</finalName>
        <plugins>
            <!--mybatis 逆向工程插件-->
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.2</version>
                <configuration>
                    <verbose>true</verbose>
                    <overwrite>true</overwrite>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.6</source>
                    <target>1.6</target>
                </configuration>
            </plugin>
        </plugins>
    </build>


    <properties>
        <spring.version>4.1.1.RELEASE</spring.version>
    </properties>



    <dependencies>
        <!-- springframe start -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-oxm</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- springframe end -->

        <!--aspectj start-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.6</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjrt</artifactId>
            <version>1.8.6</version>
        </dependency>
        <!--aspectj end-->

        <!--c3p0-->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.1</version>
        </dependency>

        <!--servlet/jsp api start-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.1</version>
            <scope>provided</scope>
        </dependency>
        <!--servlet/jsp api end-->

        <!--junit4-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>

        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.3.0</version>
        </dependency>
        <!--mybatis spring整合-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.2.3</version>
        </dependency>

        <!--mysql driver-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <!--jstl-->
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

    </dependencies>
</project>
```
### (三)、配置文件

1. web.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <!-- Spring ApplicationContext 载入 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- SpringMVC核心Servlet -->
    <servlet>
        <servlet-name>springMVC-config</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:/WEB-INF/springMVC-config.xml</param-value>
        </init-param>
    </servlet>

    <!-- 拦截所有请求 -->
    <servlet-mapping>
        <servlet-name>springMVC-config</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- spring配置文件加载 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:WEB-INF/spring-mybatis.xml</param-value>
    </context-param>

</web-app>
```
2. spring-mybatis.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-3.2.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx-3.2.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 自动搜索bean -->
    <context:annotation-config/>
    <context:component-scan base-package="com.cyan" />

    <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/Demo"/>
        <property name="username" value="root"/>
        <property name="password" value="2233"/>
    </bean>

    <!-- mybatis核心bean -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource"/>
        <!-- 这句配置mapper配置文件的位置 如果采用注解的方式这句可以省去 -->
        <!--<property name="mapperLocations" value="classpath:/WEB-INF/Mappers/*.xml" />-->
    </bean>

    <!-- 自动搜索mapper接口 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.cyan.Mapper" />
    </bean>


    <!-- 事务处理 -->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="datasource"/>
    </bean>

    <tx:advice id="txAdvice" transaction-manager="txManager">
        <tx:attributes>
            <tx:method name="insert*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="select*" propagation="SUPPORTS"/>
        </tx:attributes>
    </tx:advice>

    <aop:config>
        <aop:pointcut id="serviceCut" expression="execution(public * Service.*.*(..))" />
        <aop:advisor pointcut-ref="serviceCut" advice-ref="txAdvice" />
    </aop:config>


</beans>
```

3. springMVC-config.xml

``` xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-3.0.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">

    <!--spring可以自动去扫描base-pack下面或者子包下面的java文件，
    如果扫描到有@Component @Controller@Service等这些注解的类，则把这些类注册为bean-->
    <context:component-scan base-package="com.cyan.Controller">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        <context:include-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice"/>
    </context:component-scan>

    <mvc:annotation-driven />
    <mvc:default-servlet-handler/>

    <!-- 配置jsp文件的前后缀 “／”代表的是项目设定的Resource目录 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".jsp" />
    </bean>

</beans>
```

### (四)、详细类设计

1. －mapper
UserMapper

``` java
package com.cyan.Mapper;

import com.cyan.Model.User;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import javax.annotation.Resource;
import java.util.List;

/**
 * Created by cyan on 16/3/29.
 */

public interface UserMapper {
    @Select("select * from LoginDemo")
    public List<User> selectUser();
    @Select("select * from LoginDemo where username=#{username}")
    public List<User> selectUserByUsername(@Param("username")String username);

}
```

2. －entity
User

``` java
package com.cyan.Entity;

/**
 * Created by cyan on 16/3/29.
 */
public class User {
    private int id;
    private String username,password,slogan;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getSlogan() {
        return slogan;
    }

    public void setSlogan(String slogan) {
        this.slogan = slogan;
    }

}
```

3. －service
* IUserService

``` java
package com.cyan.Service;

import com.cyan.Entity.User;

/**
 * Created by cyan on 16/3/31.
 */
public interface IUserService {
    public User getUserByName(String name);
    public boolean verify(String username,String pwd);
}
```

* UserService
Spring中的几个标签@Component（声明一个类是Spring容器管理的类，可以细分为后面提到的三个标签）、@Controller（控制层）、@Service（服务层）、@Repository（持久层）。标签的作用是让Spring根据名字关联到这个类。

@Autowired标签默认以byType的形式注入，使用这个标签是不需要getter和setter方法的。（这次代码中因为用户名密码校验部分要用到get方法所以写上了）
可以配合@Qualifier标签根据bean的id来装配。

``` java
package com.cyan.Service;

import com.cyan.Mapper.UserMapper;
import com.cyan.Entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

/**
 * Created by cyan on 16/3/31.
 */

@Service("userService")
public class UserService implements IUserService{

    @Autowired
    private UserMapper userMapper;

    public void setUserMapper(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    @Override
    public User getUserByName(String name) {
        return userMapper.selectUserByUsername(name).get(0);
    }

    @Override
    public boolean verify(String username, String pwd) {
        if(userMapper.selectUserByUsername(username).get(0).getPassword().equals(pwd))
            return true;
        else return false;
    }

}
```

4. －controller
Login

``` java
package com.cyan.Controller;

import com.cyan.Service.IUserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Created by cyan on 16/3/29.
 */

@Controller
public class Login {

    @Autowired
    private IUserService userService;

    @RequestMapping("/index")
    public String index(){
        return "index";
    }

    @RequestMapping(value ="/login",method = RequestMethod.POST)
    public String login(HttpServletRequest req, HttpServletResponse resp){
        String username=req.getParameter("username");
        String pwd=req.getParameter("password");
        if(userService.verify(username,pwd)){
            req.getSession().setAttribute("user",userService.getUserByName(username));
            return "success";
        }
        else return "index";

    }
}
```

5. jsp页面
* index.jsp

``` java
<%--
  Created by IntelliJ IDEA.
  User: cyan
  Date: 16/3/29
  Time: 15:55
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>Login</title>
  </head>
  <body>
    <form action="/login" method="post">
      用户名:<input type="text" name="username" id="username"/>
      <br>
      密码:<input type="password" name="password" id="password"/>
      <br>
      <input type="submit" value="登录"/>
    </form>
  </body>
</html>
```

* success.jsp

``` java
<%@ page import="com.cyan.Entity.User" %><%--
  Created by IntelliJ IDEA.
  User: cyan
  Date: 16/3/31
  Time: 23:45
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>LoginSuccess</title>
</head>
<body>
登录成功!
<%
    User user=(User)session.getAttribute("user");
%>
用户名:<%=user.getUsername()%><br>
个性签名:<%=user.getSlogan()%><br>
</body>
</html>
```

### 运行结果，大家自行测试。

### 感谢阅读！

