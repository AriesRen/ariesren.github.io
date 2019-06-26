---
title: Spring面试题
date: 2018-11-02 11:19:05
tags:
categories:
---

#### 1、consul的可靠性

#### 2、spring的原理，AOP/IOC原理，使用场景

#### 3、spring bean生命周期

{% asset_img "spring bean生命周期.jpg" %}

* 实例化一个Bean，也就是我们常说的new
* 按照Spring上下文对实例化的Bean进行配置，即 IOC 注入
* 如果这个Bean已经实现了BeanNameAware接口，会调用它实现的setBeanName(String)方法，此处传递的就是spring配置文件中Bean的ID值
* 如果这个Bena已经实现了BeanFactoryAware接口，会调用它实现的setBeanFactory(BeanFactory)传递Spring工厂自身
* 如果这个Bean已经实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入Spring上下文
* 如果这个Bean关联了BenaPostProcessor接口，将会调用postProcessBeforeInitialization(Object obj, String s)方法，BeanPostPorcessor经常被用作Bean内容的更改，并且由于这个是在Bena初始化结束时调用哪个的方法，也可以被应用于内存或缓存技术。
* 如果Bena在Spring配置文件中配置了init-method属性会自动调用其配置的初始化方法。
* 如果这个Bean关联了BeanPostProcessor接口，将会调用postPorcessAfterInitialization(Object obj, String s)方法
* 以上工作完成之后就可以应用这个Bean了，那这个Bean是一个Singleton的，所以一般情况下我们调用同一个ID的Bean会是在内容地址相同的实例，当然在Spring配置文件中也可以配置非Singleton
* 当Bena不再需要时，会经过清理阶段，如果Bean实现了DisposableBena接口，会调用其实现的destroy()方法
* 最后，如果这个Bean在Spring中配置了destroy-method属性，会自动调用其配置的销毁方法。


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

#### 11、spring bean的作用域

Spring中通过scope来配置Bean的作用域，scope有五个属性，用来描述不同的作用域
* singleton： 使用该属性定义Bean时，IOC容器仅创建一个Bean实例，IOC容器每次返回的是同一个Bean实例。
* prototype：使用该属性定义Bean时，IOC容器可以创建多个Bean实例，每次返回的都是一个新的实例。
* request：该属性仅对HTTP请求产生作用，使用该属性定义Bean时，每次HTTP请求都会创建一个新的Bean，适用于WebApplicationContext环境。
* session： 该属性仅用于HTTP Session，同一个Session共享一个Bean实例。不同的Session使用不同的实例。

* global-session： 该属性仅用于HTTP Session，同Session作用域不同的是，所有的session共享一个Bean实例。

#### 12、spring boot比psinrg做了哪些改进？spring5比spring4做了哪些改进？

#### 13、如何自定义一个spirng boot starter

#### 24、Servlet的生命周期



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