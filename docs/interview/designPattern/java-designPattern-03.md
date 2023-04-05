# 1. Java 中如何实现模板方法模式？
举例：去餐厅吃饭，餐厅给我们提供了一个模板就是：看菜单，点菜，吃饭，付款，走人。注意这里“点菜和付款”是不确定的由子类来完成的，其他的则是一个模板。

1、先定义一个模板。把模板中的点菜和付款，让子类来实现。

```java
package com.yoodb.blog;

//模板方法
public abstract class RestaurantTemplate {

	// 1.看菜单
	public void menu() {
		System.out.println("看菜单");
	}

	// 2.点菜业务
	abstract void spotMenu();

	// 3.吃饭业务
	public void havingDinner(){ System.out.println("吃饭"); }

	// 3.付款业务
	abstract void payment();

	// 3.走人
	public void GoR() { System.out.println("走人"); }

	//模板通用结构
	public void process(){
		menu();
		spotMenu();
		havingDinner();
		payment();
		GoR();
	}
}
```
2、具体的模板方法子类

```java
package com.yoodb.blog;

public class RestaurantGinsengImpl extends RestaurantTemplate {

    void spotMenu() {
        System.out.println("人参");
    }

    void payment() {
        System.out.println("5快");
    }
}
```
3、具体的模板方法子类

```java
package com.yoodb.blog;

public class RestaurantLobsterImpl  extends RestaurantTemplate  {

    void spotMenu() {
        System.out.println("龙虾");
    }

    void payment() {
        System.out.println("50块");
    }
}
```
4、测试

```java
package com.yoodb.blog;

public class Client {

    public static void main(String[] args) {
        RestaurantTemplate restaurantTemplate = new RestaurantGinsengImpl();
        restaurantTemplate.process();
    }
}
```
# 2. Java 中什么是外观模式？
外观模式又叫门面模式，隐藏系统的复杂性，并向客户端提供了一个客户端可以访问系统的接口。

外观模式是为了解决类与类之家的依赖关系的，为子系统中的各类（或结构与方法）提供一个简明一致的界面，隐藏子系统的复杂性，使子系统更加容易使用。外观模式是一种结构型模式。

使用外观模式，它外部看起来就是一个接口，其实他的内部有很多复杂的接口已经被实现。

# 3. Java 中外观模式有什么使用场景？
1、当你要为一个复杂子系统提供一个简单接口时。子系统往往因为不断演化而变得越来越复杂。大多数模式使用时都会产生更多更小的类。这使得子系统更具可重用性，也更容易对子系统进行定制，但这也给那些不需要定制子系统的用户带来一些使用上的困难。Facade可以提供一个简单的缺省视图，这一视图对大多数用户来说已经足够，而那些需要更多的可定制性的用户可以越过Facade层。

2、客户程序与抽象类的实现部分之间存在着很大的依赖性。引入Facade将这个子系统与客户以及其他的子系统分离，可以提高子系统的独立性和可移植性。

3、当你需要构建一个层次结构的子系统时，使用Facade模式定义子系统中每层的入口点，如果子系统之间是相互依赖的，你可以让它们仅通过Facade进行通讯，从而简化了它们之间的依赖关系。

# 4. Java 中如何实现外观模式？

1、接口A，具体代码如下：

```java
package com.yoodb.blog;

public interface ServiceA {  
    /**
    * ServiceA 的A方法  
    * */  
    public void methodA() ;  
}  
```
2、接口A实现类，具体代码如下：

```java
package com.yoodb.blog;

public class ServiceAImpl implements ServiceA {

    /* (non-Javadoc) 
     * @see com.yoodb.blog.ServiceA#methodA() 
     */  
    @Override  
    public void methodA() {  
        System.out.println( "methodA--> is runing" );   
    }

}  
```
3、接口B，具体代码如下：

```java
package com.yoodb.blog;

public interface ServiceB {  
    /**
    * ServiceB 的B方法
    * */  
    public void methodB() ;  
}  
```
4、接口B实现类，具体代码如下：

```java
package com.yoodb.blog;

public class ServiceBImpl implements ServiceB {

    /* (non-Javadoc) 
     * @see com.yoodb.blog.ServiceA#methodA() 
     */  
    @Override  
    public void methodB() {  
     System.out.println( "methodB--> is runing" );   
    }

}  
```
5、接口C，具体代码如下：

```java
public interface ServiceC {  
    /**
    * ServiceC 的C方法
    * */  
    public void methodC() ;    
}  
```
6、接口C实现类，具体代码如下：

```java
package com.yoodb.blog;

public class ServiceCImpl implements ServiceC {


    /* (non-Javadoc) 
     * @see com.yoodb.blog.ServiceA#methodA() 
     */  
    @Override  
    public void methodC() {  
     System.out.println( "methodC--> is runing" );    
    }

}  
```
7、外观模式，核心实现代码如下：

```java
package com.yoodb.blog;

public class Facade {  
    ServiceA sa;  
    ServiceB sb;  
    ServiceC sc;

    public Facade() {  
        sa = new ServiceAImpl();  
        sb = new ServiceBImpl();  
        sc = new ServiceCImpl();  
    }

    public void methodA() {  
        sa.methodA();  
        sb.methodB();  
    }

    public void methodB() {  
        sb.methodB();  
        sc.methodC();  
    }   

    public void methodC() {  
        sc.methodC();  
        sa.methodA();  
    }

}  
```
8、测试代码如下：

```java
package com.yoodb.blog;

public class Client {

    /** 
     * @param args 
     */  
    public static void main(String[] args) {  
        ServiceA sa = new ServiceAImpl();  
        ServiceB sb = new ServiceBImpl();  
        sa.methodA();  
        sb.methodB();  
        System.out.println("=====================");  
        Facade f = new Facade();  
        f.methodA();  
        f.methodB();  
        f.methodC() ;  
    }

}
```
运行结果如下：

```sh
methodA--> is runing
methodB--> is runing
=====================
methodA--> is runing
methodB--> is runing
methodB--> is runing
methodC--> is runing
methodC--> is runing
methodA--> is runing
```
# 5. Java 中外观模式有什么优势？
1）它对客户屏蔽子系统组件，因而减少了客户处理的对象的数目并使得子系统使用起来更加方便。

2）它实现了子系统与客户之间的松耦合关系，而子系统内部的功能组件往往是紧耦合的。松耦合关系使得子系统的组件变化不会影响到它的客户。Facade模式有助于建立层次结构系统，也有助于对对象之间的依赖关系分层。Facade模式可以消除复杂的循环依赖关系。

这一点在客户程序与子系统是分别实现的时候尤为重要。在大型软件系统中降低编译依赖性至关重要。在子系统类改变时，希望尽量减少重编译工作以节省时间。用Facade可以降低编译依赖性，限制重要系统中较小的变化所需的重编译工作。Facade模式同样也有利于简化系统在不同平台之间的移植过程，因为编译一个子系统一般不需要编译所有其他的子系统。

3）如果应用需要，它并不限制它们使用子系统类。因此你可以在系统易用性和通用性之间加以选择。

# 6. Java 中什么是原型模式？
原型模式是一种创建型设计模式，通过给出一个原型对象来指明所有创建的对象的类型，然后用复制这个原型对象的办法创建出更多同类型的对象。

原型设计模式简单来说就是克隆，原型模式多用于创建复杂的或者构造耗时的实例，因为这种情况下，复制一个已经存在的实例可使程序运行更高效。

原型模式有两种表现形式：

1）简单形式；

2）登记形式，这两种表现形式仅仅是原型模式的不同实现。

简单形式的原型模式，涉及到三种角色分别如下：

1）客户(Client)角色：客户类提出创建对象的请求。

2）抽象原型(Prototype)角色：这是一个抽象角色，通常由一个Java接口或Java抽象类实现。此角色给出所有的具体原型类所需的接口。

3）具体原型（Concrete Prototype）角色：被复制的对象。此角色需要实现抽象的原型角色所要求的接口。

# 7. Java 中原型模式有什么应用场景？
1、类初始化需要消化非常多的资源，这个资源包括数据、硬件资源等。这时我们就可以通过原型拷贝避免这些消耗。

2、通过new产生的一个对象需要非常繁琐的数据准备或者权限，这时可以使用原型模式。

3、一个对象需要提供给其他对象访问，而且各个调用者可能都需要修改其值时，可以考虑使用原型模式拷贝多个对象供调用者使用，即保护性拷贝。

# 8. Java 中原型模式有哪些使用方式？
1、实现Cloneable接口。在java语言有一个Cloneable接口，它的作用只有一个，就是在运行时通知虚拟机可以安全地在实现了此接口的类上使用clone方法。在java虚拟机中，只有实现了这个接口的类才可以被拷贝，否则在运行时会抛出CloneNotSupportedException异常。

2、重写Object类中的clone方法。Java中，所有类的父类都是Object类，Object类中有一个clone方法，作用是返回对象的一个拷贝，但是其作用域protected类型的，一般的类无法调用，因此Prototype类需要将clone方法的作用域修改为public类型。

# 9. Java 中原型模式如何实现浅拷贝？
浅拷贝是指对值类型的成员变量进行值的复制,对引用类型的成员变量只复制引用,不复制引用的对象。

浅拷贝只负责克隆按值传递的数据（比如基本数据类型、String类型），而不复制它所引用的对象，换言之，所有的对其他对象的引用都仍然指向原来的对象。

1、实体类，具体代码如下：

```java
package com.yoodb;

public class Prototype implements Cloneable{
private String name;

    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    @Override
    protected Object clone() {
        // TODO Auto-generated method stub
        try {
            return super.clone();
        } catch (CloneNotSupportedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }


}
```
2、测试方法，具体代码如下：

```java
package com.yoodb;

public class TestMain {
    public static void main(String[] args) {
        Prototype pro = new Prototype();
        pro.setName("欢迎收藏blog.yoodb.com");
        Prototype prot = (Prototype) pro.clone();
        prot.setName("欢迎收藏blog.yoodb.com");
        System.out.println("original object：" + pro.getName());
        System.out.println("cloned object：" + prot.getName());
    }

}
```
3、运行结果如下：

```sh
original object：欢迎收藏blog.yoodb.com
cloned object：欢迎收藏blog.yoodb.com
```
# 10. Java 中原型模式如何实现深拷贝？
深拷贝是指对值类型的成员变量进行值的复制，对引用类型的成员变量也进行引用对象的复制。

深拷贝除了浅度复制要复制的值外，还负责复制引用类型的数据。那些引用其他对象的变量将指向被复制过的新对象，而不再是原有的那些被引用的对象。换言之，深度复制把要复制的对象所引用的对象都复制了一遍，而这种对被引用到的对象的复制叫做间接复制。

**方式一**

1、实体类，具体代码如下：

```java
//实体一
package com.yoodb;

public class Prototype implements Cloneable{
private String name;

    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    @Override
    protected Object clone() {
        // TODO Auto-generated method stub
        try {
            return super.clone();
        } catch (CloneNotSupportedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }


}
```
```java
//实体二
package com.yoodb;

public class NewPrototype implements Cloneable{
private String id;

    private Prototype prototype;
 
    public String getId() {
        return id;
    }
 
    public void setId(String id) {
        this.id = id;
    }
 
    public Prototype getPrototype() {
        return prototype;
    }
 
    public void setPrototype(Prototype prototype) {
        this.prototype = prototype;
    }
 
    @Override
    protected Object clone() {
        NewPrototype prot = null;
        try {
            prot = (NewPrototype) super.clone();
            prot.prototype = (Prototype) this.getPrototype().clone();
            return prot;
        } catch (Exception e) {
            // TODO: handle exception
        }
        return null;
    }


}
```
2、测试函数，具体代码如下：

```java
package com.yoodb;

public class TestMain {
    public static void main(String[] args) {
        //普通赋值
        Prototype pro = new Prototype();
        pro.setName("欢迎收藏blog.yoodb.com");
        NewPrototype newPro = new NewPrototype();
        newPro.setId("yoodb");
        newPro.setPrototype(pro);
        //克隆赋值
        NewPrototype proc = (NewPrototype) newPro.clone();
        proc.setId("yoodb");
        proc.getPrototype().setName("欢迎收藏blog.yoodb.com");

        System.out.println("original object id：" + newPro.getId());
        System.out.println("original object name：" + newPro.getPrototype().getName());
          
        System.out.println("cloned object id：" + proc.getId());
        System.out.println("cloned object name：" + proc.getPrototype().getName());
    }

}
```
3、运行结果如下：

```sh
original object id：yoodb
original object name：欢迎收藏blog.yoodb.com
cloned object id：yoodb
cloned object name：欢迎收藏blog.yoodb.com
```
**方式二**

利用串行化来实现深克隆，把对象写道流里的过程是串行化(Serilization)过程；把对象从流中读出来是并行化(Deserialization)过程。

1、实体类，具体代码如下：

 ```java
//实体一
package com.yoodb;

import java.io.Serializable;

public class Prototype implements Serializable{

    private static final long serialVersionUID = 1L;
     
    private String name;
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }

}
```
```java
//实体二
package com.yoodb;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

public class NewPrototype implements Serializable{

    private static final long serialVersionUID = 1L;
 
    private String id;
     
    private Prototype prototype;
 
    public String getId() {
        return id;
    }
 
    public void setId(String id) {
        this.id = id;
    }
 
    public Prototype getPrototype() {
        return prototype;
    }
 
    public void setPrototype(Prototype prototype) {
        this.prototype = prototype;
    }
 
    protected Object deepClone(){
        try {
            ByteArrayOutputStream bo = new ByteArrayOutputStream();
            ObjectOutputStream oo = new ObjectOutputStream(bo);
            oo.writeObject(this);
             
            ByteArrayInputStream bi = new ByteArrayInputStream(bo.toByteArray());
            ObjectInputStream oi = new ObjectInputStream(bi);
            return oi.readObject();
        } catch (Exception e) {
            // TODO: handle exception
        }
        return null;
    }

}
```
2、测试函数，具体代码如下：

```java
package com.yoodb;

public class TestMain {
    public static void main(String[] args) {
        //普通赋值
        Prototype pro = new Prototype();
        pro.setName("欢迎收藏www.yoodb.com");
        NewPrototype newPro = new NewPrototype();
        newPro.setId("yoodb");
        newPro.setPrototype(pro);
        //克隆赋值
        NewPrototype proc = (NewPrototype) newPro.deepClone();
        proc.setId("yoodb");
        proc.getPrototype().setName("欢迎收藏www.yoodb.com");

        System.out.println("original object id：" + newPro.getId());
        System.out.println("original object name：" + newPro.getPrototype().getName());
          
        System.out.println("cloned object id：" + proc.getId());
        System.out.println("cloned object name：" + proc.getPrototype().getName());
    }

}
```
3、运行结果如下：

```sh
original object id：yoodb
original object name：欢迎收藏www.yoodb.com
cloned object id：yoodb
cloned object name：欢迎收藏www.yoodb.com
```
# 11. Java 中什么是策略模式？
策略模式（strategy）是指把定义了一系列的算法，并将每一个算法封装起来，而且使它们还可以相互替换，策略模式让算法独立于使用它的客户而独立变化。策略模式实现需要设计一个接口，为一系列实现类提供统一的方法，多个实现类实现该接口，设计一个抽象类（可有可无，属于辅助类），提供辅助函数。

策略模式中有三个对象：

1）环境对象(Context)：该类中实现了对抽象策略中定义的接口或者抽象类的引用。

2）抽象策略对象(Strategy)：它可由接口或抽象类来实现。

3）具体策略对象(ConcreteStrategy)：它封装了实现同不功能的不同算法。

# 12. Java 中策略模式有什么应用场景？
策略模式是针对一组算法或逻辑，将每一个算法或逻辑封装到具有共同接口的独立的类中，从而使得它们之间可以相互替换。

实例1：根据不同会员等级打折力度不同的三种策略，初级会员，中级会员，高级会员（三种不同的计算）。

实例2：一个支付模块，要有微信支付、支付宝支付、银联支付等。

实例场景3：张三要到北方做买卖，走之前张三的父亲给仆人三个锦囊，说是危机关头按顺序依次拆开能解决相应的问题。场景中出现三个要素：妙计（具体策略类）、锦囊（环境事件类）、仆人（调用者）。

# 13. Java 中策略模式有什么优缺点？
策略模式优点

1、策略模式提供了管理相关的算法族的办法。策略类的等级结构定义了一个算法或行为族。恰当使用继承可以把公共的代码移到父类里面，从而避免代码重复。

2、使用策略模式可以避免使用多重条件(if-else)语句。多重条件语句不易维护，它把采取哪一种算法或采取哪一种行为的逻辑与算法或行为的逻辑混合在一起，统统列在一个多重条件语句里面，比使用继承的办法还要原始和落后。

策略模式缺点

1、客户端必须知道所有的策略类，并自行决定使用哪一个策略类。这就意味着客户端必须理解这些算法的区别，以便适时选择恰当的算法类。换言之，策略模式只适用于客户端知道算法或行为的情况。

2、由于策略模式把每个具体的策略实现都单独封装成为类，如果备选的策略很多的话，那么对象的数目就会很可观。

# 14. Java 中如何实现策略模式？
实例：假设张三要到北方做买卖，走之前张三的父亲给仆人三个锦囊，说是危机关头按顺序依次拆开能解决相应的问题。场景中出现三个要素：妙计（具体策略类）、锦囊（环境事件类）、仆人（调用者）。

1、抽象策略类（Strategy）

```java
public interface IStrategy {  
    public void operate();  
}
```
2、妙计一，初到北方地界，实现类：

```java
public class NorthernBoundary implements IStrategy {  
    @Override  
    public void operate() {  
        System.out.println("钱财打通官府，求照应，方便行事");  
    }  
}
```
3、妙计二，在当地开展买卖事宜，实现类：
```java
public class UnderstandingCustomsCulture implements IStrategy {  
    @Override  
    public void operate() {  
        System.out.println("了解当地风俗文化");  
    }  
}
```
4、妙计三，地皮流氓闹事，官府快速接入处理，实现类：

```java
public class AccessProcessing implements IStrategy {  
    @Override  
    public void operate() {  
        System.out.println("地皮流氓闹事，官府快速接入处理");  
    }  
}
```
5、环境事件类（Context）：

```java
public class ContextEvent {  
    private Strategy strategy;  
        //构造函数
        public ContextEvent(Strategy strategy){  
        this.strategy = strategy;  
    }  
    public void setStrategy(Strategy strategy){  
        this.strategy = strategy;  
    }  
    public void operate(){  
        this.strategy.operate();  
    }  
}
```
6、简单测试类：

```java
public class BlogYoodb {

    public static void main(String[] args) {  
		ContextEvent context;  
		System.out.println("----------初到北方地界使用锦囊一---------------");  
		context = new ContextEvent(new NorthernBoundary());  
		context.operate();  
		System.out.println("\n");  
		System.out.println("----------开展买卖事宜使用锦囊二---------------");  
		context.setStrategy(new UnderstandingCustomsCulture());  
		context.operate();  
		System.out.println("\n");  
		System.out.println("----------地皮流氓闹事，使用锦囊三---------------");  
		context.setStrategy(new AccessProcessing());  
		context.operate();  
		System.out.println("\n");  
    }  
}
```
# 15. Java 中什么是观察者模式？
观察者模式又称为发布/订阅（Publish/Subscribe）模式，观察者模式定义了对象间的一种一对多依赖关系，使得每当一个对象改变状态，则所有依赖于它的对象都会得到通知并被自动更新。

观察者（Observer）相当于事件监听者，被观察者（Observable）相当于事件源和事件，将观察者和被观察者的对象分离开，提高了应用程序的可维护性和重用性。执行逻辑时通知observer即可触发oberver的update，同时可传被观察者和参数。

观察者Observer：所有潜在的观察者必须实现观察者接口，这个接口只有update方法，当主题改变时，它被调用。

具体观察者ConcreteObserver: 具体观察者可以是任何实现了Observer接口的类。观察者必须注册具体主题，一边接收更新。

可观察者Subject: 主题接口，即可观察者Observable，对象使用此接口注册为观察者，或者把自己从观察者中删除，每个主题可以有多个观察者。

具体可观察者ConcreteSubject: 一个具体主题实现了主题接口，除了注册和撤销之外，具体主题还实现了notifyObservers（）方法，这个方法用来在主题状态改变时更新所有观察者。具体主题也可能有设置和获取状态的方法。

Subject（被观察者）包含了一些需要在其状态改变时通知的观察者。因此，他应该提供给观察者可以register（注册）自己和unregister（注销）自己的方法。当Subject（被观察者）发生变化的时候，也需要包含一个方法来通知所有观察者。当通知观察者的时候，可以推送更新内容，或者提供另外一个方法来获得更新内容。

JAVA提供了内置的方式来实现观察者模式，java.util.Observable和java.util.Observer接口。然而他们用的不是很广泛。因为此实现过于简单，大多数时候我们都不想最后扩展的类仅仅是实现了观察者模式，因为JAVA类不能多继承。

Java Messages Service（JMS）消息服务使用观察者模式与命令模式来实现不同的程序之间的数据的发布和订阅。MVC模型-视图-控制框架也使用观察者模式，把模型当做被观察者，视图视为观察者。视图能够注册自己到模型上来获得模型的改变。

# 16. Java 中实现观察者模式有哪两种方式？
1、推：每次都会把通知以广播的方式发送给所有观察者，所有的观察者只能被动接收。

2、拉：观察者只要知道有情况即可，至于什么时候获取内容，获取什么内容，都可以自主决定。

# 17. Java 中观察者模式有什么应用场景？
1、关联行为场景，需要注意的是，关联行为是可拆分的，而不是“组合”关系。事件多级触发场景。

2、跨系统的消息交换场景，如消息队列、事件总线的处理机制。

# 18. Java 中如何实现观察者模式？
1、定义抽象观察者，每一个实现该接口的实现类都是具体观察者。

```java
package com.yoodb.blog;

//观察者的接口，用来存放观察者共有方法
public interface Observer {
    // 观察者方法
    void update(int state);
}
```
2、定义具体观察者

```java
package com.yoodb.blog;

// 具体观察者
public class ObserverImpl implements Observer {

    // 具体观察者的属性
    private int myState;

    public void update(int state) {
        myState=state;
        System.out.println("收到消息,myState值改为："+state);
    }

    public int getMyState() {
        return myState;
    }
}
```
3、定义主题。主题定义观察者数组，并实现增、删及通知操作。

```java
package com.yoodb.blog;

import java.util.Vector;

//定义主题，以及定义观察者数组，并实现增、删及通知操作。
public class Subjecct {
//观察者的存储集合，不推荐ArrayList，线程不安全，
private Vector<Observer> list = new Vector<>();

	// 注册观察者方法
	public void registerObserver(Observer obs) {
		list.add(obs);
	}
    // 删除观察者方法
	public void removeObserver(Observer obs) {
		list.remove(obs);
	}

	// 通知所有的观察者更新
	public void notifyAllObserver(int state) {
		for (Observer observer : list) {
			observer.update(state);
		}
	}
}
```
4、定义具体的，他继承继承Subject类，在这里实现具体业务，在具体项目中，该类会有很多。

```java
package com.yoodb.blog;

//具体主题
public class RealObserver extends Subjecct {
    //被观察对象的属性
    private int state;
    public int getState(){
        return state;
    }
    public void  setState(int state){
        this.state=state;
        //主题对象(目标对象)值发生改变
        this.notifyAllObserver(state);
    }
}
```
5、运行测试

```java
package com.yoodb.blog;

public class Client {

	public static void main(String[] args) {
		// 目标对象
		RealObserver subject = new RealObserver();
		// 创建多个观察者
		ObserverImpl obs1 = new ObserverImpl();
		ObserverImpl obs2 = new ObserverImpl();
		ObserverImpl obs3 = new ObserverImpl();
		// 注册到观察队列中
		subject.registerObserver(obs1);
		subject.registerObserver(obs2);
		subject.registerObserver(obs3);
		// 改变State状态
		subject.setState(300);
		System.out.println("obs1观察者的MyState状态值为："+obs1.getMyState());
		System.out.println("obs2观察者的MyState状态值为："+obs2.getMyState());
		System.out.println("obs3观察者的MyState状态值为："+obs3.getMyState());
		// 改变State状态
		subject.setState(400);
		System.out.println("obs1观察者的MyState状态值为："+obs1.getMyState());
		System.out.println("obs2观察者的MyState状态值为："+obs2.getMyState());
		System.out.println("obs3观察者的MyState状态值为："+obs3.getMyState());
	}
}
```
# 19. Java 中解释器模式有什么优点？
解释器模式提供了一个简单的方式来执行语法，而且容易修改或者扩展语法。

一般系统中很多类使用相似的语法，可以使用一个解释器来代替为每一个规则实现一个解释器。而且在解释器中不同的规则是由不同的类来实现的，这样使得添加一个新的语法规则变得简单。

# 20. Java 中什么是适配器模式？
适配器模式是把一个类的接口变换成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起工作的两个类能够在一起工作。

适配器模式中所涉及的角色如下：

1）目标接口（Target）：客户所期待的接口。目标可以是具体的或抽象的类，也可以是接口。

2）需要适配的类（Adaptee）：需要适配的类或适配者类。

3）适配器（Adapter）：通过包装一个需要适配的对象，把原接口转换成目标接口。
