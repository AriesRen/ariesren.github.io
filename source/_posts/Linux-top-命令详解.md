---
title: Linux top 命令详解
date: 2019-02-20 10:33:09
tags: ['linux', 'top', '运维', '负载']
categories: Linux
---

Linux系统中，top命令经常用来监控Linux的系统状况。可以通过top命令查看系统的CPU、内存、运行时间、线程信息等。通过top命令可以有效分析系统的性能瓶颈在哪里。

## 一、 top命令解释

在Linux系统中执行top命令，就会进入如下界面，下面我们来逐行分析每行代表的意义。

{% asset_img top命令.png %}

### 1、 系统运行时间及平均负载

top命令的第一行表示的是系统的运行时间及平均负载，与uptime命令有相似的输出。

```shell
 top - 17:05:18 up 190 days,  3:54,  1 user,  load average: 0.00, 0.02, 0.05
-- 17:05:18            系统的当前时间
-- up 190 days,  3:54  系统已经运行的时间 190天3小时54分钟，期间没有重启
-- 1 user              系统当前登录用户数 表示系统当前只有一个用户登录
-- load average: 0.00, 0.02, 0.05 
		  	load average后面的三个数分别是5分钟、10分钟、15分钟的负载情况			   
```

load average数据是每隔5秒钟检查一次活跃的进程数，然后按照特定算法计算出的数值。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了。

### 2、任务

第二行显示的是任务或者进程的总结。进程可以处于不同的状态，这里显示了全部进程的数量。

```bash
Tasks:   65 total,   1 running,   64 sleeping,   0 stopped,   0 zombie
# 总共有65个进程，1个正在运行， 64个休眠，0个停止，0个僵尸进程
```

### 3、CPU状态

```bash
%Cpu(s):  0.9 us,  1.2 sy,  0.0 ni, 97.6 id,  0.1 wa,  0.0 hi,  0.2 si,  0.0 st
```
这里显示的是不同模式下所占CPU时间百分比，不同CPU时间表示：
* us   user, 运行用户进程的CPU时间，消耗在用户空间的时间
* sy   system，运行内核进程的CPU时间，即消耗在内核空间的时间
* ni   niced，运行已调整优先级的用户进程的CPU时间
* id   idle，空闲CPU百分比，这个值越低，表示cpu越忙
* wa   wait，用于等待IO完成的CPU时间百分比，这个值越高说明外接设备有问题
* hi   hardware interrupt，处理硬件中断的CPU时间
* si   software interrupt，处理软中断的CPU时间
* st   虚拟机被hypervisor偷去的CPU时间（如果当前处于一个hypervisor的虚拟机，实际上hypervisor也是要消耗一部分CPU时间的）。

在这里CPU的使用比率和windows概念不同，如果你不理解用户空间和内核空间，需要充电了。


### 4、内存使用

```bash
KiB Mem:   2027864 total,  1945796 used,    82068 free,    15064 buffers
KiB Swap:  1048572 total,    87168 used,   961404 free.   398840 cached Mem
```

这两行显示的是内存使用率，有点像free命令。第一行是物理内存使用，第二行是虚拟内存使用（交换空间）。

内存显示如下： 

|         | 全部可用内存 | 已使用内存 | 空闲内存 | 缓冲内存 |
|---------|------------|----------- |---------|---------|
| 物理内存 | 物理内存总量2027864 | 使用中的内存总量1945796 | 空闲内存总量82068 | 缓存的内存量15064 |
| 交换分区 | 交换分区总量1048572 | 使用的交换分区总量87168 | 空闲交换分区总量961404 | 缓存的交换区总量398840 |

这里要说明的是不能用windows的内存概念理解这些数据，如果按照windows的方式此台服务器“危矣”。


对于内存监控，在top里我们要时刻监控第五行swap交换分区的used，如果这个数值在不断的变化，说明内核在不断进行内存和swap的数据交换，这是真正的内存不够用了。


### 5、各进程的状态监控

第六行是空行，第七行开始是个任务的状态监控。

```bash
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND                                
    1 root      20   0   54428   5512   3032 S  0.0  0.3  10:29.07 systemd                                
    2 root      20   0       0      0      0 S  0.0  0.0   0:00.90 kthreadd                               
    3 root      20   0       0      0      0 S  0.0  0.0   0:25.90 ksoftirqd/0                            
    5 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kworker/0:0H                           
    7 root      rt   0       0      0      0 S  0.0  0.0   0:00.00 migration/0                            
    8 root      20   0       0      0      0 S  0.0  0.0   0:00.00 rcu_bh                                 
    9 root      20   0       0      0      0 S  0.0  0.0   1:22.61 rcu_sched                              
   10 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 lru-add-drain                          
   11 root      rt   0       0      0      0 S  0.0  0.0   0:36.39 watchdog/0                             
   13 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kdevtmpfs                              
   14 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 netns                                  
   15 root      20   0       0      0      0 S  0.0  0.0   0:01.43 khungtaskd                             
   16 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 writeback                              
   17 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kintegrityd                            
   18 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 bioset                                 
   19 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 bioset                                 
   20 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 bioset                                 
   21 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kblockd                                
   22 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 md                                     
   23 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 edac-poller                            
   24 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 watchdogd                              
   30 root      20   0       0      0      0 S  0.0  0.0   0:26.98 kswapd0            
```

PID:  进程ID，进程的唯一标识
USER： 进程所有者的实际用户名
PR： 进程的调度优先级，这些值中有些是rt，表示这些进程运行在实时态
NI： 进程的nice值（优先级）越小的值意味着越高的优先级。负值表示高优先级，正值表示低优先级
VIRT： 进程使用的虚拟内存。进程使用的虚拟内存总量，单位是kb，VIRT=SWAP+RES
RES: 驻留内存大小，驻留内存是任务使用的非交换物理内存大小。进程使用的、未被换出的物理内存大小，单位是kb，RES=CODE+DATA
SHR： SHR是进程使用的共享内存。
S： 这个是进程的状态。它有以下不同的值：
  * D - 不可中断的睡眠态
  * R - 运行态
  * S - 睡眠态
  * T - 被跟踪或已停止
  * Z - 僵尸态
%CPU： 自从上一次更新时到现在任务所使用的CPU时间百分比
%MEM： 进程使用的可用物理内存百分比
TIME+： 任务启动后到现在所使用的全部CPU时间，精确到百分之一秒。
COMMAND：运行进程所使用的命令。进程名称（命令名/命令行）

还有许多在默认情况下不会显示的输出，他们可以显示进程的页错误、有效组、组ID和其他更多的信息。




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