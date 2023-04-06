# 1. Java 中常用的集合有哪些？
   Map接口和Collection接口是所有集合框架的父接口

Collection接口的子接口包括：Set接口和List接口。

Set中不能包含重复的元素。List是一个有序的集合，可以包含重复的元素，提供了按索引访问的方式。

Map接口的实现类主要有：HashMap、Hashtable、ConcurrentHashMap以及TreeMap等。Map不能包含重复的key，但是可以包含相同的value。根据键得到值，对map集合遍历时先得到键的set集合，对set集合进行遍历，得到相应的值。

Set接口的实现类主要有：HashSet、TreeSet、LinkedHashSet等

List接口的实现类主要有：ArrayList、LinkedList、Stack以及Vector等

Iterator所有的集合类，都实现了Iterator接口，这是一个用于遍历集合中元素的接口，主要包含以下三种方法：

hasNext()是否还有下一个元素

next()返回下一个元素

remove()删除当前元素

# 2. 为什么 Map 接口不继承 Collection 接口？
1）Map提供的是键值对映射（即Key和value的映射），而Collection提供的是一组数据并不是键值对映射。

2）若果Map继承了Collection接口，那么所实现的Map接口的类到底是用Map键值对映射数据还是用Collection的一组数据呢？比如平常所用的hashMap、hashTable、treeMap等都是键值对，所以它继承Collection是完全没意义，而且Map如果继承Collection接口的话，违反了面向对象的接口分离原则。

接口分离原则：客户端不应该依赖它不需要的接口。

另一种定义是类间的依赖关系应该建立在最小的接口上。

接口隔离原则将非常庞大、臃肿的接口拆分成为更小的和更具体的接口，这样客户将会只需要知道他们感兴趣的方法。

接口隔离原则的目的是系统解开耦合，从而容易重构、更改和重新部署，让客户端依赖的接口尽可能地小。

3）Map和List、Set不同，Map放的是键值对，List、Set存放的是一个个的对象。说到底是因为数据结构不同，数据结构不同，操作就不一样，所以接口是分开的，还是接口分离原则。

# 3. Collection 和 Collections 有什么区别？
java.util.Collection是一个集合接口。它提供了对集合对象进行基本操作的通用接口方法。Collection接口在Java类库中有很多具体的实现。

Collection接口的意义是为各种具体的集合提供了最大化的统一操作方式。其直接继承接口有List与Set。

```sh
Collection
├List
│├LinkedList
│├ArrayList
│└Vector
│　└Stack
└Set
```

java.util.Collections是一个包装类。它包含有各种有关集合操作的静态多态方法。此类不能实例化，就像一个工具类，服务于Java的Collection框架。

# 4. ArrayList 和 LinkedList 有什么区别？
1）ArrayList是Array动态数组的数据结构，LinkedList是Link链表的数据结构，此外，它们两个都是对List接口的实现。前者是数组队列，相当于动态数组；后者为双向链表结构，也可当作堆栈、队列、双端队列。

2）当随机访问List时（get和set操作），ArrayList比LinkedList的效率更高，因为LinkedList是线性的数据存储方式，所以需要移动指针从前往后依次查找。

3）当对数据进行增加和删除的操作时（add和remove操作），LinkedList比ArrayList的效率更高，因为ArrayList是数组，所以在其中进行增删操作时，会对操作点之后所有数据的下标索引造成影响，需要进行数据的移动。

4）从利用效率来看，ArrayList自由性较低，因为它需要手动设置固定大小的容量，但是它的使用比较方便，只需要创建，然后添加数据，通过调用下标进行使用；而LinkedList自由性较高，能够动态的随数据量的变化而变化，但是它不便于使用。

5）ArrayList主要控件开销在于需要在List列表预留一定空间；而LinkList主要控件开销在于需要存储结点信息以及结点指针信息。

# 5. 【得物】描述一下 Java 中 HashMap 扩容过程？
当向容器添加元素的时候，会判断当前容器的元素个数，如果大于等于阈值——即当前数组的长度乘以加载因子的值的时候，就要自动扩容啦。

扩容（resize）就是重新计算容量，向HashMap对象里不停的添加元素，而HashMap对象内部的数组无法装载更多的元素时，对象就需要扩大数组的长度，以便能装入更多的元素。

当然Java中的数组是无法自动扩容的，方法是使用一个新的数组代替已有的容量小的数组，就像用一个小桶装水，如果想装更多的水，就得换大水桶。
```java
HashMap hashMap=new HashMap(cap);
```
cap =3，hashMap的容量为4； cap =4，hashMap的容量为4； cap =5，hashMap的容量为8； cap =9，hashMap的容量为16；

如果cap是2的n次方，则容量为cap，否则为大于cap的第一个2的n次方的数。

# 6. HashMap 是怎么扩容的？
当HashMap中元素个数超过数组大小*loadFactor时，需进行数组扩容。

loadFactor默认值为0.75，默认情况下，数组大小为16，HashMap中元素个数超过16 * 0.75=12的时，就把数组的大小扩展为2*16=32，即扩大一倍，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以能够预选知道HashMap中元素的个数，应该预设数组的大小，可以有效的提高HashMap的性能。

假设有1000个元素new HashMap(1000)，理论上来讲new HashMap(1024)更合适，不过上面已经提过即使是1000个元素，HashMap也会自动设置为1024。但是new HashMap(1024)，而0.75*1024 <1000, 为了可以0.75 * size >1000，必须new HashMap(2048)，避免了resize的问题。

总结：

添加元素时会检查容器当前元素个数。当HashMap的容量值超过临界值(默认16 * 0.75=12)时扩容。HashMap将会重新扩容到下一个2的指数幂（16->32->64）。调用resize方法，定义长度为新长度(32)的数组，然后对原数组数据进行再Hash。注意的是这个过程比较损耗性能。

# 7. JDK1.8 和 JDK1.7 中 ArrayList 的初始容量多少？
TODO

# 8. Arrays.asList() 有什么使用限制？
1）Arrays.asList()方法不适用于基本数据类型
```java
byte
short
int
long
float
double
boolean
```
2）Arrays.asList()方法把数组与列表链接起来，当更新其中之一时，另一个自动更新。

3）Arrays.asList()方法不支持add和remove方法。

# 9. Set 为什么是无序的？
Set系列集合添加元素无序的根本原因是底层采用哈希表存储元素。

JDK1.8以下版本：哈希表 = 数组 + 链表 + (哈希算法)

JDK1.8及以上版本：哈希表 = 数组 + 链表 + 红黑树 + (哈希算法)

当链表长度超过阈值（8）时，将链表转换为红黑树，这样大大减少了查找时间。

# 10. Comparable 和 Comparator有什么区别？
Comparable接口出自java.lang包，它有一个compareTo(Object obj)方法用来排序。

Comparator接口出自java.util包，它有一个compare(Object obj1, Object obj2)方 法用来排序。

一般对集合使用自定义排序时，需要重写compareTo()方法或compare()方法。

当需要对某一个集合实现两种排序方式，比如一个用户对象中的姓名和身份证分别采用一种排序方法。 方式一：重写compareTo()方法实现姓名、身份证排序

方式二：使用自定义的Comparator方法实现姓名、身份证排序

方法三：使用两个Comparator来实现姓名、身份证排序

其中方式二代表只能使用两个参数的形式Collections.sort()。

Collections是一个工具类，sort是其中的静态方法，是用来对List类型进行排序的，它有两种参数形式：
```java
public static <T extends Comparable<? super T>> void sort(List<T> list) {
    list.sort(null);
}
public static <T> void sort(List<T> list, Comparator<? super T> c) {
    list.sort(c);
}
```
# 11. 【得物】Java 中 HashMap 底层数据结构及主要参数？
**HashMap 典型特点：**

底层由链表+数组实现；

可以存储null键和null值；

线性不安全；

初始容量为16，扩容每次都是2的n次幂（保证位运算）；

加载因子为0.75，当Map中元素总数超过Entry数组的0.75，触发扩容操作；

并发情况下，HashMap进行put操作会引起死循环，导致CPU利用率接近100%。

1）HashMap底层实现数据结构为数组+链表的形式，JDK8及其以后的版本中使用了数组+链表+红黑树实现，解决了链表太长导致的查询速度变慢的问题。

2）简单来说，HashMap由数组+链表组成的，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的。HashMap通过key的HashCode经过扰动函数处理过后得到Hash值，然后通过位运算判断当前元素存放的位置，如果当前位置存在元素的话，就判断该元素与要存入的元素的hash值以及key是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。当Map中的元素总数超过Entry数组的0.75时，触发扩容操作，为了减少链表长度，元素分配更均匀。

**HashMap主要参数：**

DEFAULT_INITIAL_CAPACITY：默认的初始化容量，1<<4位运算的结果是16，也就是默认的初始化容量为16。当然如果对要存储的数据有一个估计值，最好在初始化的时候显示的指定容量大小，减少扩容时的数据搬移等带来的效率消耗。同时，容量大小需要是2的整数倍。

MAXIMUM_CAPACITY：容量的最大值，1 << 30位，2的30次幂。

DEFAULT_LOAD_FACTOR：默认的加载因子，设计者认为这个数值是基于时间和空间消耗上最好的数值。这个值和容量的乘积是一个很重要的数值，也就是阈值，当达到这个值时候会产生扩容，扩容的大小大约为原来的二倍。

TREEIFY_THRESHOLD：因为jdk8以后，HashMap底层的存储结构改为了数组+链表+红黑树的存储结构（之前是数组+链表），刚开始存储元素产生碰撞时会在碰撞的数组后面挂上一个链表，当链表长度大于这个参数时，链表就可能会转化为红黑树，为什么是可能后面还有一个参数，需要他们两个都满足的时候才会转化。

UNTREEIFY_THRESHOLD：介绍上面的参数时，我们知道当长度过大时可能会产生从链表到红黑树的转化，但是，元素不仅仅只能添加还可以删除，或者另一种情况，扩容后该数组槽位置上的元素数据不是很多了，还使用红黑树的结构就会很浪费，所以这时就可以把红黑树结构变回链表结构，什么时候变，就是元素数量等于这个值也就是6的时候变回来（元素数量指的是一个数组槽内的数量，不是HashMap中所有元素的数量）。

MIN_TREEIFY_CAPACITY：链表树化的一个标准，前面说过当数组槽内的元素数量大于8时可能会转化为红黑树，之所以说是可能就是因为这个值，当数组的长度小于这个值的时候，会先去进行扩容，扩容之后就有很大的可能让数组槽内的数据可以更分散一些了，也就不用转化数组槽后的存储结构了。当然，长度大于这个值并且槽内数据大于8时，那就转化为红黑树。

# 12. List、Set、Map 三者有什么区别？
List

存储数据允许不唯一集合，可以有多个元素引用相同的对象且是有序的对象。

Set

不允许重复的集合，不存在多个元素引用相同的对象。

Map

使用键值对存储方式。Map维护与Key有关联的值。两个Key可以引用相同的对象，但Key不能重复，比如Key是String类型，但也可以是任何对象。

# 13. HashMap 底层是如何实现的？
**JDK1.8之前**

JDK1.8之前HashMap底层是数组和链表结合在一起使用也就是链表散列。HashMap 通过 key的hashCode经过扰动函数处理过后得到hash值，然后通过 (n - 1) & hash 判断当前元素存放的位置（这里的n指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的hash值以及key是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。

所谓扰动函数指的就是HashMap的hash方法。使用hash方法也就是扰动函数是为了防止一些实现比较差的hashCode()方法 换句话说使用扰动函数之后可以减少碰撞。

JDK1.8的hash方法相比于JDK1.7 hash方法更加简化，但是原理不变。
```java
    static final int hash(Object key) {
      int h;
      // key.hashCode()：返回散列值也就是hashcode
      // ^ ：按位异或
      // >>>:无符号右移，忽略符号位，空位都以0补齐
      return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
  }
```
对比一下JDK1.7的HashMap的hash()方法源码:
```java
static int hash(int h) {
    // This function ensures that hashCodes that differ only by
    // constant multiples at each bit position have a bounded
    // number of collisions (approximately 8 at default load factor).

    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h >>> 4);
}
```
相比于JDK1.8的hash方法 ，JDK1.7的hash 方法的性能会稍差一点点，因为毕竟扰动了4次。

所谓 “拉链法” 就是：将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

**JDK1.8之后**

相比于之前的版本，JDK1.8之后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）（将链表转换成红黑树前会判断，如果当前数组的长度小于64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。

TreeMap、TreeSet以及JDK1.8之后的HashMap底层都用到了红黑树。红黑树就是为了解决二叉查找树的缺陷，因为二叉查找树在某些情况下会退化成一个线性结构。

# 14. HashMap 长度为什么是2的幂次方？
HashMap默认长度16，扩容是2的n次方。

HashMap为了存取高效，要尽量较少碰撞，通俗的说就是要尽量把数据分配均匀，每个链表长度大致相同，这个实现就是要把数据存到哪个链表中的算法。

取模即hash%length，计算机中直接求余效率不如位移运算，JDK源码中做了相应的优化hash&(length-1)，hash%length==hash&(length-1)的前提是length是2的n次方；

比如说10%8=2,10的二进制是1010，8的二进制是0100

>0000 1010 0000 1000

这种取余操作对应2的幂次实际上也就是除数的值之前的数据被整除，后面的余数就是被除数剩余部分为1的值。

0000 1010除以0000 1000把0000 1000高位以及自己抹除，剩下0000 0010。换成与（&）的操作，就是把n(除数)-1和被除数，取余的结果“(n-1)&hash ”。

**为什么这样可以均匀分布减少碰撞呢？**

2的n次方实即为1后面有n个0，2的n次方-1即为n个1；

比如长度为9时，3&(9-1)=0  2&(9-1)=0 ，都在0上，碰撞了；

比如长度为8时，3&(8-1)=3  2&(8-1)=2 ，不同位置上，不碰撞。

其实就是按位“与”的时候，每一位都能&1，也就是和1111……1111111进行与运算。

Hash值的范围值-2147483648到2147483647，前后加起来大概40亿的映射空间，只要哈希函数映射得比较均匀松散，一般应用是很难出现碰撞的。但问题是内存是无法存储这种量级别数组的，所以这个散列值是不能直接使用的。可以针对数组的长度取模运算，得到的余数才能用来要存放的位置也就是对应的数组下标。这个数组下标的计算方法是“(n-1) & hash”。这也就解释了HashMap的长度为什么是2的幂次方。

# 15. HashMap 为什么多线程会导致死循环？
HashMap多线程会导致死循环的主要原因在于并发下的Rehash()方法会造成元素之间会形成一个循环链表。

注意的是JDK1.8及以上解决了这个问题，但是不建议在多线程下使用HashMap，因为多线程下使用HashMap还是会存在其他问题比如数据丢失。在并发环境下推荐使用ConcurrentHashMap，后续篇幅中会写相关的文章。

# 16. 什么是泛型？
Java泛型是J2 SE1.5中引入的一个新特性，其本质是参数化类型，也就是说所操作的数据类型被指定为一个参数（type parameter），这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型接口、泛型方法。

泛型意味着编写代码可以被不同类型的对象所重用，为了编写重用性更好的代码。泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。

比如常见集合类LinkedList源码如下：
```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
    transient int size = 0;

    /**
     * Pointer to first node.
     * Invariant: (first == null && last == null) ||
     *            (first.prev == null && first.item != null)
     */
    transient Node<E> first;
	...
}
```

# 17. 泛型都有哪些规则？
1、泛型的参数类型只能是类（包括自定义类），不能是简单类型。

2、同一种泛型可以对应多个版本，这是因参数类型是不确定的，不同版本的泛型类实例是不兼容的。

3、泛型类型的参数可以有多个。

3、泛型的参数类型可以使用extends语句，称为“有界类型”。

4、泛型的参数类型可以是通配符类型，例如Class类。

# 18. 泛型有什么使用场景？
当类中要操作的引用数据类型不确定时，在JDK1.5版本前使用Object来完成扩展，JDK1.5后推荐使用泛型来完成扩展，同时保证安全性。

# 19. Java 泛型中 E、T、K、V等标记符是什么含义？
Java泛型中的标记符含义

**E**

Element在集合中使用，因为集合中存放的是元素。E是对各方法中的泛型类型进行限制，以保证同一个对象调用不同的方法时，操作的类型必定是相同的。E可以用其它任意字母代替。

**T**

Type（Java 类），T代表在调用时的指定类型。会进行类型推断。

**K**

Key（键）

**V**

Value（值）

**N**

Number（数值类型）

**?**

表示不确定java类型,是类型通配符，代表所有类型。？不会进行类型推断

**S、U、V**

2nd、3rd、4th types

# 20. Java 数组中可以使用泛型吗？
Array不支持泛型，这是因为编译时会造成类型安全问题。

Java范型停留在编译层运行时，这些范型的信息会被抹除掉。Java中这种做法不必修改JVM，减少了潜在的大幅改动而产生的风险，但是这也许反映出了Java Bytecode规范在设计之初的先天不足。

Java中Object[]数组可以是任何数组的父类或者任何一个数组都可以向上转型成它在定义时指定元素类型的父类的数组，如果这时存放不同于原始数据类型但是满足后来使用父类类型的话，编译不会有问题，但是在运行时会检查加入数组对象的类型，于是会抛出ArrayStoreException异常。

比如代码如下，就会抛出ArrayStoreException异常。
```java
String[] arrays = new String[20];
Object[] obj = arrays;
obj[0] = new Integer(1); // throws ArrayStoreException at runtime
```
注意的是List可以提供编译期的类型安全保证，而Array不能。
