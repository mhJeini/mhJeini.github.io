# 1. Java 中 while 和 do while 有什么区别？
   while是先执行判断条件，如果条见成立则继续循环，否则直接退出循环。

格式：
```java
初始化条件
while(循环条件){
    循环体
    迭代部分
}
```
实例代码如下：

```java
int i=1;
while(i <= 20){
if( i % 2 == 0){
    System.out.println(i);
}
```

do while是先执行一次循环然后在判断while后的条件，如果条件成立则继续循环，否则退出循环。

while和do while的唯一区别就是在条件一开始就不成立时do-while执行一次循环然后退出循环，而while一次循环都不执行就直接退出循环。

格式：

```java
do{
    循环体
    迭代部分
}while(循环条件);
```
初始化条件

实例代码如下：

```java
int i=1;
do{
    if( i%2 == 0){
        System.out.println(i);
    }
    i++;
}while(i <= 20 );
```
# 2. Java 中可以一次 catch 多个异常吗？
Java中可以一次catch多个异常，JDK1.7版本以后，可以通过以下代码实现：
```java
try {
    //.....
} catch ( IllegalArgumentException
 | SecurityException
 | IllegalAccessException
 |NoSuchFieldException exc) {
    //.....	 
}

```
# 3. Java 中 String.trim() 方法有什么作用？
Java中String.trim()方法的作用是返回一个字符串，并删除任何前导和尾随空格。

实例代码如下：
```java
public static void main(String arg[]) {
    String y = " 欢迎关注“Java记录”，微信公众号 ";
    String x = "欢迎关注“Java记录”，微信公众号";
    System.out.println(x.equals(y));
    y = y.trim();
    System.out.println(y.equals(x));
}

```
执行结果
```sh
false
true
```
# 4. Java 中 Log4j 日志都有哪些级别？
log4j定义了8个级别的log，除去OFF和ALL可以说分为6个级别。

优先级从高到低依次为:OFF、FATAL、ERROR、WARN、INFO、DEBUG、TRACE、ALL。

ALL 最低级别，用于打印所有日志记录。

TRACE 很低的日志级别，一般不会使用。

DEBUG 输出细粒度信息事件有助于调试应用程序，主要用于开发过程中打印一些运行信息。

INFO 消息在粗粒度级别上突出强调应用程序的运行过程，打印一些开发者关注的或者重要的信息，用于生产环境中输出程序运行的一些重要信息，但是不能滥用 避免打印过多的日志。

WARN 表示会出现潜在错误的情况，有些信息不是错误信息，但是用于给开发人员的一些提示。

ERROR 指出虽然发生错误事件 但仍然不影响系统的继续运行，打印错误和异常信息 如果不想输出太多的日志 可以使用这个级别

FATAL 指出每个严重的错误事件将会导致应用程序的退出，这个级别是重大错误可以直接停止程序。

OFF 最高等级，用于关闭所有日志记录。

# 5. Java 中 WEB-INF 目录有什么作用？
WEB-INF是Java的WEB应用的安全目录。

所谓安全是指客户端是无法直接访问的目录，必须通过服务端才可以可以访问的目录。如果需要在页面中直接访问其中的文件，必须通过web.xml文件对要访问的文件进行相应映射才能访问。

如果放在WEB-INF中，必须修改resources映射，例如：
```xml
<resources mapping="/index/**" location="/WEB-INF/jsp/index" />
```
参数含义：

mapping参数表示对应访问应用程序时的地址。

location参数表示对应应用程序中文件所在的位置。

#6. Java 中 hh:mm:ss 和 HH:mm:ss 有什么区别？
hh:mm:ss

按照12小时制的格式进行字符串格式化

如果时间处于00:00:00至12:59:59，则返回的字符串正常

如果时间处于13:00:00至23:59:59，则返回的字符串是实际时间-12小时后的值，也就是说比真实的时间少了12个小时。

HH:mm:ss

按照24小时制的格式进行字符串格式化

当时间为任意一个区间，则返回的字符串都是正常的。

实例代码：
```java
Date date = new Date();
SimpleDateFormat fromat = new SimpleDateFormat("hh:mm:ss");
System.out.println("hh:mm:ss 输出结果：" + fromat.format(date));
fromat = new SimpleDateFormat("HH:mm:ss");
System.out.println("HH:mm:ss 输出结果：" + fromat.format(date));

```
执行结果：

```sh
hh:mm:ss 输出结果：04:56:47
HH:mm:ss 输出结果：16:56:47
```

# 7. Java 中 Request 和 Response 对象都有哪些区别？
   Request和Response对象起到了服务器与客户机之间的信息传递作用。Request对象用于接收客户端浏览器提交的数据，而Response 对象的功能则是将服务器端的数据发送到客户端浏览器。

1、Request对象

QueryString：用以获取客户端附在url地址后的查询字符串中的信息。

```java
stra=Request.QueryString ["strUserld"]
```

Form：用以获取客户端在FORM表单中所输入的信息。（表单的method属性值需要为POST）

```java
stra=Request.Form["strUserld"]
```

Cookies：用以获取客户端的Cookie信息。

```java
stra=Request.Cookies["strUserld"]
```

ServerVariables：用以获取客户端发出的HTTP请求信息中的头信息及服务器端环境变量信息。

```java
stra=Request.ServerVariables["REMOTE_ADDR"]//返回客户端IP地址
```

ClientCertificate：用以获取客户端的身份验证信息

```java
stra=Request.ClientCertificate["VALIDFORM"]//对于要求安全验证的网站，返回有效起始日期。
```

2、Response对象

Response对象用于动态响应客户端请示，控制发送给用户的信息，并将动态生成响应。Response对象提供了一个数据集合cookie，它用于在客户端写入cookie值。若指定的cookie不存在，则创建它。若存在，则将自动进行更新。结果返回给客户端浏览器。

语法格式：Response.Cookies(CookieName)[(key)|.attribute]=value。这里的CookiesName是指定的Cookie的名称，如果指定了Key，则该Cookie就是一个字典，Attribute属性包括Domain，Expires，HasKeys，Path，Secure。

response的方法：

Write：向客户端发送浏览器能够处理的各种数据,包括:html代码,脚本程序等。

Redirect：response.redirect("url")的作用是在服务器端重定向于另一个网页。

End：用来终止脚本程序。

Clear：要说到Clear方法，就必须提到response的Buffer属性，Buffer属性用来设置服务器端是否将页面先输出到缓冲区。语法为：

```java
Response.Buffer=True/False
```

Flush：当Buffer的值为True时，Flush方法用于将缓冲区中的当前页面内容立刻输出到客户端。

# 8. 为什么 HashMap 负载因子是 0.75？
HashMap有两个参数影响其性能：初始容量和负载因子。

容量是哈希表中桶的数量，初始容量只是哈希表在创建时的容量。负载因子是哈希表在其容量自动扩容之前可以达到多满的一种度量。当哈希表中的条目数超出了负载因子与当前容量的乘积时，则要对该哈希表进行扩容、rehash操作（即重建内部数据结构），扩容后的哈希表将具有两倍的原容量。

通常，负载因子需要在时间和空间成本上寻求一种折衷。

负载因子过高，例如为1，虽然减少了空间开销，提高了空间利用率，但同时也增加了查询时间成本；

负载因子过低，例如0.5，虽然可以减少查询时间成本，但是空间利用率很低，同时提高了rehash操作的次数。

在设置初始容量时应该考虑到映射中所需的条目数及其负载因子，以便最大限度地减少rehash操作次数，所以，一般在使用HashMap时建议根据预估值设置初始容量，减少扩容操作。

选择0.75作为默认的负载因子，完全是时间和空间成本上寻求的一种折衷选择。

# 9. HashMap 为什么初始化值是 2 的指数幂？
1、奇数不行的解释很能被接受，在计算hash时确定落在数组的位置，计算方法是(n - 1) & hash，奇数n-1为偶数，偶数2进制的结尾都是0，经过&运算末尾都是0，会增加hash冲突。

2、为啥要是2的幂，不能是2的倍数么，比如6，10？

1）hashmap结构是数组，每个数组里面的结构是node（链表或红黑树），正常情况下，如果你想放数据到不同的位置，肯定会想到取余数确定放在那个数据里， 计算公式：

hash % n，这个是十进制计算。在计算机中， (n - 1) & hash，当n为2次幂时，会满足一个公式：(n - 1) & hash = hash % n，计算更加高效。

2）只有是2的幂数的数字经过n-1之后，二进制肯定是...11111111这样的格式，这种格式计算的位置的时候，完全是由产生的hash值类决定，而不受n-1 影响。你可能会想，

受影响不是更好么，又计算了一下 ，hash冲突可能更低了，这里要考虑到扩容了，2的幂次方*2，在二进制中比如4和8，代表2的2次方和3次方，他们的2进制结构相 似,比如

4和8 00000100 0000 1000 只是高位向前移了一位，这样扩容的时候，只需要判断高位hash,移动到之前位置的倍数就可以了，免去了重新计算位置的运算。

# 10. Java 中标识符有哪些命名规则？
标识符用作给变量、类和方法命名。

标识符以由大小写字母、数字、下划线(_)和美元符号($)组成，但是不能以数字开头。

大小写敏感

不能与Java语言的关键字重名

不能和Java类库的类名重名

不能有空格、@、#、+、-、/ 等符号

长度无限制

应该使用有意义的名称，达到见名知意的目的

不可以是true和false

# 11. Java 类命名规则是什么？
类名和接口名：通常定义为由具有含义的单词组成，所有单词的首字母大写。

长度基本上没有限制。

但是不能使用Java关键字（例如，public或 class等）作为类名。

# 12. Java 方法命名规则是什么？
方法名：通常是具有含义的单词组成，第一个单词首字母小写，其他单词的首字母都大写。

# 13. Java 变量命名规则是什么？
变量名：成员变量和方法相同，局部变量全部使用小写。

# 14. Java 常量命名规则是什么？
常量名：全部使用大写，最好使用下划线分割单词。

# 15. Java 中如何定义一个常量？
Java中定义常量的最常用方法之一是通过整数，其中整数变量是静态的。 
```java
public static int MONDAY = 0;
public static int TUESDAY = 1;
public static int WEDNESDAY = 2;
public static int THURSDAY = 3;
```
# 16. Java 中常量有哪几种类型？
    final修饰符用于声明属性，方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。

1、按照数据类型进行分类

1）基本数据类型

>整数类型 byte、short、int、long 234 小数类型 float、double 12.5 字符类型 char 'A' 布尔类型 boolean true false

2）引用数据类型

>空常量null，代表不指向任何的地址 数组 类，字符串常量String"字符串内容" 接口 枚举 注解

2、可以从表现形式上进行分类

字面值常量：看到这个常量后，就知道其值为多少。

符号常量：是用符号进行表示，看到这个常量后，能够知道其表示什么意思，但是不能知道其值为多少。

# 17. Java 中什么是重写（Override）？
重写(Override)：是存在子父之间的关系，子类里定义的方法与父类里定义的方法具有相同的方法名以及相同的返回值和参数类型。

重写规则：

1、方法名形参列表相同；

2、访问权限，子类大于等于父类；

3、返回值类型和声明异常类型子类小于父类；

实例：
```java
public class OverrideDemo {
	public static void main(String args[]) {
		GZ jx = new JingXuan(); // 公众号对象
		jx.coverage(); // 执行精选类的方法
	}
}

class GZ {
	public void coverage() {
		System.out.println("关注公众号Java记录，免费3000+道面试题。");
	}
}

class JingXuan extends GZ {
	public void coverage() {
		super.coverage(); // 应用super类的方法
		System.out.println("Java记录公众号，可以学习简历编写，了解职业规划。");
	}
}
```
编译执行结果
```sh
关注公众号Java记录，免费3000+道面试题。
Java记录公众号，可以学习简历编写，了解职业规划。
```
# 18. Java 中什么是重载（Overload）？
    重载(Overload)：是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。

重载规则：

1、被重载的方法必须改变参数列表(参数个数或类型不一样)；

2、被重载的方法可以改变返回类型；

3、被重载的方法可以改变访问修饰符；

4、被重载的方法可以声明新的或更广的检查异常；

5、方法能够在同一个类中或者在一个子类中被重载；

6、无法以返回值类型作为重载函数的区分标准；

实例：
```java
public class OverloadDemo {
	
	// 方法1 无参
	public int test() {
		System.out.println("公众号Java记录，3000+面试题!");
		return 1;
	}

	// 方法2 单一参数
	public void test(int a) {
		System.out.println("公众号Java记录，3000+面试题!!");
	}

	// 方法3 下面两个参数类型顺序不同
	public String test(long a, String s) {
		System.out.println(s);
		return "方法3返回->" + s;
	}

	//方法4
	public String test(String s, int a) {
		System.out.println(s);
		return "方法4返回->" + s;
	}

	public static void main(String[] args) {
		OverloadDemo od = new OverloadDemo();
		System.out.println(od.test());//方法1
		od.test(2);//方法2
		System.err.println(od.test(3l, "公众号Java记录，3000+面试题!"));//方法3
		System.err.println(od.test("公众号Java记录，3000+面试题!!!", 4));//方法4
	}
}
```
执行结果
```sh
公众号Java记录，3000+面试题!
1
公众号Java记录，3000+面试题!!
公众号Java记录，3000+面试题!
方法3返回->公众号Java记录，3000+面试题!
方法4返回->公众号Java记录，3000+面试题!!!
公众号Java记录，3000+面试题!!!
```
# 19. Static Nested Class 和 Inner Class 有什么区别？

Static Nested Class是被声明为静态（static）的内部类，它可以不依赖于外部类实例被实例化。而通常的内部类需要在外部类实例化后才能实例化。

# 20. Java 中常见都有哪些 RuntimeException？
NullPointerException：空指针引用异常。

ClassCastException：类型强制转换异常。

IllegalArgumentException：传递非法参数异常。

ArithmeticException：算术运算异常。

ArrayStoreException：向数组中存放与声明类型不兼容对象异常。

IndexOutOfBoundsException：下标越界异常。

NegativeArraySizeException：创建一个大小为负数的数组错误异常。

NumberFormatException：数字格式异常。

SecurityException：安全异常。

UnsupportedOperationException：不支持的操作异常。
