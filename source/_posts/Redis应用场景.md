---
title: Redis应用场景
date: 2019-04-11 15:53:34
tags:
categories:
---


<div style="text-align: center;">
{% note success %} 
### Redis 数据结构
{% endnote %}
</div>

Redis是一个开源的内存数据结构存储系统，可以用做数据库，缓存，消息中间件等。Redis中常规的数据结构有5种，string、list、set、zset、hash，同时还有bitmaps、hyperloglogs、geospatial三种不常用的数据结构。

我们经常把redis当作缓存来使用，但是本文只是基于五种常用的数据结构及其相关命令来讨论相关的应用场景，不涉及缓存。

<div style="text-align: center;">
{% note success %} 
### String
{% endnote %}
</div>

首先先看一下Redis中String的操作命令：

```bash
SET key value [expiration EX seconds | PX milliseconds] [NX|XX]
SETNX key value
SETEX key seconds value
PSETEX key milliseconds value
GET key
GETSET key value
STRLEN key
APPEND key value
SETRANGE key offseet value
GETRANGE key start end
INCR key
INCRBY key increment
INCRBYFLOAT key increment
DECR key
DECRBY key increment
MSET key value [key value ...]
MSETNX key value [key value ...]
MGET key [key ...]
```
命令具体的含义我这里就不一一赘述了。有想要了解的同学可以参考[Redis命令参考](http://redisdoc.com/)

#### 分布式锁

我们看第一条命令，`SET key value [expiration EX seconds | PX milliseconds] [NX|XX]`。

通过该命令我们可以实现基于redis的分布式锁，为了确保分布式锁可用，我们至少要确保锁的实现需要满足以下条件：
1. 互斥性，在任一时刻，只有一个客户端持有锁。
2. 不会发生死锁，即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。
3. 容错性，只要大部分Redis节点正常运行，客户端就可以加锁、解锁。
4. 解铃还须系铃人，加锁和解锁必须是同一个客户端，客户端不能把别人的锁给解了。

那该命令如果保证确保分布式锁的可用性呢？

首先，set 加入了NX参数，可以保证如果有key存在，则其他客户端set 不会成功，也就是只有一个客户端能持有锁。其次，我们可以对锁设置过期时间，即使锁的持有者后续发生了崩溃而没有解锁，锁也会因为到了过期时间自动解锁，不会发生死锁现象。 容错性方面我们可以通过redis cluster来保证锁的高可用及容错性。最后，我们将value设置为requestId，表示加锁的客户端标识，那么客户端解锁的时候就可以进行判断是否为同一个客户端。

##### 加锁

我们用代码看一下怎么使用redis来实现分布式锁，非常简单。

```java
public static boolean tryGetLock(Jedis jedis, String lockKey, String requestId, int expire) {
	String result = jedis.set(lockKey, requestId, "NX", "PX", expire);
	if ("OK".equals(result)) {
		return true; // 获取锁成功
	}
	return false; // 获取锁失败
}
```
而在网上有很多人使用jedis.setnx()和jedis.expire()组合实现加锁，代码如下：

```java
Long result = jedis.setnx(lockKey, requestId);
if (result == 1) {
	// 如果在这里程序崩溃，则无法设置过期时间，将发生死锁
	jedis.expire(lockKey, expire);
}
```
乍一看和上面的set方法结果一样，其实不然，由于这是两条redis命令，不具有原子性，所以如果在setnx 之后程序崩溃，导致锁没有设置过期时间，可能会发生死锁。


##### 解锁

```java
public static boolean releaseLock(Jedis jedis, String lockKey,String requestId) {
	String script = "if redis.call('get', KEY[1]) == ARGV[1] then return redis.call('del',KEYS[1]) else return 0 end";
	Object result = jedis.eval(script, Collections.singletonList(lockKey),Collections.singletonList(requestId));
	Long success = 1L;
	if (success.equals(result)) {
		return true;
	}
	return false;
}
```

解锁我们用一行Lua代码来实现，Lua代码的功能是获取锁对应的value，检查是否与requestId相等，如果相等则删除锁(解锁)。eval()方法是将Lua代码交给redis服务器执行。

那为什么要使用Lua语言来实现呢？因为要确保上述操作是原子性的，而eval命令执行Lua代码的时候，Lua代码将被当成一个命令去执行，直到eval命令执行完成，Redis才会执行其他命令。

#### 分布式ID生成

我们再来看第二个场景--生成分布式ID，

当使用数据库来生成ID性能不够的时候，可以尝试使用Redis来生成ID，这主要依赖Redis是单线程的，所以也可以用来生成全局唯一ID。可以用Redis的原子操作INCR和INCRBY来实现




<br>
<br>
<br>

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