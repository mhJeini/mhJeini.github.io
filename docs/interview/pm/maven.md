# 1. 什么是 Maven？
Maven即为项目对象模型（POM），它可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具软件。

Maven 除了以程序构建能力为特色之外，还提供高级项目管理工具。

由于Maven的缺省构建规则有较高的可重用性，所以常常用两三行Maven构建脚本就可以构建简单的项目。

由于Maven面向项目的方法，许多Apache Jakarta项目发文时使用Maven，而且公司项目采用Maven的比例在持续增长，相比较Gradle，在之后的篇幅中会说明，欢迎大家关注微信公众号“Java精选”。

Maven这个单词来自于意第绪语（犹太语），意为知识的积累，最初在Jakata Turbine项目中用来简化构建过程。

当时有一些项目（有各自Ant build文件），仅有细微的差别，而JAR文件都由CVS来维护。于是希望有一种标准化的方式构建项目，一个清晰的方式定义项目的组成，一个容易的方式发布项目的信息，以及一种简单的方式在多个项目中共享JARs。

#  2. 为什么选用 Maven 进行构建？
1）Maven是一个优秀的项目构建工具。

使用Maven可以比较方便的对项目进行分模块构建，这样在开发或测试打包部署时，会大大的提高效率。

2）Maven可以进行依赖的管理。

使用Maven 可以将不同系统的依赖进行统一管理，并且可以进行依赖之间的传递和继承。

Maven可以解决jar包的依赖问题，根据JAR包的坐标去自动依赖/下载相关jar，通过仓库统一管理jar包。

多个项目JAR包冗余，使用Maven解决一致性问题。

4）屏蔽开发工具之间的差异，例如：IDE，Eclipse，maven项目可以无损导入其他编辑器。

# 3. Maven 规约是什么？
src/main/java 存放项目的类文件（后缀.java文件，开发源代码）

src/main/resources 存放项目配置文件，若没有配置文件该目录可无，如Spring、Hibernate、MyBatis等框架配置文件

src/main/webapp 存放web项目资源文件（web项目需要）

src/test/java 存放所有测试类文件（后缀.java文件，测试源代码）

src/test/resources 测试配置文件，若没有配置文件该目录可无

target 文件编译过程中生成的后缀.class文件、jar、war等

pom.xml maven项目核心配置文件，管理项目构建和依赖的Jar包

Maven负责项目的自动化构建，以编译为例，Maven若果自动进行编译，需要知道Java的源文件保存位置，通过这些规约，不用开发者手动指定位置，Maven就可以清晰的知道相关文件所在位置，从而完成自动编译。

遵循**“约定>>>配置>>>编码”**。即能进行配置的不要去编码指定，能事先约定规则的不要去进行配置。这样既减轻了工作量，也能防止编译出错。

# 4. Maven 常用命令有哪些？
1）mvn clean

清理输出目录默认target/

2）mvn clean compline

编译项目主代码，默认编译至target/classes目录下

3）mvn clean install

maven安装，将生成的JAR包文件复制到本地maven仓库中，其他项目可以直接使用这个JAR包

4）mvn clean test

maven测试，但实际执行的命令有：

```sh
clean:clean
resource:resources
compiler:compile
resources:testResources
compiler:testCompile
```
maven执行test前，先自动执行项目主资源处理，主代码编译，测试资源处理，测试代码编译等工作

测试代码编译通过之后默认在target/test-calsses目录下生成二进制文件，随后执行surefile:test任务运行测试，并输出测试报告，显示运行多少次测试，失败成功等。

5）mvn celan package

maven打包，maven会在打包之前默认执行编译，测试等操作，打包成功之后默认输出在target/目录中

6）mvn help:system

打印出java系统属性和环境变量。

7）echo %MAVEN_HOME%：

查看maven安装路径。

8）mvn deploy

在整合或发布环境下执行，将终版本的包拷贝到远程的repository，使得其他的开发者或者工程可以共享。

9）mvn

检查是否安装了maven。

10）mvn dependency:list

查看当前项目中的已解析依赖

11）mvn dependency:tree

查看当前项目的依赖树

12）mvn dependency:analyse

查看当前项目中使用未声明的依赖和已声明但未使用的依赖

# 5. 什么是 Maven 存储库？
存储库是指存储所有jar和pom.xml文件的目录或位置。

Maven中有3种类型的存储库分别是：

1）本地存储库

当运行任何maven命令时Maven本地存储库都是由maven在本地系统中创建的。

2）中央储存库

Maven社区在Web上创建Maven中央存储库。

3）远程仓库

Maven远程存储库由其他第三方位于Web上。因此，需要在pom.xml文件中手动定义依赖项。

# 6. Maven 构建阶段是什么？
Maven构建生命周期经历了一组阶段，这些阶段称为构建阶段。 例如，默认生命周期由以下阶段组成。

validate 验证

compile 编译

test 测试

package 包

verify 校验

install 安装

deploy 部署

# 7. Maven 的内置构建生命周期是什么？
当开发人员构建Maven项目时，它将基于项目pom.xml配置和命令行选项执行一组明确定义的任务。 此标准任务集创建了Maven构建生命周期。

Maven有三个内置的构建生命周期。

默认（default）：处理项目的构建和部署。

清洁（clean）：处理项目清理。

站点（site）：处理项目站点文档的创建。

默认（default），清洁（clean）和站点（site）。在默认（default）的生命周期处理项目部署，将清洁（clean）的生命周期处理项目的清理，而站点（site）的生命周期处理项目站点文档的创建。

# 8. Maven 中 什么是 MOJO？
MOJO是一个Maven普通的旧Java对象。每个MOJO是Maven中的可执行目标，插件是一个或多个相关mojos的分发。

简单来说，MOJO是一个maven目标，扩展maven中没有的功能。

# 9. Maven 如何管理多模块项目依赖？
通过在父模块中声明dependencyManagement和pluginManagement，然后让子模块通过<parent>元素指定父模块，这样子模块在定义依赖时就可以只定义groupId和artifactId，自动使用父模块的version，统一整个项目依赖的版本。

举例dependencyManagement方式的父类模块

父类pom.xml配置文件部分内容：

```xml
<dependencyManagement>  
	<dependencies>  
		<dependency>  
			<groupId>org.eclipse.persistence</groupId>  
			<artifactId>org.eclipse.persistence.jpa</artifactId>  
			<version>${org.eclipse.persistence.jpa.version}</version>  
			<scope>provided</scope>  
		</dependency>
	</dependencies>
</dependencyManagement>
```
子类pom.xml配置文件部分内容：

```xml
<dependencies>
	<dependency>  
		<groupId>org.eclipse.persistence</groupId>  
		<artifactId>org.eclipse.persistence.jpa</artifactId>  
		<scope>provided</scope>  
	</dependency>  
</dependencies> 
```
Dependencies相对于dependencyManagement，所有生命在dependencies里的依赖都会自动引入，并默认被所有的子项目继承。

# 10. Maven 版本管理都有哪些规范？
版本号定义，通常下载jar包时都会有类似1.2.3-beta-2版本。

约定 < 主版本 >.< 次版本 >.< 增量版本 >-< 里程碑版本 >

“1”：表示该版本的主版本号；

“2”：表示该版本的次版本号；

“3”：表示该版本的增量版本号；

“beta-2”：表示该增量的某一个里程碑，例如SNAPSHOT快照版本、beta、rc、release稳定版等。

主版本：表示项目的重大架构变更，例如：Maven2和Maven1相去甚远；Struts1和 Struts2采用了不同的架构。

次版本：表示较大范围的功能增加和变化及Bug修复。例如Nexus1.5较1.4添加了LDAP的支持，并且修复了很多Bug，但是从总体架构来说，没有什么变化。

增量版本：顾名思义，这往往指某一个版本的里程碑。例如Maven3已经发布了很多里程碑版本，3.0-alpha-1 、3.0-alpha-2 、3.0-bata-1等。这里的版本与正式版本3.0相比，往往表示不是非常稳定，还需要进行很多测试。

# 11. Maven 下载依赖包如何更换数据源？
编译Maven项目需要自动从中央仓库（远程仓库、本地仓库）下载项目所需的依赖jar包，但是正常情况下，中央仓库服务在国外，下载速度缓慢，可以将其改为远程仓库下载所需依赖包，即阿里巴巴的镜像。

打开Maven解压目录下conf文件夹，修改settings.xml文件，更换成阿里镜像，提高下载依赖包速度。

```xml
<mirror>
	<id>nexus-aliyun</id>
	<mirrorOf>central</mirrorOf>
	<name>Nexus aliyun</name>
	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```
# 12. Maven 中 <dependencie/> 是什么？
dependencie参数依赖关系，属性如下：

- groupId：依赖项的groupId。

- artifactId：依赖项的artifactId。

- version：依赖项的version。

- scope：依赖项的适用范围。

- compile：默认值，适用于所有阶段（开发、测试、部署、运行），jar会一直存在所有阶段。 provided：只在开发、测试阶段使用，目的是不让Servlet容器和自身本地仓库的jar包冲突。如servlet.jar。 runtime：只在运行时使用，如JDBC驱动，适用运行和测试阶段。 test：只在测试时使用，用于编译和运行测试代码，不会随项目发布。 system：类似provided，需要显式提供包含依赖的jar包，Maven不会在Repository中查找它。 import：用于一个<dependencyManagement/>对另一个<dependencyManagement/>的继承。它可以实现类似Maven Spring BOM（billofmaterials）的功能。

- exclusions：排除项目中的依赖冲突时使用。

# 13. Maven 中 LASTEST、RELEASE、SNAPSHOT 有哪些区别？
LASTEST：是指某个特定构件最新的发布版或者快照版（SNAPSHOT），最近被部署到某个特定仓库的构件。

RELEASE：是指仓库中最后的一个非快照版本，代表稳定的版本。

SNAPSHOT：泛指。版本一般用于开发过程中，代表不稳定、尚处于开发中的版本。

# 14. Maven 中工程都有哪些类型？
POM工程

POM工程是逻辑工程。用在父级工程或聚合工程中。用来做jar包的版本控制。

JAR工程

将系统打包成jar工程，用作jar包使用。是最常见的本地工程Java Project。

WAR工程

将系统打包成war工程，发布在服务器上的工程。比如网站或服务。

常见的网络工程Dynamic Web Project。

war工程默认没有WEB-INF目录及web.xml配置文件，但是在IDE工具中通常会显示工程错误，需要提供完整工程结构才可以解决问题。

# 15. Maven 中有哪些依赖原则？
1）依赖路径最短优先原则。

项目Demo依赖了两个jar包，其中A-B-C-X(1.0)和A-D-X(2.0)版本。由于X(2.0)路径最短，因此项目中使用的是X(2.0)版本。

2）pom文件中申明顺序优先。

如果A-B-X(1.0)和A-C-X(2.0)版本，相同路径长度的情况下，maven会根据pom文件声明的顺序加载，如果先声明B，后声明C，那就最后的依赖就会是X(1.0)版本。

3）覆写优先原则。

子pom文件内声明的优先于父pom中的依赖包。

# 16. Maven 中 dependencies 和 dependencyManagement 有什么区别？
dependencies即使在子项目中不写该依赖项，那么子项目仍然会从父项目中继承该依赖项（全部继承）。

dependencyManagement里只是声明依赖，并不实现引入，因此子项目需要显示的声明需要用的依赖。如果不在子项目中声明依赖，是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope都读取自父pom;另外如果子项目中指定了版本号，那么会使用子项目中指定的jar版本。

dependencies中的jar直接加到项目中，管理的是依赖关系（如果有父pom和子pom，则子pom中只能被动接受父类的版本）；

dependencyManagement主要管理版本，对于子类继承同一个父类是很有用的，集中管理依赖版本不添加依赖关系，对于其中定义的版本，子pom不一定要继承父pom所定义的版本。

# 17. Maven 中如何避免子工程引用不同版本导致编译出错？
TODO

# 18. Maven 中依赖的解析机制是什么？
解析发布(RELEASE)版本：如果本地有，直接使用本地的，没有的话就会向远程仓库请求。

解析快照(SNAPSHOT)版本：合并本地和远程仓库的元数据文件groupId/artifactId/version/maven-metadata.xml ，这个文件存的版本都是带时间戳的，将最新的一个改名为不带时间戳的格式供本次编译使用。

解析版本为LATEST过于复杂，且解析的结果不稳定，不推荐在项目中使用，感兴趣的同学自己去研究，简而言之就是合并groupId/artifactId/maven-metadata.xml 找到对应的最新版本和包含快照的最新版本。

# 19. Maven 中如何解决 jar 包冲突？
第一步，查找Maven加载的时候是什么版本的jar包，通过使用“mvn dependency:tree”命令查看依赖树，或者使用IDEA Maven Helper插件。

第二步，通过Maven的依赖原则来调整坐标在pom文件的申明顺序或者使用将冲突中不想要的jar引入的jar进行<exclusions>去除掉。

项目中引入另一个maven项目依赖，通过依赖传递，会将jar包传递进来，如果不需要某个jar包就可以使用如下配置：

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-core</artifactId>
  <version>${spring.version}</version>
  <exclusions>
    <exclusion>
      <artifactId>commons-logging</artifactId>
      <groupId>commons-logging</groupId>
    </exclusion>
  </exclusions>
</dependency>
```
# 20. 什么是 Maven 插件？
Maven生命周期的每一个阶段的具体实现都是由Maven插件实现的。插件通常提供了一个目标的集合，并且可以使用下面的语法执行：


mvn [plugin-name]:[goal-name]
Maven提供了两种类型的插件：

1）Build plugins：在构建时执行，并在pom.xml的元素中配置。

2）Reporting plugins：在网站生成过程中执行，并在pom.xml的元素中配置。

一些常用插件列表：

clean：构建之后清理目标文件。删除目标目录。

compiler：编译Java源文件。

surefile：运行JUnit单元测试。创建测试报告。

jar：从当前工程中构建JAR文件。

war：从当前工程中构建WAR文件。

javadoc：为工程生成Javadoc。

antrun：从构建过程的任意一个阶段中运行一个ant任务的集合。