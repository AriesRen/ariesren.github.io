---
title: Java反序列化漏洞浅析
date: 2018-11-13 11:33:48
tags: ['安全','反序列化','Java','漏洞']
categories: 安全
---

<div style="text-align: center;">
{% note success %} 
### 0x01 前言
{% endnote %}
</div>

2015年11月6日FoxGlove Security安全团队的@breenmachine 发布了一篇长博客，介绍了如何利用Java反序列化漏洞，来攻击最新的Jenkins、Jboss、WebLogic等java应用，实现远程代码执行漏洞。

事实上，早在2015年的1月28号，Gabriel Lawrence (@gebl)和Chris Frohoff (@frohoff)在AppSecCali上给出了一个报告[5]，报告中介绍了Java反序列化漏洞可以利用Apache Commons Collections这个常用的Java库来实现任意代码执行。

确实，Apache Commons Collection这样的基础类库有非常多的Java应用都在用，一旦编程人员误用了反序列化机制，使得用户的输入可以直接被反序列化，就能导致任意代码执行，这是一个极其严重的事情。

<div style="text-align: center;">
{% note success %} 
### 0x02 Java序列化和反序列化
{% endnote %}
</div>

今天我们就以Java的反序列化漏洞做一个简单的分析。在这之前先了解一下Java的序列化和反序列化。

> 序列化就是把对象的状态信息转换为字节序列(即可以存储或传输的形式)过程
> 反序列化即逆过程，将字节流还原为对象

java序列化经常用在把对象的字节序列存储在磁盘上，另一个用途是在网络上传输对象。例如最常见的是web服务器中Session对象，当有10万用户并发访问，就有可能出现10万个session对象，内存可能吃不消，于是web容器就会把一些session先序列化到硬盘中，等要用的时候，再把保存在磁盘上的对象加载到内存中。

Java中的ObjectOutputStream类的`writeObject` 方法可以实现序列化，类`ObjectInputStream`类的readObject方法可以用于反序列化。下面是一个将字符串对象先进行序列化存储到本地文件，在通过反序列化进行恢复的代码。

```java
public class TestSerialize(){
	public static void main(String[] args){
		String s = "test";

		// 将序列化对象写入文件中
		FileOutputStream fos = new FileOutputStream("object.ser");
		ObjectOutputStream os = new ObjectOutputStream(fos);
		os.writeObject(s);
		os.close;

		// 从文件中读取对象
		FileInputStream fis = new FileInputStream("object.ser");
		ObjectInputStream ois = new ObjectInputStream(fis);

		// 通过反序列化恢复对象
		String s1 = (String)ois.readObject();
		ois.close();
	}
}
```

问题在于，如果java应用对于用户输入，即不可信的数据做了反序列化处理，那么攻击者可以通过构造恶意输入，让反序列化产生非预期的对象，非预期的对象产生过程中就有可能带来任意代码执行。

所以这个问题的根源在于`ObjecInputStream`在反序列化时，没有对生成的对象的类型做限制。

<div style="text-align: center;">
{% note success %} 
### 0x03 利用Apache Commons Collections实现远程代码执行
{% endnote %}
</div>

本篇以Apache Commons Collections为例，来解释如何构造对象，能够让程序在反序列化时，即调用readObject()时，就能直接实现远程代码执行。

Java中Map是存储键值对的数据结构。在Apache Commons Collections中实现了类`TransformedMap`，用来对Map进行某种转换，只需要调用`decorate()`函数，传入key和value的变换函数`Transformer`，就可以从任意Map对象生成相应的`TransformedMap`，decorate的函数如下：

```java
public static Map decorate(Map map, Transformer keyTransformer, Transformer valueTransformer){
	return new TransformedMap(map, keyTransformer, valueTransformer);
}
```

`Transformer`是一个接口，其中定义的`transform()`函数用来将一个对象转换为另一个对象，如下所示：

```java
public interface Transformer{
	public Object transform(Object input);
}
```

当Map中的任意key或value更改时，相应的`Transformer`就会被调用。除此之外，多个Trnansformer还能串起来，形成调用链`ChainedTransformer`。Apache Commons Collections已经实现了一些`Transformer`，其中有一个可以通过java的反射机制调用任意函数，叫做`InvokerTransformer`,代码如下：

```java
public class InvokerTransformer implements Transformer, Serializable{
	...
	public InvokerTransformer(String methodName, Class[] paramTypes, Object[] args){
		
	}
}
```

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