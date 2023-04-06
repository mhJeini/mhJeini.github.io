# 1. Java 中工厂模式有什么优势？
1、工厂模式是最常用的实例化对象模式，是用工厂方法代替new操作的一种模式。

2、利用工厂模式可以降低程序的耦合性，为后期的维护修改提供了很大的便利。

3、将选择实现类、创建对象统一管理和控制，从而将调用者跟我们的实现类解耦。

# 2. 说说你理解的 Spring 中工厂模式？
Spring IOC

看过Spring源码就知道，在Spring IOC容器创建bean的过程是使用了工厂设计模式。

Spring中无论是通过xml配置还是通过配置类还是注解进行创建bean，大部分都是通过简单工厂来进行创建的。

当容器拿到了beanName和class类型后，动态的通过反射创建具体的某个对象，最后将创建的对象放到Map中。

# 3. 为什么 Spring IOC 要使用工厂模式创建 Bean？
在实际开发中，如果A对象调用B，B调用C，C调用D的话，程序的耦合性就会变高。耦合大致分为类与类之间的依赖，方法与方法之间的依赖。

以前三层架构编程时，都是控制层调用业务层，业务层调用数据访问层时，都是是直接new对象，耦合性大大提升，代码重复量大、冗余高，对象也是创建比较随意。

为了避免这种情况，Spring使用工厂模式编写一个工厂，由工厂创建Bean，以后如果要对象就直接管工厂要就可以。

Spring IOC容器的工厂中有个静态的Map集合，是为了让工厂符合单例设计模式，即每个对象只生产一次，生产出对象后就存入到Map集合中，保证了实例不会重复影响程序效率。

# 4. Java 中工厂模式分为哪几大类？
工厂模式分为三类，如下：

1）简单工厂模式（Simple Factory），又称静态工厂方法模式，是由一个具体的类去创建其他类的实例，父类是相同的，父类是具体的， 不利于产生系列产品；

2）工厂方法模式（Factory Method），又称为多形性工厂是有一个抽象的父类定义公共接口，子类负责生成具体的对象，这样做的目的是将类的实例化操作延迟到子类中完成；

3）抽象工厂模式（Abstract Factory），又称为工具箱，提供一个创建一系列相关或相互依赖对象的接口，而无须指定他们具体的类。它针对的是有多个产品的等级结构，产生产品族但不利于产生新的产品，而工厂方法模式针对的是一个产品的等级结构。

# 5. Java 中什么是简单工厂模式？
简单工厂模式又称静态工厂方法模式，在简单工厂模式中，一个工厂类处于对产品类实例化调用的中心位置上，它决定那一个产品类应当被实例化，如同一个交通警察站在来往的车辆流中，决定放行那一个方向的车辆向那一个方向流动一样。

简单工厂模式的具体组成如下：

1）工厂类角色：这是本模式的核心，含有一定的商业逻辑和判断逻辑，用来创建产品。

2）抽象产品角色：它一般是具体产品继承的父类或者实现的接口。

3）具体产品角色：工厂类所创建的对象就是此角色的实例。在java中由一个具体类实现。

下面举例猫与狗吃饭的故事，具体代码实现如下：

1）创建共用接口，代码如下：

```java
package com.yoodb;

public interface Dinner{
    public void eat();
}
```

2）创建实现类，代码如下：

```java
package com.yoodb;

public class CatDinner implements Dinner{

    @Override
    public void eat() {
        // TODO Auto-generated method stub
        System.out.println("小猫来吃饭了！");
    }

}
```


```java
package com.yoodb;

public class DogDinner implements Dinner{

    @Override
    public void eat() {
        // TODO Auto-generated method stub
        System.out.println("小狗来吃饭了！");
    }

}
```


3）创建工厂，代码如下：

```java
package com.yoodb;

public class DinnerFactory {

    public Dinner produce(String animal){
        if(animal.equals("cat")){
            return new CatDinner();
        }else if(animal.equals("dog")){
            return new DogDinner();
        }else{
            System.out.println("请输入存在的动物！");  
            return null;  
        }
    }
}
```

或者静态化，代码如下：

```java
package com.yoodb;

public class DinnerFactory {

    public static Dinner produce(String animal){
        if(animal.equals("cat")){
            return new CatDinner();
        }else if(animal.equals("dog")){
            return new DogDinner();
        }else{
            System.out.println("请输入存在的动物！");  
            return null;  
        }
    }
}
```

4）main函数测试，代码如下：

```java
package com.yoodb;
public class FactoryTest {
    public static void main(String[] args) {
        DinnerFactory factory = new DinnerFactory();
        Dinner dinner = factory.produce("cat");
        dinner.eat();
    }
}
```

或者静态化调用，代码如下：

```java
package com.yoodb;

public class FactoryTest {
public static void main(String[] args) {

        Dinner dinner = DinnerFactory.produce("cat");
        dinner.eat();
    }
}
```

# 6. Java 中简单工厂模式有什么优缺点？
简单工厂模式优点：简单工厂模式能够根据外界给定的信息，决定究竟应该创建哪个具体类的对象。明确区分了各自的职责和权力，有利于整个软件体系结构的优化。

简单工厂模式缺点：很明显工厂类集中了所有实例的创建逻辑，容易违反GRASPR的高内聚的责任分配原则。

# 7. Java 中什么是工厂方法模式？
工厂方法模式Factory Method，又称多态性工厂模式。

工厂方法模式是简单工厂模式的进一步抽象化和推广，工厂方法模式里不再只由一个工厂类决定那一个产品类应当被实例化，这个决定被交给抽象工厂的子类去做。

工厂方法模式的组成具体如下：

1）抽象工厂角色： 这是工厂方法模式的核心，它与应用程序无关。是具体工厂角色必须实现的接口或者必须继承的父类。在java中它由抽象类或者接口来实现。

2）具体工厂角色：它含有和具体业务逻辑有关的代码。由应用程序调用以创建对应的具体产品的对象。

3）抽象产品角色：它是具体产品继承的父类或者是实现的接口。在java中一般有抽象类或者接口来实现。

4）具体产品角色：具体工厂角色所创建的对象就是此角色的实例。在java中由具体的类来实现。工厂方法模式使用继承自抽象工厂角色的多个子类来代替简单工厂模式中的“上帝类”。这样便分担了对象承受的压力且使得结构变得灵活起来。当有新的产品（即暴发户的汽车）产生时，只要按照抽象产品角色、抽象工厂角色提供的合同来生成，那么就可以被客户使用，而不必去修改任何已有的代码。

可以看出工厂角色的结构也是符合开闭原则的。

1）创建共用接口，代码如下：

```java
package com.yoodb;

public interface Dinner{
    public void eat();
}
```

2）创建实现类，代码如下：

```java
package com.yoodb;

public class CatDinner implements Dinner{

    @Override
    public void eat() {
        // TODO Auto-generated method stub
        System.out.println("小猫来吃饭了！");
    }

}
```


```java
package com.yoodb;

public class DogDinner implements Dinner{

    @Override
    public void eat() {
        // TODO Auto-generated method stub
        System.out.println("小狗来吃饭了！");
    }

}
```
3）将简单工厂模式中创建工厂类，进行如下变动代码（简单工厂模式，参见微信小程序Java精选面试题，内涵3000+道面试题）如下：

```java
package com.yoodb;

public class DinnerFactory {

    public Dinner catProduce(){
        return new CatDinner();
    }
     
    public Dinner dogProduce(){
        return new DogDinner();
    }
}
```
4）main函数测试，代码如下：

```java
package com.yoodb;

public class FactoryTest {
    public static void main(String[] args) {
        DinnerFactory factory = new DinnerFactory();
        Dinner dinner = factory.produce("cat");
        dinner.eat();
    }
}
```
# 8. Java 中工厂方法模式有什么应用场景？
在以下情况下，适用于工厂方法模式：

1）当一个类不知道它所必须创建的对象的类的时候。

2）当一个类希望由它的子类来指定它所创建的对象的时候。

3）当类将创建对象的职责委托给多个帮助子类中的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候。

# 9. Java 中什么是抽象工厂模式？
抽象工厂简单地说是工厂的工厂，抽象工厂可以创建具体工厂，由具体工厂来产生具体产品。

1）创建共用接口，代码如下：

```java
package com.yoodb;

public interface Dinner{
    public void eat();
}
```
2）创建实现类，代码如下：

```java
package com.yoodb;

public class CatDinner implements Dinner{

    @Override
    public void eat() {
        // TODO Auto-generated method stub
        System.out.println("小猫来吃饭了！");
    }

}
```

```java
package com.yoodb;

public class DogDinner implements Dinner{

    @Override
    public void eat() {
        // TODO Auto-generated method stub
        System.out.println("小狗来吃饭了！");
    }

}
```
3）创建接口，代码如下：

```java
package com.yoodb;

public interface Provider {  
    public Sender produce();  
}
```
4）创建两个工厂，代码如下：

```java
package com.yoodb;

public class CatFactory implements Provider{

    public Dinner produce(){
        return new CatDinner();
    }
}
```
```java
package com.yoodb;

public class DogFactory implements Provider{

    public Dinner produce(){
        return new DogDinner();
    }
}
```
5）main函数测试，代码如下：

```java
package com.yoodb;

public class FactoryTest {
    public static void main(String[] args) {
        Provider factory = new DogFactory();
        Dinner dinner = factory.produce();
        dinner.eat();
    }
}
```
# 10. Java 中抽象工厂模式有什么应用场景？
1）一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节，这对于所有形态的工厂模式都是重要的。

2）这个系统有多于一个的产品族，而系统只消费其中某一产品族。

3）同属于同一个产品族的产品是在一起使用的，这一约束必须在系统的设计中体现出来。

4）系统提供一个产品类的库，所有的产品以同样的接口出现，从而使客户端不依赖于实现。

# 11. Java 中代理模式有什么应用场景？
Spring AOP、日志打印、异常处理、事务控制、权限控制等。

# 12. Java 中代理模式有几种分类？
Java中代理模式有3种分类：

1、静态代理（静态定义代理类）。

2、动态代理（动态生成代理类，也称为Jdk自带动态代理）。

3、Cglib 、javaassist（字节码操作库）。

# 13. Java 中三种代理模式有什么区别？
1、静态代理：简单代理模式，是动态代理的理论基础。常见使用在代理模式。

2、jdk动态代理：使用反射完成代理。需要有顶层接口才能使用，常见是mybatis的mapper文件是代理。

3、cglib动态代理：也是使用反射完成代理，可以直接代理类（jdk动态代理不行），使用字节码技术，不能对final类进行继承。（需要导入jar包）。

# 14. Java 中代理模式如何实现 CGLIB 动态代理？
CGLIB动态代理和jdk代理一样，使用反射完成代理，不同的是他可以直接代理类（jdk动态代理不行，他必须目标业务类必须实现接口），CGLIB动态代理底层使用字节码技术，CGLIB动态代理不能对 final类进行继承。（CGLIB动态代理需要导入jar包）。

CGLIB动态代理原理是利用asm开源包，对代理对象类的class文件加载进来，通过修改其字节码生成子类来处理。

1）接口，具体代码如下：

```java
package com.yoodb.blog;

public interface UserDao {
    void save();
}
```
2）实现业务逻辑类，具体代码如下：

```java
package com.yoodb.blog;

//接口实现类
public class UserDaoImpl implements UserDao {
    public void save() {
        System.out.println("保存数据方法");
    }
}
```
3）代理主要类，具体代码如下：

```java
package com.yoodb.blog;

import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;
import java.lang.reflect.Method;


public class CglibProxy implements MethodInterceptor {
    private Object targetObject;
    // 这里的目标类型为Object，则可以接受任意一种参数作为被代理类，实现了动态代理
    public Object getInstance(Object target) {
        // 设置需要创建子类的类
        this.targetObject = target;
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(target.getClass());
        enhancer.setCallback(this);
        return enhancer.create();
    }

	//代理实际方法
	public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
		System.out.println("开启事物");
		Object result = proxy.invoke(targetObject, args);
		System.out.println("关闭事物");
		// 返回代理对象
		return result;
	}
}
```
4）测试CGLIB动态代理，具体代码如下：

```java
package com.yoodb.blog;

public class Test {
public static void main(String[] args) {
    CglibProxy cglibProxy = new CglibProxy();
        UserDao userDao = (UserDao) cglibProxy.getInstance(new UserDaoImpl());
        userDao.save();
    }
}
```
# 15. Java 中什么是建造者模式？
建造模式是一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示，将其复杂的内部创建封装在内部，对于外部调用的人来说，只需要传入建造者和建造工具，对于内部是如何建造成成品的，调用者无需关心。

建造者模式涉及四个角色分别如下：

1、抽象建造者（Builder）角色：给 出一个抽象接口，以规范产品对象的各个组成成分的建造。一般而言，此接口独立于应用程序的商业逻辑。模式中直接创建产品对象的是具体建造者 (ConcreteBuilder)角色。具体建造者类必须实现这个接口所要求的两种方法：一种是建造方法(buildPart1和 buildPart2)，另一种是返还结构方法(retrieveResult)。一般来说，产品所包含的零件数目与建造方法的数目相符。换言之，有多少 零件，就有多少相应的建造方法。

2、具体建造者（ConcreteBuilder）角色：担任这个角色的是与应用程序紧密相关的一些类，它们在应用程序调用下创建产品的实例。这个角色要完成的任务包括：

1）实现抽象建造者Builder所声明的接口，给出一步一步地完成创建产品实例的操作。 2）在建造过程完成后，提供产品的实例。

3、导演者（Director）角色：担任这个角色的类调用具体建造者角色以创建产品对象。应当指出的是，导演者角色并没有产品类的具体知识，真正拥有产品类的具体知识的是具体建造者角色。

4、产品（Product）角色：产品便是建造中的复杂对象。一般来说，一个系统中会有多于一个的产品类，而且这些产品类并不一定有共同的接口，而完全可以是不相关联的。

# 16. Java 中建造者模式有什么使用场景？
1、需要生成的对象具有复杂的内部结构。

2、需要生成的对象内部属性本身相互依赖。

建造者模式与工厂模式的区别是建造者模式更加关注与零件装配的顺序。

JAVA中的StringBuilder就是建造者模式创建的，它把一个单个字符的char数组组合起来，而Spring不是建造者模式，它提供的操作应该是对于字符串本身的一些操作，而不是创建或改变一个字符串。

# 17. Java 中如何实现建造者模式？
1、创建产品对象，具体代码如下：

```java
package com.yoodb;

public class Product {
private String partA;
private String partB;

    public String getPartA() {
        return partA;
    }
 
    public void setPartA(String partA) {
        this.partA = partA;
    }
 
    public String getPartB() {
        return partB;
    }
 
    public void setPartB(String partB) {
        this.partB = partB;
    }

}
```
2、创建接口，具体代码如下：

```java
package com.yoodb;

public interface Builder {
public void buildPartA();
public void buildPartB();

    public Product getResult();

}
```
3、实现Builder的接口以构造和装配该产品的各个部件， 定义并明确它所创建的表示，具体代码如下：

```java
package com.yoodb;

public class ConcreteBuilder implements Builder {

    private Product product = new Product();
     
    @Override
    public void buildPartA() {
        // TODO Auto-generated method stub
        product.setPartA("产品-yoodb-001");
    }
 
    @Override
    public void buildPartB() {
        // TODO Auto-generated method stub
        product.setPartB("产品-yoodb-002");
    }
 
    @Override
    public Product getResult() {
        // TODO Auto-generated method stub
        return product;
    }

}
```
4、构造一个使用Builder接口的对象，具体代码如下：

```java
package com.yoodb;

public class Director {
private Builder builder;

    public Director(Builder builder) {
        this.builder = builder;
    }
     
    public void construct(){
        builder.buildPartA();
        builder.buildPartB();
    }

}
```
5、测试实现，具体代码如下：

```java
package com.yoodb;

public class ClientMain {
    public static void main(String[] args) {
        Builder builder = new ConcreteBuilder();
        Director director = new Director(builder);
        director.construct();
        Product product = builder.getResult();
        System.out.println(product.getPartA());
        System.out.println(product.getPartB());
    }
}
```
# 18. Java 中什么是模板方法模式？
模板方法模式是所有模式中最为常见的几个模式之一，是基于继承的代码复用的基本技术。

模板方法模式是通过把不变行为搬到超类，去除子类里面的重复代码提现它的优势，它提供了一个很好的代码复用平台。

当不可变和可变的方法在子类中混合在一起的时候，不变的方法就会在子类中多次出现，这样如果某个方法需要修改则需要修改很多个地方，虽然这个问题就应该在设计之初想好。此时模板方法模式就起到了作用，通过模板方法模式把这些重复出现的方法搬到单一的地方，这样就可以帮助子类摆脱重复不变的纠缠。

# 19. Java 中什么时候使用模板方法模式？
实现一些操作时整体步骤很固定，但是其中一小部分需要改变，这时候可以使用模板方法模式，将容易变的部分抽象出来，供子类实现。

# 20. Java 中模板方法模式有什么应用场景？
在很多框架中都有使用模板方法模式。

例如：数据库访问的封装、Junit单元测试、servlet中关于doGet/doPost方法的调用等等。
