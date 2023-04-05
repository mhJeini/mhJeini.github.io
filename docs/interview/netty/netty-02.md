# 1. Reactor 模型中有哪几个关键组件？
1、Reactor Reactor在一个单独的线程中运行，负责监听和分发事件，分发给适当的处理程序来对IO事件做出反应。它就像公司的电话接线员，它接听来自客户的电话并将线路转移到适当的联系人

2、Handlers处理程序执行I/O事件要完成的实际事件，类似于客户想要与之交谈的公司中的实际官员。Reactor通过调度适当的处理程序来响应I/O事件，处理程序执行非阻塞操作。

# 2. Reactor 线程模型消息处理流程？
1、Reactor线程通过多路复用器监控IO事件。

2、如果是连接建立的事件，则由acceptor线程来接受连接，并创建handler来处理之后该连接上的读写事件。

3、如果是读写事件，则Reactor会调用该连接上的handler进行业务处理。

# 3. Reactor 线程模型有几种模式？
单Reactor单线程模式

仅由一个线程来进行事件监控和事件处理，即整个消息处理流程都在一个线程中完成。

单Reactor多线程模式

对于连接上的读写事件，会使用线程池中的线程来执行该连接上的handler操作，即对读写事件的处理不会阻塞Reactor线程。

主从Reactor多线程模式

在单Reactor多线程模式的基础上，使用两个Reactor线程分别对建立连接事件和读写事件进行监听，每个Reactor线程拥有一个多路复用器。当主Reactor线程监听到连接建立事件后，创建SocketChannel，然后将SocketChannel注册到子Reactor线程的多路复用器中，使子Reactor线程监听连接的读写事件。

# 4. Netty 中如何解决 TCP 粘包和拆包问题？
Netty提供了3种类型的解码器对TCP 粘包/拆包问题进行处理：

定长消息解码器： FixedLengthFrameDecoder。发送方和接收方规定一个固定的消息长度，不够用空格等字符补全，这样接收方每次从接受到的字节流中读取固定长度的字节即可，长度不够就保留本次接受的数据，再在下一个字节流中获取剩下数量的字节数据。

分隔符解码器： LineBasedFrameDecoder或DelimiterBasedFrameDecoder。LineBasedFrameDecoder是行分隔符解码器，分隔符为\n或\r\n；DelimiterBasedFrameDecoder是自定义分隔符解码器，可以定义一个或多个分隔符。接收端在收到的字节流中查找分隔符，然后返回分隔符之前的数据，没找到就继续从下一个字节流中查找。

数据长度解码器： LengthFieldBasedFrameDecoder。将发送的消息分为header和body，header存储消息的长度（字节数），body是发送的消息的内容。同时发送方和接收方要协商好这个header的字节数，因为int能表示长度，long也能表示长度。接收方首先从字节流中读取前n（header的字节数）个字节（header），然后根据长度读取等量的字节，不够就从下一个数据流中查找。

# 5. Java 中 BIO、NIO、AIO 有什么区别？
BIO

Block IO是指同步阻塞式IO，就是平常使用的传统IO，它的特点是模式简单使用方便，并发处理能力低。

服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。

BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。

NIO Non IO是指同步非阻塞IO，是传统IO的升级，客户端和服务器端通过Channel（通道）通讯，实现了多路复用。

同步非阻塞，服务器实现模式为一个请求一个线程，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。

NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。

AIO

Asynchronous IO是指NIO的升级，也叫NIO2，实现了异步非堵塞IO，异步IO的操作基于事件和回调机制。

服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理。

AIO方式适用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。

# 6. EventloopGroup 和 EventLoop 有什么联系？
EventLoop可以理解为线程，那么Group就是线程池或者线程组，Netty的线程模型中，每一个channel需要绑定到一个固定的EventLoop进行后续的处理，Netty就是根据某一个算法从线程组中选择一个线程进行绑定的。

# 7. Netty 核⼼组件有哪些？分别有什么作⽤？
Channel：Netty对网络操作的抽象，包括了一些常见的网络IO操作，比如read,write等等，最常见的实现类：NioServerSocketChannel和NioSocketChannel，对应Bio中的ServerSocketChannel和SocketChannel。

EventLoop：负责监听网络事件并调用事件处理器进行相关的处理。

ChannelFuture：Netty是异步的，所有操作都可以通过ChannelFuture来实现绑定一个监听器然后执行结果成功与否的业务逻辑，或者把通过调用sync方法，把异步方法变成同步的。

ChannelHandler和ChannelPipeline：Netty底层的处理器是一个处理器链，链条上的每一个处理器都可以对消息进行处理，选择继续传递或者到此为止，一般我们业务会实现自己的解码器/心跳处理器和实际的业务处理Handler，然后按照顺序绑定到链条上。

# 8. Bootstrap 和 ServerBootstrap 了解过吗？
BootStarp和ServerBootstrap都是一个启动引导类，相当于如果要使用Netty需要把相关信息配置到启动引导类中，这样Netty才能正确工作。

Bootstrap是客户端引导类，核心方法是connect，传入一个EventLoopGroup就可以

ServerBootstrap是服务断引导类，核心方法是bind，需要传入两个EventLoopGroup，一个负责接受连接，一个负责处理具体的连接上的网络IO任务。

# 9. NioEventLoopGroup 默认构造方法启动几个线程？
NioEventLoopGroup默认的构造方法启动线程数为CPU核⼼数*2。

# 10. 什么是长连接？
连接是指TCP协议中如果两端想要传递数据，首先需要通过三次握手建立连接，握手完毕，连接就建立完毕，但是这个过程是比较消耗网络资源的。

短连接是指一轮数据传输完毕后，就断开连接，实现和管理都很方便，但是频繁的建立断开连接比较消耗网络资源。

长连接是指数据传输完毕后，不断开连接，下次有数据发送需求的时候再使用这个连接，省去了握手的过程。

# 11. Netty 零拷贝提体现在哪里，与操作系统上的有什么区别？
Zero-copy就是在操作数据时, 不需要将数据buffer从一个内存区域拷贝到另一个内存区域。 少了一次内存的拷贝，CPU的效率就得到的提升。在OS层面上的Zero-copy 通常指避免在 用户态(User-space) 与 内核态(Kernel-space)之间来回拷贝数据。Netty的Zero-copy完全是在用户态(Java 层面)的, 更多的偏向于优化数据操作。

Netty的接收和发送ByteBuffer采用DIRECT BUFFERS，使用堆外直接内存进行Socket读写，不需要进行字节缓冲区的二次拷贝。如果使用传统的堆内存（HEAP BUFFERS）进行Socket读写，JVM会将堆内存Buffer拷贝一份到直接内存中，然后才写入Socket中。相比于堆外直接内存，消息在发送过程中多了一次缓冲区的内存拷贝。

Netty提供了组合Buffer对象，可以聚合多个ByteBuffer对象，用户可以像操作一个Buffer那样方便的对组合Buffer进行操作，避免了传统通过内存拷贝的方式将几个小Buffer合并成一个大的Buffer。

Netty的文件传输采用了transferTo方法，它可以直接将文件缓冲区的数据发送到目标Channel，避免了传统通过循环write方式导致的内存拷贝问题。

# 12. 简单描述一下 Netty 的执行流程？
1、创建ServerBootStrap实例；

2、设置并绑定Reactor线程池：EventLoopGroup，EventLoop就是处理所有注册到本线程的Selector上面的Channel；

3、设置并绑定服务端的channel；

4、创建处理网络事件的ChannelPipeline和handler，网络时间以流的形式在其中流转，handler完成多数的功能定制：比如编解码 SSl安全认证；

5、绑定并启动监听端口；

6、当轮训到准备就绪的channel后，由Reactor线程：NioEventLoop执行pipline中的方法，最终调度并执行channelHandler。
