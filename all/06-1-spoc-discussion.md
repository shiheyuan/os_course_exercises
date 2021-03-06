# 调度算法概念(lec 15) spoc 思考题


- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。


## 个人思考题

### 15.1 处理机调度概念

1. 如何判断操作系统是否是可抢先的？
2. 在什么时候可以进行处理机调度？

 > 调度时机的所有可能

2. 当操作系统的处理机调度导致线程切换时，暂停进程的当前指令指针可能在什么位置？用户态代码或内核代码？给出理由。

### 15.2 调度准则

1. 处理机调度的目标是什么？

 > CPU利用率（CPU使用率、吞吐率、周转时间、等待时间）

 > 用户感受（周转时间、等待时间、响应时间、响应时间的方差）

 > 公平性（CPU时间分配公平性）

1. 尝试在ucore上写一个外排序程序，然后分析它的执行时间分布统计（每次切换后开始执行时间和放弃CPU的时间、当前用户和内核栈信息）。
2. 在Linux上有一个应用程序time，可以统计应用程序的执行时间信息。请分析它是如何统计进程执行时间信息的。如可能，请在ucore上实现相同功能的应用程序。下面是可能的参考。
   - [Linux用户态程序计时方式详解](http://www.cnblogs.com/clover-toeic/p/3845210.html)
   - [Get Source Code for any Linux Command](http://www.thegeekstuff.com/2010/02/get-source-code-for-any-linux-command/)
   - [How does time command work](http://unix.stackexchange.com/questions/29800/how-does-time-command-work)
   - http://cvs.opensolaris.org/source/xref/onnv/onnv-gate/usr/src/cmd/time/time.c
   - https://github.com/illumos/illumos-gate/blob/master/usr/src/cmd/time/time.c
   

3. 尝试获取一个操作系统的调度算法的性能统计数据（CPU使用率、进程执行）。
 
### 15.3 先来先服务、短进程优先和最高响应比优先调度算法

1. 尝试描述下列处理机调度算法的工作原理和算法优缺点。
   - 先来先服务算法（FCFC、FIFO）
   - 短进程优先算法（SPN、SJF）
   - 最高响应比优先算法（HRRN）

2. 如何调试你的调度算法？

### 15.4 时间片轮转、多级反馈队列、公平共享调度算法和ucore调度框架
 
1. 尝试描述下列处理机调度算法的工作原理和算法优缺点。
   - 时间片轮转算法（RR）
   - 多级反馈队列算法（MLFQ）
   - 公平共享调度算法（FSS）

1. RR算法选择时间片长度的依据有哪些？
2. 为什么要引入调度框架？定义调度算法接口需要考虑哪些因素？
3. 尝试跟踪ucore中调度算法的选择依据。
4. 请描述在MLFQ中，如何提升或降低进程的优先级？

### 15.5 实时调度和多处理器调度

1. 尝试描述下列处理机调度算法的工作原理和算法优缺点。
   - 速率单调调度算法(RM, Rate Monotonic) 
   - 最早截止时间优先算法 (EDF, Earliest Deadline First) 

2. 有兴趣的同学，请阅读下面论文，然后说明实时调度面临的主要困难是什么？
   - [Buttazzo, “Rate monotonic vs. EDF: Judgement Day”, EMSOFT 2003.](http://www.arl.wustl.edu/~gorinsky/cited/SPTS_Judgment_Buttazzo_2005.pdf)
   - [单调速率及其扩展算法的可调度性判定](http://www.jos.org.cn/ch/reader/create_pdf.aspx?file_no=20040602)

3. 多处理机调度中每个处理机一个就绪队列与整个系统一个就绪队列有什么不同？

### 15.6 优先级反置

1. 什么是优先级反置现象？

2. 什么是优先级继承(Priority Inheritance)和优先级天花板协议(priority ceiling protocol)？它们的区别是什么？

3. 在单CPU情况下，基于以前的OS知识，能否设计一个更简单的方法（也许执行效率会低一些）解决优先级反置现象？
4. ppt中“优先级继承”页里的图示有误，你能指出来吗？


## 小组练习与思考题

### (1)(spoc) 理解并实现FIFO调度算法

可基于“python, ruby, C, C++，LISP、JavaScript”等语言模拟实现，并给出测试，在试验报告写出设计思路和测试结果分析。

请参考[scheduler-homework.py](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab6/scheduler-homework.py)代码或独自实现。
最后统计采用不同调度算法的每个任务的相关时间和总体的平均时间：

　- turnaround time　周转时间
　- response time 响应时间
　- wait time　等待时间

#### 对模拟环境的抽象
- 任务/进程，及其执行时间
  Job 0 (length = 1)
  Job 1 (length = 4)
  Job 2 (length = 7)

 - 何时切换？
 - 如何统计？
 
#### 执行结果

采用FIFO调度算法

```
 ./scheduler-homework.py  -p FIFO
ARG policy FIFO
ARG jobs 3
ARG maxlen 10
ARG seed 0

Here is the job list, with the run time of each job: 
  Job 0 ( length = 9 )
  Job 1 ( length = 8 )
  Job 2 ( length = 5 )


** Solutions **

Execution trace:
  [ time   0 ] Run job 0 for 9.00 secs ( DONE at 9.00 )
  [ time   9 ] Run job 1 for 8.00 secs ( DONE at 17.00 )
  [ time  17 ] Run job 2 for 5.00 secs ( DONE at 22.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 9.00  Wait 0.00
  Job   1 -- Response: 9.00  Turnaround 17.00  Wait 9.00
  Job   2 -- Response: 17.00  Turnaround 22.00  Wait 17.00

  Average -- Response: 8.67  Turnaround 16.00  Wait 8.67

```
### (2)(spoc) 理解并实现SJF调度算法

可基于“python, ruby, C, C++，LISP、JavaScript”等语言模拟实现，并给出测试，在试验报告写出设计思路和测试结果分析。

#### 执行结果

采用SJF调度算法
```
 ./scheduler-homework.py  -p SJF
ARG policy SJF
ARG jobs 3
ARG maxlen 10
ARG seed 0

Here is the job list, with the run time of each job: 
  Job 0 ( length = 9 )
  Job 1 ( length = 8 )
  Job 2 ( length = 5 )


** Solutions **

Execution trace:
  [ time   0 ] Run job 2 for 5.00 secs ( DONE at 5.00 )
  [ time   5 ] Run job 1 for 8.00 secs ( DONE at 13.00 )
  [ time  13 ] Run job 0 for 9.00 secs ( DONE at 22.00 )

Final statistics:
  Job   2 -- Response: 0.00  Turnaround 5.00  Wait 0.00
  Job   1 -- Response: 5.00  Turnaround 13.00  Wait 5.00
  Job   0 -- Response: 13.00  Turnaround 22.00  Wait 13.00

  Average -- Response: 6.00  Turnaround 13.33  Wait 6.00
```
### (3)(spoc) 理解并实现RR调度算法

可基于“python, ruby, C, C++，LISP、JavaScript”等语言模拟实现，并给出测试，在试验报告写出设计思路和测试结果分析。

#### 执行结果


采用RR调度算法
```
 ./scheduler-homework.py  -p RR
ARG policy RR
ARG jobs 3
ARG maxlen 10
ARG seed 0

Here is the job list, with the run time of each job: 
  Job 0 ( length = 9 )
  Job 1 ( length = 8 )
  Job 2 ( length = 5 )


** Solutions **

Execution trace:
  [ time   0 ] Run job   0 for 1.00 secs
  [ time   1 ] Run job   1 for 1.00 secs
  [ time   2 ] Run job   2 for 1.00 secs
  [ time   3 ] Run job   0 for 1.00 secs
  [ time   4 ] Run job   1 for 1.00 secs
  [ time   5 ] Run job   2 for 1.00 secs
  [ time   6 ] Run job   0 for 1.00 secs
  [ time   7 ] Run job   1 for 1.00 secs
  [ time   8 ] Run job   2 for 1.00 secs
  [ time   9 ] Run job   0 for 1.00 secs
  [ time  10 ] Run job   1 for 1.00 secs
  [ time  11 ] Run job   2 for 1.00 secs
  [ time  12 ] Run job   0 for 1.00 secs
  [ time  13 ] Run job   1 for 1.00 secs
  [ time  14 ] Run job   2 for 1.00 secs ( DONE at 15.00 )
  [ time  15 ] Run job   0 for 1.00 secs
  [ time  16 ] Run job   1 for 1.00 secs
  [ time  17 ] Run job   0 for 1.00 secs
  [ time  18 ] Run job   1 for 1.00 secs
  [ time  19 ] Run job   0 for 1.00 secs
  [ time  20 ] Run job   1 for 1.00 secs ( DONE at 21.00 )
  [ time  21 ] Run job   0 for 1.00 secs ( DONE at 22.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 22.00  Wait 13.00
  Job   1 -- Response: 1.00  Turnaround 21.00  Wait 13.00
  Job   2 -- Response: 2.00  Turnaround 15.00  Wait 10.00

  Average -- Response: 1.00  Turnaround 19.33  Wait 12.00

```

### (4)理解并实现MLFQ调度算法

可基于“python, ruby, C, C++，LISP、JavaScript”等语言模拟实现，并给出测试，在试验报告写出设计思路和测试结果分析。

### (5)理解并实现stride调度算法

可基于“python, ruby, C, C++，LISP、JavaScript”等语言模拟实现，并给出测试，在试验报告写出设计思路和测试结果分析。

### (6)理解并实现EDF实时调度算法

可基于“python, ruby, C, C++，LISP、JavaScript”等语言模拟实现，并给出测试，在试验报告写出设计思路和测试结果分析。

### (7)理解并实现RM实时调度算法

可基于“python, ruby, C, C++，LISP、JavaScript”等语言模拟实现，并给出测试，在试验报告写出设计思路和测试结果分析。

### (8)理解并实现优先级反置方法

可基于“python, ruby, C, C++，LISP、JavaScript”等语言模拟实现，并给出测试，在试验报告写出设计思路和测试结果分析。
