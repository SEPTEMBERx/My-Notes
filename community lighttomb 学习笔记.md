# community lighttomb 学习笔记

## git 常用命令

git add .

git commit -m "message"

git pull

git push 

git merge

## Bootstrap

在页面前面引入依赖文件

```html
<head>
    <title>The Lighttomb</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-	8" />
<script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
    <link rel="stylesheet" href="css/bootstrap.min.css">
    <link rel="stylesheet" href="css/bootstrap-theme.min.css">
    <script src="js/bootstrap.min.js" type="application/javascript"></script>
</head>
```

直接从库中调取模板修改

```
https://v3.bootcss.com/components/#navbar-default
```

# IDEA 使用技巧

## 快捷键

Shift + Enter

Shift + F6

## Debug

断点

# Oauth 原理及应用

![1558023090157](C:\Users\gs63vr\AppData\Roaming\Typora\typora-user-images\1558023090157.png)

# H2数据库配置

# Mybatis 逆向工程

## Mapper文件 在 Maven环境中的问题

1. "_"分隔的字段名会自动转化为驼峰形式

2. mybatis默认使用的路径和文件名(无需配置)：src/main/resources/generatorConfig.xml

3. 配置抽离

   ```xml
   <properties resource="application.properties" />
   <jdbcConnection
           driverClass="${spring.datasource.driver-class-name}"
           connectionURL="${spring.datasource.url}"
           userId="${spring.datasource.username}"
           password="${spring.datasource.password}">
   </jdbcConnection>
    <!-- targetProject:生成Model类的位置 -->
   <sqlMapGenerator 
           targetPackage="${Model.targetPackage}"
   		targetProject="${Model.targetProject}">
   <!-- enableSubPackages:是否让schema作为包的后缀 -->
           <property name="enableSubPackages" value="false" />
   </sqlMapGenerator>
   
   ```

# MyBatis映射相关文件

## **查询条件类 xxxExample.java**

Example.java 中包含 

- 静态内部类 Criteria

- 静态内部类 Criterion 

- 抽象静态类内部类 GeneratedCriteria

Criteria 继承了 抽象静态类内部类(Example类的内部)  GeneratedCriteria

```java
public static class Criteria extends GeneratedCriteria {
        protected Criteria() {
            super();
        }
    }
```

 内部的构造方法为super 指向的父类构造方法 

```java
protected GeneratedCriteria() {
    super();
    criteria = new ArrayList <Criterion>();
}
```



Example类用于构造复杂的筛选条件

**Criterion**

Criterion是最基本，最底层的Where条件，用于字段级的筛选。主要有以下这些：

```mysql
只有一个条件不需要参考vaule
field IS NULL
field IS NOT
与一个参考值进行算数运算
field > value
field >= value
field =value
field <> value
field <= value
field < value
field LIKE value
field NOT LIKE value
field BETWEEN value1 AND value2
field IN (item1, item2,...)
field NOT IN (item1, item2, ...)
```

LIKE模糊查询的%,?字符只能在构造查询条件是手动指定。

Mybatis Generator会为每个字段生成如上所示的Criterion，理论上可以构造任何筛选条件，如果字段较多生成的Example类会很大。

## **xxxMapper.xml文件**

sql语句映射配置文件

- mapper元素需要有一个唯一的namespace属性，必须为对应的Mapper接口全名。

  ```xml
  <mapper namespace="com.demo.mappers.PersonMapper">
  ```

- 每个sql语句定义都有一个id，必须与对应的Mapper接口中方法同名，可以指定参数类型和返回类型。

  ```
  <select id="selectByExampleWithBLOBs"   parameterType="com.demos.models.PersonExample"    resultMap="ResultMapWithBLOBs" >
  ```

- 每个sql语句可以定义parameterType和resultType指定参数和返回值类型，也可单独定义parameterMap(见后面的存储过程部分)和resultMap元素然后在sql语句元素中引用。 

# Flyway 

## Why database migrations?

![img](https://flywaydb.org/assets/balsamiq/Environments.png)

code side 有 Version Control 等方法解决

- Recreate a database from scratch  重整数据库
- Make it clear at all times what state a database is in在任何情况下弄清数据库在何种状态
- Migrate in a deterministic way from your current version of the database to a newer one
- 以一个确定的方式将你的现有版本数据库迁移到新的数据库中

## How

核心是用于记录所有版本演化和状态的MetaData表

## 1.point **Flyway** to an **empty database**.

1. Flyway 创建一个**schema history table**

2. used to track the state of the database   

   用来跟踪数据库状态

3. **scanning** the filesystem or the classpath of the application for **migrations**

   扫描迁移用的应用的类路径或文件系统

4. The migrations are then **sorted** based on their **version number** and applied in order:

   随后迁移被按照版本号整理排序

5. As each migration gets applied, the schema history table is updated accordingly:

   当每个迁移完成时,Schema table 随之更新

| installed_rank | version |  description  | type |        script         |  checksum  | installed_by |     installed_on      | execution_time | success |
| :------------: | :------ | :-----------: | :--: | :-------------------: | :--------: | :----------- | :-------------------: | :------------: | :-----: |
|       1        | 1       | Initial Setup | SQL  | V1__Initial_Setup.sql | 1996767037 | axel         | 2016-02-04 22:23:00.0 |      546       |  true   |
|       2        | 2       | First Changes | SQL  | V2__First_Changes.sql | 1279644856 | axel         | 2016-02-06 09:18:00.0 |      127       |  true   |

## 2. **migrating to newer versions**.

1. **scanning** the filesystem or the classpath of the application for **migrations**

   扫描迁移用的应用的类路径或文件系统

2. The migrations are checked against the schema history table.

   对照Schema history 检查迁移

3.  If their version number is lower or equal to the one of the version marked as current, they are ignored. 

   不高于现有版本号的迁移会被忽略.

4. The remaining migrations are the **pending migrations**: available, but not applied.

   其余迁移为等待迁移状态 : 可用但未执行

5. then **sorted by version number** and **executed in order**:

   随后按版本号排序 并顺序执行

6.  **schema history table** is **updated** accordingly:

   Schema History表 随之更新

## 使用Flyway

### 引入依赖

```xml
<project xmlns="...">
    ...
    <build>
        <plugins>
            <plugin>
                <groupId>org.flywaydb</groupId>
                <artifactId>flyway-maven-plugin</artifactId>
                <version>5.2.4</version>
                <configuration>
                    <url>jdbc:h2:file:./target/foobar</url>
                    <user>sa</user>
                    <password>ssss</password>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>com.h2database</groupId>
                        <artifactId>h2</artifactId>
                        <version>1.4.197</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
</project>
```

### resource/db/migaration 创建VX_XXXXXX.sql文件 写入sql语句

![img](file:///C:\Users\gs63vr\AppData\Roaming\Tencent\Users\527392111\TIM\WinTemp\RichOle\4Z$OJ_I1BHJCGG555YU8_]3.png)

#### 命名规则

![img](https://upload-images.jianshu.io/upload_images/9714208-fd8811d63491bbc8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/249/format/webp)



### terminal 执行  mvn flyway:migrate

![1558192602652](C:\Users\gs63vr\AppData\Roaming\Typora\typora-user-images\1558192602652.png)



参考自: [官方文档](https://flywaydb.org/getstarted/)

# 其他知识

Windows 截图快捷键 Win + Shift + S

- ## UserMapper 中

```java
    @Select("SELECT * FROM User where cookie = #{cookie}")
    User findByToken(@Param("cookie") String token);
```

@Param("cookie") 对应的是

UserMapper.xml中的

```xml

    <resultMap id="BaseResultMap" type="com.communitylight.Model.User">
        <id column="ID" jdbcType="INTEGER" property="id"/>
        <result column="COOKIE" jdbcType="CHAR" property="cookie"/>     
    </resultMap>
```



# Proxifier

流式编程

<https://www.jianshu.com/p/5b97e86aa1ee>

<https://www.cnblogs.com/jun-ma/p/4838260.html>



# HttpSession

### 理解

一个session就是一系列某用户和服务器间的通讯。

一个session的建立是从一个用户向服务器发第一个请求开始，而以用户显式结束或session超时为结束。

### 工作原理

1.当一个用户向服务器发送第一个请求时，服务器为其建立一个session，并为此session创建一个标识号；

2.这个用户随后的所有请求都应包括这个标识号。服务器会校对这个标识号以判断请求属于哪个session。

### session标识号（sessionID）两种实现

- cookies

  

- URL重写

### HttpSession的使用

servlet容器就创建了一个HttpSession对象

其中存储了和本session相关的信息

在一个servlet中有多少个不同用户连接，就会有多少个HttpSession对象

### 使用的机理是：

1.从请求中提取HttpSession对象；
2.增加或删除HttpSession中的属性；
3.根据需要关闭HttpSession或使其失效。

```XMl
 <li class="dropdown" th:if="${session.user!=null}">
 	<a href="#" class="dropdown-toggle" data-toggle="dropdown" 		     role="button" aria-haspopup="true"
    aria-expanded="false" th:text=" ${session.user.getUserName()} "> 		<span class="caret"></span>
     </a>
</li>
```

***session.user.getUserName() session 中的 user 对象调用 getUserName ( ) 方法***





