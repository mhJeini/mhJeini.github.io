# 1. 什么是 Netty？
Netty是一款基于NIO（Nonblocking I/O，非阻塞IO）开发的网络通信框架，对比于BIO（Blocking I/O，阻塞IO），它的并发性能得到了很大提高。难能可贵的是，在保证快速和易用性的同时，并没有丧失可维护性和性能等优势。使用它可以快速简单地开发网络应用程序。

Netty极大地简化并优化了TCP和UDP套接字服务器等网络编程,并且性能以及安全性等很多方面甚至都要更好。

支持多种协议如FTP、SMTP、HTTP以及各种二进制和基于文本的传统协议。

官方话术就是Netty成功地找到了一种在不妥协可维护性和性能的情况下实现易于开发，性能，稳定性和灵活性的方法。

# 2. Netty 都有哪些特点？
1、Netty是一款基于NIO（Nonblocking IO，非阻塞IO）开发的网络通信框架，对比于BIO（Blocking I/O，阻塞IO），他的并发性能得到了很大提高。

2、Netty的传输依赖于零拷贝特性，尽量减少不必要的内存拷贝，实现了更高效率的传输。

3、Netty封装了NIO操作的很多细节，统一的API，支持多种传输类型，阻塞和非阻塞的，提供了易于使用调用接口。

4、Netty简单而强大的线程模型。

5、Netty自带编解码器解决TCP粘包/拆包问题。

6、Netty自带各种协议栈。

7、Netty真正的无连接数据包套接字支持。

7、Netty比直接使用Java核心API有更高的吞吐量、更低的延迟、更低的资源消耗和更少的内存复制。

8、Netty安全性不错，有完整的SSL/TLS 以及 StartTLS支持。

9、Netty是活跃的开源项目，版本迭代周期短，bug 修复速度快。。

10、Netty成熟稳定，经历了大型项目的使用和考验，而且很多开源项目都使用到了Netty， 比如经常接触的Dubbo、RocketMQ等等。

# 3. Netty 有哪些应用场景？
作为RPC框架的网络通信工具： 分布式系统中，不同服务节点之间经常需要相互调用，这个时候就需要RPC框架。

不同服务节点之间的通信是如何做的呢？可以使用Netty来做。比如调用另外一个节点的方法的话，至少是要让对方知道调用的是哪个类中的哪个方法以及相关参数。

实现一个自己的HTTP服务器： 通过Netty可以实现一个简单的HTTP服务器。说到HTTP服务器的话，作为Java后端开发，一般使用Tomcat比较多。一个最基本的HTTP服务器可要以处理常见的HTTPMethod的请求，比如POST请求、GET请求等。

实现一个即时通讯系统： 使用Netty可以实现一个可以聊天类似微信的即时通讯系统，这方面的开源项目还是比较多的，可以自行去Github找一找。或者关注微信公众号Java精选，据说经常更新一些不错的框架。

实现消息推送系统： 市面上有很多消息推送系统都是基于Netty来实现的。

典型的应用还有：阿里分布式服务框架Dubbo，默认使用Netty作为基础通信组件，还有RocketMQ也是使用Netty作为通讯的基础。

# 4. 什么是 Netty 的零拷贝？
Netty的零拷贝主要包含三个方面：

Netty的接收和发送ByteBuffer采用DIRECT BUFFERS，使用堆外直接内存进行Socket 读写，不需要进行字节缓冲区的二次拷贝。如果使用传统的堆内存（HEAP BUFFERS）进行Socket读写，JVM会将堆内存Buffer拷贝一份到直接内存中，然后才写入Socket 中。相比于堆外直接内存，消息在发送过程中多了一次缓冲区的内存拷贝。

Netty提供了组合Buffer对象，可以聚合多个 ByteBuffer对象，用户可以像操作一个Buffer 那样方便的对组合 Buffer 进行操作，避免了传统通过内存拷贝的方式将几个小Buffer合并成一个大的Buffer。

Netty 的文件传输采用了transferTo方法，它可以直接将文件缓冲区的数据发送到目标 Channel，避免了传统通过循环write方式导致的内存拷贝问题。

# 5. Netty 有哪些优势？
使用简单：封装了NIO的很多细节，使用更简单。

功能强大：预置了多种编解码功能，支持多种主流协议。

定制能力强：可以通过ChannelHandler对通信框架进行灵活地扩展。

性能高：通过与其他业界主流的NIO框架对比，Netty的综合性能最优。

稳定：Netty修复了已经发现的所有NIO的bug，让开发人员可以专注于业务本身。

社区活跃：Netty是活跃的开源项目，版本迭代周期短，bug修复速度快。

# 6. Netty 高性能表现在哪些方面？
传输： IO模型在很大程度上决定了框架的性能，相比于bio，netty建议采用异步通信模式，因为nio一个线程可以并发处理N个客户端连接和读写操作，这从根本上解决了传统同步阻塞IO一连接一线程模型，架构的性能、弹性伸缩能力和可靠性都得到了极大的提升。正如代码中所示，使用的是NioEventLoopGroup和NioSocketChannel来提升传输效率。

协议： Netty默认提供了对Google Protobuf的支持，也可以通过扩展Netty的编解码接口，用户可以实现其它的高性能序列化框架。

线程： netty使用了Reactor线程模型，但Reactor模型不同，对性能的影响也非常大，下面介绍常用的Reactor线程模型有三种，分别如下：

1、Reactor单线程模型：单线程模型的线程即作为NIO服务端接收客户端的TCP连接，又作为NIO客户端向服务端发起TCP连接，即读取通信对端的请求或者应答消息，又向通信对端发送消息请求或者应答消息。理论上一个线程可以独立处理所有IO相关的操作，但一个NIO线程同时处理成百上千的链路，性能上无法支撑，即便NIO线程的CPU负荷达到100%，也无法满足海量消息的编码、解码、读取和发送，又因为当NIO线程负载过重之后，处理速度将变慢，这会导致大量客户端连接超时，超时之后往往会进行重发，这更加重了NIO线程的负载，最终会导致大量消息积压和处理超时，NIO线程会成为系统的性能瓶颈。

2、Reactor多线程模型：有专门一个NIO线程用于监听服务端，接收客户端的TCP连接请求；网络IO操作(读写)由一个NIO线程池负责，线程池可以采用标准的JDK线程池实现。但百万客户端并发连接时，一个nio线程用来监听和接受明显不够，因此有了主从多线程模型。

3、主从Reactor多线程模型：利用主从NIO线程模型，可以解决1个服务端监听线程无法有效处理所有客户端连接的性能不足问题，即把监听服务端，接收客户端的TCP连接请求分给一个线程池。因此，在代码中可以看到，我们在server端选择的就是这种方式，并且也推荐使用该线程模型。在启动类中创建不同的EventLoopGroup实例并通过适当的参数配置，就可以支持上述三种Reactor线程模型。

# 7. Netty 和 Tomcat 有什么区别？
作用不同：Tomcat是Servlet容器，可以视为Web服务器，而Netty是异步事件驱动的网络应用程序框架和工具用于简化网络编程，例如TCP和UDP套接字服务器。

协议不同：Tomcat是基于http协议的Web服务器，而Netty能通过编程自定义各种协议，因为Netty本身自己能编码/解码字节流，所有Netty可以实现，HTTP服务器、FTP服务器、UDP服务器、RPC服务器、WebSocket服务器、Redis的Proxy服务器、MySQL的Proxy服务器等等。

# 8. Netty 中有那些重要组件？
Channel：Netty网络操作抽象类，它除了包括基本的I/O操作，如bind、connect、read、write等。

EventLoop：主要是配合Channel处理I/O操作，用来处理连接的生命周期中所发生的事情。

ChannelFuture：Netty框架中所有的I/O操作都为异步的，因此我们需要ChannelFuture的addListener()注册一个ChannelFutureListener监听事件，当操作执行成功或者失败时，监听就会自动触发返回结果。

ChannelHandler：充当了所有处理入站和出站数据的逻辑容器。ChannelHandler主要用来处理各种事件，这里的事件很广泛，比如可以是连接、数据接收、异常、数据转换等。

ChannelPipeline：为ChannelHandler链提供了容器，当channel创建时，就会被自动分配到它专属的ChannelPipeline，这个关联是永久性的。

# 9. Netty 发送消息有几种方式？
Netty 有两种发送消息的方式：

一种是直接写入Channel中，消息从ChannelPipeline当中尾部开始移动；

另一种是写入和ChannelHandler绑定的ChannelHandlerContext中，消息从ChannelPipeline中的下一个ChannelHandler中移动。

# 10. 默认情况 Netty 起多少线程？何时启动？
Netty默认是CPU处理器数的两倍，bind完之后启动。

# 11. Netty 和 Java NIO 有什么区别，为什么不直接使用 JDK NIO 类库？
1、NIO的类库和API繁杂，使用麻烦，你需要熟练掌握Selector,ServerSocketChannel、SocketChannel、ByteBuffer等。

2、需要具备其他的额外技能做铺垫，例如熟悉Java多线程编程。这是因为NIO编程涉及到Reactor模式，你必须对多线程和网络编程非常熟悉，才能写出高质量的NIO程序。

3、可靠性能力补齐，工作量和难度非常大。例如客户端面临断连重连、网络闪断、半包读写、失败缓存、网络拥塞和异常码流的处理等问题，NIO编程的特点是功能开发相对容易，但是可靠性能力补齐的工作量和难度都非常大。

4、JDK NIO的BUG，例如epoll bug，它会导致Selector空轮询，最终导致CPU 100%。官方验证例子基于以上原因，在大多数场景下，不建议直接使用JDK的NIO类库，除非你精通NIO编程或者有特殊的需求。在绝大多数的业务场景中，我们可以使用NIO框架Netty来进行NIO编程，它既可以作为客户端也可以作为服务端，同时支持UDP和异步文件传输，功能非常强大。

# 12. Netty 支持哪些心跳类型设置？
readerIdleTime：为读超时时间（即测试端一定时间内未接受到被测试端消息）。

writerIdleTime：为写超时时间（即测试端一定时间内向被测试端发送消息）。

allIdleTime：所有类型的超时时间。

# 13. Netty 粘包和拆包是如何处理的，有哪些实现？
消息定长，报文大小固定长度，不够空格补全，发送和接收方遵循相同的约定，这样即使粘包了通过接收方编程实现获取定长报文也能区分。

包尾添加特殊分隔符，例如每条报文结束都添加回车换行符（例如FTP协议）或者指定特殊字符作为报文分隔符，接收方通过特殊分隔符切分报文区分。

将消息分为消息头和消息体，消息头中包含表示信息的总长度（或者消息体长度）的字段。

# 14. 同步和异步有什么区别？
同步：发出一个功能调用时，在没有得到结果之前，该调用就不返回。也就是必须一件一件事做,等前一件做完了才能做下一件事。例如普通B/S模式（同步）：提交请求->等待服务器处理->处理完毕返回 这个期间客户端浏览器不能干任何事。

异步：当一个异步过程调用发出后，调用者不能立刻得到结果。实际处理这个调用的部件在完成后，通过状态、通知和回调来通知调用者。例如 ajax请求（异步）: 请求通过事件触发->服务器处理（这是浏览器仍然可以作其他事情）->处理完毕。

# 15. 阻塞和非阻塞有什么区别？
阻塞：阻塞调用是指调用结果返回之前，当前线程会被挂起（线程进入非可执行状态，在这个状态下，cpu不会给线程分配时间片，即线程暂停运行）。函数只有在得到结果之后才会返回。有人也许会把阻塞调用和同步调用等同起来，实际上他是不同的。对于同步调用来说，很多时候当前线程还是激活的，只是从逻辑上当前函数没有返回,它还会抢占cpu去执行其他逻辑，也会主动检测io是否准备好。

非阻塞：指在不能立刻得到结果之前，该函数不会阻塞当前线程，而会立刻返回。

# 16. Netty 中有哪些线程模型？
Netty通过Reactor模型基于多路复用器接收并处理用户请求，内部实现了两个线程池，boss线程池和work线程池，其中boss线程池的线程负责处理请求的accept事件，当接收到accept事件的请求时，把对应的socket封装到一个NioSocketChannel中，并交给work线程池，其中work线程池负责请求的read和write事件，由对应的Handler处理。

单线程模型： 所有I/O操作都由一个线程完成，即多路复用、事件分发和处理都是在一个Reactor线程上完成的。既要接收客户端的连接请求,向服务端发起连接，又要发送/读取请求或应答/响应消息。一个NIO线程同时处理成百上千的链路，性能上无法支撑，速度慢，若线程进入死循环，整个程序不可用，对于高负载、大并发的应用场景不合适。

多线程模型： 有一个NIO线程（Acceptor）只负责监听服务端，接收客户端的TCP连接请求；NIO线程池负责网络IO的操作，即消息的读取、解码、编码和发送；1个NIO线程可以同时处理N条链路，但是1个链路只对应1个NIO线程，这是为了防止发生并发操作问题。但在并发百万客户端连接或需要安全认证时，一个Acceptor线程可能会存在性能不足问题。

主从多线程模型： Acceptor线程用于绑定监听端口，接收客户端连接，将SocketChannel从主线程池的Reactor线程的多路复用器上移除，重新注册到Sub线程池的线程上，用于处理I/O的读写等操作，从而保证mainReactor只负责接入认证、握手等操作。

# 17. Java NIO 包括哪些组成部分？
Buffer：与Channel进行交互，数据是从Channel读入缓冲区，从缓冲区写入Channel中的。

flip方法 ：反转此缓冲区，将position给limit，然后将position置为0，其实就是切换读写模式。

clear方法 ：清除此缓冲区，将position置为0，把capacity的值给limit。

rewind方法 ：重绕此缓冲区，将position置为0。

DirectByteBuffer可减少一次系统空间到用户空间的拷贝。但Buffer创建和销毁的成本更高，不可控，通常会用内存池来提高性能。直接缓冲区主要分配给那些易受基础系统的本机I/O操作影响的大型、持久的缓冲区。如果数据量比较小的中小应用情况下，可以考虑使用heapBuffer，由JVM进行管理。

Channel：表示IO源与目标打开的连接，是双向的，但不能直接访问数据，只能与Buffer 进行交互。通过源码可知，FileChannel的read方法和write方法都导致数据复制了两次。

Selector可使一个单独的线程管理多个Channel，open方法可创建Selector，register方法向多路复用器器注册通道，可以监听的事件类型：读、写、连接、accept。注册事件后会产生一个SelectionKey：它表示SelectableChannel 和Selector 之间的注册关系，wakeup方法：使尚未返回的第一个选择操作立即返回，唤醒的。

原因是：注册了新的channel或者事件；channel关闭，取消注册；优先级更高的事件触发（如定时器事件），希望及时处理。

Selector在Linux的实现类是EPollSelectorImpl，委托给EPollArrayWrapper实现，其中三个native方法是对epoll的封装，而EPollSelectorImpl. implRegister方法，通过调用epoll_ctl向epoll实例中注册事件，还将注册的文件描述符(fd)与SelectionKey的对应关系添加到fdToKey中，这个map维护了文件描述符与SelectionKey的映射。

fdToKey有时会变得非常大，因为注册到Selector上的Channel非常多（百万连接）；过期或失效的Channel没有及时关闭。fdToKey总是串行读取的，而读取是在select方法中进行的，该方法是非线程安全的。

Pipe：两个线程之间的单向数据连接，数据会被写到sink通道，从source通道读取。

NIO的服务端建立过程：Selector.open()：打开一个Selector；ServerSocketChannel.open()：创建服务端的Channel；bind()：绑定到某个端口上。并配置非阻塞模式；register()：注册Channel和关注的事件到Selector上；select()轮询拿到已经就绪的事件。

# 18. 说一说 NIOEventLoopGroup 源码处理过程？
NioEventLoopGroup(其实是MultithreadEventExecutorGroup) 内部维护一个类型为 EventExecutor children [], 默认大小是处理器核数 * 2, 这样就构成了一个线程池，初始化EventExecutor时NioEventLoopGroup重载newChild方法，所以children元素的实际类型为NioEventLoop。

线程启动时调用SingleThreadEventExecutor的构造方法，执行NioEventLoop类的run方法，首先会调用hasTasks()方法判断当前taskQueue是否有元素。如果taskQueue中有元素，执行 selectNow() 方法，最终执行selector.selectNow()，该方法会立即返回。如果taskQueue没有元素，执行 select(oldWakenUp) 方法

select ( oldWakenUp) 方法解决了 Nio 中的 bug，selectCnt 用来记录selector.select方法的执行次数和标识是否执行过selector.selectNow()，若触发了epoll的空轮询bug，则会反复执行selector.select(timeoutMillis)，变量selectCnt 会逐渐变大，当selectCnt 达到阈值（默认512），则执行rebuildSelector方法，进行selector重建，解决cpu占用100%的bug。

rebuildSelector方法先通过openSelector方法创建一个新的selector。然后将old selector的selectionKey执行cancel。最后将old selector的channel重新注册到新的selector中。rebuild后，需要重新执行方法selectNow，检查是否有已ready的selectionKey。

接下来调用processSelectedKeys 方法（处理I/O任务），当selectedKeys != null时，调用processSelectedKeysOptimized方法，迭代 selectedKeys 获取就绪的 IO 事件的selectkey存放在数组selectedKeys中, 然后为每个事件都调用 processSelectedKey 来处理它，processSelectedKey 中分别处理OP_READ；OP_WRITE；OP_CONNECT事件。

最后调用runAllTasks方法（非IO任务），该方法首先会调用fetchFromScheduledTaskQueue方法，把scheduledTaskQueue中已经超过延迟执行时间的任务移到taskQueue中等待被执行，然后依次从taskQueue中取任务执行，每执行64个任务，进行耗时检查，如果已执行时间超过预先设定的执行时间，则停止执行非IO任务，避免非IO任务太多，影响IO任务的执行。

每个NioEventLoop对应一个线程和一个Selector，NioServerSocketChannel会主动注册到某一个NioEventLoop的Selector上，NioEventLoop负责事件轮询。

Outbound事件都是请求事件, 发起者是Channel，处理者是unsafe，通过Outbound事件进行通知，传播方向是tail到head。Inbound 事件发起者是unsafe，事件的处理者是 Channel, 是通知事件，传播方向是从头到尾。

内存管理机制，首先会预申请一大块内存Arena，Arena由许多Chunk组成，而每个Chunk默认由2048个page组成。Chunk通过AVL树的形式组织Page，每个叶子节点表示一个Page，而中间节点表示内存区域，节点自己记录它在整个Arena中的偏移地址。当区域被分配出去后，中间节点上的标记位会被标记，这样就表示这个中间节点以下的所有节点都已被分配了。大于8k的内存分配在poolChunkList中，而PoolSubpage用于分配小于8k的内存，它会把一个page分割成多段，进行内存分配。

ByteBuf的特点：支持自动扩容（4M），保证put方法不会抛出异常、通过内置的复合缓冲类型，实现零拷贝（zero-copy）；不需要调用flip()来切换读/写模式，读取和写入索引分开；方法链；引用计数基于AtomicIntegerFieldUpdater用于内存回收；PooledByteBuf采用二叉树来实现一个内存池，集中管理内存的分配和释放，不用每次使用都新建一个缓冲区对象。UnpooledHeapByteBuf每次都会新建一个缓冲区对象。

# 19. JDK 原生 NIO 程序有什么问题？
JDK原生也有一套网络应用程序API，但是存在一系列问题，主要如下：

1、NIO的类库和API繁杂，使用麻烦，你需要熟练掌握Selector、ServerSocketChannel、SocketChannel、ByteBuffer等。

2、需要具备其它的额外技能做铺垫，例如熟悉Java多线程编程，因为NIO编程涉及到Reactor模式，你必须对多线程和网路编程非常熟悉，才能编写出高质量的NIO程序

3、可靠性能力补齐，开发工作量和难度都非常大。例如客户端面临断连重连、网络闪断、半包读写、失败缓存、网络拥塞和异常码流的处理等等，NIO编程的特点是功能开发相对容易，但是可靠性能力补齐工作量和难度都非常大

4、JDK NIO的BUG，例如臭名昭著的epoll bug，它会导致Selector空轮询，最终导致CPU 100%。官方声称在JDK1.6版本的update18修复了该问题，但是直到JDK1.7版本该问题仍旧存在，只不过该bug发生概率降低了一些而已，它并没有被根本解决。

# 20. 什么是 Reactor 线程模型？
Reactor是反应堆的意思，Reactor模型，是指通过一个或多个输入同时传递给服务处理器的服务请求的事件驱动处理模式。

Reactor一种事件驱动处理模型，类似于多路复用IO模型，包括三种角色：Reactor、Acceptor和Handler。Reactor用来监听事件，包括：连接建立、读就绪、写就绪等。然后针对监听到的不同事件，将它们分发给对应的线程去处理。其中acceptor处理客户端建立的连接，handler对读写事件进行业务处理。

服务端程序处理传入多路请求，并将它们同步分派给请求对应的处理线程，Reactor模式也叫Dispatcher模式，即I/O多了复用统一监听事件，收到事件后分发（Dispatch给某进程），是编写高性能网络服务器的必备技术之一。
