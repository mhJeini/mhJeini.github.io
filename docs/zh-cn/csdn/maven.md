
## 下载地址：[Maven – Download Apache Maven](https://maven.apache.org/download.cgi)

​![maven 下载页面](https://img-blog.csdnimg.cn/b55d723e6b93470aafd30b0c3727e974.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

## 环境变量配置

### 在系统变量中添加"MAVEN_HOME" 下载后maven的目录

![](https://img-blog.csdnimg.cn/dfc376cabeb74fee931181c9f39be72d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

###  "M2_HOME" maven目录下得bin目录

### 系统变量path中配置 "%MAVEN_HOME%\bin"
![](https://img-blog.csdnimg.cn/63ba04a65a0642b79a41f08f8204a32f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASmllTmlM,size_20,color_FFFFFF,t_70,g_se,x_16)

## 修改settings.xml文件 我的路径：E:\maven\apache-maven-3.8.4\conf\settings.xml

```xml
    <!-- 阿里云镜像 -->
    <mirrors>
        <mirror>         
        <id>nexus-aliyun</id>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url> 
        <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf> 
        </mirror> 
    </mirrors>
```

```xml
    <localRepository>
        <!-- 本地仓库路径 -->
        E:\maven\maven_jar
    </localRepository>
```
>***努力改变自己***