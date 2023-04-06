# 1. 什么是不可变对象？有什么好处？
不可变对象是指对象一旦被创建，状态就不能再改变，任何修改都会创建一个新的对象。

比如String、Integer及其它包装类。

不可变对象最大的好处是线程安全。

# 2. 静态变量和实例变量有什么区别？
静态变量：独立存在的变量，只是位置放在某个类下，可以直接类名加点调用静态变量名使用。并且是项目或程序一启动运行到该类时就直接常驻内存。不需要初始化类再调用该变量。用关键字static声明。静态方法也是同样，可以直接调用。

实例变量：相当于该类的属性，需要初始化这个类后才可以调用。如果这个类未被再次使用，垃圾回收器回收后这个实例也将不存在了，下次再使用时还需要初始化这个类才可以再次调用。

1）存储区域不同：静态变量存储在方法区属于类所有，实例变量存储在堆当中；

2）静态变量与类相关，普通变量则与实例相关；

3）内存分配方式不同。

4）生命周期不同。

需要注意的是从静态变量在jdk7以后和字符串常量池一起存储在了堆中，JDK1.8开始用于实现方法区的PermSpace被MetaSpace取代。

# 3. Object 类都有哪些公共方法？
1）equals(obj)；

判断其他对象是否“等于”此对象。

2）toString();

表示返回对象的字符串。通常，ToString方法返回一个“以文本方式表示”此对象的字符串。结果应该是一个简洁但信息丰富的表达，很容易让人阅读。建议所有子类都重写此方法。

**3）getClass(); ** 返回此对象运行时的类型。

4）wait();

表示当前线程进入等待状态。

5）finalize();

用于释放资源。

6）notify();

唤醒在该对象上等待的某个线程。

7）notifyAll();

唤醒在该对象上等待的所有线程。

8）hashCode();

返回对象的哈希代码值。用于哈希查找，可以减少在查找中使用equals的次数，重写equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到。

9）clone();

实现对象的浅复制，实现Cloneable接口才可以调用这个方法，否则抛出CloneNotSupportedException异常。

# 4. Java 创建对象有哪几种方式？

| 关键字      | 作用                     |
| ----------------------------------- | ------------------------ |
| 创建对象	| 构造方法说明 |
| 使用new关键字	| 调用构造方法 |
| 使用Class类的newInstance方法	| 调用构造方法 |
| 使用Constructor类的newInstance方法	| 调用构造方法 |
| 使用clone方法	| 没有调用构造方法 |
| 使用反序列化	| 没有调用构造方法 |

# 5. a==b 与 a.equals(b) 有什么区别？
假设a和b都是对象，则a==b是比较两个对象的引用，只有当a和b指向的是堆中的同一个对象才会返回 true，而a.equals(b)是进行逻辑比较，当内容相同时，返回true，所以通常需要重写该方法来提供逻辑一致性的比较。

多数情况下需要重写这个方法，如String类重写equals()用于比较两个不同对象，但是包含的字母相同的比较：

```java
public boolean equals(Object obj) {
	if (this == obj) {// 相同对象直接返回true
		return true;
	}
	if (obj instanceof String) {
		String anotherString = (String)obj;
		int n = value.length;
		if (n == anotherString.value.length) {
			char v1[] = value;
			char v2[] = anotherString.value;
			int i = 0;
			while (n-- != 0) {
				if (v1[i] != v2[i])
					return false;
				i++;
			}
			return true;
		}
	}
	return false;
}
```

# 6. Object 中 equals() 和 hashcode() 有什么联系？
Java的基类Object提供了一些方法，其中equals()方法用于判断两个对象的地址是否相等（可以理解成是否是同一个对象），地址相等则认为是对象相等，hashCode()方法用于计算对象的哈希码。equals()和hashCode()都不是final方法，都可以被重写（overwrite）。

hashCode()方法是为对象产生整型的hash值，用作对象的唯一标识。

hashCode()方法常用于基于hash的集合类，如Hashtable、HashMap等，根据Java规范使用equal()方法来判断两个相等的对象，必须具有相同的hashcode。

将对象放入到集合中时，首先要判断放入对象的hashcode是否已经存在，不存在则直接放入集合。

如果hashcode相等，然后通过equal()方法判断要放入对象与集合中的其他对象是否相等，使用equal()判断不相等，则直接将该元素放入集合中，反之不放入集合中。

# 7. hashcode() 中可以使用随机数字吗？
hashcode()中不可以使用随机数字，这是因为对象的hashcode值必须是相同的。

# 8. Java 中 & 和 && 有什么区别？
&是位操作

&&是逻辑运算符

逻辑运算符具有短路特性，而&不具备短路特性。

来看一下代码执行结果：
```java
public class Test{

    static String name;
 
    public static void main(String[] args){
        if(name!=null & name.equals("")){
            System.out.println("ok");
        }else{
            System.out.println("error");
        }
    }
}
```
执行结果：
```java
Exception in thread "main" java.lang.NullPointerException
	at com.jingxuan.JingXuanApplication.main(JingXuanApplication.java:25)
```
上述代码执行时抛出空指针异常，若果&替换成&&，则输出日志是error。

# 9. 一个 .java 类文件中可以有多少个非内部类？
一个.java类文件中只能出现一个public公共类，但是可以有多个default修饰的类。如果存在两个public修饰的类时，会报如下错误：
```java
The public type Test must be defined in its own file
```
# 10. Java 中如何正确退出多层嵌套循环？
1）使用lable标签和break方式；

lable是跳出循环标签。

break lable;是跳出循环语句。

当执行跳出循环语句时会跳出循环标签下方循环的末尾后面。
```java
lable:
for(int i=0;i<3;i++){
	for(int j=0;j<3;j++){
		System.out.println(i);
		if(i == 2) {
			break lable;
		}
		
	}
}
```
上述代码在执行过程中，当i=2时，执行跳出循环语句，控制台只输出i=0和i=1的结果，执行继续for循环后面的代码。
```shell
0
0
0
1
1
1
2
执行for后面的程序代码
```
2）通过在外层循环中添加标识符，比如定义布尔类型bo = false，当bo=true跳出循环语句。

# 11. 浅拷贝和深拷贝有什么区别？
浅拷贝是指被复制对象的所有变量都含有与原来的对象相同的值，而所有的对其他对象的引用仍然指向原来的对象。换言之，浅拷贝仅仅复制所考虑的对象，而不复制它所引用的对象。

深拷贝是指被复制对象的所有变量都含有与原来的对象相同的值，而那些引用其他对象的变量将指向被复制过的新对象，并且不再是原有的那些被引用的对象。换言之，深拷贝把要复制的对象所引用的对象都复制了一遍。

# 12. Java 中 final关键字有哪些用法？
Java代码中被final修饰的类不可以被继承。

Java代码中被final修饰的方法不可以被重写。

Java代码中被final修饰的变量不可以被改变，如果修饰引用类型，那么表示引用类型不可变，引用类型指向的内容可变。

Java代码中被final修饰的方法，JVM会尝试将其内联，以提高运行效率。

Java代码中被final修饰的常量，在编译阶段会存入常量池中。

# 13. String s = new String("abc"); 创建了几个String对象？
String s = new String("abc"); 创建了2个String对象。

一个是字符串字面常数，在字符串常量池中。

一个是new出来的字符串对象，在堆中。

# 14. String 和 StringBuffer 有什么区别？
String是不可变对象，每次对String类型进行操作都等同于产生了一个新的String对象，然后指向新的String对象。因此尽量避免对String进行大量拼接操作，否则会产生很多临时对象，导致GC开始工作，影响系统性能。

StringBuffer是对象本身操作，而不是产生新的对象。因此在有大量拼接的情况下，建议使用StringBuffer。

String是线程安全的，而StringBuffer也是线程安全的。

需要注意是Java从JDK5开始，在编译期间进行了优化。如果是无变量的字符串拼接时，那么在编译期间值都已经确定了的话，javac工具会直接把它编译成一个字符常量。比如：
```java
String str = "关注微信公众号" + "“Java精选”" + "，面试经验、专业规划、技术文章等各类精品Java文章分享！";
```
在编译期间会直接被编译成如下：

```java
String str = "关注微信公众号“Java精选”，面试经验、专业规划、技术文章等各类精品Java文章分享！";
```
# 15. Java 中 3*0.1 == 0.3 返回值是什么？
3*0.1==0.3返回值是false

这是由于在计算机中浮点数的表示是误差的。所以一般情况下不进行两个浮点数是否相同的比较。而是比较两个浮点数的差点绝对值，是否小于一个很小的正数。如果条件满足，就认为这两个浮点数是相同的。
```java
System.out.println(3*0.1 == 0.3);
System.out.println(3*0.1);

System.out.println(4*0.1==0.4);
System.out.println(4*0.1);
```
执行结果如下：
```sh
false
0.30000000000000004
true
0.4
```
# 16. a=a+b 和 a+=b 有什么区别吗？
+=操作符会进行隐式自动类型转换，a+=b隐式的将相加操作结果类型强制转换为持有结果的类型，而a=a+b则不会自动进行类型转换。
```java
byte a = 127;
byte b = 127;
a = a + b;
a += b;
```
a = a + b; 编译报错：
```java
Type mismatch: cannot convert from int to byte
```
# 17. Java 中 int a[] 和 int []a 有什么区别？
采用int a[]这种写法是为了沿袭C、C++的写法。

Java中为了说明所有东西都是对象常采用int[] a写法。

# 18. Java 中 Math. round(-1.5) 等于多少？
Math.round(-1.5)的返回值是-1。
```java
public class Test {
	public static void main(String[] args){
		System.out.println(Math.round(1.3));   //1
		System.out.println(Math.round(1.4));   //1
		System.out.println(Math.round(1.5));   //2
		System.out.println(Math.round(1.6));   //2
		System.out.println(Math.round(1.7));   //2
		System.out.println(Math.round(-1.3));  //-1
		System.out.println(Math.round(-1.4));  //-1
		System.out.println(Math.round(-1.5));  //-1
		System.out.println(Math.round(-1.6));  //-2
		System.out.println(Math.round(-1.7));  //-2
	}
}
```
四舍五入的原理上参数加0.5，然后做向下取整。也可以按照如下方式：

1、小数点后第一位小于5，运算结果为参数整数部分。 2、小数点后第一位大于5，运算结果为参数整数部分绝对值+1，符号（即正负）不变。 3、小数点后第一位等于5，正数运算结果为整数部分+1，负数运算结果为整数部分。

# 19. String 类的常用方法都有哪些？
String 类型常用方法有很多，见表格内容：

| 返回类型 |	方法名 |	功能说明 |
| ----- | ------------------------ | --------------------------------|
| int	    | length()	                | 得到字符串的字符个数 | 
| byte[]	| getByte()	                | 将字符串转换成字节数组 | 
| char[]	| toCharArray()	            | 将字符串转换成字符数组 | 
| String	| split(String)	            | 将字符串按照指定内容劈开 |
| boolean	| equals()	                | 判断两个字符串的内容是否相同，可以重新equals |
| boolean	| equalsIgnoreCase(String)	| 忽略太小写比较两个字符串的内容是否相同 |
| boolean	| contains(String)	        | 判断字符串里面是否包含指定的内容 |
| boolean	| startsWith(String)	    | 判断字符串是否以指定的内容开头 |
| boolean	| endsWith(String)	        | 判断字符串是否以指定的内容结尾 |
| String	| toUpperCase()	            | 将字符串全部转换成大写 |
| String	| toLowerCase()	            | 将字符串全部转换成小写 |
| String	| replace(String,String)	 | 将某个内容全部替换成指定内容 |
| String	| replaceAll(String,String)	 | 将某个内容全部替换成指定内容，支持正则 |
| String	| repalceFirst(String,String)	 | 将第一次出现的某个内容替换成指定的内容 |
| String	| substring(int)	        | 从指定下标开始一直截取到字符串的最后 |
| String	| substring(int,int)	    | 从下标x截取到下标y-1对应的元素 |
| String	| trim()	                | 去除字符串的前后空格 |
| char	    | charAt(int)	            | 得到指定下标位置对应的字符 |
| int	    | indexOf(String)	        | 得到指定内容第一次出现的下标 |
| int	    | lastIndexOf(String)	    | 得到指定内容最后一次出现的下标 |

还包括valueOf()方法，由基本数据型态转换成String：String类别中已经提供了将基本数据型态转换成String的static方法 ，也就是String.valueOf()这个参数多载的方法，有以下几种：

1）String.valueOf(boolean b): 将boolean变量b转换成字符串

2）String.valueOf(char c): 将char变量c转换成字符串

3）String.valueOf(char[] data): 将char数组data转换成字符串

4）String.valueOf(char[] data, int offset, int count): 将char数组data中由data[offset]开始取count个元素转换成字符串

5）String.valueOf(double d): 将double变量d转换成字符串

6）String.valueOf(float f): 将float变量f转换成字符串

7）String.valueOf(int i): 将int变量i转换成字符串

# 20. Java 中 IO 流有哪几种？
IO流从功能角度可以分为输入流（input）、输出流（output）。

IO流从类型角度可以分为字节流和字符流。

字符流和字节流是根据处理数据的不同来区分的。字节流按照8位传输，字节流是最基本的，所有文件的储存是都是字节（byte）的储存，在磁盘上保留的并不是文件的字符而是先把字符编码成字节，再储存这些字节到磁盘。

字节流按8位传输以字节为单位输入输出数据，字符流按16位传输以字符为单位输入输出数据。

字节流可用于任何类型的对象，包括二进制对象，而字符流只能处理字符或者字符串。

字节流提供了处理任何类型的IO操作的功能，但它不能直接处理Unicode字符，而字符流就可以。

字符流处理的单元为2个字节的Unicode字符，分别操作字符、字符数组或字符串，而字节流处理单元为1个字节， 操作字节和字节数组。因此字符流是由Java虚拟机将字节转化为2个字节的Unicode字符为单位的字符而成的，对多国语言支持性比较好。

字节流和字符流分别由四个抽象类来表示（每种流包括输入和输出两种所以一共四个）:InputStream、OutputStream、Reader、Writer。