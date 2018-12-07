---
title: Spring面试题
date: 2018-11-02 11:19:05
tags:
categories:
---

#### 1、consul的可靠性

#### 2、spring的原理，AOP/IOC原理，使用场景

#### 3、spring bean生命周期

#### 4、什么是依赖注入
DI、IOC是同一个概念。依赖注入是当一个对象需要依赖另一个对象的协助时，创建、管理被依赖对象的工作由Spring来完成，而不是由调用者完成，因此称为控制反转，创建被依赖对象的实例也是由spirng容器来创建，并注入给调用者，因此称为依赖注入。

#### 5、Spring在SSM中起什么作用
- spring： 是一个轻量级框架
- 作用： Bean工厂，用来管理Bean的声明周期和框架集成
- 两大核心： 
	- IOC/DI(控制反转/依赖注入)，由spring控制将所需的对象注入到相应的类中，spring顶层容器为BeanFactory
	- AOP：面向切面编程

#### 6、Spring的事务
- 编程式事务： 编程方式管理事务，灵活，但难管理
- 声明式事务： 将业务代码和事务管理分离，用注解和xml配置来管理事务

#### 7、IOC在项目中的作用
IOC解决了对象之间的依赖问题，把所有的Bean的依赖关系通过注解或者配置文件关联起来尽心管理，降低和耦合度。

#### 8、Spring DI的注入方式
- 构造注入
- set注入
- 接口注入

#### 9、IOC、AOP实现原理
- IOC：通过反射机制生成对象进行注入
- AOP：通过动态代理

#### 10、Spring MVC的架构/工作流程图
{% asset_img springmvc流程图.jpg %}

#### 11、spring bean的作用域和生命周期

#### 12、spring boot比psinrg做了哪些改进？spring5比spring4做了哪些改进？

#### 13、如何自定义一个spirng boot starter




<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- 信息流广告 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4127326375481893"
     data-ad-slot="9105526840"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>