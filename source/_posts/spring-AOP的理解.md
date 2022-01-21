---
title: 'Spring,AOP的理解'
date: 2021-12-25 18:29:23
tags: 
- 笔记 
- Spring
cover: https://s2.loli.net/2021/12/29/l5LApiFxqOTB1Vf.png
top_img: /img/15.jpg
---

# 一、Spring是什么？

Spring是一个轻量级的2EE框架。它是一个容器框架，用来装`javabean`(java对象)，中间层框架(万能胶)可以起到一个连接作用，比如把Struts和hibernate粘合在一起运作，可以让我们的企业开发更快，更简洁。
spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器框架。
* 1、从大小与开销两方面而言，spring都是轻量级的
* 2、通过控制反转(IoC)的技术达到松耦合的目的
* 3、提供了面向切面编程的丰富支持，允许通过分离应用的业务逻辑与系统级服务进行内聚性的开发
* 4、包含并管理应用对象(Bean)的配置和生命周期，这个意义上是一个容器
* 5、将简单的组件配置，组合成为复杂的应用，这个意义上是一个框架
    

 # 二、AOP的理解

系统是由许多不同的组件所组成的，每一个组件各负责一块特定功能。除了实现自身核心功能之外，这些组件还经常承担着额外的职责。例如日志、事物管理和安全这样的核心服务经常融入到自身具有核心业务逻辑的组件中去，这些系统服务经常被称为横切关注点，因为它们会跨越系统多个组件。

当我们需要为分散的对象引入公共行为的时候，OOP则显得无能为力。也就是说，OOP允许你定义从上到下的关系，但并不适合定义从左到右的关系，例如日志功能。

日志代码往往水平的散布在所有对象层，而与它所散布到的对象的核心功能搞无关系，在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。

AOP：将程序中的交叉业务了逻辑(比如安全、日志、事物等)，封装成一个切面，然后注入到目标对象(具体业务逻辑)中去。AOP可以对某个对象或某些对象的功能进行增强，比如对象中的方法进行增强，可以在执行某个方法之前额外的做一些事情，在某个方法执行之后额外做一些事情。

# 三、AOP(讲义)

## 1、概述

（1）在软件业，AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP( Oriented Object Programming)的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

（2）说到AOP，我们就不得不来提一下软件的纵向和横向问题。从纵向结构来看就是我们软件系统的各个模块，它主要负责处理我们的核心业务（例如商品订购、购物车查看）；而从横向结构来看，我们几乎每个系统又包含一些公共模块（例如 权限、日志模块等）。这些公共模块分布于我们各个核心业务之中（例如订购和查看商品明细的过程都需要检查用户权限、记录系统日志等）。这样一来不仅在开发过程中要处处关注公共模块的处理而且开发后维护起来也是十分麻烦。而有了AOP之后将应用程序中的商业逻辑同对其提供支持的通用服务进行分离，使得开发人 员可以更多的关注核心业务开发。

![软件纵向与横向结构](https://s2.loli.net/2021/12/26/82rfukJS35LcOPs.png)

## 2、AOP开发中的相关术语

### * (1) 链接点(Joinpoint)
程序执行的某个特定位置：如类开始初始化前、类初始化后、类某个方法调用前、调用后、方法抛出异常后。一个类或一段程序代码拥有一些具有边界性质的特定点，这些点中的特定点就称为“连接点”。Spring仅支持方法的连接点，即仅能在方法调用前、方法调用后、方法抛出异常时以及方法调用前后这些程序执行点织入增强。（可以被切入的点）

### * (2) 切点(Pointcut)
每个程序类都拥有多个连接点，如一个拥有两个方法的类，这两个方法都是连接点，即连接点是程序类中客观存在的事物。AOP通过“切点”定位特定的连接点。连接点相当于数据库中的记录，而切点相当于查询条件。切点和连接点不是一对一的关系，一个切点可以匹配多个连接点。在Spring中，切点通过org.springframework.aop.Pointcut接口进行描述，它使用类和方法作为连接点的查询条件，Spring AOP的规则解析引擎负责切点所设定的查询条件，找到对应的连接点。其实确切地说，不能称之为查询连接点，因为连接点是方法执行前、执行后等包括方位信息的具体程序执行点，而切点只定位到某个方法上，所以如果希望定位到具体连接点上，还需要提供方位信息。（已经被切入的点）

### * (3) 通知/增强(Advice)
增强是织入到目标类连接点上的一段程序代码，在Spring中，增强除用于描述一段程序代码外，还拥有另一个和连接点相关的信息，这便是执行点的方位。结合执行点方位信息和切点信息，我们就可以找到特定的连接点。（在切入点添加通过，增强它的功能）

### * (4) 目标对象(Target)
增强逻辑的织入目标类。如果没有AOP，目标业务类需要自己实现所有逻辑，而在AOP的帮助下，目标业务类只实现那些非横切逻辑的程序逻辑，而性能监视和事务管理等这些横切逻辑则可以使用AOP动态织入到特定的连接点上。（代理的目标对象）

### * (5) 引介(Introduction)
引介是一种特殊的增强，它为类添加一些属性和方法。这样，即使一个业务类原本没有实现某个接口，通过AOP的引介功能，我们可以动态地为该业务类添加接口的实现逻辑，让业务类成为这个接口的实现类。 （理解一种特殊的通知）

### * (6) 织入(Weaving)
织入是将增强添加对目标类具体连接点上的过程。AOP像一台织布机，将目标类、增强或引介通过AOP这台织布机天衣无缝地编织到一起。根据不同的实现技术，AOP有三种织入的方式：
a、编译期织入，这要求使用特殊的Java编译器。
b、类装载期织入，这要求使用特殊的类装载器。
c、动态代理织入，在运行期为目标类添加增强生成子类的方式。
Spring采用动态代理织入，而AspectJ采用编译期织入和类装载期织入。（通知放到切入点）

### * (7) 代理(Proxy)
一个类被AOP织入增强后，就产出了一个结果类，它是融合了原类和增强逻辑的代理类。根据不同的代理方式，代理类既可能是和原类具有相同接口的类，也可能就是原类的子类，所以我们可以采用调用原类相同的方式调用代理类。

### * (8) 切面(Aspect)
切面由切点和增强（引介）组成，它既包括了横切逻辑的定义，也包括了连接点的定义，Spring AOP就是负责实施切面的框架，它将切面所定义的横切逻辑织入到切面所指定的连接点中。（是切入点和通知的结合）

## AOP相关概念定义：

* **Aspect（切面）**： Aspect 声明类似于 Java 中的类声明，在 Aspect 中会包含着一些 Pointcut 以及相应的 Advice。
* **Joint point（连接点）**：表示在程序中明确定义的点，典型的包括方法调用，对类成员的访问以及异常处理程序块的执行等等，它自身还可以嵌套其它 joint point。
* **Pointcut（切点）**：表示一组 joint point，这些 joint point 或是通过逻辑关系组合起来，或是通过通配、正则表达式等方式集中起来，它定义了相应的 Advice 将要发生的地方。
* **Advice（增强）**：Advice 定义了在 Pointcut 里面定义的程序点具体要做的操作，它通过 before、after 和 around 来区别是在每个 joint point 之前、之后还是代替执行的代码。
* **Target（目标对象）**：织入 Advice 的目标对象.。
* **Weaving（织入）**：将 Aspect 和其他对象连接起来, 并创建 Adviced object 的过程

## 四、AOP其他知识点
### 1、通配符使用
在配置文件的切入点定义中，可以使用通配符*，该通配符可以表示返回值、类的全类名、方法名
如：
``` java
<aop:pointcut expression="execution(* *.*())" id="pointcut"/>
```

### 2、后置通知
``` java
<aop:config>
   <!-- 定义切入点（切入单个方法） -->
        <aop:pointcut expression="execution(* *.update())" id="pointcut"/>
   <!-- 定义切面，引用通知类 -->
   <aop:aspect ref="transactionAdvice">
      <!-- method="before" 通知类型   pointcut="pointcut"  选择切入点 -->
     <aop:after method="after" pointcut-ref="pointcut"/>
   </aop:aspect>
</aop:config>
```

### 3、环绕通知
``` java
<aop:config>
   <!-- 定义切入点（切入所有方法） -->
        <aop:pointcut expression="execution(* *.*())" id="pointcut"/>
   <!-- 定义切面，引用通知类 -->
   <aop:aspect ref="transactionAdvice">
      <!-- method="before" 通知类型   pointcut="pointcut"  选择切入点 -->
     <aop:around method="around" pointcut-ref="pointcut"/>
   </aop:aspect>
</aop:config>
```
### 4、异常通知
``` java
public void save(String n) {
		int i=9/0;
		System.out.println("***保存数据***");
	}
<aop:after-returning method="afterReturning" pointcut-ref="pointcut"/>
<aop:after-throwing method="afterException" pointcut-ref="pointcut"/>
```
### 5、通知汇总
``` java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	 http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context 
		http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/aop 
		http://www.springframework.org/schema/aop/spring-aop-4.3.xsd">
<!--  Spring AOP实现方式有两种，一种使用JDK动态代理，
另一种通过CGLIB来为目标对象创建代理。如果被代理的目标实现了至少一个接口，
则会使用JDK动态代理，所有该目标类型实现的接口都将被代理。 -->		
<aop:aspectj-autoproxy proxy-target-class="true"/>
<!-- 目标对象 -->
<bean id="userService" class="com.ahbvc.spring.aop.service.UserServiceImpl"></bean>
<!-- 通知类/增强类 -->
<bean id="transactionAdvice" class="com.ahbvc.spring.aop.advice.TransactionAdvice"></bean>
<!-- 将通知织入到目标对象，即开始面向切面编程 -->
<aop:config>
   <!-- 定义切入点 -->
        <aop:pointcut expression="execution(* *.save(..))" id="pointcut"/>
   <!-- 定义切面，引用通知类 -->
   <aop:aspect ref="transactionAdvice">
      <!-- method="before" 通知类型   pointcut="pointcut"  选择切入点 -->
      <aop:before method="before" pointcut-ref="pointcut"/>
      <aop:after method="after" pointcut-ref="pointcut"/>
      <aop:after-returning method="afterReturning" pointcut-ref="pointcut"/>
     <aop:around method="around" pointcut-ref="pointcut"/>
     <aop:after-throwing method="afterException" pointcut-ref="pointcut"/>
   </aop:aspect>
</aop:config>
</beans>
```
### 6、方法带参数
``` java
<aop:pointcut expression="execution(* *.*(..))" id="pointcut"/>
```

### 7、spring官方文档定义

![19.png](https://s2.loli.net/2021/12/26/9iXyrGh3xID6S1n.png)