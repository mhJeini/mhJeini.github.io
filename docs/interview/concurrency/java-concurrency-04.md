# 1. 如何检测一个线程是否拥有锁？
java.lang.Thread中有一个方法叫holdsLock()，它返回true如果当且仅当当前线程拥有某个具体对象的锁。

# 2. Java 中如何获取线程堆栈？
**kill -3 [java pid]**

不会在当前终端输出，它会输出到代码执行的位置或指定的位置。例如，kill -3 tomcat pid，输出堆栈到log目录下。

**Jstack [java pid]**

在当前终端显示，也可以重定向到指定文件中。

**-JvisualVM：Thread Dump**

打开JvisualVM后，都是界面操作，过程比较简单。

# 3. Thread 类中 yield() 方法有什么作用？
yield()方法使当前线程从执行状态（运行状态）变为可执行态（就绪状态）。

当前线程到了就绪状态，至于哪个线程会从就绪状态变成执行状态，有可能是当前线程，也可能是其他线程，这就看系统的分配了。

# 4. Java 中 ConcurrentHashMap 并发度是什么？
ConcurrentHashMap把实际map划分成若干部分来实现它的可扩展性和线程安全。这种划分是使用并发度获得的，它是ConcurrentHashMap类构造函数的一个可选参数，默认值为16，这样在多线程情况下就能避免争用。

在JDK1.8版本之后，ConcurrentHashMap摒弃了Segment（锁段）的概念，而是启用了一种全新的方式实现，利用CAS算法。同时加入了更多的辅助变量来提高并发度，具体内容可以关注微信公众号“Java精选”，源码解析。

# 5. Java 中 Semaphore 是什么？
Java中Semaphore是一种新的同步类，它是一个计数信号。

从概念上讲，信号量维护了一个许可集合。如有必要，在许可可用前会阻塞每一个acquire()，然后再获取该许可。

每个release()添加一个许可，从而可能释放一个正在阻塞的获取者。但是，不使用实际的许可对象，Semaphore只对可用许可的号码进行计数，并采取相应的行动。信号量常常用于多线程的代码中，比如数据库连接池。

# 6. Java 线程池中 submit() 和 execute() 方法有什么区别？
submit()和execute()两个方法都可以向线程池提交任务。

execute()方法的返回类型是void，它定义在Executor接口中。

submit()方法可以返回持有计算结果的Future对象，它定义在ExecutorService接口中，它扩展了Executor接口，其它线程池类像ThreadPoolExecutor和ScheduledThreadPoolExecutor都有这些方法。

# 7. Java 中 volatile 变量和 atomic 变量有什么不同？
Volatile变量可以确保先行关系，即写操作会发生在后续的读操作之前, 但它并不能保证原子性。例如用volatile修饰count变量那么count++操作就不是原子性的。

AtomicInteger类提供的atomic()方法可以让这种操作具有原子性如getAndIncrement()方法会原子性的进行增量操作把当前值加一，其它数据类型和引用变量也可以进行相似操作。

# 8. 如何保证运行中的线程暂停一段时间？
使用Thread类的sleep()方法可以让线程暂停一段时间。

需要注意的是，这并不会让线程终止，一旦从休眠中唤醒线程，线程的状态将会被改变为Runnable，并且根据线程调度，它将得到执行。

# 9. 线程优先级是什么？
每一个线程都是有优先级的，一般来说，高优先级的线程在运行时会具有优先权，但这依赖于线程调度的实现，这个实现是和操作系统相关的(OS dependent)。我们可以定义线程的优先级，但是这并不能保证高优先级的线程会在低优先级的线程前执行。线程优先级是一个int变量(从1-10)，1代表最低优先级，10代表最高优先级。

java的线程优先级调度会委托给操作系统去处理，所以与具体的操作系统优先级有关，如非特别需要，一般无需设置线程优先级。

# 10. 什么是线程调度器和时间分片？
线程调度器（Thread Scheduler）是一个操作系统服务，它负责为Runnable状态的线程分配CPU时间。一旦我们创建一个线程并启动它，它的执行便依赖于线程调度器的实现。

同上一个问题，线程调度并不受到Java虚拟机控制，所以由应用程序来控制它是更好的选择（也就是说不要让你的程序依赖于线程的优先级）。

时间分片（Time Slicing）是指将可用的CPU时间分配给可用的Runnable线程的过程。分配CPU时间可以基于线程优先级或者线程等待的时间。

# 11. JVM 中什么参数用于控制线程的栈堆大小？
-Xss 每个线程的栈大小

# 12. 什么是 Java 内存模型？
Java内存模型定义了java虚拟机在计算机内存中的工作方式。

JMM决定了一个线程对共享变量的写入何时对另一个线程可见。

从抽象的角度来看，JMM定义了线程和主内存之间的抽象关系：

>线程之间的共享变量存储在主内存中，每一个线程都有一个私有的本地内存，本地内存中存储了该线程以读/写共享变量的副本。

# 13. 多线程实现的方式有几种？
1）继承Thread类创建线程

Thread类本质上是实现了Runnable接口的一个实例，代表一个线程的实例。

启动线程的唯一方法是通过Thread类的start()实例方法。

start()方法将启动一个新线程，并执行run()方法。

这种方式实现多线程比较简单，通过自己的类直接继承Thread，并重写run()方法，就可以启动新线程并执行自己定义的run()方法。

2）实现Runnable接口创建线程

如果自己的类已经继承了两一个类，就无法再继承Thread，因此可以实现一个Runnable接口

3）实现Callable接口，通过FutureTask包装器来创建Thread线程

4）使用`ExecutorService`、`Callable`、`Future`实现有返回结果的线程

`ExecutorService`、`Callable`、`Future`三个接口实际上都是属于Executor框架。

在JDK1.5中引入的新特征，不需要为了得到返回值而大费周折。主要需要返回值的任务必须实现Callable接口；不需要返回值的任务必须实现Runnabel接口。

执行Callable任务后，可以获取一个Future对象，在该对象上调用get()方法可以获取到Callable任务返回的Object了。注意的是get()方法是阻塞的，线程没有返回结果时，该方法会一直等待。

# 14. synchronized 和 ReentrantLock 有什么区别？
synchronized是和if、else、for、while一样的关键字，而ReentrantLock是类，这是二者的本质区别。

ReentrantLock提供了比synchronized更加灵活的特性，可以被继承，存在方法、变量等，ReentrantLock比synchronized的扩展性体现在以下几点：

1）ReentrantLock可以对获取锁的等待时间进行设置，这样就避免了死锁

2）ReentrantLock可以获取各种锁的信息

3）ReentrantLock可以灵活地实现多路通知

synchronized竞争锁时会一直等待，而ReentrantLock可以尝试获取锁，并得到获取结果。

synchronized获取锁无法设置超时，而ReentrantLock可以设置获取锁的超时时间。

synchronized无法实现公平锁，而ReentrantLock可以满足公平锁，即先等待先获取到锁。

synchronized控制等待和唤醒需要结合加锁对象的wait()和notify()、notifyAll()，而ReentrantLock控制等待和唤醒需要结合Condition的await()和signal()、signalAll()方法。

synchronized是JVM层面实现的；ReentrantLock是JDK代码层面实现。

synchronized锁机制是操作对象头中Mark Word，而ReentrantLock底层调用的是Unsafe的park()方法加锁。

synchronized在加锁代码块执行完或出现异常时自动释放锁；ReentrantLock不会自动释放锁，需要在finally{}代码块显示释放。

# 15. Java 中 volatile 关键字有什么作用？
Java语言提供了弱同步机制，即volatile变量，以确保变量的更新通知其他线程。

volatile变量具备变量可见性、禁止重排序两种特性。

volatile变量不会被缓存在寄存器或者对其他处理器不可见的地方，因此在读取volatile类型的变量时总会返回最新写入的值。

volatile变量的两种特性：

**变量可见性**

保证该变量对所有线程可见，这里的可见性指的是当一个线程修改了变量的值，那么新的值对于其他线程是可以立即获取的。

**禁止重排序**

volatile禁止了指令重排。比sychronized更轻量级的同步锁。在访问volatile 变量时不会执行加锁操作，因此也就不会使执行线程阻塞，因此volatile变量是一种比sychronized关键字更轻量级的同步机制。

volatile适合场景：一个变量被多个线程共享，线程直接给这个变量赋值。

当对非volatile 变量进行读写的时候，每个线程先从内存拷贝变量到CPU缓存中。如果计算机有多个CPU，每个线程可能在不同的CPU上被处理，这意味着每个线程可以拷贝到不同CPU cache中。而声明变量是volatile的，JVM 保证了每次读变量都从内存中读，跳过CPU cache这一步。

适用场景

值得说明的是对volatile变量的单次读/写操作可以保证原子性的，如long和double类型变量，但是并不能保证“i++”这种操作的原子性，因为本质上i++是读、写两次操作。在某些场景下可以代替Synchronized。但是，volatile的不能完全取代Synchronized的位置，只有在一些特殊的场景下，才能适用volatile。

总体来说，需要必须同时满足下面两个条件时才能保证并发环境的线程安全：

1）对变量的写操作不依赖于当前值（比如 “i++”），或者说是单纯的变量赋值（boolean flag = true）。

2）该变量没有包含在具有其他变量的不变式中，也就是说，不同的volatile变量之间，不 能互相依赖。只有在状态真正独立于程序内其他内容时才能使用volatile。

# 16. ConcurrentHashMap 和 Hashtable 有什么区别？
ConcurrentHashMap和Hashtable都可以用于多线程的环境，但当Hashtable的大小增加到一定的时候，性能会急剧下降，因为迭代时需要被锁定很长的时间。

HashTable的任何操作都会把整个表锁住，是阻塞的。好处是：总能获取最实时的更新，比如说线程A调用putAll()写入大量数据，期间线程B调用get()，线程B就会被阻塞，直到线程A完成putAll()，因此线程B肯定能获取到线程A写入的完整数据。坏处是所有调用都需要排队，效率较低。

ConcurrentHashMap是设计为非阻塞的。在更新时会局部锁住某部分数据，但不会把整个表都锁住。同步读取操作则是完全非阻塞的。好处是在保证合理的同步前提下，效率很高。坏处是：严格来说，读取操作不能保证反映最近的更新。例如线程A调用putAll()写入大量数据，期间线程B调用get()，则只能get()到目前为止已经顺利插入的部分数据。

JDK8的版本，与JDK6的版本有很大差异。实现线程安全的思想也已经完全变了，它摒弃了Segment（分段锁）的概念，而是启用了一种全新的方式实现，利用CAS算法。它沿用了与它同时期的HashMap版本的思想，底层依然由数组+链表+红黑树的方式思想，但是为了做到并发，又增加了很多复制类，例如TreeBin、Traverser等对象内部类。CAS算法实现无锁化的修改至操作，他可以大大降低锁代理的性能消耗。这个算法的基本思想就是不断地去比较当前内存中的变量值与你指定的一个变量值是否相等，如果相等，则接受你指定的修改的值，否则拒绝你的操作。因为当前线程中的值已经不是最新的值，你的修改很可能会覆盖掉其他线程修改的结果。

# 17. 线程池都有哪些拒绝策略？
当线程池的任务缓存队列已满且线程池中的线程数目达到maximumPoolSize时，如果还有任务到来就会采取任务拒绝策略，通常有以下四种策略：

ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。

ThreadPoolExecutor.DiscardPolicy：丢弃任务，但是不抛出异常。

ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新提交被拒绝的任务

ThreadPoolExecutor.CallerRunsPolicy：由调用线程（提交任务的线程）处理该任务。

# 18. 什么是线程池？
线程池就是创建若干个可执行的线程放入一个池（容器）中，有任务需要处理时，会提交到线程池中的任务队列，处理完之后线程并不会被销毁，而是仍然在线程池中等待下一个任务。

# 19. 为什么要使用线程池？
Java中如果每个请求到达就创建一个新线程，开销是相当大的。在实际使用中，服务器在创建和销毁线程上花费的时间和消耗的系统资源都相当大，甚至可能要比在处理实际的用户请求的时间和资源要多的多。

除了创建和销毁线程的开销之外，活动的线程也需要消耗系统资源。如果在一个JVM中创建太多的线程，可能会使系统由于过度消耗内存或“切换过度”而导致系统资源不足。

为了防止资源不足，服务器应用程序需要采取一些办法来限制任何给定时刻处理的请求数目，尽可能减少创建和销毁线程的次数，特别是一些资源耗费比较大的线程的创建和销毁，尽量利用已有对象来进行服务，这就是“池化资源”技术产生的原因。

线程池主要用来解决线程生命周期开销问题和资源不足问题。通过对多个任务重复使用线程，线程创建的开销就被分摊到了多个任务上了，而且由于在请求到达时线程已经存在，所以消除了线程创建所带来的延迟。这样，就可以立即为请求服务，使用应用程序响应更快。

另外，通过适当的调整线程中的线程数目可以防止出现资源不足的情况。

# 20. Java 中创建线程池有哪些参数？
从java.util.concurrent.ThreadPoolExecutor源码中可以看出，线程池的构造函数有7个参数，分别是corePoolSize、maximumPoolSize、keepAliveTime、unit、workQueue、threadFactory、handler。

```java
public ThreadPoolExecutor(int corePoolSize,
						  int maximumPoolSize,
						  long keepAliveTime,
						  TimeUnit unit,
						  BlockingQueue<Runnable> workQueue) {
	this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
		 Executors.defaultThreadFactory(), defaultHandler);
}
```

1、corePoolSize 线程池核心线程大小

线程池中会维护一个最小的线程数量，即使这些线程处理空闲状态，他们也不会被销毁，除非设置了allowCoreThreadTimeOut。这里的最小线程数量即是corePoolSize。

2、maximumPoolSize 线程池最大线程数量

一个任务被提交到线程池以后，首先会找有没有空闲存活线程，如果有则直接将任务交给这个空闲线程来执行，如果没有则会缓存到工作队列（后面会介绍）中，如果工作队列满了，才会创建一个新线程，然后从工作队列的头部取出一个任务交由新线程来处理，而将刚提交的任务放入工作队列尾部。线程池不会无限制的去创建新线程，它会有一个最大线程数量的限制，这个数量即由maximunPoolSize指定。

3、keepAliveTime 空闲线程存活时间

一个线程如果处于空闲状态，并且当前的线程数量大于corePoolSize，那么在指定时间后，这个空闲线程会被销毁，这里的指定时间由keepAliveTime来设定

4、unit 空闲线程存活时间单位

keepAliveTime的计量单位

5、workQueue 工作队列

新任务被提交后，会先进入到此工作队列中，任务调度时再从队列中取出任务。jdk中提供了四种工作队列：

1）ArrayBlockingQueue

基于数组的有界阻塞队列，按FIFO排序。新任务进来后，会放到该队列的队尾，有界的数组可以防止资源耗尽问题。当线程池中线程数量达到corePoolSize后，再有新任务进来，则会将任务放入该队列的队尾，等待被调度。如果队列已经是满的，则创建一个新线程，如果线程数量已经达到maxPoolSize，则会执行拒绝策略。

2）LinkedBlockingQuene

基于链表的无界阻塞队列（其实最大容量为Interger.MAX），按照FIFO排序。由于该队列的近似无界性，当线程池中线程数量达到corePoolSize后，再有新任务进来，会一直存入该队列，而不会去创建新线程直到maxPoolSize，因此使用该工作队列时，参数maxPoolSize其实是不起作用的。

3）SynchronousQuene

一个不缓存任务的阻塞队列，生产者放入一个任务必须等到消费者取出这个任务。也就是说新任务进来时，不会缓存，而是直接被调度执行该任务，如果没有可用线程，则创建新线程，如果线程数量达到maxPoolSize，则执行拒绝策略。

4）PriorityBlockingQueue

具有优先级的无界阻塞队列，优先级通过参数Comparator实现。

6、threadFactory 线程工厂

创建一个新线程时使用的工厂，可以用来设定线程名、是否为daemon线程等等

7、handler 拒绝策略

当工作队列中的任务已到达最大限制，并且线程池中的线程数量也达到最大限制，这时如果有新任务提交进来，就会执行拒绝策略。
