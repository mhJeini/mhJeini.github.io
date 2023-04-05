# 1. Mybatis 中如何解决实体类属性名和表字段名不一致问题？
方式一：通过在查询的sql语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。Java精选面试题微信小程序内涵3000+道面试题。

```sql
<select id="selectOrder" parametertype="int" resultetype="com.jingxuan.Order">
	select order_id id, order_no orderno ,order_price price form orders where order_id=#{id};
</select>
```
方式二：通过来映射字段名和实体类属性名的一一对应的关系。

```sql
<select id="getOrder" parameterType="int" resultMap="orderresultmap">
	select * from orders where order_id=#{id}
</select>

<resultMap type="com.jingxuan.Order" id="orderresultmap">
	<!–用id属性来映射主键字段–>
	<id property="id" column="order_id">

	<!–用result属性来映射非主键字段，property为实体类属性名，column为数据表中的属性–>
	<result property = "orderno" column ="order_no"/>
	<result property="price" column="order_price" />
</reslutMap>
```
# 2. Mybatis 中如何获取自动生成的主键值？
方式一：对于支持自动生成主键的数据库，如Mysql、sqlServer，可以通过Mybatis元素useGeneratedKeys返回当前插入数据主键值到输入类中。

```sql
<insert id="insertJingXuan" useGeneratedKeys="true" keyProperty="id" parameterType="com.jx.domain.IdentityUser">
    insert into identity_user(name)
    values(#{name,jdbcType=VARCHAR})
</insert>
```
方式二：对于不支持自动生成主键的数据库。Oracle、DB2等，可以用元素selectKey回当前插入数据主键值到输入类中。

```sql
<insert id="insertJingXuan" useGeneratedKeys="true" keyProperty="id" parameterType="com.jx.domain.IdentityUser">
　  <selectKey keyProperty="id" resultType="String" order="BEFORE">
        SELECT  REPLACE(UUID(),'-','')  
    </selectKey>
    insert into identity_user(name)
    values(#{name,jdbcType=VARCHAR})
</insert>
```
selectKey元素说明

keyProperty：selectKey语句结果应该被设置的目标属性。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表。

keyColumn：匹配属性的返回结果集中的列名称。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表。

resultType：结果的类型。MyBatis通常可以推算出来，但是为了更加确定写上也不会有什么问题。MyBatis允许任何简单类型用作主键的类型，包括字符串。如果希望作用于多个生成的列，则可以使用一个包含期望属性的Object或一个Map。

order：这可以被设置为BEFORE或AFTER。如果设置为BEFORE，那么它会首先选择主键，设置keyProperty然后执行插入语句。如果设置为AFTER，那么先执行插入语句，然后是selectKey元素-这和像Oracle的数据库相似，在插入语句内部可能有嵌入索引调用。

statementType：MyBatis支持STATEMENT，PREPARED和CALLABLE语句的映射类型，分别代表PreparedStatement和CallableStatement类型。

# 3. 什么是 MyBatis 接口绑定？有哪些实现方式？
接口绑定是指在MyBatis中任意定义接口，然后把接口里面的方法和SQL语句绑定，我们直接调用接口方法就可以，这样比起原来了SqlSession提供的方法我们可以有更加灵活的选择和设置。

接口绑定有两种实现方式：

一种是通过注解绑定，就是在接口的方法上面加上@Select、@Update等注解，里面包含Sql语句来绑定；

另外一种就是通过xml里面写SQL来绑定，在这种情况下，要指定xml映射文件里面的namespace必须为接口的全路径名。当Sql语句比较简单时候，用注解绑定，当SQL语句比较复杂时候，用xml绑定，一般用xml绑定的比较多。

# 4. Mybatis 中分页插件的原理是什么？
Mybatis分页插件的基本原理是使用mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，然后重写sql，根据dialect方言，添加对应的物理分页语句和物理分页参数。

# 5. Mybatis 插件运行原理，如何编写一个插件？
mybatis仅可以编写针对parameterhandler、resultsethandler、statementhandler、executor这4种接口的插件。

mybatis使用jdk的动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行这4种接口对象的方法时，就会进入拦截方法，具体就是invocationhandler的invoke()方法，当然，只会拦截那些你指定需要拦截的方法。

编写插件：实现mybatis的interceptor接口并重写intercept()方法，然后在给插件编写注解，指定要拦截哪一个接口的哪些方法即可，并在配置文件中配置编写的插件。

# 6. Mybatis 中 Mapper 编写有哪几种方式？
方式一：接口实现类继承SqlSessionDaoSupport

使用此种方法需要编写mapper接口，mapper接口实现类、mapper.xml文件。

1）在sqlMapConfig.xml中配置mapper.xml的位置：

```xml
<mappers>
        <mapper resource="mapper.xml 文件的地址" />
        <mapper resource="mapper.xml 文件的地址" />
</mappers>
```
2）定义mapper接口：

3）实现类集成SqlSessionDaoSupport：mapper方法中可以this.getSqlSession()进行数据增删改查。

4）spring 配置：

```xml
<bean id="对象ID" class="mapper 接口的实现">
    <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
</bean>
```
方式二：使用org.mybatis.spring.mapper.MapperFactoryBean

1）在sqlMapConfig.xml中配置mapper.xml的位置，如果mapper.xml和mappre接口的名称相同且在同一个目录，这里可以不用配置

```xml
<mappers>
        <mapper resource="mapper.xml 文件的地址" />
        <mapper resource="mapper.xml 文件的地址" />
</mappers>
```
2）定义mapper接口：

① mapper.xml中的namespace为mapper接口的地址

② mapper接口中的方法名和mapper.xml中的定义的statement的id保持一致

③ Spring中定义：

```xml
<bean id="" class="org.mybatis.spring.mapper.MapperFactoryBean">
    <property name="mapperInterface" value="mapper 接口地址" />
    <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>
```
方式三：使用mapper扫描器

1）mapper.xml文件编写：

mapper.xml中的namespace为mapper接口的地址；

mapper接口中的方法名和mapper.xml中的定义的statement的id保持一致；

如果将mapper.xml和mapper接口的名称保持一致则不用在sqlMapConfig.xml中进行配置。

2）定义mapper接口：

注意mapper.xml的文件名和mapper的接口名称保持一致，且放在同一个目录

3）配置mapper扫描器：

```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="mapper接口包地址" />
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
</bean>
```
4）使用扫描器后从spring容器中获取mapper的实现对象。

# 7. Mybatis 中有哪些 Executor 执行器？它们之间有什么区别？
Mybatis有三种基本的Executor执行器，SimpleExecutor、ReuseExecutor、BatchExecutor。

SimpleExecutor： 每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。

ReuseExecutor： 执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map<String, Statement>内，供下一次使用。简言之，就是重复使用Statement对象。

BatchExecutor： 执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理。与JDBC批处理相同。

作用范围：Executor的这些特点，都严格限制在SqlSession生命周期范围内。

# 8. Mybatis 中如何指定使用哪种 Executor 执行器？
Mybatis配置文件中，可以指定默认的ExecutorType执行器类型，也可以手动给DefaultSqlSessionFactory的创建SqlSession的方法传递ExecutorType类型参数。

# 9. Mybatis 是否可以映射 Enum 枚举类？
Mybatis可以映射枚举类，不单可以映射枚举类，Mybatis可以映射任何对象到表的一列上。

映射方式为自定义一个TypeHandler，实现TypeHandler的setParameter()和getResult()接口方法。

TypeHandler有两个作用，一是完成从javaType至jdbcType的转换，二是完成jdbcType至javaType的转换，体现为setParameter()和getResult()两个方法，分别代表设置sql问号占位符参数和获取列查询结果。

# 10. Mybatis映射文件中A标签使用include引用B标签内容，B标签能否定义在A标签的后面，还是说必须定义在A标签的前面？
虽然Mybatis解析Xml映射文件是按照顺序解析的，但是，被引用的B标签依然可以定义在任何地方，Mybatis都可以正确识别。

原理是Mybatis解析A标签，发现A标签引用了B标签，但是B标签尚未解析到，尚不存在，此时，Mybatis会将A标签标记为未解析状态，然后继续解析余下的标签，包含B标签，待所有标签解析完毕，Mybatis会重新解析那些被标记为未解析的标签，此时再解析A标签时，B标签已经存在，A标签也就可以正常解析完成了。

# 11. Mybatis 的 Xml 映射文件和 Mybatis 内部数据结构之间的映射关系？
Mybatis将所有Xml配置信息都封装到All-In-One重量级对象Configuration内部。

在Xml映射文件中，<parameterMap>标签会被解析为ParameterMap对象，其每个子元素会被解析为ParameterMapping对象。

<resultMap>标签会被解析为ResultMap对象，其每个子元素会被解析为ResultMapping对象。每一个<select>、<insert>、<update>、<delete>标签均会被解析为MappedStatement对象，标签内的sql会被解析为BoundSql对象。

# 12. 通常一个mapper.xml文件，都会对应一个Dao接口，这个Dao接口的工作原理是什么？Dao接口里的方法，参数不同时，方法能重载吗？
Mapper接口的工作原理是JDK动态代理，Mybatis运行时会使用JDK动态代理为Mapper接口生成代理对象proxy，代理对象会拦截接口方法，根据类的全限定名+方法名，唯一定位到一个MapperStatement并调用执行器执行所代表的sql，然后将sql执行结果返回。

Mapper接口里的方法，是不能重载的，因为是使用 全限名+方法名 的保存和寻找策略。

Dao接口即Mapper接口。接口的全限名，就是映射文件中的namespace的值；接口的方法名，就是映射文件中Mapper的Statement的id值；接口方法内的参数，就是传递给sql的参数。

当调用接口方法时，接口全限名+方法名拼接字符串作为key值，可唯一定位一个MapperStatement。在Mybatis中，每一个SQL标签，都会被解析为一个MapperStatement对象。

举例：com.mybatis3.mappers.StudentDao.findStudentById，可以唯一找到namespace为com.mybatis3.mappers.StudentDao下面 id 为 findStudentById 的 MapperStatement。

# 13. MyBatis 中 Mapper 编写有哪几种方式？
第一种： 接口实现类继承SqlSessionDaoSupport：使用此种方法需要编写mapper接口，mapper接口实现类、mapper.xml文件。

1）在sqlMapConfig.xml中配置mapper.xml的位置：

```xml
<mappers>
        <mapper resource="mapper.xml 文件的地址" />
        <mapper resource="mapper.xml 文件的地址" />
</mappers>
```
2）定义mapper接口：

3）实现类集成SqlSessionDaoSupport：mapper方法中可以this.getSqlSession()进行数据增删改查。

4）spring 配置：

```xml
<bean id="对象ID" class="mapper 接口的实现">
    <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
</bean>
```
第二种： 使用org.mybatis.spring.mapper.MapperFactoryBean：

1）在sqlMapConfig.xml中配置mapper.xml的位置，如果mapper.xml和mappre接口的名称相同且在同一个目录，这里可以不用配置

```xml
<mappers>
        <mapper resource="mapper.xml 文件的地址" />
        <mapper resource="mapper.xml 文件的地址" />
</mappers>
```
2）定义mapper接口：

① mapper.xml中的namespace为mapper接口的地址

② mapper接口中的方法名和mapper.xml中的定义的statement的id保持一致

③ Spring中定义：

```xml
<bean id="" class="org.mybatis.spring.mapper.MapperFactoryBean">
    <property name="mapperInterface" value="mapper 接口地址" />
    <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>
```
第三种： 使用mapper扫描器：

1）mapper.xml文件编写：

mapper.xml中的namespace为mapper接口的地址；

mapper接口中的方法名和mapper.xml中的定义的statement的id保持一致；

如果将mapper.xml和mapper接口的名称保持一致则不用在sqlMapConfig.xml中进行配置。

2）定义mapper接口：

注意mapper.xml的文件名和mapper的接口名称保持一致，且放在同一个目录

3）配置mapper扫描器：

```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="mapper接口包地址" />
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
</bean>
```
4）使用扫描器后从spring容器中获取mapper的实现对象。

# 14. MyBatis 中 mapper 如何实现传递多个参数？
TODO

# 15. MyBatis 逻辑分页和物理分页有什么区别？
逻辑分页是一次性查询很多数据，然后再在结果中检索分页的数据。这样做弊端是需要消耗大量的内存、有内存溢出的风险、对数据库压力较大。

物理分页是从数据库查询指定条数的数据，弥补了一次性全部查出的所有数据的种种缺点，比如需要大量的内存，对数据库查询压力较大等问题。

# 16. 简述一下 MyBatis 的工作原理？
在DatasourceAutoConfiguration完成后，会去执行MybatisAutoConfiguration。该自动配置类会开启MybatisProperties类的初始化。先读取MyBatis配置文件，用加载映射文件。（SQL映射文件，其中配置了操作数据库的SQL语句）

构造会话工厂：通过MyBatis的环境等配置信息构建会话工厂SqlSessionFactory。

创建会话对象：有会话工厂创建SqlSession对象，该对象包括了执行SQL语句的所有方法。

Executor执行器：根据SqlSession传递的参数动态的生成需要执行的SQL语句，同时负责查询缓存的维护。

Mappedstatement对象：用于存储要映射的SQL语句的id、参数等信息。

输入参数映射：参数类型可以为Map、List等集合类型也可以使用基本数据类型和POJO类型 输出结果映射：和输入类似。
