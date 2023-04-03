## xml + <bean/>

新建项目，导入spring包

```xml
    <!-- spring上下文 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.9</version>
    </dependency>

    <!-- 第三方 -->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.2.16</version>
    </dependency>
```

创建Dog类，Cat类
```java
    public class Dog {
    }

    public class Cat {
    }
```

添加spring配置文件，application.xml

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!-- xml声明自己定义的类 -->
        <bean id="dog" class="com.cj.domain.Dog"></bean>
        <bean class="com.cj.domain.Cat"></bean>

        <!-- xml声明第三方类-->
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"></bean>
    </beans>
```

在main方法中运行，初始化上下文，获取bean
```java
    public static void main(String[] args) {
        //获取上下文对象
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        //获取所有定义的bean名称
        String[] beanDefinitionNames = applicationContext.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            System.out.println(beanDefinitionName);
        }
    }
```

控制台打印

![a](../../img/bean-1.png)