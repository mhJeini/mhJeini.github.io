# 1. a.hashCode() 有什么用？与 a.equals(b) 有什么关系？
hashCode()方法是为对象产生整型的hash值，用作对象的唯一标识。它常用于基于hash的集合类，如Hashtable、HashMap等等。根据Java规范，使用equals()方法来判断两个相等的对象，必须具有相同的hashcode。

将对象放入到集合中时，首先判断要放入对象的hashcode是否已经在集合中存在，不存在则直接放入集合。如果hashcode相等，然后通过equals()方法判断要放入对象与集合中的任意对象是否相等：如果equals()判断不相等，直接将该元素放入集合中，否则不放入。

# 2. Java 中 Files 类常用方法都有哪些？
Files.exists()：检测文件路径是否存在。

Files.createFile()：创建文件。

Files.createDirectory()：创建文件夹。

Files.delete()：删除一个文件或目录。

Files.copy()：复制文件。

Files.move()：移动文件。

Files.size()：查看文件个数。

Files.read()：读取文件。

Files.write()：写入文件。

# 3. 抽象类能使用 final 修饰吗？
abstract类为抽象类，即该类只关心子类具有的功能，而不是功能的具体实现。

抽象类不能实例化，其作用是为了被其他类继承，因此final关键字不能修饰抽象类。

抽象类是被用于继承的，final修饰代表不可修改、不可继承的。

# 4. switch 中能否使用 String 作为参数？
语法格式：switch(expr)

expr参数可以是一个枚举常量（枚举类由整型或字符类型实现）或一个整数表达式，甚至一个字符串（JDK1.7及以上版本支持String）。

如果是整数表达式可以是基本类型int或其包装类Integer。由于byte、short和char类型都可以隐式转换为int（之前篇幅已阐述过隐式的概念，此处不再过多说明，可关注微信公众号“Java精选”，精品文章每日更新），因此这些类型以及它们对应的包装类都可以作为expr参数。

需要注意的是因为long、float、double等类型都不能够隐式转换为int类型，所以它们不能作为expr参数。如果一定要使用它们，必须将其强制转换为int类型才可以。

在JDK1.7之前版本只能支持byte、short、char、int或者其对应的包装类以及Enum类型；从JDK1.7及以上版本开始支持String类型。

从本质来讲，switch对字符串的支持，其实就是int类型值得匹配。其原理是通过对case后面的String对象调用hashCode()方法，得到一个int类型的hash值，然后用这个hash值来唯一标识这个case。如果可以匹配，接着会调用字符串的String.equals()方法进行判断，若是没有匹配成功，说明不存在。

String变量不能为null，同时switch中case子句中使用的字符串也不能为null。目前为止，switch不支持long类型。

# 5. Java 中字符型常量和字符串常量有什么区别？
形式区别

字符常量是用单引号引起的一个字符，而字符串常量是用双引号引起的若干个字符。

含义区别

字符常量相当于一个整形值（ASCII值），可以用于表达式运算，而字符串常量代表一个地址值，表示该字符串在内存中存放位置。

占内存大小

字符常量只占2个字节，而字符串常量占若干个字节，由字符串中的字符个数决定。

# 6. Java 中 throw 和 throws 有什么区别？
throws是用来声明一个方法可能抛出的所有异常信息，throws是将异常声明但是不处理，而是将异常往上传，谁调用就交由谁处理；而throw则是指抛出的一个具体的异常类型。

throws表示抛出异常，由该方法的调用者来处理，在方法声明后面是异常类名，可以是多个异常类名；而throwhrow表示抛出异常，由该方法体内的语句来处理，用在方法体内，其后面是异常类对象名，且只能抛出一个异常对象名。
```java
public static void javaJingXuan() throws Exception,NullPointerException {
	throw new Exception();
}
```
# 7. Java 中如何将字符串反转，列举几种方式？
1）使用StringBuffer或StringBuilder类中的reverse()方法
```java
public static String reverseBuffer(String str) {
return new StringBuffer(str).reverse().toString();
}

public static String reverseBuilder(String str) {
return new StringBuilder(str).reverse().toString();
}
```
2）使用String中toCharArray()方法，首先将字符串转化为char类型数组，然后将各个字符进行重新拼接。
```java
public static String reverseArray(String str) {
	char[] chars = str.toCharArray();
	String reverse = "";
	for (int i = chars.length - 1; i >= 0; i--) {
		reverse += chars[i];
	}
	return reverse;
}
```
3）使用String中CharAt()方法取出字符串中的各个字符，。
```java
public static String reverseChar(String str) {
	String reverse = "";
	int length = str.length();
	for (int i = 0; i < length; i++) {
		reverse = str.charAt(i) + reverse;
	}
	return reverse;
}
```

# 8. Java 中什么是隐式的类型转换？
隐式的类型转换又称为自动类型转换，就是一个类型赋值给另一个类型，没有显式的告诉编译器发生了转，但是需要注意的是并不是所有的类型都支持隐式的类型转换。

隐式的类型转换必须满足转换前的数据类型的位数要低于转换后的数据类型，例如short数据类型的位数为16位，就可以自动转换位数为32的int类型，同样float数据类型的位数为32，可以自动转换为64位的double类型。
```shell
short a = 1000;
int b = a;
long c = b;
```

数据类型转换必须满足如下规则：

1）不能对boolean类型进行类型转换。

2）不能把对象类型转换成不相关类的对象。

3）在把容量大的类型转换为容量小的类型时必须使用强制类型转换。

4）转换过程中不能导致溢出或损失精度。

# 9. Java 中 i++ 和 ++i 有什么区别？
概念区别

i++表示先引用i变量的数值然后再对i进行加1的操作，而++i是先对i变量进行加1的操作，然后再引用i变量的数值。

表达式形式区别

i++将“++”放在变量的后面，++i将“++”放在变量的前面。

预算优先级区别

i++中的“++”运算符的优先级比++i中“++”运算符的优先级高。
```java
int i = 1,j = 1,h = 1;
j = i++;
System.out.println(j);

i = 1;
h = ++i;
System.out.println(h);
```

执行j = i++，先将i变量的值1赋值给j，此时j=1，i才等于2；执行h = ++i，会先将i变量的值1加1变成2，然后赋值给h，此时h的值为2。

# 10. Java 中 Integer a= 128 与 Integer b = 128 相等吗？
Java中Integer a=128与Integer b=128是不相等。
```java
Integer a= 127;
Integer b= 127;
Integer c= 128;
Integer d= 128;
System.out.println(a==b); //返回true;
System.out.println(c==d); //返回false;
System.out.println(a.equals(b)); //返回true;
System.out.println(c.equals(d)); //返回true;
```
引用类型中==是比较的对象内存地址；而基本数据类型中==是比较的值。

IntegerCache.low默认是-128；IntegerCache.high默认是127。

如果Integer类型的值是-128到127之间，那么自动装箱时不会new新的Integer对象，而是直接引用常量池中的Integer对象，因此超过范围c==d的结果是false。

# 11. 两个对象 hashCode() 相同，equals()判断一定为 true 吗？
两个对象hashCode()相同，使用equals()方法判断不一定为true。

两个对象hashCode()相同，只能说明哈希值相同，不代表这两个键值对相等。
```java
String str1 = "通话";
String str2 = "重地";
// str1: 1179395 | str2: 1179395
System.out.println(String.format("str1: %d | str2: %d",str1.hashCode(),str2.hashCode()));
// false
System.out.println(str1.equals(str2));
```

# 12. Java 中如何生成随机数？
随机数在日常开发中是比较常用的，也是开发人员必须要掌握的。Java中产生随机数列举两种方式（其他方式就不一一列举了）。

方式一：new Random()

借助java.util.Random类来产生一个随机数，这也是最常用的一种。其中构造方法有两个分别是Random()和Random(long seed)。

Random()方法是以当前时间为默认种子来产生随机数，Random(long seed)方法是以指定的种子值来产生随机数。产生随机数后可以根据不同的方式产生不同类型的数。

种子就是产生随机数的第一次使用值，其机制是通过一个方法，将这个种子的值转化为随机数空间中的某一个点上且产生的随机数均匀的散布在空间中。以后产生的随机数都与前一个随机数有关。实例代码如下：
```java
public static void main(String[] args){
	Random r = new Random(0);
		for(int i=0 ; i<5 ;  i++) {	
			int ran1 = r.nextInt(100);
			System.out.println(ran1);
		}
}

```
执行结果如下：
```java
60
48
29
47
15
```
执行编译后输出上述结果，若是采用Random r = new Random()，那么产生的随机数也不相同，这是因为种子不一致的结果。

方式二：Math.random()

此方法返回的数值是[0.0,1.0)的double型数值，由于double类数的精度很高，可以在一定程度下看做随机数，借助int类型来进行类型转换就可以得到整数随机数，例如获取（1-100）的整数值，代码如下：
```java
public static void main(String[] args){    
    int max = 100,min = 1;
    int ran2 = (int) (Math.random()*(max-min)+min); 
    System.out.println(ran2);
}
```

# 13. Java 中常量和变量有哪些区别？
常量在定义时要使用final关键字修饰。

常量定义实例代码如下：
```java
final String jxDetail="关注“Java 精选”微信公众号";
```
变量的值可以改变而常量的值在初始化以后是无法改变的。

JAVA中可以通过三个元素来描述变量：变量类型、变量名以及变量值。

变量定义实例代码如下：
```java
String jxDetail="关注“Java 精选”微信公众号";
```
其中String具有不可变性，重新赋值后会生成新的String对象，jxDetail变量名实际是指向对象地址的引用，“关注“Java 精选”微信公众号”为具体的值。

# 14. Java 中变量命名有哪些规则？
Camel标记法

首字母是小写的，接下来的单词都以大写字母开头。

Pascal标记法

首字母是大写的，接下来的单词都以大写字母开头。

匈牙利标记法

在以Pascal标记法的变量前附加小写序列说明该变量的类型。

基本原则是：变量名=属性+类型+对象描述，其中每一对象的名称都要求有明确含义，可以取对象名字全称或名字的一部分。命名要基于容易记忆容易理解的原则，并尽量保证名字的连贯性。

Java中一般使用匈牙利标记法，基本结构为scope_typeVariableName。

使用1-3字符前缀来表示数据类型，3个字符的前缀必须小写，前缀后面是由表意性强的一个单词或多个单词组成的名字，而且每个单词的首写字母大写，其它字母小写，这样保证了对变量名能够进行正确的断句。

例如定义一个整形变量，用来记录文档数量：intDocCount，其中int表明数据类型，后面为表意的英文名，每个单词首字母大写。

# 15. Java 中 this 和 super 有哪些用法区别？
### **this**

this表示一个对象的引用，它指向正在执行方法的对象。特别的是在构造方法中，通过this关键字调用其他构造方法时，必须放在第一行，否则编译器会报错。并且在构造方法中，只能通过this调用一次其他构造方法。

this是自身的一个对象，代表对象本身，大致分为3种用法如下：

1、普通直接引用当前对象本身；

2、形参和成员名重名，用this来区分；

3、引用构造方法 ，this（参数），应该为构造函数中的第一条语句，调用的是本类中另外一种形式的构造方法。

### **super**

super表示指向父类的引用，如果构造方法没有显示地调用父类的构造方法，那么编译器会自动加上一个默认的super()方法调用。如果父类由没有默认的无参构造方法，编译器就会报错，super()语句必须是构造方法的第一个子句。

定义子类的一个对象时，会先调用子类的构造函数，然后在调用父类的构造函数，如果父类函数足够多的话，会一直调用到最终的父类构造函数，函数调用时会使用栈空间，所以按照入栈的顺序，最先进入的是子类的构造函数，然后才是邻近的父类构造函数，最后再栈顶的是最终的父类构造函数，构造函数执行是则按照从栈顶到栈底的顺序依次执行。

super大致分为3种用法如下：

1、普通的直接引用，与this类似，只不过它是父类对象，可以通过它调用父类成员；

2、子类中的成员变量或方法与父类中的成员变量或方法同名，可以使用super区分；

3、引用构造方法，super（参数）：调用父类中的某一个构造方法（应该为构造方法中的第一条语句）。

# 16. Java 中 static 可以修饰局部变量吗？
Java中static关键字不可以修饰局部变量。

static是用于修饰成员变量和成员方法的，它随着类的加载而加载，随着类的消失而消失，存在于方法区的静态区，被其修饰的成员能被类的所有对象共享，即作用域为全局；而局部变量存在于栈，用完后就会释放。作用域为局部代码块。

# 17. Java 中 short s1=1; s1=s1+1; 有错吗？
Java中short s1=1; s1=s1+1; 是错误的。

Java中short s1=1; s1=s1+1; 报错如下：

Type mismatch: cannot convert from int to short
分析：值1是int类型，s1是short类型，而“+”是算术运算符，通过“+”运算后s1自动转为int类型，如果把int类型的结果赋给byte、short类型的结果，必须加上强制声明。

# 18. Java 中 short s1=1; s1+=1; 有错吗？
Java中short s1=1; s1+=1;是正确的。

“+=”是赋值运算符，不牵涉与其它类型的数字计算，也不会转成int类型的，因此不会报错。

注意的是表达式x += i不是表达式x = x + i的简写方式。

表达式x = x + i使用的是简单赋值操作符=，而表达式x += i使用的是复合赋值操作符，复合赋值表达式会自动地将所执行计算的结果转型为其左侧变量的类型。

# 19. Java 中 数组有 length() 方法吗？ String 呢？
Java中数组是没有length()方法的，但是有length这个属性，用于计算数组的长度。

Java中String类型是有length()方法的，用于计算字符串的长度。
```java
/**
* Returns the length of this string.
* The length is equal to the number of <a href="Character.html#unicode">Unicode
* code units</a> in the string.
*
* @return  the length of the sequence of characters represented by this
*          object.
*/
public int length() {
    return value.length;
}
```
# 20. Java 中 Error 和 Exception都有哪些区别？
Error类和Exception类都继承自Throwable类。

Error类一般是指与虚拟机相关的问题，如系统崩溃、虚拟机错误、内存空间不足、方法调用栈溢等。对于这类错误的导致的应用程序中断，仅靠程序本身无法恢复和预防，遇到这样的错误，一般是终止程序。

Exception类表示程序可以处理的异常，可以通过捕获异常从而可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常。

Exception类又分为运行时异常（RuntimeException）和受检查的异常（CheckedException），运行时异常;ArithmaticException、IllegalArgumentException，编译能通过，但是一运行就终止了，程序不会处理运行时异常，出现这类异常，程序会终止。而受检查的异常，要么用try。。。catch捕获，要么用throws字句声明抛出，交给它的父类处理，否则编译不会通过。

1）Exception（异常）是应用程序中可能的可预测、可恢复问题。一般大多数异常表示中度到轻度的问题。异常一般是在特定环境下产生的，通常出现在代码的特定方法和操作中。在EchoInput类中，当试图调用readLine方法时，可能出现IOException异常。

Exception类有一个重要的子类RuntimeException。RuntimeException类及其子类表示“JVM常用操作”引发的错误。例如，若试图使用空值对象引用、除数为零或数组越界，则分别引发运行时异常（NullPointerException、ArithmeticException）和ArrayIndexOutOfBoundException。

2）Error（错误）表示运行应用程序中较严重问题。大多数错误与代码编写者执行的操作无关，而表示代码运行时JVM（Java虚拟机）出现的问题。例如，当JVM不再有继续执行操作所需的内存资源时，将出现OutOfMemoryError。