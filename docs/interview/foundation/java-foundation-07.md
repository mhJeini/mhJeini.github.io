# 1. Naming 类 bind() 和rebind() 方法有什么区别？
bind()方法负责把指定名称绑定给远程对象。

rebind()方法负责把指定名称重新绑定到一个新的远程对象。如果该名称已经绑定过了，先前的绑定会被替换掉。

# 2. Java 中 使得 RMI 程序正确运行有哪些步骤？
为了让RMI程序能正确运行必须要包含以下几个步骤：

编译所有的源文件。

使用rmic生成stub。

启动rmiregistry。

启动RMI服务器。

运行客户端程序。

# 3. RMI 的 stub扮演了什么样的角色？
远程对象的stub扮演了远程对象的代表或者代理的角色。调用者在本地stub上调用方法，它负责在远程对象上执行方法。当stub的方法被调用的时候，会经历以下几个步骤：

1）初始化到包含了远程对象的JVM的连接。

2）序列化参数到远程的JVM。

3）等待方法调用和执行的结果。

4）反序列化返回的值或者是方法没有执行成功情况下的异常。

5）把值返回给调用者。

# 4. Java 中引用数据类型有哪些？它们与基本数据类型有什么区别？
引用数据类型分3种：类、接口、数组。

简单来说，只要不是基本数据类型。都是引用数据类型。 那它们有什么不同呢？

1、从概念方面来说

1）基本数据类型：变量名指向具体的数值

2）引用数据类型：变量名不是指向具体的数值，而是指向存数据的内存地址，也及时hash值。

2、从内存的构建方面来说（内存中有堆内存和栈内存两者）

1）基本数据类型：被创建时，在栈内存中会被划分出一定的内存。并将数值存储在该内存中。

2）引用数据类型：被创建时，首先会在栈内存中分配一块空间，然后在堆内存中也会分配一块具体的空间用来存储数据的具体信息，即hash值，然后由栈中引用指向堆中的对象地址。

# 5. Java中抛出 Throwable 结构有哪几种类型？
Java可抛出(Throwable)的结构分为三种类型：被检查的异常(CheckedException)，运行时异常(RuntimeException)，错误(Error)。

1、运行时异常

定义：RuntimeException及其子类都被称为运行时异常。

特点：

Java编译器不会检查它。也就是说，当程序中可能出现这类异常时，倘若既"没有通过throws声明抛出它"，也"没有用try-catch语句捕获它"，还是会编译通过。例如，除数为零时产生的ArithmeticException异常，数组越界时产生的IndexOutOfBoundsException异常，fail-fast机制产生的ConcurrentModificationException异常（java.util包下面的所有的集合类都是快速失败的，“快速失败”也就是fail-fast，它是Java集合的一种错误检测机制。当多个线程对集合进行结构上的改变的操作时，有可能会产生fail-fast机制。记住是有可能，而不是一定。

例如：假设存在两个线程（线程1、线程2），线程1通过Iterator在遍历集合A中的元素，在某个时候线程2修改了集合A的结构（是结构上面的修改，而不是简单的修改集合元素的内容），那么这个时候程序就会抛出 ConcurrentModificationException 异常，从而产生fail-fast机制，这个错叫并发修改异常。Fail-safe，java.util.concurrent包下面的所有的类都是安全失败的，在遍历过程中，如果已经遍历的数组上的内容变化了，迭代器不会抛出ConcurrentModificationException异常。如果未遍历的数组上的内容发生了变化，则有可能反映到迭代过程中。这就是ConcurrentHashMap迭代器弱一致的表现。ConcurrentHashMap的弱一致性主要是为了提升效率，是一致性与效率之间的一种权衡。要成为强一致性，就得到处使用锁，甚至是全局锁，这就与Hashtable和同步的HashMap一样了。）等，都属于运行时异常。

常见的五种运行时异常：

ClassCastException（类转换异常）

IndexOutOfBoundsException（数组越界）

NullPointerException（空指针异常）

ArrayStoreException（数据存储异常，操作数组是类型不一致）

BufferOverflowException

2、被检查异常

定义：Exception类本身，以及Exception的子类中除了"运行时异常"之外的其它子类都属于被检查异常。

特点 ： Java编译器会检查它。 此类异常，要么通过throws进行声明抛出，要么通过try-catch进行捕获处理，否则不能通过编译。例如，CloneNotSupportedException就属于被检查异常。

当通过clone()接口去克隆一个对象，而该对象对应的类没有实现Cloneable接口，就会抛出CloneNotSupportedException异常。被检查异常通常都是可以恢复的。 如：

IOException

FileNotFoundException

SQLException

被检查的异常适用于那些不是因程序引起的错误情况，比如：读取文件时文件不存在引发的FileNotFoundException。然而，不被检查的异常通常都是由于糟糕的编程引起的，比如：在对象引用时没有确保对象非空而引起的NullPointerException。

3、错误

定义 : Error类及其子类。

特点 : 和运行时异常一样，编译器也不会对错误进行检查。

当资源不足、约束失败、或是其它程序无法继续运行的条件发生时，就产生错误。程序本身无法修复这些错误的。例如，VirtualMachineError就属于错误。出现这种错误会导致程序终止运行。OutOfMemoryError、ThreadDeath。

Java虚拟机规范规定JVM的内存分为了好几块，比如堆，栈，程序计数器，方法区等。

# 6. 运行时异常与一般异常有何异同？
异常表示程序运行过程中可能出现的非正常状态，运行时异常表示虚拟机的通常操作中可能遇到的异常，是一种常见运行错误。

java编译器要求方法必须声明抛出可能发生的非运行时异常，但是并不要求必须声明抛出未被捕获的运行时异常。

# 7. 列举 5 个 JDK1.8 引入的新特性？
Java8在Java历史上是一个开创新的版本。

下面JDK8中5个主要的特性：

Lambda表达式，允许像对象一样传递匿名函数。

StreamAPI，充分利用现代多核CPU，可以写出很简洁的代码。

Date与TimeAPI，最终，有一个稳定、简单的日期和时间库可供使用。

扩展方法，接口中可以使用静态、默认方法。

重复注解，可以将相同的注解在同一类型上使用多次。

# 8. Java 中 DOM 和 SAX 解析器有什么不同？
DOM解析器将整个XML文档加载到内存来创建一棵DOM模型树，这样可以更快的查找节点和修改XML结构，而SAX解析器是一个基于事件的解析器，不会将整个XML文档加载到内存。

由于这个原因，DOM比SAX更快，也要求更多的内存，不适合于解析大XML文件。

# 9. Java 中 Serializable 和 Externalizable 有什么区别？
Serializable接口是一个序列化Java类的接口，以便于它们可以在网络上传输或者可以将它们的状态保存在磁盘上，是JVM内嵌的默认序列化方式，成本高、脆弱而且不安全。

Externalizable允许你控制整个序列化过程，指定特定的二进制格式，增加安全机制。

# 10. 写出一个正则表达式来判断一个字符串是否是一个数字？
一个数字字符串，只能包含数字，如0到9以及+、-开头，通过这个信息，如下代码，正则表达式来判断给定的字符串是不是数字。
```java
import java.util.regex.Pattern;
import java.util.regex.Matcher;
public boolean isNumeric(String str){
    Pattern pattern = Pattern.compile("[0-9]*"); 
    Matcher isNum = pattern.matcher(str); 
    if( !isNum.matches() ){
        return false;
    } 
    return true;
}
```
# 11. OOP 中的 组合、聚合和关联有什么区别？
如果两个对象彼此有关系，就说他们是彼此相关联的。组合和聚合是面向对象中的两种形式的关联。组合是一种比聚合更强力的关联。组合中一个对象是另一个的拥有者，而聚合则是指一个对象使用另一个对象。

如果对象A是由对象B组合的，则A不存在的话，B一定不存在，但是如果A对象聚合了一个对象B，则即使A不存在了，B也可以单独存在。

# 12. Java 中 YYYY 和 yyyy 有什么区别？
TODO

# 13. Java 中 UUID 是什么？
什么是UUID？

UUID是Universally Unique Identifier的缩写，它是在一定的范围内（从特定的名字空间到全球）唯一的机器生成的标识符。UUID具有以下涵义：

1）经由一定的算法机器生成

为了保证UUID的唯一性，规范定义了包括网卡MAC地址、时间戳、名字空间（Namespace）、随机或伪随机数、时序等元素，以及从这些元素生成UUID的算法。UUID的复杂特性在保证了其唯一性的同时，意味着只能由计算机生成。

2）非人工指定，非人工识别

UUID是不能人工指定的，除非你冒着UUID重复的风险。UUID的复杂性决定了“一般人“不能直接从一个UUID知道哪个对象和它关联。

3）在特定的范围内重复的可能性极小

UUID的生成规范定义的算法主要目的就是要保证其唯一性。但这个唯一性是有限的，只在特定的范围内才能得到保证，这和UUID的类型有关（参见UUID的版本）。

UUID是16字节128位长的数字，通常以36字节的字符串表示，示例如下：
```sh
3F2504E0-4F89-11D3-9A0C-0305E82C3301
```
其中的字母是16进制表示，大小写无关。

GUID（Globally Unique Identifier）是UUID的别名；但在实际应用中，GUID通常是指微软实现的UUID。

UUID的版本

UUID具有多个版本，每个版本的算法不同，应用范围也不同。

首先是一个特例－－Nil UUID－－通常我们不会用到它，它是由全为0的数字组成，如下：
```sh
00000000-0000-0000-0000-000000000000
```
UUID Version 1：基于时间的UUID

基于时间的UUID通过计算当前时间戳、随机数和机器MAC地址得到。由于在算法中使用了MAC地址，这个版本的UUID可以保证在全球范围的唯一性。但与此同时，使用MAC地址会带来安全性问题，这就是这个版本UUID受到批评的地方。如果应用只是在局域网中使用，也可以使用退化的算法，以IP地址来代替MAC地址－－Java的UUID往往是这样实现的（当然也考虑了获取MAC的难度）。

UUID Version 2：DCE安全的UUID

DCE（Distributed Computing Environment）安全的UUID和基于时间的UUID算法相同，但会把时间戳的前4位置换为POSIX的UID或GID。这个版本的UUID在实际中较少用到。

UUID Version 3：基于名字的UUID（MD5）

基于名字的UUID通过计算名字和名字空间的MD5散列值得到。这个版本的UUID保证了：相同名字空间中不同名字生成的UUID的唯一性；不同名字空间中的UUID的唯一性；相同名字空间中相同名字的UUID重复生成是相同的。

UUID Version 4：随机UUID

根据随机数，或者伪随机数生成UUID。这种UUID产生重复的概率是可以计算出来的，但随机的东西就像是买彩票：你指望它发财是不可能的，但狗屎运通常会在不经意中到来。

UUID Version 5：基于名字的UUID（SHA1）

和版本3的UUID算法类似，只是散列值计算使用SHA1（Secure Hash Algorithm 1）算法。

UUID的应用

从UUID的不同版本可以看出，Version 1/2适合应用于分布式计算环境下，具有高度的唯一性；Version 3/5适合于一定范围内名字唯一，且需要或可能会重复生成UUID的环境下；至于Version 4，我个人的建议是最好不用（虽然它是最简单最方便的）。

通常我们建议使用UUID来标识对象或持久化数据，但以下情况最好不使用UUID：

1）映射类型的对象。比如只有代码及名称的代码表。

2）人工维护的非系统生成对象。比如系统中的部分基础数据。

对于具有名称不可重复的自然特性的对象，最好使用Version 3/5的UUID。比如系统中的用户。如果用户的UUID是Version 1的，如果不小心删除了再重建用户，你会发现人还是那个人，用户已经不是那个用户了。虽然标记为删除状态也是一种解决方案，但会带来实现上的复杂性。

UUID生成器

我没想着有人看完了这篇文章就去自己实现一个UUID生成器，所以前面的内容并不涉及算法的细节。下面是一些可用的Java UUID生成器：

1）Java UUID Generator (JUG)：开源UUID生成器，LGPL协议，支持MAC地址。

2）UUID：特殊的License，有源码。

3）Java 5以上版本中自带的UUID生成器：好像只能生成Version 3/4的UUID。

此外，Hibernate中也有一个UUID生成器，但是，生成的不是任何一个（规范）版本的UUID，强烈不建议使用。

JAVA UUID 生成

GUID是一个128位长的数字，一般用16进制表示。算法的核心思想是结合机器的网卡、当地时间、一个随即数来生成GUID。从理论上讲，如果一台机器每秒产生10000000个GUID，则可以保证（概率意义上）3240年不重复。

UUID是1.5中新增的一个类，在java.util下，用它可以产生一个号称全球唯一的ID
```java
package com.jingxuan;
import java.util.UUID;
public class UTest {
    public static void main(String[] args) {
        UUID uuid = UUID.randomUUID();
        System.out.println(uuid);
    }
}
```
UUID(Universally Unique Identifier)全局唯一标识符,是指在一台机器上生成的数字，它保证对在同一时空中的所有机器都是唯一的。按照开放软件基金会(OSF)制定的标准计算，用到了以太网卡地址、纳秒级时间、芯片ID码和许多可能的数字。由以下几部分的组合：当前日期和时间(UUID的第一个部分与时间有关，如果你在生成一个UUID之后，过几秒又生成一个UUID，则第一个部分不同，其余相同)，时钟序列，全局唯一的IEEE机器识别号（如果有网卡，从网卡获得，没有网卡以其他方式获得），UUID的唯一缺陷在于生成的结果串会比较长。

# 14. 访问修饰符 public、private、protected 及不写（默认）时的区别？
区别如下：

| 作用域	       | 当前类    | 	同包	 | 子类 | 	其他 |
|------------|--------| ------ | ------- | ------- |
| public     | 	√     | 	√ | 	√ | 	√ |
| protected  | 	√     | 	√ | 	√ | 	× |
| default    | 	√     | 	√ | 	× | 	× |
| private    | 	√     | 	× | 	× | 	× |

类的成员不写访问修饰时默认为default。默认对于同一个包中的其他类相当于公开（public），对于不是同一个包中的其他类相当于私有（private）。受保护（protected）对子类相当于公开，对不是同一包中的没有父子关系的类相当于私有。

# 15. Java 有没有 goto？
goto是Java中的保留字，在目前版本的Java中没有使用。（根据James Gosling（Java之父）编写的《The Java Programming Language》一书的附录中给出了一个Java关键字列表，其中有goto和const，但是这两个是目前无法使用的关键字，因此有些地方将其称之为保留字，其实保留字这个词应该有更广泛的意义，因为熟悉C语言的程序员都知道，在系统类库中使用过的有特殊意义的单词或单词的组合都被视为保留字）。

# 16. static 关键字为何不能修饰局部变量？
static关键字修饰的变量或方法是属于类的，在编译时就已经确定了；而普通变量或方法是属于该由类生成的对象，需要在实例化后才能确定。

因此，若static关键字修饰了方法的局部变量，一方面方法需要在实例化之后才能确定，另一方面static修饰的变量需要在编译时确定，这就会导致矛盾。

# 17. Java 中 new 一个对象的过程中发生了什么？
TODO

# 18. Integer 为什么使用 .equas 比较不用 == 比较？
源码如下：

```java
public static Integer valueOf(int i) {
if (i >= IntegerCache.low && i <= IntegerCache.high)
return IntegerCache.cache[i + (-IntegerCache.low)];
return new Integer(i);
}
```
如上范围超过后new新的对象返回所有==比较地址为false。如果范围没有超过-127至128 则为true。

# 19. 什么是反射？
反射是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为 Java 语言的反射机制。

# 20. Java 中那些场景下使用反射机制？
JDBC中，利用反射动态加载数据库驱动程序。
```java
Class.forName("com.mysql.Driver.class");// 加载MySQL驱动类
```
Web服务器中利用反射调用Sevlet的服务方法。
Eclispe等开发工具利用反射动态刨析对象的类型与结构，动态提示对象的属性和方法。
很多框架都用到反射机制，注入属性，调用方法，如Spring。
