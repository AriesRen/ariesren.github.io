---
title: Java面试题
date: 2018-11-02 11:18:14
tags: ['java','面试']
categories: 面试
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

简单的说，HashMap是由数组和链表组成的，主体是数组，链表的作用主要是为了解决哈希冲突而存在的。在JDK1.8之后，链表长度超过8之后，会转换为红黑树。HashMap的默认容量为16，阈值为0.75，总容量超过0.75时，会进行2倍扩容。

#### 4、JDK中有哪些线程池

Java中通过Executors提供四种线程池：
* newCachedTheadPool： 创建一个可缓存的线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无回收，则创建线程。此线程池不会对线程池大小做限制，线程池大小完全依赖系统能够创建的最大线程大小。
* newFixedThreadPool： 创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待
* newScheduleThreadPool： 创建一个定长线程池，支持定时及周期性任务执行
* newSingleThreadExecutor： 创建一个单线程化的线程池，他只会用唯一的工作线程来执行任务，保证所有任务按照先定顺序（FIFO，LIFO优先级执行）

#### 5、TCP/UDP区别

**相同点**： 都处于OSI七层模型的网络层，都是传输层协议，都能保护网络层的传输，双方通信都需要开放端口。

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

#### 7、反射的机制，说说反射的用途和实现，反射是不是很慢，我们在项目中是否应该避免使用反射。

#### 8、Object类中的方法

#### 9、对象比较是否相等

#### 10、toString方法的常用地方，为什么要重写该方法

#### 11、HashMap put方法怎么判断是否是重复方法

#### 12、Set和List的区别

#### 13、ArrayList和LinkedList的区别，List和Map的区别， ArrayList和Vector的区别

* ArrayList和LinkedList区别：
	* ArrayList内部是基于数组实现的，因此对于随机访问快，新增删除慢
	* LinkedList内部是基于链表实现的，因此新增删除快，随机访问慢。
* List和Map的区别：
	* List是存储单列数据的集合，存储的数据都是有序并且是可以重复的
	* Map是存储双列数据的集合，通过键值对存储数据，存储的数据是无序的，Key值不能重复，value值是可以重复的。
* ArrayList和Vector的区别：
	* ArrayList是不同步的，也就是不是线程安全的类
	* Vector是同步的，线程安全

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

#### 24、线程池的原理，为什么要创建线程池？创建线程池的方式？

使用线程池的好处： 线程可以重复利用，减少创建、销毁线程带来的系统资源的开销，提高性能

#### 25、线程的生命周期，什么时候会出现僵死进程？

#### 26、创建线程池有哪几个核心参数，如何合理配置线程池的大小？

#### 27、volatile、ThreadLocal的使用场景和原理

#### 28、Synchronized、Volatile区别，Synchronized锁粒度，模拟死锁场景、原子性与可见性。

#### 29、JVM内存模型、GC机制和原理

#### 30、GC分那两种，Minor GC和Full GC有什么区别，什么情况下会触发Full GC，分别采用什么算法。

#### 31、JVM里有几种classloader，为什么会有多种。

JVM里有三种类加载器：BootStrap Loader 负责加载系统类，ExtClassLoader负责加载扩展类，AppClassLoader负责加载应用类。

他们的分工不一样，各自负责不同的区域，另外也是为了实现委托模型。

当执行java \*.class的时候，java会帮助我们找到jre，接着找到jre内部的jvm.dll，这个才是真正的java虚拟机，最后加载动态库，激活java虚拟机。虚拟机激活后，会先做一些初始化的动作，比如读取系统参数，一旦初始化动作完成，就会产生第一个类加载器-Bootstrap Loader，Bootstrap Loader是由C++编写的，该Loader所做的初始化工作中，除了一些基本的初始化动作之外，最重要的就是加载Launcher.java中的ExtClassLoader，并设定其parent为null，但其实其父加载器就是Bootstrap Loader。然后Bootstrap Loader在要求加载Launcher.java中的AppClassLoader，并设定其Parent为ExtClassLoader。需要注意的是Launcher$ExtClassLoader和Launcher$AppClassLoader都是由BootstrapLoader加载的，所以Parent和由哪个类加载没有关系。

#### 32、什么是双亲委派机制，介绍双亲委派的运作过程和好处

双亲委派模式的工作原理是，如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器去执行，如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归，请求最终将到达顶层的启动类加载器。如果父加载器可以完成加载任务，就成功返回；如果如果父加载器无法完成加载任务，子加载器才会尝试自己去加载，这就是双亲委托模型。

采用双亲委派模型的害处是Java类随着它的类加载器一起具备了一种带有优先级的层次关系，通过这种层级关系可以避免类的重复加载，当父亲已经加载了该类时，就没有必要子ClassLoader再加载一次。其次是考虑到安全因素，java核心api中定义类型不会随意被替换，比如通过网络传递一个java.lang.Integer的类，通过双亲委派模型传递到父类加载器，而启动类加载器在核心Java API中已经发现了这个类，所以并不会加载网络传递过来的Java.lang.Integer，而是直接返回已经加载过的Integer，这样便可以防止核心API被人随意篡改。

#### 33、什么情况下需要破坏双亲委派机制

1、基础类调用用户代码

双亲委派很好地解决了各个类加载器的基础类的同一问题（越基础的类由越上层的加载器进行加载），基础类之所以称为“基础”，是因为它们总是作为被用户代码调用的API，但世事往往没有绝对的完美。如果基础类又要调用回用户的代码，那该么办？一个典型的例子就是JNDI服务，JNDI服务现在已经是Java的标准服务。JNDI的目的是对资源进行集中管理和查找，但是它需要调用有独立厂商实现并部署在应用程序ClassPath下的JNDI接口提供者（如mysql连接驱动、sql连接驱动）的代码，但是启动类加载器不识别这些代码。

为了解决这个问题，Java设计团队引入了一个不太优雅的设计：线程上下文类加载器（Thread Context ClassLoader）。有了线程上下文类加载器，JNDI就可以使用它去加载所需要的SPI代码，也就是父类加载器请求子类加载器去完成类加载的动作，这种行为实际上打破了双薪委派模型层次结构来逆向使用类加载器。JAVA中所有涉及SPI的加载动作基本上都是采用这种方式，例如JNDI、JDBC、JCE、JAXB等。

2、OSGi模块化热部署

OSGI实现模块化热部署的关键是它自定义的类加载器机制的实现，每一个程序模块都有一个自己的类加载器，当需要等换一个模块时，就把模块连同类加载器一起换掉以实现代码的热替换。


#### 34、常见的JVM调优方法有哪些？可以调整哪个参数，调成什么值。

#### 35、红黑树的实现原理和应用场景

#### 36、NIO是什么，适用于何种场景

#### 37、八种基本数据类型的大小，以及他们的封装类

byte、short、int、char、float、double、long、boolean
1、2、4、2、4、8、8、1
Byte、Short、Integer、Character、Float、Double、Long、Boolean

#### 38、引用数据类型

类、接口类型、数组类型、枚举类型、注解类型

基本数据类型在创建时，在栈上给其划分一块内存，将数值直接存储在栈上。
引用数据类型在创建时，首先在栈上给其引用分配一块内存，而对象的具体信息都存储在堆内存中，然后由栈上的引用指向堆中对象的地址。

#### 39、switch能否用string做参数

jdk7之前只能用byte、short、char、int这几个基本数据类型和其对应的封装类型。switch后面的括号内只能放置int类型的数据，由于byte、short、char都可以自动转为int类型，所以可以支持。

jdk7之后整形、枚举类型、字符串都可以，但是jdk7并没有新的指令处理switch string，而是通过string.hashcode，将string转换为int进行判断。

#### 40、equals和==的区别

1、使用==比较原生类型如 boolean、int、char等，使用equals比较对象
2、==是判断两个变量或者实例是不是指向同一个内存空间。equals是判断两个变量或者实例所指向的内存空间的值是不是相同
3、==是指对内存地址进行比较，equals是对字符串的内容进行比较
4、==是指引用是否相同，equals指的是值是否相同。

















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