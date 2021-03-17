# 多线程-基础知识分享
## 前言
!>主要内容是: 自定义线程池、以及浅谈线程的死锁.
## java多线程
java 语言内置多线程支持，一个java程序实际上是一个JVM进程， jvm用一个主线程来执行main()方法， 在main方法中 又可以启动多个线程。  
####  线程和进程的区别
* 线程是比进程更小的运行单位： 多个线程可以共享一个进程 堆和方法共享区，同时 线程有拥有自己独立的 本地方法栈，虚拟机方法栈和程序计数器。  
* 一个进程包含一个或多个线程。  
* 线程是由操作系统调度的最小任务单位  
* 多进程的之间通信 比多线程间通信慢， 但是多进程比多线程稳定性更高。  

#### 定义一个新线程
*  方式一：继承extends Thread  重写 run()   创建实例 调用 .start() 方法来启动定义的新线程  
*  方式二：实现 runnable 接口  重写 run()   创建runnable实例 ，创建Thread实例并且把创建的runnable传入Thread中， 调用 .start()方法来启动线程。  

#### 线程的状态及执行
- 线程的执行没有先后顺序
- Thread.sleep(100) 当前线程暂停100毫秒
- 多线程是交替运行 无法确定先后，  如果要等待一个线程执行完 则调用 .join()方法来等一个线程执行完。
- run()方法是 java虚拟机 自动调用的，  源码可知 run是 native修饰的 只能被 jvm 调用。
- 在main方法中 直接调用run()并不会启动新线程。
- 线程的6种状态 ：

状态|说明|备注
:----|:----|:----
new|创建|创建Thread实例
runnable|运行中|执行start()方法
Bloacked|阻塞|
waiting|等待|wait() 等待其他线程来notify  
timed waiting|计时等待|sleep(1000)//休息一秒自动启动
Terminated|已终止|run()方法结束 （正常结束，抛出异常， Thread.stop()） 三种结束情况

## 线程及线程池
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163037.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163103.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163118.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163131.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163145.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163204.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163415.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163431.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163453.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163521.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163537.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163553.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163606.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163631.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163649.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163705.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163721.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163739.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163756.png)  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210305163814.png)  
## 合理的配置线程池
要想合理的配置线程池，就必须首先分析任务特性，可以从以下几个角度来进行分析：    
- 任务的性质：CPU密集型任务，IO密集型任务和混合型任务。    
- 任务的优先级：高，中和低。  
- 任务的执行时间：长，中和短。  
- 任务的依赖性：是否依赖其他系统资源，如数据库连接。  
任务性质不同的任务可以用不同规模的线程池分开处理。
  
!> CPU密集型任务配置尽可能少的线程数量，如配置Ncpu+1个线程的线程池。  
IO密集型任务则由于需要等待IO操作，线程并不是一直在执行任务，则配置尽可能多的线程，如2*Ncpu。
混合型的任务,如果可以拆分，则将其拆分成一个CPU密集型任务和一个IO密集型任务，只要这两个任务执行的时间相差不是太大，  
那么分解后执行的吞吐率要高于串行执行的吞吐率，如果这两个任务执行时间相差太大，则没必要进行分解。  
我们可以通过Runtime.getRuntime().availableProcessors()方法获得当前设备的CPU个数。  
优先级不同的任务可以使用优先级队列PriorityBlockingQueue来处理。它可以让优先级高的任务先得到执行，  
需要注意的是如果一直有优先级高的任务提交到队列里，那么优先级低的任务可能永远不能执行。  
执行时间不同的任务可以交给不同规模的线程池来处理，或者也可以使用优先级队列，让执行时间短的任务先执行。  
依赖数据库连接池的任务，因为线程提交SQL后需要等待数据库返回结果，如果等待的时间越长CPU空闲时间就越长，  
那么线程数应该设置越大，这样才能更好的利用CPU。  
建议使用有界队列，有界队列能增加系统的稳定性和预警能力，可以根据需要设大一点，比如几千。  

有一次我们组使用的后台任务线程池的队列和线程池全满了，不断的抛出抛弃任务的异常，通过排查发现是数据库出现了问题，  
导致执行SQL变得非常缓慢，因为后台任务线程池里的任务全是需要向数据库查询和插入数据的，所以导致线程池里的工作线程全部阻塞住，  
任务积压在线程池里。如果当时我们设置成无界队列，线程池的队列就会越来越多，有可能会撑满内存，导致整个系统不可用，而不只是后台任务出现问题。  
当然我们的系统所有的任务是用的单独的服务器部署的，而我们使用不同规模的线程池跑不同类型的任务，但是出现这样问题时也会影响到其他任务。


