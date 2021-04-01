# MyBatis

## 1、概述

MyBatis是一个优秀的基于java的持久层框架，它内部封装了jdbc，使开发者只需要关注sql语句本身，而不需要花费精力去处理加载驱动、创建连接、创建Statement等繁杂的过程

MyBatis通过xml或注解的方式将要执行的各种statement配置起来，并通过java对象和statement中sql的动态参数进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射为java对象并返回。

采用ORM思想解决了实体和数据库映射的问题，对jdbc进行了封装，屏蔽了 jdbc api 底层访问细节，是我们不用与 jdbc api 细节打交道，就可以完成对数据库的持久化操作。

![01三层架构](E:\BaiduNetdiskDownload\31.会员版(2.0)-就业课(2.0)-Mybatis\56.mybatis\mybatis\mybatis_day01\截图\01三层架构.png)

**JDBC问题分析**

>- 数据库连接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库连接池可解决此问题
>- Sql语句在代码中硬编码，造成代码不易维护，实际应用sql变化的可能性较大，sql变动需要改变java代码
>- 使用PreparedStatement向占有位符号传参数存在硬编码，因为sql语句的where条件不一定，可能多也可能少，修改sql还要修改代码，系统不易维护
>
>- 对结果集解析存在硬编码（查询列名），sql 变化导致解析代码变化，系统不易维护，如果能将数据库记录封装成pojo对象解析比较方便



注：**硬编码和软编码**

>软编码可以在运行时确定，修改；而硬编码是不能够改变的
>
>java小例子： int a=2,b=2;
>硬编码：if(a==2) return false;
>不是硬编码 if(a==b) return true;

## 2、MyBatis的入门

### 2.1、mybatis环境搭建

>- 第一步：创建Maven工程并导入坐标
>- 第二步：创建实体类和dao接口
>
>- 第三步：创建MyBatis的主文件：SqlMapConfig.xml
>- 第四步：创建映射配置文件：IUserDao.xml

环境搭建注意事项：

>- mybatis的映射配置文件位置必须和dao接口的包结构相同
>
>- 映射配置文件的mapper标签namespace属性取值必须是dao接口的全限定类名
>- 映射配置文件的操作配置（select），id属性的取值必须时dao接口的方法名

**当我们遵从了上面三点之后，我们在开发中就无须再写dao的实现类**

![image-20200923233145432](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200923233145432.png)

### 2.2、入门案例

**基于xml的使用**

>- 第一步：读取配置文件
>- 第二步：创建SqlSessionFactory工厂
>- 第三步：创建SqlSession
>- 第四步：创建Dao接口的代理对象
>- 第五步：执行dao中的方法
>- 第六步：释放资源

注意事项：不要忘记在映射配置文件中告知mybatis要封装到哪个实体中

​					配置方式：指定实体类的全限定类名

**基于注解的使用**

把IUserDao.xml注解移除，在dao接口的方法上使用@Select注解，并且指定SQL语句，并且需要在SqlMapConfig.xml中的mapper配置时，使用class属性指定到dao接口的全限定类名

**模式分析**

![入门案例的分析](E:\BaiduNetdiskDownload\31.会员版(2.0)-就业课(2.0)-Mybatis\56.mybatis\mybatis\mybatis_day01\截图\入门案例的分析.png)

>也可以实现dao实现类，但是为了简便，所以都不采用dao实现类的方法
>
>但是MyBatis是支持写dao实现类的

## 3、自定义MyBatis分析

MyBatis在使用代理dao的方式实现增删改查时做什么事呢？

​	只有两件事：

​			第一：创建代理对象

​			第二：在代理对象中调用selectList

![查询所有的分析](E:\BaiduNetdiskDownload\31.会员版(2.0)-就业课(2.0)-Mybatis\56.mybatis\mybatis\mybatis_day01\截图\查询所有的分析.png)

![自定义Mybatis分析](E:\BaiduNetdiskDownload\31.会员版(2.0)-就业课(2.0)-Mybatis\56.mybatis\mybatis\mybatis_day01\截图\自定义Mybatis分析.png)

**自定义MyBatis能通过入门案例看到的类**

>- class Resource
>- class SqlSessionFactoryBuilder
>- interface SqlSessionFactory
>- interface SqlSession

![image-20200927090630557](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200927090630557.png)

**配置properties**

- 可以在标签内部配置连接数据库信息。也可以通过属性引用外部配置文件信息

>1、resource属性：常用的
>
>​		用于指定配置文件的位置，是按照类路径的写法来写，并且必须存在类路径下
>
>2、url属性：是要求按照Url的写法来写地址
>
>​		URL：Uniform Resource Locator 统一资源定位符：它是可以唯一标识一个资源的位置
>
>​		它的写法：
>
>​					http://localhost:8080/mybatisserver/demoServelt
>
>​					协议       主机	   端口     URI
>
>​		URI：Uniform Resource Identifier 统一资源标识符 ：它是在资源中唯一定位一个资源的。

**typeAliases**

在前面我们讲的 Mybatis 支持的默认别名，我们也可以采用自定义别名方式来开发。

在 SqlMapConfig.xml 中配置：

```xml
<typeAliases>
	<!-- 单个别名定义 --> 
	<typeAlias alias=*"user"* type=*"com.itheima.domain.User"*/>
	<!-- 批量别名定义，扫描整个包下的类，别名为类名（首字母大写或小写都可以） --> 
	<package name=*"com.itheima.domain"*/>
	<package name=*"**其它包**"*/>
</typeAliases>
```



## 4、mybatis连接池以及事务控制

### 4.1、mybatis连接池使用及分析

- 连接池：就是一个存储连接的容器，可以减少获取连接所消耗的时间

  容器其实就是一个集合对象，该集合必须是安全的，不能两个线程拿到同意连接

  该集合还必须实现的特性：先进先出

- mybatis连接池：提供三种连接配置

  >配置的位置：
  >
  >​		主配置文件SqlMapConfig.xml中的dataSource标签，type属性就是表示采用何种连接池方式
  >
  >type属性的取值：
  >
  >​		POOLED：采用传统的javax.sql.DataSource规范中的连接池，mybatis中有针对规范的实现
  >
  >​		UNPOOLED：采用传统的获取连接的方式，虽然也是实现了javax.sql.DataSource，但没有使用池的思想
  >
  >​		JNDI：采用服务器提供的JNDI技术实现来获取DataSource对象，不同的服务器所能拿到的DataSource是不一样的。注意：如果不是web或者maven的war工程，是不能使用的

**POOLED UNPOOLED**

![无标题2](E:\BaiduNetdiskDownload\31.会员版(2.0)-就业课(2.0)-Mybatis\56.mybatis\mybatis\mybatis_day03\截图\无标题2.png)

### 4.2、mybatis事务控制分析

它通过`SqlSession`对象的commit()和rollback()方法

## 5、mybatis基于XML配置的动态SQL语句使用

mappers配置文件的几个标签（<if> <where><foreach><sql>）

### if标签

```xml
<select id="findByUser" resultType="user" parameterType="user">
    select * from user where 1=1
    <if test="username!=null and username != '' ">
    and username like #{username}
    </if> <if test="address != null">
    and address like #{address}
    </if>
</select>
```

> where 1=1的作用：
>
> 1=1永真，主要用来构件动态SQL,为了防止多条件SQL中只出现where子句的情况，从而导致SQL出错
>
> 1<>1永假，用于只取结构而不取数据的场合

### where标签

为了简化上面where 1=1 的条件封装，我们可采用用<where>标签来简化开发

```xml
<select id="findByUser" resultType="user" parameterType="user"> 
    <include refid="defaultSql"></include> 
    <where> 
        <if test="username!=null and username != '' ">
            and username like #{username}
        </if> 
        <if test="address != null">
    		and address like #{address}
    	</if>
    </where>
</select>
```

### foreach标签

```xml
<!-- 查询所有用户在 id 的集合之中 --> 
<select id="findInIds" resultType="user" parameterType="queryvo">
<!-- select * from user where id in (1,2,3,4,5); --> 
    <include refid="defaultSql"></include> 
    <where> 
        <if test="ids != null and ids.size() > 0"> 
            <foreach collection="ids" open="id in ( " close=")" item="uid" separator=",">
            	#{uid}
            </foreach>
		</if>
	</where>
</select>
SQL 语句：
select 字段 from user where id in (?)
<foreach>标签用于遍历集合，它的属性：
    collection:代表要遍历的集合元素，注意编写时不要写#{}
    open:代表语句的开始部分
    close:代表结束部分
```

#### include sql 标签

MyBatis中对于重复的Sql，可以使用include标签引用，最终达到重用的目的

```xml
<sql id="defaultSql">
    select * from user
</sql>
```

```xml
<select id="findAll" resultType="user">
    <include refid="defaultSql"></include>
</select>
```



## 6、mybatis多表操作（掌握）

表之间的关系：一对一，一对多，多对一，多对多

多对一：

> 方式一、使用专门的po类作为输出类型，其中定了sql查询结果集所有的字段
>
> 方式二、使用resultMap，定义专门的resultMap用于映射一对一的结果（如定义相关dto）

一对多：

>collection 是用于建立一对多中集合属性的对应关系
>
>ofType 用于指定集合元素的数据类型

## 7、mybatis延迟加载策略

**引言**

通过前面的学习，我们已经掌握了Mybatis中的一对一，一对多，多对多关系的配置及实现，可以实现对象的关联查询。实际开发过程中很多时候我们并不需要总是在加载用户信息时就一定要加载他的账户信息。此时就是我们所说的延迟加载。

**延迟加载**

就是在需要数据的时候才加载，不需要用到数据时就不加载数据。延迟加载也称懒加载。

>好处：先从单表查询，需要时再从关联表去关联查询，大大提高数据库性能，因为查询单表要比关联查询多张表速度要快。
>
>坏处：因为只有当需要用到数据时，才会进行数据库查询，这样在大批量数据查询时，因为查询工作也要消耗时间，所以可能造成用户等待时间变长，造成用户体验下降。

**实现方法**

我们需要在 Mybatis 的配置文件 SqlMapConfig.xml 文件中添加延迟加载的配置。

```xml
<!-- 开启延迟加载的支持 -->
<setting name="lazyLoadingEnabled" value="true"/>
<setting name="aggressiveLazyLoading" value="false"/>
```

1、使用`assocation`实现延迟加载

2、使用`Collection`实现延迟加载

## 8、mybatis缓存

​		像大多数的持久层框架一样，MyBatis也提供了缓存策略，通过缓存策略来减少数据库的查询次数，从而提高新性能。

​		MyBatis中缓存分为一级缓存和二级缓存

![image-20200929095700179](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200929095700179.png)

### 8.1、一级缓存

一级缓存是SqlSession级别的缓存，只要SqlSession没有flush或close，它就存在。

**一级缓存配置**

我们来看看如何使用MyBatis一级缓存。开发者只需在MyBatis的配置文件中，添加如下语句，就可以使用一级缓存。共有两个选项，`SESSION`或`STATEMENT`，默认是`SESSION`级别，即在一个MyBatis会话中执行的所有语句，都会共享这一缓存。一种是`STATEMENT`级别，可以理解为缓存只对当前执行的这一个`Statement`有效。

一级缓存是SqlSession范围的缓存，当调用SqlSession的修改、添加、删除、commit()、close()等方式时，就会清空一级缓存。

![image-20200929100536015](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200929100536015.png)

第一次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，如果没有，从数据库中查询信息。

得到用户信息，当用户信息存储到一级缓存中。

如果sqlSession去执行commit操作（执行插入、更新、删除），清空sqlSession中的一级缓存，这样做的目的是为了让缓存中存储的是最新的信息，避免脏读。

第二次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，缓存中有，直接从缓存中读取用户信息

**总结**：

>- MyBatis一级缓存的声明周期和SqlSession一致
>- MyBatis一级缓存内部设计简单，只是一个没有容量限定的HashMap，在缓存的功能性上有所欠缺
>
>- MyBatis的一级缓存最大范围是SqlSession内部，有多个SqlSession或者分布式的环境下，数据库写操作会引起脏数据，建议设置缓存级别为Statement

### 8.2、二级缓存

二级缓存是 mapper 映射级别的缓存，多个 SqlSession 去操作同一个 Mapper 映射的 sql 语句，多个 SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 

![image-20200929101814910](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200929101814910.png)

首先开启mybatis的二级缓存

sqlSession1 去查询用户信息，查询到用户信息会将查询数据存储到二级缓存中

如果sqlSession3 去执行相同 mapper 映射下 sql ，执行 commit 提交，将会清空该 mapper 映射下的二级缓存区域数据

sqlSession2 去查询与sqlSession1 相同的用户信息，首先回去缓存中找是否存在数据，如果存在直接从缓存中取出数据

**二级缓存开启与关闭**

- 第一步：在SqlMapConfig.xml文件开启二级缓存

```xml
<settings>
	<!-- 开启二级缓存的支持 -->
    <setting name="cacheEnabled" value="true">
</settings>
因为cacheEnabled的取值默认就为 true，所以这一步可以省略不配置。为 true 代表开启二级缓存；为 false 代表不开启二级缓存
```

- 第二步：配置相关的mapper映射文件

```xml
<cache>标签表示当前这个 mapper 映射将使用二级缓存，区分的标准就看 mapper 的 namespace 值。
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper 
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd"> 
<mapper namespace="com.itheima.dao.IUserDao">
	<!-- 开启二级缓存的支持 -->
	<cache></cache>
</mapper>
```

- 第三步：配置 statement 上面的 useCache 属性

```xml
<!-- 根据id查询 -->
<select id="findById" resultType="user" parameterType="INT" useCache="true">
	select * from user where id = #{uid}
</select>

将 UserDao.xml 映射文件中的<select>标签中设置 useCache=”true”代表当前这个 statement 要使用
二级缓存，如果不使用二级缓存可以设置为 false。
注意：针对每次查询都需要最新的数据 sql，要设置成 useCache=false，禁用二级缓存。
```

**二级缓存注意事项**

当我们在使用二级缓存的时候，所缓存的类一定要实现 `java.io.Serializable`接口，这种就可以使用序列化的方式来保存对象。

> 参考文档：https://tech.meituan.com/2018/01/19/mybatis-cache.html

### 8.3、总结

- MyBatis的二级缓存相对一级缓存来说，实现了`SqlSession`之间缓存数据的共享，同时粒度更加的细，能够到`namespace`级别，通过`Cache`接口实现类不同的组合，对`Cache`的可控性也强
- MyBatis在多表查询时，极大可能会出现脏数据，有设计上的缺陷，安全使用二级缓存的条件比较苛刻
- 在分布式环境下，由于默认的MyBatis Cache实现都是基于本地的，分布式环境下必然出现读取到脏数据，需要使用集中式缓存将MyBatis的Cache接口实现，有一定的开发成本，直接使用Redis、Memcached等分布式缓存可能成本更低，安全性也更高。

## 9、Mybatis 标签

![](D:\mystudy\图片\1001990-20180420091927414-873899959.png)

MyBatis 中的sql标签定义SQL片段。

include标签引用，可以复用SQL片段

## 10、MyBatis常用注解

@Insert：实现新增

@Update：实现更新

@Delete：实现删除

@Select：实现查询

@Result：实现结果集封装

@Results：实现多个结果集封装

@ResultMap：实现引用@Results定义的封装

@One：实现一对一结果的封装集

@Many：实现一对多结果的封装集

@SelectProvider：实现动态SQL映射

@CacheNamesapce：实现注解二级缓存的使用

面试题：

https://blog.csdn.net/ThinkWon/article/details/101292950









官方文档：https://mybatis.org/mybatis-3/zh/getting-started.html

 