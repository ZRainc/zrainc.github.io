# Spring

## 1、spring简介

Spring 是分层的 Java SE/EE 应用 full-stack 轻量级开源框架，以 IoC（Inverse Of Control：

反转控制）和 AOP（Aspect Oriented Programming：面向切面编程）为内核，提供了展现层 Spring 

MVC 和持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多

著名的第三方框架和类库，逐渐成为使用最多的 Java EE 企业应用开源框架。

**spring**优势

- **方便解耦、简化开发**：通过spring提供的的Ioc容器，可以将对象间的依赖关系交由spring控制，避免硬编码所造成的过度程序耦合。
- **AOP编程的支持**：通过spring的AOP功能，方便进行面向切面的编程，许多不容易用传统OOP实现的功能可以轻松实现。
- **声明式的事务支持**：可以将我们从单调的事务管理代码中解脱出来，通过声明式方式灵活的进行事物的管理，提供开发效率和质量。
- **方便程序的测试**：可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情。
- **方便集成各种优秀的框架**：Spring 可以降低各种框架的使用难度，提供了对各种优秀框架（Struts、Hibernate、Hessian、Quartz等）的直接支持。
- **降低** **JavaEE API** **的使用难度**：Spring 对 JavaEE API（如 JDBC、JavaMail、远程调用等）进行了薄薄的封装层，使这些 API 的使用难度大为降低。
- **Java** **源码是经典学习范例**：Spring 的源代码设计精妙、结构清晰、匠心独用，处处体现着大师对 Java 设计模式灵活运用以及对 Java 技术的高深造诣。它的源代码无意是 Java 技术的最佳实践的范例。

**spring体系结构**

![image-20200823010054841](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200823010054841.png)

## 2、spring IOC

**程序的耦合和解耦**

- 耦合性：程序间的关联程度的度量，包括**类之间的依赖，方法间的依赖**

- 解耦：降低程序间的依赖关系，实际开发要做到：**编译期不依赖，运行时才依赖**

- 解决思路：

  >第一步：使用反射来创建对象，而避免使用new关键字
  >
  >第二步：通过读取配置文件来获取要创建的对象全限定类名

- 曾经案例中的问题

- 工厂模式解耦

### 2.1、IOC 容器

具有依赖注入功能的容器，它可以创建对象，IOC 容器负责实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。通常new一个实例，控制权由程序员控制，而"控制反转"是指new实例工作不由程序员来做而是交给Spring容器来做。

### 2.2、**BeanFactory** **和** **ApplicationContext** **的区别**

- BeanFactory 才是 Spring 容器中的顶层接口。

- ApplicationContext 是它的子接口。

- BeanFactory 和 ApplicationContext 的区别：

  创建对象的时间点不一样。

  ApplicationContext：采用立即加载模式，只要一读取配置文件，默认情况下就会创建对象。

  BeanFactory：采用延迟加载模式，什么时候根据id获取对象了，什么时候创建对象。
  
- ApplicationContext的三个常用实现类

  >**ClassPathXmlApplicationContext** :它可以加载类路径下的配置文件，要求配置文件必须在类路径下。不在的话加载不了。（更常用）
  >
  >**FileSystemXmlApplicationContext**:它可以加载磁盘任意路径下的配置文件(必须有访问权限)
  >
  >**AnnotationConfigApplicationContext**：它是用于读取注解创建容器的。

- **ApplicationContext**适合单例（更多使用），**BeanFactory**适合多例

### 2.3、Spring Bean 

- Bean是一个被实例化，组装，并通过Spring IOC容器所管理的对象，这些bean是由容器所提供的配置元数据所创建的。

- > Bean在计算机英语中，有可重用组件的含义。
  >
  > Java Bean:用Java语言编写的可重用组件。

- **作用：**用于配置对象让 spring 来创建的。默认情况下它调用的是类中的无参构造函数。如果没有无参构造函数则不能创建成功。

- **属性：**

  1、id：给对象在容器中提供一个唯一标识。用于获取对象。

  2、class：指定类的全限定类名。用于反射创建对象。默认情况下调用无参构造函数。

  3、scope：指定对象的作用范围。

  > singleton :默认值，单例的.
  >
  > prototype :多例的.
  >
  > request :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 request 域中.
  >
  > session :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 session 域中.
  >
  > global session :WEB 项目中,应用在 Portlet 环境.如果没有 Portlet 环境那么globalSession 相当于 session.

  4、init-method：指定类中的初始化方法名称。

  5、destroy-method：指定类中销毁方法名称。

- **Spring 与bean的关系**

![Spring Bean 定义](https://atts.w3cschool.cn/attachments/image/20181029/1540796406991057.jpg)

### 2.4、**Spring Bean作用域**

当在Spring中定义一个Bean时，你必须声明该Bean的作用域选项

Spring框架支持以下五种作用域，分别为singleton、prototype、request、session和global session

| 作用域         | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| singleton      | 在spring IoC容器仅存在一个Bean实例，Bean以单例方式存在，默认值 |
| prototype      | 每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean()时，相当于执行newXxxBean() |
| request        | 每次HTTP请求都会创建一个新的Bean，该作用域仅适用于WebApplicationContext环境 |
| session        | 同一个HTTP Session共享一个Bean，不同Session使用不同的Bean，仅适用于WebApplicationContext环境 |
| global-session | 一般用于Portlet应用环境，该运用域仅适用于WebApplicationContext环境 |

**global-session**：用于多台服务器之间的信息共享

![全局session](E:\BaiduNetdiskDownload\57.spring\spring\spring_day01\截图\全局session.png)

### 2.5、**Spring Bean生命周期**

- 理解Spring Bean的生命周期很容易，当一个Bean被实例化时，它可能需要执行一些初始化使它转换成可执行的状态。同样，当bean不在需要时，并且从容器移除时，可能需要做一些清除工作。

- **Bean的生命周期可表达为**：Bean的定义——Bean的初始化——Bean的使用——Bean的销毁

- 为了定义安装和拆卸一个 bean，我们只要声明带有 **init-method** 和/或 **destroy-method** 参数的 。init-method 属性指定一个方法，在实例化 bean 时，立即调用该方法。同样，destroy-method 指定一个方法，只有从容器中移除 bean 之后，才能调用该方法。

- 单例对象

  >出生：当容器创建时对象出生
  >
  >活着：只有容器还在，对象一直或者
  >
  >死亡：容器销毁，对象死亡
  >
  >总结：单例对象的生命周期和容器相同

- 多例对象

  >出生：当我们使用对象时spring框架为我们创建
  >
  >或者：对象只要还在使用就一直或者
  >
  >死亡：当对象长时间不用，且没有别的对象引用时，由Java的垃圾回收器回收

### 2.6、**spring中基于XML的IOC搭建**



## 3、依赖注入

Spring框架的核心功能之一就是通过依赖注入的方式来管理Bean之间的依赖关系。

**依赖注入**：我们在编写程序时，通过控制反转，把对象的创建交给了Spring，但是代码中不可能出现没有依赖的情况。IOC解耦只是降低了他们的依赖关系，但不会消除。例如，我们的业务层仍会调用持久层的方法。这种业务层和持久层的依赖关系，在使用spring后，就由来维护了。简单地说，就是等框架将持久层的对象传入业务层，而不用我们自己去获取。

**实例**

假设你有一个包含文本编辑器组件的应用程序，并且你想要提供拼写检查。标准代码看起来是这样的：

```java
public class TextEditor {
    private SpellChecker spellChecker;
    public TextEditor() {
        spellChecker = new SpellChecker;
    }
}
```

在这里我们所做的就是创建一个TextEditor和SpellChecker之间的依赖关系，而在控制反转的场景中，我们会这样做：

```java
public class TextEditor {
    private SpellChecker spellChecker;
    public TextEditor(SpellChecker spellChecker){
        this.spellChecker = spellChecker;
    }
} 
```

在这里，TextEditor 不应该担心 SpellChecker 的实现。SpellChecker 将会独立实现，并且在 TextEditor 实例化的时候将提供给 TextEditor，整个过程是由 Spring 框架的控制。

- 能注入的数据

  >1、基本类型和String
  >
  >2、其他bean类型（在配置文件中或者注解配置过的bean）
  >
  >3、复杂类型/集合类型

### 3.1、构造函数注入

- 使用的标签：constructor-arg

- 标签出现的位置：bean标签的内部

- 标签中的属性

  ​			type：用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型

  ​			index：用于指定要注入的数据给构造函数中指定索引位置的参数赋值，索引的位置是从0开始的

  ​			name：用于指定给构造函数中指定名称的参数赋值（常用的）

  ​			value：用于提供基本类型和String类型的数据

  ​			ref：用于指定其他bean类型数据。它指的就是在的Ioc核心容器中出现的bean对象

- 优势：在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功

- 弊端：改变了bean对象的实例方式，使我们在创建对象时，如果用不到这些数据，也必须提供

### 3.2、set方法注入

- 涉及的标签：property

- 出现的位置：bean标签的内部

- 标签的属性

  ​			name：用于指定注入时所调用的set方法名称

  ​			value：用于提供基本类型和String类型的数据

  ​			ref：用于指定其他bean类型数据。它指的就是在的Ioc核心容器中出现的bean对象

- 优势：创建对象没有明确的限制，可以直接使用默认构造函数

- 弊端：如果某个成员必须有值，则获取对象是有可能set方法没有执行

  **复杂类型的注入/集合类型的注入**
  
  >用于给List结构集合注入的标签：list array set
  >
  >用于给Map结构结婚注入的标签：map props
  >
  >**总结**：结构相同，标签互换

### 3.3、使用注解

```
 * xml的配置
 * <bean id="accountService" class="com.itheima.service.impl.AccountserviceImpl"
 * scope=""    init-method=""  destroy-method="">
 * <property name="" value="" | ref=""></property>
 * </bean>
 *
 * 用于创建对象的注解--他们的作用就和在XML配置文件中编写一个<bean>标签实现的功能是一样的
 * 		Component:
 * 			作用：用于把当前类对象存入spring容器中
 * 			属性：
 *				 value：用于指定bean的id。当我们不写时，他的默认值是当前类名，且首字母改小写
 * 		Controller：一般用在表现层
 * 		Service：一般用在业务层
 * 		Repository：一般用在持久层
 * 		以上三个注解他们的作用和属性与Component是一样的，
 * 		他们三个是spring框架为我们提供明确的三层使用的注解，是我们三层对象更清晰
 *
 *
 * 用于注入数据的注解--他们的作用就和在XML配置文件中的bean标签写一个<property>标签的作用是一样的
 * 		Autowired:
 * 			作用：自动按照类型注入，只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入 
 *				成功
 * 			如果ioc容器中没有任何bean的类型和要注入的变量类型匹配，就报错。
 * 			如果ioc容器中有多个类型匹配时，
 * 			出现位置：可以是变量上，也可以是方法上。
 * 			细节：使用注解注入时，set方法不是必须的
 * 
 * 		Qualifier:
 * 			作用：在自动按照类型的注入基础之上，再按照Bean的id注入。它再给字段注入时不能单独使用，必须和
 * 				Autowire一起使用;但给方法参数注入时，可以独立使用。
 *				与Autowired一起使用，解决按类型匹配找到多个Bean的问题
 * 			属性：value：指定bean的id
 * 		Resource:
 * 			作用：直接按照Bean的id注入，可以独立使用
 *				 默认按名称装配，当找不到名称匹配的bean再按类型装配
 * 			属性：name：指定bean的id
 * 		以上三个注入都只能注入其他bean类型数据，而基本类型和String类型无法使用上述注解实现
 * 		另外，集合类型的注入只能通过XML来实现
 *
 * 		Value:
 * 			作用：用于注入基本类型和String类型的数据
 * 			属性：value:用于指定数据的值，它可以使用spring中SpEL(也就是Spring中的el表达式)
 * 			SpEL的写法：${表达式}
 * 			用于改变作用范围的注解--他们的作用就和在<bean>标签中使用scope属性实现的功能是一样的
 * 		Scope:
 * 			作用：用于指定bean的作用范围
 * 			属性：value:指定范围的取值。常用取值：singleton、prototype
 * 和生命周期相关注解--他们的作用就和在<bean>标签中使用init-method和destroy-method属性实现的功能是一    样的					
 * 		PreDestroy
 * 			作用：用于指定销毁方法
 * 		PostConstruct
 * 			作用：用于指定初始化方法

```

![自动按照类型注入](E:\BaiduNetdiskDownload\57.spring\spring\spring_day02\截图\自动按照类型注入.png)

```java
*              spring中的新注解
*              Configuration
*                  作用：表明当前类是一个注解
*                  细节：当配置类作为AnnotationConfigApplicationContext对象创建参数时，该注解可以不写
*              ComponentScan:
*                  作用：用于通过注解指定spring容器创建是要扫描的包
*                  属性：
*                      value：和basePackages的作用是一样的，都是用于指定创建容器时要扫描的包
*                             我们使用此注解就相当于在xml中配置了：<context:component-scan base-package="com.itheima"></context:component-scan>
*              Bean:
*                  作用：用于把当前方法的返回值作为bean对象存入spring容器中
*                  属性：
*                      name：用于指定bean的id，当不写时，默认值是当前方法的名称
*                  细节：
*                      当我们使用注解配置方法时，如果方法有参数，spring框架会去容器中找有没有可用的bean对象
*                      查找的方式和Autowired注解的作用是一样的
*              Import:
*                  作用：用于导入其他配置文件
*                  属性：
*                      value：用于指定其他配置文件的字节码
*                          当我们使用Import的注解之后，有Import注解的类就是父配置类，而导入的配置类都是子配置
*              PropertySource:
*                  作用：用于指定properties文件的位置
*                  属性：
*                      value：指定文件的名称和路径
*                          关键字：classpath，标识类路径下
```

### 3.4、Junit

- 应用程序的入口：main()方法

- Junit单元测试中，没有main()方法也能执行

  ​		Junit集成了一个main方法

  ​		该方法会判断当前测试类中哪些方法有@Test注解

  ​		Junit就让有Test注解的方法执行

- Junit不会管我们是否采用Sring框架

  ​		在执行测试方法时，Junit根本不知道我们是不是使用了spring框架

  ​		所以也就不会为我们读取配置文件/配置类创建Spring核心容器

- 由以上三点可知：

  ​		当测试方法执行时，没有Ioc容器，就算写了Autowired，也无法实现注入



**Spring整合Junit配置**

- 导入spring整合Junit的jar包(坐标)

- 使用Junit提供的一个注解把原有的main方法替换了，替换成spring提供的

  ​	@Runwith

- 告知spring的运行器，spring和ioc创建是基于xml还是注解的，并且说明位置

  ​	@ContextConfiguration

  ​				locations：指定xml文件的位置，加上classpath关键字，表示在类路径下

  ​				classes：指定注解类所在的位置

当我们使用spring 5.x版本的时候，要求Junit的jar必须是4.12及以上。



## 4、spring aop

事务控制

需要使用ThreadLoacl对象把Connection和当前线程绑定，从而使一个线程中只有一个能控制事物的对象

![事务控制](E:\BaiduNetdiskDownload\57.spring\spring\spring_day03\截图\事务控制.png)

### 4.1、动态代理

![代理](E:\BaiduNetdiskDownload\57.spring\spring\spring_day03\截图\代理.png)

- 特点：

  字节码随用随创建，随用随加载。

  它与静态代理的区别也在于此。因为静态代理是字节码一上来就创建好，并完成加载。

  装饰者模式就是静态代理的一种体现。

- 作用
  不修改源码的基础上对方法增强

- 分类

  **基于接口的动态代理**

  ​	涉及的类：Proxy

  ​	提供者：官方

  ​	如何创建代理对象：使用Proxy类中的newProxyInstance方法

  ​	创建代理对象的要求：被代理类最少实现一个接口，如果没有则不能使用

  ​	newProxyInstance方法的参数：

  ​				ClassLoader：类加载器

  ​						它是用于加载代理对象字节码的，和被代理对象使用相同的类加载器。固定写法

  ​				Class[]：字节码数组

  ​						它是用于让代理对象和被代理对象有相同的方法

  ​				InvocationHandler：用于提供增强的代码

  ​						它是让我们写如何代理。我们一般都是写一个该接口的实现类，通常情况下都是匿名内部类，						但不是必须的，此接口的实现类是谁用谁写

  ​						**invoke方法**：执行被代理对象的任何接口方法都会经过该方法

  ​							方法参数的含义：

  ​								proxy：代理对象的引用

  ​								method：当前执行的方法

  ​								args：当前执行方法所需的参数

  ​							返回值：和被代理对象方法有相同的返回值

  **基于子类的动态代理**：

  ​			涉及的类：Enhancer

  ​			提供者：第三方cglib库

  ​			如何创建代理对象：使用Enhancer类中的create方法

  ​			创建代理对象的要求：被代理类不能时最终类	

  ​			create方法的参数：

  ​				Class：字节码

  ​						它是用于指定被代理对象的字节码

  ​				Callback：用于提供增强的代码

  ​						它是让我们写如何代理。我们一般都是写一个该接口的实现类，通常情况下都是匿名内部类，

  ​						但不是必须的，此接口的实现类是谁用谁写

  ​						我们一般写的都是该接口的子接口实现类：MethodInterceptor

  ​						**intercept方法**：执行被代理对象的任何接口方法都会经过该方法

  ​							方法参数的含义：

  ​								proxy：代理对象的引用

  ​								method：当前执行的方法

  ​								args：当前执行方法所需的参数

  ​							（以上三个参数与基于接口的动态代理中invoke方法的参数是一样）

  ​								MethodProxy：当前执行方法的代理对象

  ​							返回值：和被代理对象方法有相同的返回值

### 4.2、AOP相关概念

**Joinpoint连接点**

所谓连接点是指那些被拦截到的点。在 spring 中,这些点指的是方法,因为 spring 只支持方法类型的

连接点。

**Pointcut切入点**

所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义。

**Advice(通知/增强)**

所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知

通知的类型：前置通知、后置通知、异常通知、最终通知、环绕通知

**Introduction(引介)**

引介是一种特殊的通知在不修改类代码的前提下，Introduction 可以在运行期为类动态地添加一些方法或Field

**Target(目标对象)**

代理的目标对象

**Weaving(织入)**

是指把增强应用到目标对象来创建新的代理对象的过程

spring 采用动态代理织入，而 AspectJ 采用编译器织入和类装载期织入

**Proxy(代理)**

一个类被AOP织入增强后，就产生一个结果代理类

**Aspect(切面)**

是切入点和通知（引介）的结合。

















