# 1. Java 中常用的并发集合有哪些？
**并发List**

Vector和CopyOnWriteArrayList是两个线程安全的List，Vector读写操作都用了同步，相对来说更适用于写多读少的场合，CopyOnWriteArrayList在写的时候会复制一个副本，对副本写，写完用副本替换原值，读的时候不需要同步，适用于写少读多的场合。

**并发Set**

CopyOnWriteArraySet基于CopyOnWriteArrayList来实现的，只是在不允许存在重复的对象这个特性上遍历处理了一下。

**并发Map**

ConcurrentHashMap是专用于高并发的Map实现，内部实现进行了锁分离，get操作是无锁的。

**并发Queue**

在并发队列上JDK提供了两套实现，一个是以ConcurrentLinkedQueue为代表的高性能队列，一个是以BlockingQueue接口为代表的阻塞队列。ConcurrentLinkedQueue适用于高并发场景下的队列，通过无锁的方式实现，通常ConcurrentLinkedQueue的性能要优于BlockingQueue。BlockingQueue的典型应用场景是生产者-消费者模式中，如果生产快于消费，生产队列装满时会阻塞，等待消费。

**并发Dueue**

Queue是一种双端队列，它允许在队列的头部和尾部进行出队和入队的操作。Dueue实现类有非线程安全的LinkedList、ArrayDueue和线程安全的LinkedBlockingDueue。LinkedBlockingDueue没有进行读写锁的分离，因此同一时间只能有一个线程对其操作，因此在高并发应用中，它的性能要远远低于LinkedBlockingQueue，更低于ConcurrentLinkedQueue。

**并发锁重入锁ReentrantLock**

ReentrantLock是一种互斥锁的实现，就是一次最多只能一个线程拿到锁；

**读写锁ReadWriteLock**

读写锁有读取和写入两种锁，读取锁允许多个读取的线程同时持有，而写入锁只能有一个线程持有。

**条件Condition**

调用Condition对象的相关方法，可以方便的挂起和唤醒线程。

# 2. 什么是 HashSet？
HashSet实现了Set接口，它不允许集合中有重复的值，当我们提到HashSet时，第一件事情就是在将对象存储在HashSet之前，要先确保对象重写equals()和hashCode()方法，这样才能比较对象的值是否相等，以确保set中没有储存相等的对象。如果我们没有重写这两个方法，将会使用这个方法的默认实现。
```java
public boolean add(Object o);
```
该方法用来在Set中添加元素，当元素值重复时则会立即返回false，如果成功添加的话会返回true。

# 3. 什么是 HashMap？
HashMap实现了Map接口，Map接口对键值对进行映射。

Map中不允许重复的键。Map接口有两个基本的实现，HashMap和TreeMap。TreeMap保存了对象的排列次序，而HashMap则不能。HashMap允许键和值为null。

HashMap是非synchronized的，但collection框架提供方法能保证HashMap synchronized，这样多个线程同时访问HashMap时，能保证只有一个线程更改Map。
```java
public Object put(Object Key,Object value);
```
该方法用来将元素添加到map中。

# 4. HashSet 和 HashMap 有什么区别？
HashMap实现了Map接口，而HashSet实现了Set接口。

HashMap储存键值对，而HashSet仅仅存储对象。

HashMap使用put()方法将元素放入map中，而使用add()方法将元素放入set中。

HashMap中使用键对象来计算hashcode值，而HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false

HashMap比较快，因为是使用唯一的键来获取对象，而HashSet较HashMap来说比较慢。

# 5. 说一说 HashMap 的特性？
1、HashMap存储键值对实现快速存取，允许为null。key值不可重复，若key值重复则覆盖。

2、非同步，线程不安全。

3、底层是hash表，不保证有序，比如插入的顺序。

# 6. HashMap 中 put 是如何实现的？
1、计算关于key的hashcode值（与Key.hashCode的高16位做异或运算）

2、如果散列表为空时，调用resize()初始化散列表

3、如果没有发生碰撞，直接添加元素到散列表中去

4、如果发生了碰撞(hashCode值相同)，进行三种判断

1）若key地址相同或者equals后内容相同，则替换旧值

2）如果是红黑树结构，就调用树的插入方法

3）链表结构，循环遍历直到链表中某个节点为空，尾插法进行插入，插入之后判断链表个数是否到达变成红黑树的阙值8；也可以遍历到有节点与插入元素的哈希值和内容相同，进行覆盖。

5、如果桶满了大于阀值，则resize进行扩容

# 7. HashMap 参数 loadFacto 作用是什么？
loadFactor表示HashMap的拥挤程度，影响hash操作到同一个数组位置的概率。默认loadFactor等于0.75，当HashMap里面容纳的元素已经达到HashMap数组长度的75%时，表示HashMap太挤了，需要扩容，在HashMap的构造器中可以定制loadFactor。

# 8. HashMap 中一般使用什么类型元素作为 Key？
HashMap一般采用String、Integer 等类作为key、因为这些类底层已经重写了hashcode、equals方法，用的是final修饰类在多线程情况下相对安全。

# 9. HashMap 超出负载因子 0.75 时有什么操作？
默认的负载因子大小为0.75，也就是指当一个map填满了75%的bucket时候，和其它集合类(如ArrayList等)一样，将会创建原来HashMap大小的两倍的bucket数组，来重新调整map的大小，并将原来的对象放入新的bucket数组中。这个过程叫作rehashing，因为它调用hash方法找到新的bucket位置。

通俗的讲就是如果超过阙值会进行扩容操作，扩容后的数组大小是原数组的2倍，将原来的元素重新hashing放入到新的散列表中去。

# 10. Java 中两个对象 hashCode 相等会产生什么问题？
Java中两个对象hashCode相等会产生哈希碰撞，若key值相同则替换旧值，不然链接到链表后面，链表长度超过阙值8就转为红黑树存储。

# 11. Java 中两个键 hashCode 相等，如何获取对象？
HashCode相同，通过equals比较内容获取值对象。

# 12. 为什么不直接将key作为哈希值而是与高16位做异或运算？
数组位置确定用的是与运算，仅仅最后四位有效，设计者将key的哈希值与高16为做异或运算使得在做&运算确定数组的插入位置时，此时的低位实际是高位与低位的结合，增加了随机性，减少了哈希碰撞的次数。

HashMap默认初始化长度为16，并且每次自动扩展或者是手动初始化容量时，必须是2的幂。

# 13. Java 中 List 和 Array 如何相互转换？
**List to Array**

List 提供了toArray的接口，所以可以直接调用转为object型数组
```java
List<String> list = new ArrayList<String>();
Object[] array=list.toArray();
```
上述方法存在强制转换时会抛异常，下面此种方式更推荐：可以指定类型
```java
String[] array=list.toArray(new String[list.size()]);
```
**Array to List**

最简单的方法
```java
String[] array = {"java", "c"};
List<String> list = Arrays.asList(array);

public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}
```
# 14. ArrayList 和 CopyOnWriteArrayList 有什么区别？
1）CopyOnWriteArrayList和ArrayList继承于AbstractList不同，CopyOnWriteArrayList没有继承AbstractList，它仅仅只是实现了List接口。

2）ArrayList中iterator()方法返回的Iterator迭代器是在AbstractList中实现的；而CopyOnWriteArrayList是自己实现Iterator迭代器。

3）ArrayList中Iterator迭代器实现类中调用next()方法时，会调用checkForComodification()方法比较expectedModCount和modCount的值大小；而CopyOnWriteArrayList的Iterator迭代器实现类中，没有所谓的checkForComodification()方法，更不会抛出ConcurrentModificationException异常。

# 15. Java 中迭代集合如何避免 ConcurrentModificationException？
遍历集合的时可以使用并发集合类来避免ConcurrentModificationException异常。

例如使用CopyOnWriteArrayList集合，而不是ArrayList集合。

# 16. Java 中如何确保一个集合不能被修改？
可以使用Collections包下的unmodifiableMap()方法，通过这个方法返回的map是不可以修改的。这样改变集合的任何操作都会抛出Java.lang.UnsupportedOperationException异常。

Collections包也提供了对list和set集合的方法。
```java
Collections.unmodifiableList(List)
Collections.unmodifiableSet(Set)
```

也可以使用Collections.unmodifiableCollection(Collection c)方法来创建一个只读集合，这样改变集合的任何操作都会抛出Java.lang.UnsupportedOperationException异常。

示例代码：
```java
List<String> list = new ArrayList<>();
list. add("微信公众号");
Collection<String> unmlist = Collections.unmodifiableCollection(list);
unmlist.add("Java精选"); // 运行时此行报错
System.out.println(list.size());
```

# 17. Java 中如何优化 ArrayList 集合插入万条数据量？
ArrayList的底层是数组实现且数组的默认值是10，如果插入10000条要不断的扩容，耗费时间，所以我们调用ArrayList的指定容量的构造器方法ArrayList(int size) 就可以实现不扩容，就提高了性能。

# 18. Java 集合中都有哪些根接口？
Java集合类是一种非常实用的工具类，主要用于保存、盛装其它数据（集合里只能保存对象），因此集合类也被成为容器类。

所有的集合类都位于java.util包下，在java.util.concurrent下还提供了一些支持多线程的集合类。

Java的集合类主要由两个接口派生而来：Collection和Map，这两个是Java集合框架的根接口。

**Collection接口**

Collection派生出三个子接口，Set代表不可重复的无序集合、List代表可重复的有序集合、Queue是java提供的队列实现；Collection是最基本的集合接口，它提供了一些通用的方法，供子接口调用。

**Map接口**

Map实现类都用于保存具有映射关系的数据，它们保存的数据都是key-value对，如果要查找Map中的数据，总是根据key来获取，所以key是不可重复的，它用于标识集合里的每项数据。

# 19. Java 中如何快速删除链表中某个节点？
TODO

# 20. Java 中 ConcurrentModificationException 异常出现的原因？
```java
public class Test {
    public static void main(String[] args)  {
        ArrayList<Integer> list = new ArrayList<Integer>();
        list.add(2);
        Iterator<Integer> iterator = list.iterator();
        while(iterator.hasNext()){
            Integer integer = iterator.next();
            if(integer==2)
                list.remove(integer);
        }
    }
}
```
执行上段代码是有问题的，会抛出ConcurrentModificationException异常。

原因分析：调用list.remove()方法，导致modCount和expectedModCount的值不一致。
```java
final void checkForComodification() {
    if (modCount != expectedModCount)
    throw new ConcurrentModificationException();
}
```
解决办法：在迭代器中如果要删除元素的话，需要调用Iterator类的remove方法。
```java
public class Test {
    public static void main(String[] args)  {
        ArrayList<Integer> list = new ArrayList<Integer>();
        list.add(2);
        Iterator<Integer> iterator = list.iterator();
        while(iterator.hasNext()){
            Integer integer = iterator.next();
            if(integer==2)
                iterator.remove();   //注意这个地方
        }
    }
}
```
