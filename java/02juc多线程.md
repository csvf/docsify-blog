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
*  方式一：继承`extends Thread`  重写 `run()`   创建实例 调用 .start() 方法来启动定义的新线程  
*  方式二：实现 `runnable` 接口  重写 `run()`   创建`runnable`实例 ，创建`Thread`实例并且把创建的runnable传入Thread中， 调用 .start()方法来启动线程。  

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
Terminated|已终止|run()方法结束 （正常结束，抛出异常， `Thread.stop()`） 三种结束情况
###  中断线程
例如: 下载视频文件    
其他线程给需要中断的线程发一个信号， 在主线程main 方法中 调用 t.interrupt 方法来中断t线程，  
在t线程的 run方法中 获取 isInterrupt() 中断状态 线程等待时获取到 `interruptException`即终止线程。   
或者在需要终止的线程中 定义一个 `volatile boolean`  变量 。在run()方法中判断为`true`继续执行，`false`时终止。  
在`main`主线程中 修改 `volatile`变量 ， 定义的变量终止执行。  
### volatile 
关键是用来修饰线程间共享变量的。 每次访问变量总是获取主内存的最新值， 每次修改变量后，立刻回写到主内存中。  
### 守护线程
t.setDaemon(true) 把一个线程变为守护线程， 守护线程是为其他线程服务的线程， Jvm中当非守护线程执行完毕后，虚拟机退出。    
守护线程不能持有资源，如打开文件等。  
### 多线程同步
多线程同时修改变量时会造成逻辑错误需要使用`synchronized`进行加锁   
    1. 加锁方式一：使用`synchronized`对需要同步的语句块进行加锁，  
不同的线程访问同一变量时 需要对同一个实例对象加锁，即同一把锁   
`synchronized(Main.Object){ //加锁语句块}` 原子操作不需要加锁       
例如在Main.java中定义一个静态变量 Object    
    2. 加锁方式二： 使用`synchronized` 修饰方法，把方法变为同步代码块 该方法同一时刻只允许一个线程访问。  
原子操作不需要加锁， 如 基本类型赋值（`long` 和 `double` ）除外 ； 对引用类型赋值   
### 线程安全的类 synchronized
`LocalDate`  `String`  `Integer` 等不能被继承的类成员变量一旦创建不能更改，只能读 不能写 不用同步   
`Math` 等没有成员变量的类， 没有成员变量 
正确使用`synchronized`方法的类 如`StringBuffer`  
### 非线程安全的类 
不能在多线程中共享实例并且修改 `例如ArrayList`, 可以以只读的方式在多线程中共享。  
### 死锁
java的线程锁是`可重入的锁`，  即 在同步代码块中   仍然可以设置对同一实例的同步代码块   
例如:    
 ```java
synchronized(Main.Object){
    system.out.println("获得同步锁：ddd");
    synchronized(Main.Object){
     System.out.println("获得同步锁：重入ddd")
    }
}
```
>当两个线程分别获取了不同的锁，又同时等待被对方占用的锁的时 就产生了**死锁**。 死锁无法解除，只能强制结束JVM进程。

线程1
```java
synchronized(Main.ObjectA){
    system.out.println("获得同步锁：ddd");
    synchronized(Main.ObjectB){
     System.out.println("获得同步锁：重入ddd")
    }
}
```
线程2
```java
synchronized(Main.ObjectB){
  system.out.println("获得同步锁：ddd");
  synchronized(Main.ObjectA){
     System.out.println("获得同步锁：重入ddd")
  }
}
```
- 线程1 获得  ObjectA锁  .... 线程2获得 ObjectB锁   
- 线程1往下执行 		..... 线程2往下执行  
- 线程1试图获取ObjectB锁 ...  线程2试图获取ObjectA锁  
- 由于ObjectB锁被 线程2占用， 导致线程1处于等待状态；  
- ObjectA锁被线程1占用，导致线程2处于等待状态。 两个线程无限制的等待下去就产生了死锁。  
 >避免死锁的方法： 多线程获取锁的顺序必须完全一致。

### wait、notify 
用于多线程协调运行 ：  
必须在已获得锁 `synchronized`的对象上 执行 `wait /notify`  `notifyAll` 方法     
在 synchronized代码块中调用 `wait`  `notify`  `notifyAll` 方法  ， `wait` 通常在while循环中使用。  
wait 释放锁 进入等待状态，被唤醒后需要重新获取锁才能继续执行。    
notify 用来唤醒其他等待线程， 可能有多个线程在等待， 保险起见 使用`notifyAll` 唤醒所有等待的线程。  
- 应用场景:  线程A 向`Taskqueue`队列中写入内容，  线程B不断的从`TaskQueue`中读取信息，    
当队列中的元素为空时 线程B进入等待状态， 线程A写入队列后唤醒B   
###  锁Lock
`java.util.concurrent`包是jdk1.5引入的   
- **ReentrantLock**  
第一步 实例化     
`Lock lock = new  ReentrantLock();`  
第二步 获取锁 可能会失败 ，要放在try 同步块之前。  
`lock.lock();`    
第三步用完之后解锁unloack();  
```java
try{ 
 //同步块
 }finally{ 
 lock.unlock();
}
``` 

!>`ReentrantLock()` 可以替代 `synchronized`   
`Condition` 用来替换 与 `synchronized`对应的 `wait` `notify` `notifyAll` 等待和唤醒 线程功能    
`ReentrantLock() `获取锁更安全， 在获取锁之前可以 使用tryLock()的方法 设置超时间， 如果超时没获取到锁 则放弃获取锁的操作。    
`ReentrantLock()` 必须使用 `try {}finally{}` 方式正确释放锁。  
```java
//Condition的方法
 Lock lock = new Reentrant();
 Condition con = lock.newCondition();

 con.await()方法  >>>  this.wait()
 con.signalAll()方法 >>> this.notifyAll()
 con.signal()方法     >>> this.notify()

```


- **ReadWriteLock**   
`ReadWriteLock`： 可以生成读锁或者写锁  当加读锁的时候 允许多线程访问。
`Lock lock  = new ReadWriteLock();`  
`Lock rlock = lock.readLock();` //获取读锁  
`Lock wlock = lock.writeLock();`//获取写锁  
`ReadWriteLock` 常用在读多写少的场景。  
!>同一时刻 允许多个线程同时读， 只允许一个线程写。  
与 ReentrantLock相比 性能有所提升，`ReentrantLock` 同一时刻只允许一个线程访问(`读` 或者 `写` 同一时刻只允许一个线程访问
当多个线程读取的时候性能不高)。  
适合同一个实例有多线程的读多 写少的场景。      

### 线程安全的对象集合

* `java.util.concurrent` 集合中提供了线程安全的Blocking  集合   
多线程访同时访问 `Blocking`集合是安全的， 使用 `concurrent`包中的集合 在多线程读写时可以比避免自己写同步代码   

|接口|线程不安全|线程安全|
| ---- | ---- |---- |
| List | ArrayList |CopyonWriteArrayList |
| Set | HashSet TreeSet |CopyonWriteArraySet |
| Map | HashMap |concurrentHashMap |
| Queue | LinkedList ArrayDeque |ArrayBlockingQueue LinkedBlockingQueque |
| Deque | LinkedList ArrayDeque |LinkedBlockingDeque |

* `java.util.concurrent.atomic` 原子操作  实现无锁的线程安全操作  
Atomic 通常用于多线安全的序列生成器，计数器，累加器。  

```java
 AtomicLong at =  new AtomicLong();
 at.incrementAndGet();
```

|线程不安全类型|线程安全类型|
|---- |---- |
|Integer |AtomicInteger |
|Long |AtomicLong |
|Boolean |AtomicBoolean |
|整型Array |AtomicIntegerArray |
|长整型Array |AtomicLongArray |
|对象 |AtomicReference |





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


