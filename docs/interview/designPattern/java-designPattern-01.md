# 1. 什么是设计模式？
设计模式（Design pattern） 是解决软件开发某些特定问题而提出的一些解决方案也可以理解成解决问题的一些思路，通过设计模式可以帮助我们增强代码的可重用性、可扩充性、 可维护性、灵活性好。使用设计模式最终的目的是实现代码的高内聚和低耦合。

高内聚低耦合是软件工程中的概念，是判断软件设计好坏的标准，主要用于程序的面向对象的设计，主要看类的内聚性是否高，耦合度是否低。

目的是使程序模块的可重用性、移植性大大增强。

通常程序结构中各模块的内聚程度越高，模块间的耦合程度就越低。

内聚是从功能角度来度量模块内的联系，一个好的内聚模块应当恰好做一件事，它描述的是模块内的功能联系；耦合是软件结构中各模块之间相互连接的一种度量，耦合强弱取决于模块间接口的复杂程度、进入或访问一个模块的点以及通过接口的数据。

# 2. 为什么要使用设计模式？
1）设计模式是前人根据经验总结出来的，使用设计模式，就相当于是站在了前人的肩膀上。

2）设计模式使程序易读。熟悉设计模式的人应该能够很容易读懂运用设计模式编写的程序。

3）设计模式能使编写的程序具有良好的可扩展性，满足系统设计的开闭原则。比如策略模式，就是将不同的算法封装在子类中，在需要添加新的算法时，只需添加新的子类，实现规定的接口，即可在不改变现有系统源码的情况下加入新的系统行为。

4）设计模式能降低系统中类与类之间的耦合度。比如工厂模式，使依赖类只需知道被依赖类所实现的接口或继承的抽象类，使依赖类与被依赖类之间的耦合度降低。

5）设计模式能提高代码的重用度。比如适配器模式，就能将系统中已经存在的符合新需求的功能代码兼容新的需求提出的接口 。

6）设计模式能为常见的一些问题提供现成的解决方案。

7）设计模式增加了重用代码的方式。比如装饰器模式，在不使用继承的前提下重用系统中已存在的代码。

# 3. 设计模式有多少种，都有哪些设计模式？
Java有23种设计模式

设计模式总体分为三大类：

创建型模式，共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。

结构型模式，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。

行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

其实还有两类：并发型模式和线程池模式。

# 4. 设计模式的六大原则是什么？
1）开闭原则（Open Close Principle）

开闭原则就是说对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。所以一句话概括就是：为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类，后面的具体设计中我们会提到这点。

2）里氏代换原则（Liskov Substitution Principle）

里氏代换原则(Liskov Substitution Principle LSP)面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP是继承复用的基石，只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。里氏代换原则是对“开-闭”原则的补充。实现“开-闭”原则的关键步骤就是抽象化。而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

3）依赖倒转原则（Dependence Inversion Principle）

这个是开闭原则的基础，具体内容：真对接口编程，依赖于抽象而不依赖于具体。

4）接口隔离原则（Interface Segregation Principle）

大概意思是指使用多个隔离的接口，比使用单个接口要好。还是一个降低类之间的耦合度的意思，从这儿我们看出，其实设计模式就是一个软件的设计思想，从大型软件架构出发，为了升级和维护方便。所以上文中多次出现：降低依赖，降低耦合。

5）迪米特法则（最少知道原则）（Demeter Principle）

为什么叫最少知道原则，就是说：一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。

6）合成复用原则（Composite Reuse Principle）

原则是尽量使用合成/聚合的方式，而不是使用继承。

# 5. 什么是高内聚、低耦合？
内聚关注模块内部的元素结合程度，耦合关注模块之间的依赖程度。

1）内聚性

又称块内联系。指模块的功能强度的度量，即一个模块内部各个元素彼此结合的紧密程度的度量。若一个模块内各元素（语名之间、程序段之间）联系的越紧密，则它的内聚性就越高。

所谓高内聚是指一个软件模块是由相关性很强的代码组成，只负责一项任务，也就是常说的单一责任原则。

2）耦合性

也称块间联系。指软件系统结构中各模块间相互联系紧密程度的一种度量。模块之间联系越紧密，其耦合性就越强，模块的独立性则越差。模块间耦合高低取决于模块间接口的复杂性、调用的方式及传递的信息。

对于低耦合，粗浅的理解是：一个完整的系统，模块与模块之间，尽可能的使其独立存在。也就是说，让每个模块，尽可能的独立完成某个特定的子功能。模块与模块之间的接口，尽量的少而简单。如果某两个模块间的关系比较复杂的话，最好首先考虑进一步的模块划分。这样有利于修改和组合。

# 6. 什么是单例模式？
单例模式的定义就是确保某一个类仅有一个实例，而且自行实例化并向整个系统提供这个实例，这个类称为单例类，它提供全局访问的方法。

单例模式分为：懒汉式单例、饿汉式单例、登记式单例三种。

单例模式特点：

1)单例类只能有一个实例。

2)单例类必须自己创建自己的唯一实例。

3)单例类必须给所有其他对象提供这一实例。

单例模式的优点是内存中只有一个对象，节省内存空间；避免频繁的创建销毁对象，可以提高性能；避免对资源的多重占用，简化访问；为整个系统提供一个全局访问点。

单例模式的缺点是不适用于变化频繁的对象；滥用单利将带来一些问题，如为了节省资源将数据库连接池对象设计为的单利类，可能会导致共享连接池对象的程序过多而出现连接池溢出；如果实例化的对象长时间不被利用，系统会认为该对象是垃圾而被回收，这可能会导致对象状态的丢失。

从名字上来说饿汉和懒汉，饿汉就是类一旦加载，就把单例初始化完成，保证getInstance的时候单例是已经被初始化，而懒汉比较懒，只有当调用getInstance的时候，才回去初始化这个单例。

1、懒汉式单例，具体代码如下：

```java
package com.yoodb;
//懒汉式单例类.在第一次调用的时候实例化自己
public class Singleton {
//私有的无参构造方法
private Singleton(){

    }
    //注意，这里没有final
    private static Singleton single = null;
    //静态工厂方法
    public synchronized  static Singleton getInstance(){
        if(single == null){
            single = new Singleton();
        }
        return single;
    }
}
```

2、饿汉式单例，具体代码如下：

```java
package com.yoodb;

public class Singleton {
//私有的无参构造方法
private Singleton(){

    }
    //自动实例化
    private final static Singleton single = new Singleton();
    //静态工厂方法
    public static synchronized Singleton getInstance(){
        return single;
    }
}
```

3、登记式单例，具体代码如下：

```java
package com.yoodb;

import java.util.HashMap;
import java.util.Map;

public class Singleton {
public static Map<String,Singleton> map = new HashMap<String,Singleton>();
static{
//将类名注入下次直接获取
Singleton single = new Singleton();
map.put(single.getClass().getName(),single);
}
//保护式无参构造方法
protected Singleton(){}

    public static Singleton getInstance(String name){
        if(name == null){
            name = Singleton.class.getName(); 
        }
        if(map.get(name) == null){
            try {
                map.put(name, Singleton.class.newInstance());
            } catch (InstantiationException | IllegalAccessException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        return map.get(name);
    }
     
    public String create() {      
return "创建一个单例！";      
}

    public static void main(String[] args) {
         Singleton single = Singleton.getInstance(null);  
         System.out.println(single.create());  
    }
}
```

# 7. 单例模式中饿汉式和懒汉式有什么区别？
1、线程安全

饿汉式是线程安全的，可以直接用于多线程而不会出现问题，懒汉式就不行，它是线程不安全的，如果用于多线程可能会被实例化多次，失去单例的作用。

2、资源加载

饿汉式在类创建的同时就实例化一个静态对象，不管之后会不会使用这个单例，都会占据一定的内存资源，相应的在调用时速度也会更快。

懒汉式顾名思义，会延迟加载，在第一次使用该单例时才会实例化对象出来，第一次掉用时要初始化，如果要做的工作比较多，性能上会有些延迟，第一次调用之后就和饿汉式。

# 8. 单例模式都有哪些应用场景？
1）需要生成唯一序列；

2）需要频繁实例化然后销毁代码的对象；

3）有状态的工具类对象；

4）频繁访问数据库或文件的对象。

例如：项目中读取配置文件、数据库连接池参数、spring中的bean默认是单例、线程池（threadpool）、缓存（cache）、日志参数等程序的对象。

需要注意的事这些场景只能有一个实例，如果生成多个实例，就会导致许多问题产生，如：程序行为异常、资源使用过量或数据不一致等。

# 9. 什么是代理模式？
代理模式是对象的结构模式，代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用。在一些情况下，一个客户不想或者不能够直接引用一个对象，而代理对象可以在客户端和目标对象之间起到中介的作用。

在代理模式中的介绍如下：

1)抽象对象角色：声明了目标对象和代理对象的共同接口，这样一来在任何可以使用目标对象的地方都可以使用代理对象。

2)目标对象角色：定义了代理对象所代表的目标对象。

3)代理对象角色：代理对象内部含有目标对象的引用，从而可以在任何时候操作目标对象；代理对象提供一个与目标对象相同的接口，以便可以在任何时候替代目标对象。代理对象通常在客户端调用传递给目标对象之前或之后，执行某个操作，而不是单纯地将调用传递给目标对象。

举例如逢年过节加班比较忙没有时间去买火车票，打电话给附近的票务中心叫他们帮忙代购；但是票务中心并非卖票的，只有火车站才是真正出售票的地点，票务中心其实是通过火车站实现的。

# 10. Java 中代理模式如何实现静态代理？
如果代理类在程序运行前就已经存在，那么这种代理方式被成为静态代理，这种情况下的代理类一般都是在Java代码中定义的。

通常情况下， 静态代理中的代理类和委托类会实现同一接口或是派生自相同的父类。下面说一下如何实现静态代理模式。

接口（或抽象类），具体代码如下：

```java
package com.yoodb;

public interface Business {
public void doAction();
}
此处是真正意义上实现业务逻辑的类，具体代码如下：


package com.yoodb;

public class BusinessImpl implements Business {

    @Override
    public void doAction() {
        // TODO Auto-generated method stub
        System.out.println("实现业务逻辑.");
    }

}
```

自己并未实现业务逻辑接口，而是调用实现，具体代码如下：
```java
package com.yoodb;

public class BusinessImplProxy implements Business{

    private BusinessImpl impl;
     
    @Override
    public void doAction() {
        // TODO Auto-generated method stub
        if(impl == null){
            impl = new BusinessImpl();
        }
        doBefore();
        impl.doAction();
        doAfter();
    }
    private void doBefore(){  
        System.out.println("调用目标对象前做相关操作");  
    }

    private void doAfter(){  
        System.out.println("调用目标对象后做相关操作");  
    }
}
```


单元测试，具体代码如下：

```java
package com.yoodb;

public class MainTest {
    public static void main(String[] args) {
        Business business = new BusinessImplProxy();
        business.doAction();
    }
}
```

运行之后结果如下：

```sh
调用目标对象前做相关操作
实现业务逻辑.
调用目标对象后做相关操作
```

# 11. Java 中代理模式如何实现动态代理？
Java动态代理类位于Java.lang.reflect包下，一般主要涉及到以下两个类：

1）Interface InvocationHandler：该接口中仅定义了一个方法Object：invoke(Object obj,Method method, Object[] args)。在实际使用时，第一个参数obj一般是指代理类，method是被代理的方法，args为该方法的参数数组；这个抽象方法在代理类中动态实现。

2）Proxy：该类即为动态代理类，作用类似于上例中的ProxySubject，其中主要包含以下内容：

Protected Proxy(InvocationHandler h)：构造函数，估计用于给内部的h赋值。

Static Class getProxyClass (ClassLoader loader, Class[] interfaces)：获得一个代理类，其中loader是类装载器，interfaces是真实类所拥有的全部接口的数组。

Static Object newProxyInstance(ClassLoader loader, Class[] interfaces, InvocationHandler h)：返回代理类的一个实例，返回后的代理类可以当作被代理类使用(可使用被代理类的在Subject接口中声明过的方法)。

接口（或抽象类），具体代码如下：

```java
package com.yoodb;

public interface Business {
    public void doAction();
}
```
此处是真正意义上实现业务逻辑的类，具体代码如下：

```java
package com.yoodb;

public class BusinessImpl implements Business {

    @Override
    public void doAction() {
        // TODO Auto-generated method stub
        System.out.println("实现业务逻辑.");
    }

}
```

动态代理方式，具体代码如下：

```java
package com.yoodb;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class DynamicBusinessProxy implements InvocationHandler{

    private Object obj;
     
    public DynamicBusinessProxy() {}
     
    public DynamicBusinessProxy(Object obj) {
        this.obj = obj;
    }
 
    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        // TODO Auto-generated method stub
        Object result = null;
        doBefore();
        result = method.invoke(obj, args);
        doAfter();     
        return result;
    }
 
    private void doBefore(){  
        System.out.println("调用目标对象前做相关操作");  
    }

    private void doAfter(){  
        System.out.println("调用目标对象后做相关操作");  
    }

    public static Object factory(Object obj)  
    {  
    Class<? extends Object> cls = obj.getClass();
        return Proxy.newProxyInstance(cls.getClassLoader(),cls.getInterfaces(),new DynamicBusinessProxy(obj));  
    }
}
```

单元测试，具体代码如下：

```java
package com.yoodb;

public class MainTest {
public static void main(String[] args) {
    Business business = (Business) DynamicBusinessProxy.factory(new BusinessImpl());
    business.doAction();
}
}
```

运行之后结果如下：

```sh
调用目标对象前做相关操作
实现业务逻辑.
调用目标对象后做相关操作
```

# 12. Java 中什么是解释器模式？
解释器模式是类的行为模式。给定一个语言之后，解释器模式可以定义出其文法的一种表示，并同时提供一个解释器。客户端可以使用这个解释器来解释这个语言中的句子。

该模式涉及角色如下：

1）抽象表达式(Expression)角色：声明一个所有的具体表达式角色都需要实现的抽象接口。这个接口主要是一个interpret()方法，称做解释操作。

2）终结符表达式(Terminal Expression)角色：实现了抽象表达式角色所要求的接口，主要是一个interpret()方法；文法中的每一个终结符都有一个具体终结表达式与之相对应。比如有一个简单的公式R=R1+R2，在里面R1和R2就是终结符，对应的解析R1和R2的解释器就是终结符表达式。

3）非终结符表达式(Nonterminal Expression)角色：文法中的每一条规则都需要一个具体的非终结符表达式，非终结符表达式一般是文法中的运算符或者其他关键字，比如公式R=R1+R2中，“+"就是非终结符，解析“+”的解释器就是一个非终结符表达式。

4）环境(Context)角色：这个角色的任务一般是用来存放文法中各个终结符所对应的具体值，比如R=R1+R2，我们给R1赋值100，给R2赋值200。这些信息需要存放到环境角色中，很多情况下我们使用Map来充当环境角色就足够了。

# 13. Java 中如何实现解释器模式？
Java中实现解释器模式，说一说演示代码。

1、实体类Context,具体代码如下：

```java
package com.yoodb;

public class Context {
private Integer numo;

    private Integer numt;
     
 
    public Context(Integer numo, Integer numt) {
        this.numo = numo;
        this.numt = numt;
    }
 
    public Integer getNumo() {
        return numo;
    }
 
    public void setNumo(Integer numo) {
        this.numo = numo;
    }
 
    public Integer getNumt() {
        return numt;
    }
 
    public void setNumt(Integer numt) {
        this.numt = numt;
    }

}
```

2、一个接口，具体代码如下：

```java
package com.yoodb;

public interface Expression {
    public int interpret(Context context);
}
```

3、两个接口实现类，一个类是参数相加，一个类是参数相减，具体代码如下：

```java
package com.yoodb;

public class Minus implements Expression {

    @Override
    public int interpret(Context context) {
        // TODO Auto-generated method stub
         return context.getNumo()-context.getNumt();  
    }

}
```

```java
package com.yoodb;


public class Plus implements Expression {

    @Override
    public int interpret(Context context) {
        // TODO Auto-generated method stub
        return context.getNumo() + context.getNumt();  
    }

}
```

4、单元测试，具体代码如下：

```java
package com.yoodb;

public class MainTest {
    public static void main(String[] args) {
        int result = new Minus().interpret((new Context(new Plus().interpret(new Context(2015, 8)), 4)));
        System.out.println(result);
    }
}
```

运行结果如下：

```shell
2019
```

# 14. Java 中什么是替换法则（LSP）？
替换法则（LSP）是指使用指向基类（超类）的引用的函数，必须能够在不知道具体派生类（子类）对象类型的情况下使用。

# 15. Java 中为什么不允许从静态方法中访问非静态变量？
Java中不能从静态上下文访问非静态数据只是因为非静态变量是跟具体的对象实例关联的，而静态的却没有和任何实例关联。

# 16. 微服务架构的六种常用设计模式是什么？
代理设计模式

聚合设计模式

链条设计模式

聚合链条设计模式

数据共享设计模式

异步消息设计模式

# 17. Java 中单例模式有什么优缺点？
**单例模式优点**

1、在单例模式中，活动的单例只有一个实例，对单例类的所有实例化得到的都是相同的一个实例。这样就防止其它对象对自己的实例化，确保所有的对象都访问一个实例。

2、单例模式具有一定的伸缩性，类自己来控制实例化进程，类就在改变实例化进程上有相应的伸缩性。

3、提供了对唯一实例的受控访问。

4、由于在系统内存中只存在一个对象，因此可以节约系统资源，当需要频繁创建和销毁的对象时单例模式无疑可以提高系统的性能。

5、允许可变数目的实例。

6、避免对共享资源的多重占用。

**单例模式缺点**

1、不适用于变化的对象，如果同一类型的对象总是要在不同的用例场景发生变化，单例就会引起数据的错误，不能保存彼此的状态。

2、由于单利模式中没有抽象层，因此单例类的扩展有很大的困难。

3、单例类的职责过重，在一定程度上违背了“单一职责原则”。

4、滥用单例将带来一些负面问题，如为了节省资源将数据库连接池对象设计为的单例类，可能会导致共享连接池对象的程序过多而出现连接池溢出；如果实例化的对象长时间不被利用，系统会认为是垃圾而被回收，这将导致对象状态的丢失。

# 18. Java 中单例模式使用时有哪些注意事项？
1、使用时不能用反射模式创建单例，否则会实例化一个新的对象。

2、使用懒单例模式时注意线程安全问题。

3、饿单例模式和懒单例模式构造方法都是私有的，因而是不能被继承的，有些单例模式可以被继承（如登记式模式）。

# 19. Java 中单例模式如何防止反射漏洞攻击？
```shell
public class Singleton {
private static boolean flag = false;

	private Singleton() {

		if (flag == false) {
			flag = !flag;
		} else {
			throw new RuntimeException("关注Java精选公众号，提醒单例模式被攻击！");
		}
	}

	public static void main(String[] args) {

	}
}
```

# 20. 什么是工厂模式？
工厂模式提供了一种创建对象的最佳方式。在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。实现了创建者和调用者分离，工厂模式分为简单工厂、工厂方法、抽象工厂模式。
