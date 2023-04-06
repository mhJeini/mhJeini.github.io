# 1. 什么是 JVM？
Java程序的跨平台特性主要是指字节码文件可以在任何具有Java虚拟机的计算机或者电子设备上运行，Java虚拟机中的Java解释器负责将字节码文件解释成为特定的机器码进行运行。

因此在运行时，Java源程序需要通过编译器编译成为.class文件。

众所周知java.exe是java class文件的执行程序，但实际上java.exe程序只是一个执行的外壳，它会装载jvm.dll（windows下，下皆以windows平台为例，linux下和solaris下其实类似，为：libjvm.so），这个动态连接库才是java虚拟机的实际操作处理所在。

JVM是JRE的一部分。它是一个虚构出来的计算机，是通过在实际的计算机上仿真模拟各种计算机功能来实现的。

JVM有自己完善的硬件架构，如处理器、堆栈、寄存器等，还具有相应的指令系统。Java语言最重要的特点就是跨平台运行。

使用JVM就是为了支持与操作系统无关，实现跨平台。所以，JAVA虚拟机JVM是属于JRE的，而现在我们安装JDK时也附带安装了JRE(当然也可以单独安装JRE)。

# 2. 堆和栈的概念，它们有什么区别和联系？
在说堆和栈之前，我们先说一下JVM（虚拟机）内存的划分：

Java程序在运行时都要开辟空间，任何软件在运行时都要在内存中开辟空间，Java虚拟机运行时也是要开辟空间的。JVM运行时在内存中开辟一片内存区域，启动时在自己的内存区域中进行更细致的划分，因为虚拟机中每一片内存处理的方式都不同，所以要单独进行管理。

JVM内存的划分有五片：

1）寄存器；

2）本地方法区；

3）方法区；

4）栈内存；

5）堆内存。

**重点来说一下堆和栈：**

栈内存：栈内存首先是一片内存区域，存储的都是局部变量，凡是定义在方法中的都是局部变量（方法外的是全局变量），for循环内部定义的也是局部变量，是先加载函数才能进行局部变量的定义，所以方法先进栈，然后再定义变量，变量有自己的作用域，一旦离开作用域，变量就会被释放。栈内存的更新速度很快，因为局部变量的生命周期都很短。

堆内存：存储的是数组和对象（其实数组就是对象），凡是new建立的都是在堆中，堆中存放的都是实体（对象），实体用于封装数据，而且是封装多个（实体的多个属性），如果一个数据消失，这个实体也没有消失，还可以用，所以堆是不会随时释放的，但是栈不一样，栈里存放的都是单个变量，变量被释放了，那就没有了。堆里的实体虽然不会被释放，但是会被当成垃圾，Java有垃圾回收机制不定时的收取。

比如主函数里的语句 int [] arr=new int [3];在内存中是怎么被定义的：

主函数先进栈，在栈中定义一个变量arr,接下来为arr赋值，但是右边不是一个具体值，是一个实体。实体创建在堆里，在堆里首先通过new关键字开辟一个空间，内存在存储数据的时候都是通过地址来体现的，地址是一块连续的二进制，然后给这个实体分配一个内存地址。数组都是有一个索引，数组这个实体在堆内存中产生之后每一个空间都会进行默认的初始化（这是堆内存的特点，未初始化的数据是不能用的，但在堆里是可以用的，因为初始化过了，但是在栈里没有），不同的类型初始化的值不一样。所以堆和栈里就创建了变量和实体：

**那么堆和栈是怎么联系起来的呢？**

>刚刚说过给堆分配了一个地址，把堆的地址赋给arr，arr就通过地址指向了数组。所以arr想操纵数组时，就通过地址，而不是直接把实体都赋给它。这种我们不再叫他基本数据类型，而叫引用数据类型。称为arr引用了堆内存当中的实体。可以理解为c或“c++”的指针，Java成长自“c++”和“c++”很像，优化了“c++”

如果当int [] arr=null;

arr不做任何指向，null的作用就是取消引用数据类型的指向。

当一个实体，没有引用数据类型指向的时候，它在堆内存中不会被释放，而被当做一个垃圾，在不定时的时间内自动回收，因为Java有一个自动回收机制，（而“c++”没有，需要程序员手动回收，如果不回收就越堆越多，直到撑满内存溢出，所以Java在内存管理上优于“c++”）。自动回收机制（程序）自动监测堆里是否有垃圾，如果有，就会自动的做垃圾回收的动作，但是什么时候收不一定。

**所以堆与栈的区别很明显：**

1）栈内存存储的是局部变量而堆内存存储的是实体；

2）栈内存的更新速度要快于堆内存，因为局部变量的生命周期很短；

3）栈内存存放的变量生命周期一旦结束就会被释放，而堆内存存放的实体会被垃圾回收机制不定时的回收。

# 3. Class.forName 和 ClassLoader 有什么区别？
在java中对类进行加载可以使用Class.forName()和ClassLoader。 ClassLoader遵循双亲委派模型，最终调用启动类加载器的类加载器，实现的功能是“通过一个类的全限定名来获取描述此类的二进制字节流”，获取到二进制流后放到JVM中。

Class.forName()方法实际上也是调用的ClassLoader来实现的。

通过分析源码可以得出：最后调用的方法是forName()方法，方法中的第2个参数默认设置为true，该参数表示是否对加载的类进行初始化，设置为true时会对类进行初始化，这就意味着会执行类中的静态代码块以及对静态变量的赋值等操作。

也可以自行调用Class.forName(String name, boolean initialize,ClassLoader loader)方法手动选择在加载类的时候是否要对类进行初始化。

JDK源码中对参数initialize的描述是：if {@code true} the class will be initialized，大概意思是说：当值为true，则加载的类将会被初始化。

# 4. 常用的垃圾收集器有哪些？
**Serial 收集器（新生代）**

Serial即串行，以串行的方式执行，是单线程的收集器。它只会使用一个线程进行垃圾收集工作，GC线程工作时，其它所有线程都将停止工作。

作为新生代垃圾收集器的方式：-XX:+UseSerialGC

**ParNew 收集器（新生代）**

Serial收集器的多线程版本，需注意的是ParNew在单核环境下性能比Serial差，在多核条件下有优势。

作为新生代垃圾收集器的方式：-XX:+UseParNewGC

**Parallel Scavenge 收集器（新生代）**

多线程的收集器，目标是提高吞吐量（吞吐量 = 运行用户程序的时间 / （运行用户程序的时间 + 垃圾收集的时间））。

作为新生代垃圾收集器的方式：-XX:+UseParallelGC

**Serial Old 收集器（老年代）**

Serial收集器的老年代版本。

作为老年代垃圾收集器的方式：-XX:+UseSerialOldGC

**Parallel Old 收集器（老年代）**

Parallel Scavenge收集器的老年代版本。

作为老年代垃圾收集器的方式：-XX:+UseParallelOldGC

**CMS 收集器（老年代）**

CMS（Concurrent Mark Sweep），收集器几乎占据着JVM老年代收集器的半壁江山，它划时代的意义就在于垃圾回收线程几乎能做到与用户线程同时工作。

作为老年代垃圾收集器的方式：-XX:+UseConcMarkSweepGC

**G1 收集器（新生代 + 老年代）**

面向服务端应用的垃圾收集器，在多CPU和大内存的场景下有很好的性能。HotSpot开发团队赋予它的使命是未来可以替换掉CMS收集器。

作为老年代垃圾收集器的方式：-XX:+UseG1GC

# 5. 生产环境中应用的 JVM 参数有哪些？
```shell
-server
-Xms6000M
-Xmx6000M
-Xmn500M
-XX:PermSize=500M
-XX:MaxPermSize=500M
-XX:SurvivorRatio=65536
-XX:MaxTenuringThreshold=0
-Xnoclassgc
-XX:+DisableExplicitGC
-XX:+UseParNewGC
-XX:+UseConcMarkSweepGC
-XX:+UseCMSCompactAtFullCollection
-XX:CMSFullGCsBeforeCompaction=0
-XX:+CMSClassUnloadingEnabled
-XX:-CMSParallelRemarkEnabled
-XX:CMSInitiatingOccupancyFraction=90
-XX:SoftRefLRUPolicyMSPerMB=0
-XX:+PrintClassHistogram
-XX:+PrintGCDetails
-XX:+PrintGCTimeStamps
-XX:+PrintHeapAtGC
-Xloggc:log/gc.log
```

# 6. 什么情况下会发生栈内存溢出？
栈溢出(StackOverflowError)

栈是线程私有的，他的生命周期与线程相同，每个方法在执行时会创建一个栈帧，用来存储局部变量表，操作数栈，动态链接，方法出口灯信息。局部变量表又包含基本数据类型，对象引用类型。

若线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常。

若虚拟机在动态扩展栈时无法申请到足够的内存空间，则抛出OutOfMemoryError异常。

# 7. 常用的 JVM 调优配置参数有哪些？
假设参数为-Xms20m -Xmx20m -Xss256k

XX比X的稳定性更差，并且版本更新不会进行通知和说明。

**1、-Xms**

s为strating，表示堆内存起始大小

**2、-Xmx**

x为max，表示最大的堆内存，一般来说-Xms和-Xmx的设置为相同大小，当heap自动扩容时，会发生内存抖动，影响程序的稳定性

**3、-Xmn**

n为new，表示新生代大小，-Xss规定了每个线程虚拟机栈（堆栈）的大小。

**4、-XX:SurvivorRator=8**

SurvivorRatio定义新生代中Eden区域和Survivor区域（From幸存区或To幸存区）的比例，默认为8，也就是说Eden占新生代的8/10，From幸存区和To幸存区各占新生代的1/10。

**5、-XX:PretenureSizeThreshold=3145728**

表示当创建（new）的对象大于3M的时候直接进入老年代

**6、-XX:MaxTenuringThreshold=15**

表示当对象的存活的年龄（minor gc一次加1）大于多少时，进入老年代

**7、-XX:-DisableExplicirGC**

表示是否（+表示是，-表示否）打开GC日志

# 8. 什么是类加载器？
类加载器是指把类文件加载到虚拟机中，通过一个类的全限定名（包名+类型）来获取描述该类的二进制字节流。

类加载器是Java语言的一项创新，最开始是为了满足Java Applet的需求而设计的。

类加载器目前在层次划分、程序热部署和代码加密等领域被广泛使用。

# 9. 类加载器分为哪几类？
JVM默认提供了系统类加载器（JDK1.8），包括如下：

Bootstrap ClassLoader（系统类加载器）

Application ClassLoader（应用程序类加载器）

Extension ClassLoader（扩展类加载器）

Customer ClassLoader（自定义加载器）

# 10. 可以自定义一个 java.lang.String 吗？
答案是可以的，但是不能被加载使用。

这个主要是因为加载器的委托机制，在类加载器的结构图中，BootStrap是顶层父类，ExtClassLoader是BootStrap类的子类，ExtClassLoader是AppClassLoader的父类。当使用java.lang.String类时，Java虚拟机会将java.lang.String类的字节码加载到内存中。

加载某个类时，优先使用父类加载器加载需要使用的类。加载自定义java.lang.String类，其使用的加载器是AppClassLoader，根据优先使用父类加载器原理，AppClassLoader加载器的父类为ExtClassLoader，这时加载String使用的类加载器是ExtClassLoader，但类加载器ExtClassLoader在jre/lib/ext目录下并不能找到自定义java.lang.String类。

然后使用ExtClassLoader父类的加载器BootStrap，父类加载器BootStrap在JRE/lib目录的rt.jar找到了String.class，将其加载到内存中。

# 11. Java 中如何判断 JVM 是 32 位 或 64 位？
使用sun.arch.data.model或os.arch来获取某系统的属性信息，来判断JVM是32位还是64位。

# 12. Java 中能否保证 GC 执行吗？
Java中不能保证GC的执行。

虽然可以通过程序调用System.gc()或者Runtime.gc()方法，但是没有办法保证GC的执行。

# 13. Java 中什么是分区收集算法？
分区收集算法就是将整个堆空间划分为连续的不同的小区间region。

每个小区间独立使用，独立回收。这种算法的优势是可以控制一次回收多少个小区间。

根据目标停顿时间，每次合理地回收若干个小区间（而不是整个堆），从而减少一次GC所产生的停顿。

分代收集法是目前大部分JVM所采用的方法，其核心思想是根据对象存活的不同生命周期将内存划分为不同的域，一般情况下将GC堆划分为老生代(Tenured/Old Generation)和新生代(YoungGeneration)。

老生代的特点是每次垃圾回收时只有少量对象需要被回收，新生代的特点是每次垃圾回收时都有大量垃圾需要被回收，因此可以根据不同区域选择不同的算法。

# 14. Java 中什么是强引用？
强引用是指把一个对象赋值给一个引用变量，这个引用变量就是一个强引用，Java中最常见的就是强引用。

当一个对象被强引用变量引用时，那么是不可能被垃圾回收机制回收的，因此强引用是造成Java内存泄漏的主要原因之一。

代码如下：

```java
Object obj = new Object();
```

当内存空间不足时，Java虚拟机宁可抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足的问题。

当强引用对象不使用时，需弱化从而使垃圾回收机制能够回收，如下：

```java
obj = null;
```

当obj设置为null或超出对象的生命周期范围，此时GC会认为这个对象不存在引用，就可以回收这个对象。

# 15. Java 中什么是软引用？
软引用可用来实现内存敏感的高速缓存。

软引用需要使用SoftReference类来实现，对于只有软引用的对象来说，当系统内存足够时不会被垃圾回收器回收，但是系统内存空间不足时会被垃圾回收器回收。

软引用通常用来实现内存敏感的高速缓存。

```java
String str = new String("abc");
SoftReference<String> softReference = new SoftReference<String>(str);
```

注意的是软引用对象是在JVM内存不不足时才会被回收，而调用System.gc()方法只是起到通知作用，具体JVM何时扫描回收对象是JVM状态决定的。

# 16. Java 中什么是弱引用？
弱引用需要使用WeakReference类来实现，相对比软引用的生命周期更短。

对于只有弱引用的对象来说，垃圾回收机制一运行，垃圾回收器线程扫描它所管辖的内存区域过程中，不管当前内存空间是否足够，都会回收它的内存。但是由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。

```java
String str = new String("abc");
WeakReference<String> weakReference = new WeakReference<>(str);
str = null;
```

注意的是如果一个对象是很少使用且希望在使用时随时就能获取到该对象，但又不想影响此对象的垃圾收集，那么建议使用Weak Reference来记住此对象。

# 17. Java 中什么是虚引用？
虚引用与其他几种引用都不同，虚引用不会决定对象的生命周期。如果一个对象仅有虚引用，那么就和没有任何引用一样，在任何时候都可能被垃圾回收器回收。

虚引用需要使用PhantomReference类来实现，不能单独使用必须和引用队列联合使用。虚引用的应用场景是跟踪对象被垃圾回收的状态。

```java
String str = new String("abc");
ReferenceQueue queue = new ReferenceQueue();// 虚引用必须与一个引用队列关联
PhantomReference pr = new PhantomReference(str, queue);
```

# 18. Java 中引用类型有什么区别？
Java中有4种引用类型。

引用类型的级别和强度由高到低依次为：强引用->软引用->弱引用->虚引用。

| 引用类型	| 回收时间	| 使用说明	| 终止时间|
| ------------- | ------------- | ------------- | ------------- |
| 强引用	| 从来不会	| 对象的一般状态	| JVM停止运行时终止|
| 软引用	| 当内存不足时	| 对象缓存	| 内存不足时终止|
| 弱引用	| 正常垃圾回收时	| 对象缓存	| 垃圾回收后终止|
| 虚引用	| 正常垃圾回收时	| 跟踪对象的垃圾回收状态	| 垃圾回收后终止|
# 19. 什么是双亲委派模型？
双亲委派模型要求除了顶层的启动类加载器外，其他的类加载器都应当有自己的父类加载器。

这里类加载器之间的父子关系一般不会以继承关系来实现，而是都使用组合关系来复用父加载器的代码。

**工作过程**

如果一个类加载器收到了类加载的请求时，它首先不会自己去尝试加载这个类，而是把这个请求委派给自己的父类加载器去加载，每一个层次的类加载器都是如此。

因此所有的加载请求最终都应该传递到顶层的启动类加载器中，只有当父类加载器反馈自己无法完成这个请求（它的搜索范围中没有找到所需的类）时，子加载器才会尝试自己去加载。

**好处**

Java类随着它的类加载器一起具备了一种带有优先级的层次关系。例如类Object，它放在rt.jar中，无论哪一个类加载器要加载这个类，最终都是委派给启动类加载器进行加载。

因此Object类在程序的各种类加载器环境中都是同一个类，判断两个类是否相同是通过classloader.class这种方式进行的，所以哪怕是同一个class文件如果被两个classloader加载，那么他们也是不同的类。

# 20. Java 中类加载器都有哪些？
类加载器按照层次，从顶层到底层，分为以下三种：

1）启动类加载器（Bootstrap ClassLoader）

负责将存放在JAVA_HOME/lib下的或被-Xbootclasspath参数所指定路径中的，并且是虚拟机识别的类库加载到虚拟机内存中。

注意的是启动类加载器无法被Java程序直接引用。

2）扩展类加载器（Extension ClassLoader）

负责加载JAVA_HOME/lib/ext目录中的或者被java.ext.dirs系统变量所指定的路径中的所有类库。

注意的是开发者可以直接使用扩展类加载器。

3）应用程序类加载器（Application ClassLoader）

ClassLoader中getSystemClassLoader()方法的返回值，所以一般也称它为系统类加载器。

负责加载用户类路径Classpath上所指定的类库，可直接使用这个加载器，如果应用程序没有自定义自己的类加载器，一般情况下该加载器就是程序中默认的类加载器。
