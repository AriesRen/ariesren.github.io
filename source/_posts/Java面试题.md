---
title: Java面试题
date: 2018-11-02 11:18:14
tags:
categories:
---

#### 1、HashMap和HashTable区别
HashMap是HashTable的轻量实现（非线程安全），他们都实现的Map接口，主要区别在于：线程安全，同步，性能

* HashTable继承Dictionary，HashMap继承的是java2出现的Map接口；
* HashMap允许将null作为key或value，hashtable不允许；
* HashMap是非同步的，HashTable是同步的(synchronized),所以HashMap线程不安全，而HashTable是线程安全的，多个线程可以共享一个HashTbale而不需要为自己的方法实现同步。Java5提供了ConcurrentMap，用来替代HashTable，比HashTable扩展性好；
* 由于HashMap是非线程安全的，所以单一线程访问，HashMap性能要高于HashTable；
* HashMap的迭代器（Iterator）是fail-fast迭代器，HashTable的enumerator迭代器不是fail-fast的。
* HashMap把HashTable的contains方法去掉了，换成了containsValue和containsKey
* HashTable中数组默认大小是11，扩容方法是old\*2+1;HashMap默认大小是16，扩容每次为2的指数大小

#### 2、Object的hashcode方法，equals方法，常用的地方

#### 3、HashMap的原理应用场景

#### 4、JDK中有哪些线程池

#### 5、TCP/UDP区别

**相同点**： 

都处于OSI七层模型的网络层，都是传输层协议，都能保护网络层的传输，双方通信都需要开放端口。

**异同点**：

| 	|               TCP 					  |                UDP   				|
|---|-----------------------------------------|-------------------------------------|
|1	|Transmission Control Protocol 传输控制协议| User Data Protocol 用户数据报协议 	| 
|2	|TCP的传输是可靠传输						  | UDP的传输是不可靠传输               	|
|3  |TCP是基于连接的协议，在正式收发数据前，必须和对方建立可靠的连接|UDP是和TCP相对应的协议，他是面向非连接的协议，他不与对方建立连接，而是直接把数据包发送出去|
|4  |TCP是一种可靠的通信服务，负载相对而言比较大，TCP用套接字(socket)或者端口进行通信|UDP是一种不可靠的网络服务，负载相对较小|
|5  |TCP和UP的结构不同，TCP包括序号、确认信号、数据偏移、控制标志(通常URG、ACK、PSH、RST、SYN、FIN)、窗口、检验和、紧急指针、选项等信息|UDP包含长度和检验和信息|
|6  |TCP提供超时重发，丢弃重复数据，检验数据，流量控制等，保证数据从一端传到另一端 |UDP不提供可靠性，他只是把应用程序传给IP层的数据发送出去，但是不能保证他们到达目的端|
|7  |TCP发送数据包前会在通信双方间建立三次握手，确保双方准备好，在传输数据包期间，TCP会根据链路中数据流量的大小来调节传送的速率，传输时如果发现有丢包，会有严格的重传机制，故而传输速度很慢|UDP在传输数据报前不用在客户端和服务器之间建立连接，且没有超时重发机制，故而传输速度很快|
|8  |TCP支持全双工和并发的TCP连接，提供确认、重传、拥塞控制|UDP适用于对系统性能要求高于数据完整性的要求，需要简短快捷的数据交换、需要多播和广播的应用环境|


#### 6、查找一个数组的中位数

#### 7、反射的机制

#### 8、Object类中的方法

#### 9、对象比较是否相等

#### 10、toString方法的常用地方，为什么要重写该方法

#### 11、HashMap put方法怎么判断是否是重复方法

#### 12、Set和List的区别

#### 13、ArrayList和LinkedList的区别

#### 14、TreeSet对存入的数据有什么要求吗？

#### 15、HashSet是不是线程安全的

#### 16、Java中有哪些线程安全的Map

#### 17、CocurrentHashMap是怎么做到线程安全的

#### 18、如何保证线程安全问题

#### 19、volatile原子性问题？为什么i++不支持原子性

#### 20、CAS操作

#### 21、lock和synchronized区别

#### 22、公平锁和非公平锁

#### 23、Java读写锁，读写锁解决的问题



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