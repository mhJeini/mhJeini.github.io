# 1. Java 中常见线程安全的 Map 都有哪些？
Java中常见线程安全的Map有Hashtable、synchronizedMap和ConcurrentHashMap。

**Hashtable**

使用方式，代码如下：
```java
Map<String,Object> hashtable=new Hashtable<String,Object>();
```
查看源码可以看出put()、get()、containsKey()等方法都使用了synchronized关键字修饰实现同步，因此它是线程安全的。
```java
public synchronized V put(K key, V value) {//部分源代码jdk1.8
	// Make sure the value is not null
	if (value == null) {
		throw new NullPointerException();
	}

	// Makes sure the key is not already in the hashtable.
	Entry<?,?> tab[] = table;
	int hash = key.hashCode();
	int index = (hash & 0x7FFFFFFF) % tab.length;
	@SuppressWarnings("unchecked")
	Entry<K,V> entry = (Entry<K,V>)tab[index];
	for(; entry != null ; entry = entry.next) {
		if ((entry.hash == hash) && entry.key.equals(key)) {
			V old = entry.value;
			entry.value = value;
			return old;
		}
	}

	addEntry(hash, key, value, index);
	return null;
}

@SuppressWarnings("unchecked")
public synchronized V get(Object key) {
	Entry<?,?> tab[] = table;
	int hash = key.hashCode();
	int index = (hash & 0x7FFFFFFF) % tab.length;
	for (Entry<?,?> e = tab[index] ; e != null ; e = e.next) {
		if ((e.hash == hash) && e.key.equals(key)) {
			return (V)e.value;
		}
	}
	return null;
}

public synchronized boolean containsKey(Object key) {
	Entry<?,?> tab[] = table;
	int hash = key.hashCode();
	int index = (hash & 0x7FFFFFFF) % tab.length;
	for (Entry<?,?> e = tab[index] ; e != null ; e = e.next) {
		if ((e.hash == hash) && e.key.equals(key)) {
			return true;
		}
	}
	return false;
}
```
**SynchronizedMap**

使用方式，代码如下：
```java
Map<String,Object> synchronizedMap = Collections.synchronizedMap(new Hashtable<String,Object>());

```
查看源码可以看出其实是加了一个对象锁，在每次操作hashmap时都需要先获取这个对象锁，且这个对象锁使用了synchronized关键字修饰，锁的性能与Hashtable相差无几。
```java
SynchronizedMap(Map<K,V> m, Object mutex) {//部分源代码jdk1.8
	this.m = m;
	this.mutex = mutex;
}

public int size() {
	synchronized (mutex) {return m.size();}
}
public boolean isEmpty() {
	synchronized (mutex) {return m.isEmpty();}
}
public boolean containsKey(Object key) {
	synchronized (mutex) {return m.containsKey(key);}
}
public boolean containsValue(Object value) {
	synchronized (mutex) {return m.containsValue(value);}
}
public V get(Object key) {
	synchronized (mutex) {return m.get(key);}
}

public V put(K key, V value) {
	synchronized (mutex) {return m.put(key, value);}
}
public V remove(Object key) {
	synchronized (mutex) {return m.remove(key);}
}
```

**ConcurrentHashMap**

使用方式，代码如下：
```java
Map<String,Object> concurrentHashMap = new ConcurrentHashMap<String,Object>();
```
ConcurrentHashMap是目前使用最多的线程安全的集合，也是最推荐的一个集合。

查看源码可以发现线程安全是通过cas+synchronized+volatile来实现的，其中也可看出它的锁是分段锁，所以它的性能相对来说是比较好的，整体实现还是比较复杂的。
```java
public V put(K key, V value) {//部分源代码jdk1.8
	return putVal(key, value, false);
}

/** Implementation for put and putIfAbsent */
final V putVal(K key, V value, boolean onlyIfAbsent) {
	if (key == null || value == null) throw new NullPointerException();
	int hash = spread(key.hashCode());
	int binCount = 0;
	for (Node<K,V>[] tab = table;;) {
		Node<K,V> f; int n, i, fh;
		if (tab == null || (n = tab.length) == 0)
			tab = initTable();
		else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
			if (casTabAt(tab, i, null,
						 new Node<K,V>(hash, key, value, null)))
				break;                   // no lock when adding to empty bin
		}
		else if ((fh = f.hash) == MOVED)
			tab = helpTransfer(tab, f);
		else {
			V oldVal = null;
			synchronized (f) {
				if (tabAt(tab, i) == f) {
					if (fh >= 0) {
						binCount = 1;
						for (Node<K,V> e = f;; ++binCount) {
							K ek;
							if (e.hash == hash &&
								((ek = e.key) == key ||
								 (ek != null && key.equals(ek)))) {
								oldVal = e.val;
								if (!onlyIfAbsent)
									e.val = value;
								break;
							}
							Node<K,V> pred = e;
							if ((e = e.next) == null) {
								pred.next = new Node<K,V>(hash, key,
														  value, null);
								break;
							}
						}
					}
					else if (f instanceof TreeBin) {
						Node<K,V> p;
						binCount = 2;
						if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
													   value)) != null) {
							oldVal = p.val;
							if (!onlyIfAbsent)
								p.val = value;
						}
					}
				}
			}
			if (binCount != 0) {
				if (binCount >= TREEIFY_THRESHOLD)
					treeifyBin(tab, i);
				if (oldVal != null)
					return oldVal;
				break;
			}
		}
	}
	addCount(1L, binCount);
	return null;
}

```

# 2. Java 泛型有什么优点？
JDK1.5版本引入了泛型，所有的集合接口和实现都大量地使用它。

类型安全，提供编译期间的类型检测。泛型允许为集合提供一个可以容纳的对象类型，如果添加其它类型的任何元素，它会在编译时报错，避免运行时出现异常ClassCastException。

泛型前后兼容。

泛化代码，代码可以更多的重复利用，泛型使得代码整洁，不需要使用显式转换和instanceOf操作符。

泛型性能比较高，用GJ（泛型Java）编写的代码可以为Java编译器和虚拟机带来更多的类型信息，这些信息对Java程序做进一步优化提供条件。

# 3. Java 迭代器 Iterator 是什么？
迭代器模式是Java中常用的设计模式之一，主要用于顺序访问集合对象的元素，无需知道集合对象的底层实现。

Iterator是可以遍历任何Collection集合的对象，为各种容器提供了公共的操作接口，隔离对容器的遍历操作和底层实现，从而解耦。

Collection集合中使用迭代器方法来获取迭代器实例，迭代器取代了Java集合框架中Enumeration。迭代器允许调用者在迭代过程中移除元素。

Iterator的缺点是增加新的集合类需要对应增加新的迭代器类，迭代器类与集合类成对增加。

# 4. Iterator 和 Enumeration 接口有哪些区别？
Enumeration的速度是Iterator的两倍，也使用更少的内存。Enumeration是非常基础的，也满足了基础的需要。但是，与Enumeration相比，Iterator更加安全，因为当一个集合正在被遍历的时候，它会阻止其它线程去修改集合。

迭代器取代了Java集合框架中的Enumeration。迭代器允许调用者从集合中移除元素，而Enumeration不能做到。为了使它的功能更加清晰，迭代器方法名已经经过改善。

# 5. Iterator 和 Enumeration 接口有哪些区别？
Iterator是JDK 1.2版本中添加的接口，支持HashMap、ArrayList等集合遍历接口。Iterator是支持fail-fast机制，当多个线程对同一个集合的内容进行操作时可能产生fail-fast事件。

Iterator有3个方法接口，Iterator能读取集合数据且可以对数据进行删除操作，而Enumeration只有2个方法接口，通过Enumeration只能读取集合的数据，而不能对数据进行修改。

Enumeration接口的处理性能是Iterator的两倍且内存使用也更少，但是Iterator接口比Enumeration要安全很多，主要是因为其他线程不能够修改正在被iterator遍历的集合中的对象。同时，Iterator允许调用者删除底层集合里面的元素，这对Enumeration接口来说是不可能的。

迭代器取代了Java集合框架中的Enumeration。迭代器允许调用者从集合中移除元素，而Enumeration不能实现。

Enumeration是JDK 1.0版本中添加的接口。Enumeration的方法接口为Vector、Hashtable等类提供了遍历接口。Enumeration本身不支持同步，而在Vector、Hashtable实现Enumeration时添加了同步。

# 6. Iterator 和 ListIterator 都有哪些区别？
1）Iterator可以遍历Set和List集合，而ListIterator只能遍历List集合。

2）ListIterator和Iterator都有hasNext()和next()方法，可以实现顺序向后遍历，但是ListIterator有hasPrevious()和previous()方法，可以实现逆向遍历。

3）ListIterator接口使用nextIndex()和previousIndex()方法可以获取当前的索引位置，而Iterator不具备此功能。

4）ListIterator和Iterator都可实现删除对象，但是ListIterator接口使用set()方法可以实现对象的修改，而Iierator仅能遍历，不能修改。

# 7. Java 中遍历 List 集合都有哪些方式？
```java
List<String> list = new ArrayList<>();

for (int i = 0; i < list.size(); i++) {
	System.out.println(list.get(i));
}

//使用for-each循环
for(String obj : list){
	System.out.println(obj);
}

//使用迭代器 iterator
Iterator<String> it = list.iterator();
while(it.hasNext()){
  String obj = it.next();
  System.out.println(obj);
}

```
遍历List集合时使用迭代器方式线程安全，可以保障遍历的集合元素不被修改，否则抛出异常ConcurrentModificationException。

# 8. Java 中什么是 fail-safe？
fail-safe（安全失败）采用安全失败机制的集合容器，在遍历时不是直接在集合内容上访问的，而是先复制原有集合内容，在拷贝的集合上进行遍历。

# 9. Java 中什么是 fail-fast？
fail-fast（快速失败）是Java对java.util包下的所有集合类的是一种错误检测机制。

# 10. fail-fast 与 fail-safe 有什么区别？
fail-fast机制，是一种错误检测机制。它只能被用来检测错误，因为JDK并不保证fail-fast机制一定会发生。若在多线程环境下使用fail-fast机制的集合，建议使用“java.util.concurrent包下的类”去取代“java.util包下的类”。

当多个线程对同一个集合进行操作时，某线程访问集合的过程中，该集合的内容被其他线程所改变，即其它线程通过add()、remove()、clear()等方法改变了modCount的值，此时就会抛出ConcurrentModificationException异常，产生fail-fast。

Iterator迭代器中fail-fast属性与当前的集合共同起作用，Iterator的安全失败是基于对底层集合做拷贝，因此它不会受到集合中任何改动的影响。

Java.util包中的所有集合类都快速失败的，即为被设计为fail-fast；而java.util.concurrent中的集合类都是安全失败的，即为fail-safe。

fail-fast事件产生是通过抛出ConcurrentModificationException异常，而fail-safe事件的产生从不抛出ConcurrentModificationException异常。

# 11. Java 中 UnsupportedOperationException 是什么？
UnsupportedOperationException是用于表示该操作不支持的异常。

在JDK的类中已经被大量运用，比如在集合框架java.util.Collections.UnmodifiableCollection类所有add()和remove()方法操作中抛出这个异常。

# 12. Java 中如何判断 “java.util.LinkedList” 字符串实现 List 接口？
TODO

# 13. Java 中如何获取 List 集合泛型类型？
实现获取List集合泛型类型，实例代码如下：
```java
public static void main(String[] args) throws Exception{	
	Class<?> clazz = A.class;
	Field field = clazz.getField("lists");
	ParameterizedType type = (ParameterizedType) field.getGenericType();
	System.out.println(type.getActualTypeArguments()[0]);
}

class A {
	public List<String> lists = new ArrayList<>();
}

```
输出结果
```sh
class java.lang.String
```

# 14. Java 中 List 集合如何排序？
实例代码如下：

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class SortList {
	public static void main(String[] args) {

		List<Integer> list = Arrays.asList(1, 10,5, 4, 2, 9, 7, 3, 8, 6);
		System.out.print("原始数据：");
		list.forEach(n -> {
			System.out.print(n + ", ");
		});
		System.out.print("\n\r升序排列：");
		Collections.sort(list);
		list.forEach(n -> {
			System.out.print(n + ", ");
		});

		System.out.print("\n\r降序排列：");
		Collections.reverse(list);
		list.forEach(n -> {
			System.out.print(n + ", ");
		});
	}
}

```
执行结果如下：
```sh
原始数据：1, 10, 5, 4, 2, 9, 7, 3, 8, 6, 

升序排列：1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 

降序排列：10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 

```
# 15. HashMap 集合如何按 value 值排序？
HashMap是一个数组和链表组成的一种链表散列结构，存储方式是根据key的hash值来决定存储的位置，因此存储后的元素不会维持插入时的顺序。

如果需要控制某个类的次序且本身不支持排序，可以通过java.util.Comparator接口建立一个类比较器进行排序从而实现排序。

将HashMap中的entryset取出放入一个ArrayList中，对ArrayList中的entryset进行排序，以达到对HashMap中value值排序的效果。

实例代码如下：
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.Map.Entry;

public class SortHashMap {
	public static void main(String[] args) {
	
		Map<String, Integer> map = new HashMap<>();
		map.put("A", 4);
		map.put("B", 2);
		map.put("C", 1);
		map.put("D", 3);

		List<Map.Entry<String, Integer>> list = new ArrayList<>(map.entrySet());
		Collections.sort(list, new Comparator<Map.Entry<String, Integer>>() {
			@Override
			public int compare(Entry<String, Integer> o1, Entry<String, Integer> o2) {
				return o1.getValue() - o2.getValue();
			}
		});

		Iterator<Map.Entry<String, Integer>> iter = list.iterator();
		while (iter.hasNext()) {
			Map.Entry<String, Integer> item = iter.next();
			String key = item.getKey();
			int value = item.getValue();
			System.out.println("键：" + key + "   值：" + value);
		}
	}
}

```
执行结果如下：
```sh
键：C   值：1
键：B   值：2
键：D   值：3
键：A   值：4
```
# 16. Java 中如何创建和遍历单链表？
```java
public class LinkList {
	public Node head;
	public Node current;

	//方法：向链表中添加数据  
	public void add(int data) {
		// 判断链表为空
		if (head == null) {// 如果头结点为空，说明这个链表还没有创建，那就把新的结点赋给头结点
			head = new Node(data);
			current = head;
		} else {
			// 创建新的结点，放在当前节点的后面（把新的结点合链表进行关联）
			current.next = new Node(data);
			// 把链表的当前索引向后移动一位
			current = current.next; // 此步操作完成之后，current结点指向新添加的那个结点
		}
	}

	//方法：遍历链表（打印输出链表。方法的参数表示从节点node开始进行遍历  
	public void print(Node node) {
		if (node == null) {
			return;
		}

		current = node;
		while (current != null) {
			System.out.println(current.data);
			current = current.next;
		}
	}

	class Node {
		//注：此处的两个成员变量权限不能为private，因为private的权限是仅对本类访问。  
		int data; // 数据域
		Node next;// 指针域

		public Node(int data) {
			this.data = data;
		}
	}

	public static void main(String[] args) {
		LinkList list = new LinkList();
		//向LinkList中添加数据  
		for (int i = 0; i < 10; i++) {
			list.add(i);
		}

		list.print(list.head);// 从head节点开始遍历输出
	}

}
```
执行结果
```sh
0
1
2
3
4
5
6
7
8
9
```
查看上述代码，Node节点采用的是内部类来表示。使用内部类的最大好处是可以和外部类进行私有操作的互相访问。

内部类访问的特点是：内部类可以直接访问外部类的成员，包括私有；外部类要访问内部类的成员，必须先创建对象。

为了方便添加和遍历的操作，在LinkList类中添加一个成员变量current，用来表示当前节点的索引。

遍历链表的方法中，参数node表示从node节点开始遍历，不一定要从head节点遍历。

# 17. Java 中求单链表中节点的个数？
Java中计算单链表中节点的个数需要注意检查链表是否为空。时间复杂度为O（n）。

核心代码：
```java
// 方法：获取单链表的长度
public int getLength(Node head) {
	if (head == null) {
		return 0;
	}

	int length = 0;
	Node current = head;
	while (current != null) {
		length++;
		current = current.next;
	}

	return length;
}
```

# 18. Java 中如何查找单链表中的中间结点？
分析思路

设置两个指针first和second，两个指针同时向前走，second指针每次走两步，first指针每次走一步，直到second指针走到最后一个结点时，此时first指针所指的结点就是中间结点。

注意链表为空，链表结点个数为1和2的情况下，时间复杂度为O（n）。
```java
// 方法：查找链表的中间结点
public Node findMidNode(Node head) {

	if (head == null) {
		return null;
	}

	Node first = head;
	Node second = head;
	// 每次移动时，让second结点移动两位，first结点移动一位
	while (second != null && second.next != null) {
		first = first.next;
		second = second.next.next;
	}

	// 直到second结点移动到null时，此时first指针指向的位置就是中间结点的位置
	return first;
}
```

通过上述代码可以看出，当n为偶数时，得到的中间结点是第n/2+1个结点。比如链表有6个节点时，得到的是第4个节点。

# 19. Java 中如何合并两个有序单链表后依然有序？
举例：

>链表1： 1->2->3->4 链表2： 2->3->4->5 合并后： 1->2->2->3->3->4->4->5

**解题思路**

逐一比较链表1和链表2，类似于归并排序。尤其要注意两个链表都为空和其中一个为空的情况。只需要O(1)的空间。时间复杂度为O (max(len1,len2))

代码实现：
```java
public Node head;
public Node current;

class Node {
	int data; // 数据域
	Node next;// 指针域

	public Node(int data) {
		this.data = data;
	}
}
// 两个参数代表的是两个链表的头结点
public Node mergeLinkList(Node head1, Node head2) {

	if (head1 == null && head2 == null) { // 如果两个链表都为空
		return null;
	}
	if (head1 == null) {
		return head2;
	}
	if (head2 == null) {
		return head1;
	}

	Node head; // 新链表的头结点
	Node current; // current结点指向新链表
	// 一开始，我们让current结点指向head1和head2中较小的数据，得到head结点
	if (head1.data < head2.data) {
		head = head1;
		current = head1;
		head1 = head1.next;
	} else {
		head = head2;
		current = head2;
		head2 = head2.next;
	}

	while (head1 != null && head2 != null) {
		if (head1.data < head2.data) {
			current.next = head1; // 新链表中，current指针的下一个结点对应较小的那个数据
			current = current.next; // current指针下移
			head1 = head1.next;
		} else {
			current.next = head2;
			current = current.next;
			head2 = head2.next;
		}
	}

	// 合并剩余的元素
	if (head1 != null) { // 说明链表2遍历完了，是空的
		current.next = head1;
	}

	if (head2 != null) { // 说明链表1遍历完了，是空的
		current.next = head2;
	}

	return head;
}
```

测试代码：
```java
public static void main(String[] args) {
	LinkList list1 = new LinkList();
	LinkList list2 = new LinkList();
	// 向LinkList中添加数据
	for (int i = 0; i < 4; i++) {
		list1.add(i);
	}
	for (int i = 3; i < 8; i++) {
		list2.add(i);
	}
	LinkList list3 = new LinkList();
	list3.head = list3.mergeLinkList(list1.head, list2.head); // 将list1和list2合并，存放到list3中

	list3.print(list3.head);// 从head节点开始遍历输出
}
```

执行结果
```sh
0
1
2
3
3
4
5
6
7
```
# 20. Java 中如何实现单链表的反转？
举例

>链表： 1->2->3->4 反转之后： 4->2->2->1

解题思路

从头到尾遍历原链表，每遍历一个结点，将其摘下放在新链表的最前端。注意链表为空和只有一个结点的情况。时间复杂度为O（n）

代码实现
```java
import java.util.Stack;

public class LinkList {
	public Node head;
	public Node current;

	class Node {
		int data; // 数据域
		Node next;// 指针域

		public Node(int data) {
			this.data = data;
		}
	}

	// 方法：从尾到头打印单链表
	public void reversePrint(Node head) {

		if (head == null) {
			return;
		}

		Stack<Node> stack = new Stack<Node>(); // 新建一个栈
		Node current = head;

		// 将链表的所有结点压栈
		while (current != null) {
			stack.push(current); // 将当前结点压栈
			current = current.next;
		}

		// 将栈中的结点打印输出即可
		while (stack.size() > 0) {
			System.out.println(stack.pop().data); // 出栈操作
		}
	}
}

```
