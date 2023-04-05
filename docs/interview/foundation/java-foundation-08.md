# 1. Java 反射有什么作用？
反射机制是在运行时，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意个对象，都能够调用它的任意一个方法。在java中，只要给定类的名字，就可以通过反射机制来获得类的所有信息。

这种动态获取的信息以及动态调用对象的方法的功能称为Java语言的反射机制。

# 2. Java 中反射机制有什么优缺点？
优点：

1）能够运行时动态获取类的实例，提高灵活性；

2）与动态编译结合

缺点：

1）使用反射性能较低，需要解析字节码，将内存中的对象进行解析。

解决方案：

1、通过setAccessible(true)关闭JDK的安全检查来提升反射速度；

2、多次创建一个类的实例时，有缓存会快很多；

3、ReflflectASM工具类，通过字节码生成的方式加快反射速度；

4、相对不安全，破坏了封装性（因为通过反射可以获得私有方法和属性）。

# 3. 如何使用 Java 的反射？
1、通过一个全限类名创建一个对象

1）Class.forName(“全限类名”); 例如：com.mysql.jdbc.Driver Driver类已经被加载到 jvm中，并且完成了类的初始化工作就行了

2）类名.class; 获取Class<？> clz 对象

3）对象.getClass();

2、获取构造器对象，通过构造器new出一个对象

1）Clazz.getConstructor([String.class]);

2）Con.newInstance([参数]);

3、通过class对象创建一个实例对象（就相当与new类名（）无参构造器) 1）Cls.newInstance();

4、通过class对象获得一个属性对象

1）Field c=cls.getFields()：获得某个类的所有的公共（public）的字段，包括父类中的字段。

2）Field c=cls.getDeclaredFields()：获得某个类的所有声明的字段，即包括public、private和proteced，但是不包括父类的声明字段

5、通过class对象获得一个方法对象

1）Cls.getMethod(“方法名”,class……parameaType);（只能获取公共的）

2）Cls.getDeclareMethod(“方法名”);（获取任意修饰的方法，不能执行私有）

3（M.setAccessible(true);（让私有的方法可以执行）

6、让方法执行

1）Method.invoke(obj实例对象,obj可变参数);-----（是有返回值的）
 
# 4. 简单阐述一下 Java 反射 API？
反射API用来生成JVM中的类、接口或则对象的信息。

Class类：反射的核心类，可以获取类的属性，方法等信息。

Field类：Java.lang.reflec包中的类，表示类的成员变量，可以用来获取和设置类之中的属性值。

Method类：Java.lang.reflec包中的类，表示类的方法，它可以用来获取类中的方法信息或者执行方法。

Constructor类：Java.lang.reflec包中的类，表示类的构造方法。

# 5. 按照技术所服务的领域来划分，Java技术体系可以分为哪四个平台？
• Java Card：支持一些Java小程序（Applets）运行在小内存设备（如智能卡）上的平台。 • Java ME（Micro Edition）：支持Java程序运行在移动终端（手机、PDA）上的平台，对 Java API 有所 精简，并加入了针对移动终端的支持，这个版本以前称为J2ME。 • Java SE（Standard Edition）：支持面向桌面级应用（如 Windows 下的应用程序）的 Java 平台，提供 了完整的 Java 核心 API，这个版本以前称为 J2SE。 • Java EE（Enterprise Edition）：支持使用多层架构的企业应用（如 ERP、CRM 应用）的 Java 平 台，除了提供 Java SE API 外，还对其做了大量的扩充并提供了相关的部署支持，这个版本以前称为 J2E E。

# 6. 为什么 for 循环中不建议使用“+”进行字符串拼接？
TODO

# 7. Java 中构造函数什么时候被调用执行？
在java语言中，构造函数又称构造方法。特殊性在于，与普通方法的区别是，他与类名相同，不返回结果也不加void返回值。构造函数的作用是初始化对象，即在创建对象时被系统调用（与普通方法不同，程序不能显示调用构造函数）。

构造函数还能够被重载，即可以传入参数，当程序中包含有带参的构造函数时，系统将不会再提供的无参构造函数。构造函数特点：没有函数返回值，构造函数名与类名相同；当创建类对象的时候调用其对应的构造方法去创建。每创建一个类的实例都去初始化它的所有变量是乏味的。如果一个对象在被创建时就完成了所有的初始工作，将是简单的和简洁的。因此，Java在类里提供了一个特殊的成员函数，叫做构造函数（Constructor）。

一个构造函数是对象被创建时初始对象的成员函数。它具有和它所在的类完全一样的名字。一旦定义好一个构造函数，创建对象时就会自动调用它。构造函数没有返回类型，即使是void类型也没有。这是因为一个类的构造函数的返回值的类型就是这个类本身。构造函数的任务是初始化一个对象的内部状态，所以用new操作符创建一个实例后，立刻就会得到一个清楚、可用的对象。

构造方法是一种特殊的方法，具有以下特点：

构造方法的方法名必须与类名相同。

构造方法没有返回类型，也不能定义为void，在方法名前面不声明方法类型。

构造方法的主要作用是完成对象的初始化工作，它能够把定义对象时的参数传给对象的域。

构造方法不能由编程人员调用，而要系统调用。

一个类可以定义多个构造方法，如果在定义类时没有定义构造方法，则编译系统会自动插入一个无参数的默认构造器，这个构造器不执行任何代码。

构造方法可以重载，以参数的个数，类型，或排列顺序区分。

# 8. 空 "" 字符串有什么作用？
可用于拼接字符串或字节，字节拼接成字符串参考如下：
```java
public class Test {
    public static void main(String[] args) {
        String s= "";
        for (char  i = 'a'; i < 'd'; i++) {
            s = s + i;//输出abc
//          s = i + s;//输出cba
        }
        System.out.println(s);
    }
}
```

# 9. switch 语句能否作用在 byte 上，能否作用在 long 上，能否作用在 String上？
java中switch语句可以作用在byte上，因为byte能自动转为int；

java中switch语句不能作用在long上，long转int不能自动转，需要强转；

在jdk1.7版本以后switch语句可以作用在String上。

# 10. Java 中 final 关键字修饰一个变量时，是引用不能变，还是引用的对象不能变？
Java中final关键字修饰一个变量时是引用不能变，即对象的指向不能变，但引用的对象即引用里的值是可以变得，因为它没有使用final关键字修饰。

基本数据类型的值是不能更改的，比如
```java
final int a = 1;a = 2;
```
那肯定不能正常编译通过。

因为a是final修饰的不可改变；a原来指向1，后来指向2；a的指向改变了。

```java
final int[] arr= {1,2,3}; arr[0]=2;
```
这种数组是可以的。因为arr的指向并没有变，只不过它里边的值可以变。

# 11. System.out.println() 分别代表什么含义？
System.out.println()是日常开发中使用最多的输出语句，其中System是一个类，out是这个类中的一个静态常量对象，是PrintStream类型的，println()是PrintStream类的方法，用于输出信息。

# 12. Java 中 super.getClass() 方法调用有什么作用？
```java
import java.util.Date; 
public class Test extends Date {
    public static void main(String[] args) { 
        new Test().test(); 
    }   
    public void test() { 
        System.out.println(super.getClass().getName());  
    }
}
```

其中输出的结果是Test。

原因：由于getClass()在Object类中定义成了final，子类不能覆盖该方法，所以，在test方法中调用super.getClass().getName()方法，等效于调用getClass().getName()方法，所以，super.getClass().getName()方法返回的也应该是Test。

如果想得到父类的名称，应该用如下代码：

```java
getClass().getSuperClass().getName();
```
# 13. PreparedStatement 与 Statement 有什么区别？
PreparedStatement是预编译语句执行者，数据库对sql语句进行预编译；Statement是执行时对sql语句进行编译、

Statement存在sql注入的问题，PreparedStatement解决了这个问题、

PreparedStatement的执行效率比Statement高

PreparedStatement中使用“?”占位符，设置参数更方便。

# 14. Java 中什么是序列化，如何实现序列化？
序列化机制（包括序列化和反序列化）的本质是用流将对象读到内存和写入外存。

序列化机制的意义就是将对象脱离程序运行独立存在。

应用场景是在RMI（远程方法调用）中应用，即通过网路或跨平台传输对象，而RMI是javaEE开发基础，所以javaEE要求传递的参数与返回值都实现序列化机制。

序列化是用流将java对象转成二进制写入硬盘或网络。

反序列化是用流将二进制数据转为java对象写入内存。

实现序列化需要实现Serializable或Externalizable接口，如果某个成员变量是引用数据类型，那么要求该引用类也是可序列化的。如果类中每个成员变量不想被序列化，可以用transient关键字修饰。

序列化通常与IO中的ObjectInputStream（readObject方法）和ObjectOutputStream（writeObject方法）搭配使用。


