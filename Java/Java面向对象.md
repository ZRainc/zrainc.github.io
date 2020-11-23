# 面向对象的三大特性：封装、继承和多态

## 一、封装

### 1、概念

- 面向对象的语言是对客观世界的模拟，客观世界里的成员变量都是隐藏在对象内部的，外界无法直接操作和修改。封装可以被认为是一个保护屏障，防止该类的代码和数据被其他类随意访问。要访问该类的数据，必须通过指定的方式。适当的封装可以让代码更容易理解和维护，也加强代码的安全性。
- 原则：将**属性**隐藏起来，若要访问某个属性时，提供公共的方法对其访问。

### 2、封装的步骤

1、使用`private`关键字来修饰成员变量

2、对需要访问的成员变量，提供对应的一对`getXxx`、`setXxx`方法

### 3、封装的操作--private关键字

- private是一个权限修饰符，代表最小权限
- 可以修饰成员变量和成员方法
- 被private修饰的成员变量和成员方法，只有在本类中才能访问

### 4、封装优化1——this关键字

- **this的含义**：this表示所在类当前对象的引用（地址值），即对象自己的引用

>方法被哪个对象调用，this就代表那个对象，即谁调用，this就代表谁

- **为什么要使用this**：

  在修改了 setXxx() 的形参变量名后，方 法并没有给成员变量赋值！这是由于形参变量名与成员变量名重名，导致成员变量名被隐藏，方法中的变量名，无 法访问到成员变量，从而赋值失败。所以，我们只能使用this关键字，来解决这个重名问题。

```java
public class Student {
	private String name;
    private int age;
    
    public void setName(String name){
        //name = name;
        this.name = name;
    }
    
    public void setAge(int age){
        //age = age;
        this.age = age;
    }
}
```

使用this关键字修饰方法中的变量，解决成员变量被隐藏的问题

### 5、封装优化2——构造方法

- **当一个对象被创建时，构造方法用来初始化该对象，给对象的成员变量赋初始值**

  >无论你是是否自定义构造方法，所有的类都有构造方法，因为Java自动提供了一个无参构造方法，一旦自定义了构造方法，Java提供的默认无参构造方法就会失效

```java
public class Student {
	private String name;
	private int age;
	
    //无参构造
    public Student() {}
    
    //有参构造
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
 }
```

**注意事项**

- 如果你不提供构造方法，系统会给出无参的构造方法
- 如果你提供了构造方法，系统默认的无参构造方法将失效
- 构造方法可以重载，既可以定义参数，也可以不定义参数

### 6、标准代码——JavaBean

`JavaBean`是 Java语言编写类的一种标准规范。符合`JavaBean`的类，要求类必须是具体的和公共的，并且具有无 

参数的构造方法，提供用来操作成员变量的 set 和 get 方法。 

```java
public class ClassName{ 
    //成员变量 
    //构造方法 
    //无参构造方法【必须】 
    //有参构造方法【建议】 
    //成员方法 
    //getXxx() 
    //setXxx() 
}
```

## 二、继承

### 1、概念

- **由来** ——多个类中存在相同属性和行为时，将这些内容抽取到单独一个类中，那么多个类无需重复定义这些属性和行为，只要**继承**那一个类即可。
- **定义**——就是子类继承父类的属性和方法，使得子类对象具有父类相同的属性、相同的行为。子类可以直接访问父类中**非私有**的属性和行为。
- **好处**——提高代码复用性。**类与类产生关系，是多态的前提**

**继承是多态的前提**，没有继承就没有多态。

继承主要解决的问题是：***共性抽取***

### 2、格式

```java
class 父类 {
    ...
}

class 子类 extends 父类 {
    ...
}

```

具体实例：

```java
//定义员工类-父类
class Employee {
    //定义name属性
    String name;
    //定义员工的工作方法
    public void work() {
        System.out.pringln("尽心尽力的工作")
    }
}

//定义讲师类Teacher 继承Employee
class Teacher extends Employee {
    //定义一个打印name的方法
    public void printName() {
        System.out.println("name=" + name);
    }
}

//定义测试类
public class testDemo {
    public static void main(String[] args){
        //创建一个讲师类对象
        Teacher t = new Teacher();
        
        //为该员工的姓名赋值
        t.name = "小明";
        
        //调用该员工的printName()方法
        t.printName();  // name=小明
        
        //调用Teacher继承来的work()方法
        t.work();   //尽心尽力的工作
        
    }
}
```

### 3、继承后的特点——成员变量

当类与类之间发生关系，其中类中的各个成员变量，又产生了哪些影响呢

**成员变量不重名**

- 如果子类与父类之间不出现重名的成员变量，这是访问是不影响的

```java
class Fu {
    //Fu中的成员变量
	int num = 5;
}

class Zi extends Fu {
    //
    int num2 = 6;
    
    public void show(){
        // 访问父类中的num， 
        System.out.println("Fu num="+num); // 继承而来，所以直接访问。 
        // 访问子类中的num2 
        System.out.println("Zi num2="+num2);
    }
}

class extendDemo {
    public static void main(String[] args) {
		//创建子类对象
        Zi zi = new Zi();
        //调用子类中的show()方法
        zi.show();
    }
}

//演示结果： 
Fu num = 5 
Zi num2 = 6
```

**成员变量重名**

- 如果子类父类出现**重名**的成员变量，这时的访问是**有影响的**

```java
class Fu {
    // Fu中的成员变量
    int num = 5;
}

class Zi extends Fu {
    // Zi中的成员变量
    int num = 6;
    public void show(){
        //访问父类中的num
        System.out.println("Fu num=" + num);
        //访问子类中的num
        System.out.println("Zi num=" + num);
    }
}

class extendsDemo {
    public static void main(String[] args){
        //实例化子类对象
        Zi zi = new Zi();
        //调用子类的show()方法
        zi.show();
    }
}

//演示结果： 
Fu num = 6
Zi num2 = 6
```

子类出现和父类同名的成员变量时，在子类中访问父类的非私有成员变量时，需要使用`super`关键字，修饰父类的成员变量

此时，子类方法需要修改为：

```java
class Zi extends Fu {
	//子类中的成员变量
    int num = 6;
    public void show() {
        //访问父类中的num
        System.out.println("Fu num=" + super.num);
        //访问子类中的num
        System.out.println("Zi num=" + this.num);
    }
}

//演示结果： 
Fu num = 5 
Zi num = 6
```

>小贴士：Fu类中的成员变量是非私有的，子类可以直接访问。若Fu类中的成员变量是私有的，子类是不能直接访问的，通常编码时，我们遵循封装的原则，使用private修饰成员方法，那么如何访问父类中的私有成员方法呢？对，可以在父类中提供公共的getXxx方法和SetXxx方法。

### 4、继承后的特点——成员方法

**成员方法不重名**

- 如果子类父类出现**不重名**的成员方法，这时的调用是**没有影响**的。对象调用方法时，会现在子类中找有没有对应的方法，若子类中存在就执行子类中的方法，若子类中不存在就会执行父类相应的方法。代码如下：

```java
class Fu {
	public void show(){
		System.out.println("Fu类中的show方法执行");
	}
}

class Zi extends Fu{
    public void show2 {
        System.out.pringtln("Zi类中的show2方法执行")
    }
}

public class extendsDemo {
    public static void main(String[] args){
        //实例化子类
        Zi z = new Zi();
        //子类没有show方法，但是可以找到父类中的show方法去执行
        z.show();
        z.show2();
    }
}

//演示结果： 
Fu类中的show方法执行 
Zi类中的show2方法执行
```

**成员方法的重名——重写**

如果子类父类出现方法名相同的成员方法，这是的访问是一中特殊情况，叫做**方法重写**

- **方法重写**：子类中出现和父类中一摸一样的方法时（**返回值类型、方法名和参数列表都相同**），会出现覆盖效果，也称为重写或复写。**声明不变，重新实现**

```java
class Fu {
	public void show(){
        System.out.println("Fu show");
    }
}

class zi extends Fu {
    //子类重写了父类的方法
    public void show(){
        System.out.pringln("Zi show");
    }
}

public class extendsDemo {
    public static void main(String[] args){
        Zi z = new Zi();
        //子类中有show方法，只执行重写后的show方法
        z.show();
    }
}

//演示结果
Zi show
```

**重写的应用**

```java
class Phone {
	public void sendMessage(){
        System.out.pringln("发短信");
    }
    public void call(){
        System.out.println("打电话");
    }
    public void showNum(){
        System.out.println("来电显示好吗");
    }
}

//智能手机
class NewPhone extends Phone {
    
    //重写来电显示功能，并增加自己的显示姓名和图片功能
    public void showNum(){
        //调用父类已经存在的功能使用super
        super.showNum();
        //增加自己特有的显示姓名和图片功能
        System.out.println("显示来电姓名");
        System.out.pringln("显示头像");
    }
}

public class ExtendDemo{
    public static void main(String[] args){
        //实例化子类
		NewPhone np = new NewPhone();
        
        //调用父类继承的方法
        np.call();
        
        //调用子类重写的方法
        np.showNum();
    }
}
```

>小贴士：这是重写时，用到super.父类成员方法，表示调用父类的成员方法

**注意事项**

1、子类方法覆盖父类方法时，必须要保证权限大于等于父类权限

2、子类方法覆盖父类方法时，返回值类型，方法名和参数列表都要一样

### 5、继承后的特点——构造函数

当类之间产生了关系，其中各类中的构造方法，又产生了哪些影响呢？ 

首先我们要回忆两个事情，构造方法的定义格式和作用。 

1、构造方法名字与类名是一致的。所以子类是无法继承父类的构造方法

2、构造方法的作用是初始化成员变量的。所以在子类的初始化过程中，必须先执行父类的初始化动作。子类的构造方法中默认有一个`super()`，表示调用父类的构造方法，父类成员白变量初始化后，才可以子类用。代码：

```java
class Fu {
	private int n;
    Fu(){
        System.out.println("Fu()");
    }
}

class Zi extends Fu {
    Zi(){
        //super(),调用父类的构造方法
        super();
        System.out.println("Zi()");
    }
}

public class ExtendsDemo {
	public static void main(String[] args){
		Zi() z = new Zi();
    }
}

//输出结果
Fu()
Zi()
```

### 6、super和this

**父类空间优于子类对象产生**

- 在每次创建子类对象时，先初始化父类空间，再创建其子类对象本身。目的在于子类对象中包含了其对应的父类空间，便可以包含其父类的成员，如果父类成员非private修饰，则子类可以随意使用父类成员。代码体现在子类的构造方法调用时，一定先调用父类的构造方法。

**super和this的含义**

- **super**：代表父类的**存储空间标识**（可以理解为父亲的引用）
- **this**：代表**当前对象的引用**（谁调用就代表谁）

**super和this的用法**

- 访问成员

  ```java
  class Animal {
  	public void eat(){
          System.out.println("animal : eat");
      }
  }
  
  class Cat extends Animal {
      public void eat() {
          System.out.println("cat : eat");
      }
      public void eatTest() {
          this.eat(); //this  调用本类的方法
  		super.eat();//super 调用父类的方法
      }
      
  }
  
  public class ExtendsDemo {
      public static void main(String[] args){
          Animal a = new Animal();
          a.eat();
          Cat c = new Cat();
          c.eatTest();
      }
  }
  
  输出结果：
  animal : eat
  cat : eat
  animal : eat
  ```

- 访问构造方法

  >子类的每个构造方法中均有默认的super()，调用父类的空参构造。手动调用父类构造会覆盖默认的super()
  >
  >super()和this()都只能再构造方法的第一行，所以不能同时出现

### 7、继承的特点

- Java只支持单继承，不能多继承

  >顶层父类是Object类，所有的类默认继承Object，作为父类

### 8、总结

**重写(Override)和重载(OverLoad)**

> **重写**：方法名称一样，参数列表也一样，子类方法的返回值必须小于等于父类方法的返回值范围
>
> 两同：方法名称、参数列表
>
> 两小：返回值类型、抛出异常（小于等于）
>
> 一大：访问修饰符（大于等于）
>
> - 必须保证父子之类的方法名称一样，参数列表也一样
>
>   @Override：写在方法面前，用来检测是不是有效的正确覆盖重写；这个注解就算不写，只要满足要求，也是正确覆盖重写
>
> - 子类方法的返回值必须【小于等于】父类方法的返回值范围
>
> - 子类方法的权限必须【大于等于】父类方法的权限修饰符
>
>   tips：public > protected > (default) > private
>
> **重载**：方法名称一样，参数列表不一样

**继承关系中，父子类构造方法的访问特点**

>- 子类构造方法有一个默认隐藏的【super()】调用，所以一定是父类构造器，后执行子类构造
>
>- 子类构造可以通过super关键字来调用父类重载构造
>
>- super的父类构造调用，必须是子类构造方法的第一个语句
>
>  ***总结***：子类必须调用父类的构造方法，不写则赠送super()；写了则用指定的super()方法调用，super只能有一个，还必须是第一个。

**super关键字的用法**

>- 在子类的成员方法中，访问父类的成员变量
>- 在子类的成员方法中，方法父类的成员方法
>- 在子类的构造方法中，访问父类的构造方法

**this关键字用法**

>- 在本类方法中，访问本类的成员变量
>
>- 在本类的成员方法中，访问本类的另一个成员方法
>
>- 在本类的构造方法中，访问本类的另一个构造方法
>
>  ***注意***：this也必须是第一个语句，唯一一个；super和this两种构造调用，不能同时使用

## 三、抽象类

### 1、概述

**由来**：父类中的方法，被他们的子类们重写，子类的各自实现都不尽相同。那么父类的方法声明和方法主体，只有声明还有意义，而方法主体没有意义了。我们把没有方法主体的方法称为**抽象方法**。Java语法规定，包含抽象方法的类就是**抽象类**

**定义**：

- **抽象方法**：没有方法体的方法
- **抽象类**：包含抽象方法的类

### 2、abstract使用格式

**抽象方法**

```java
public abstract void run();
```

**抽象类**

```java
public abstrat class Animal{
	public abstract void run();
}
```

**抽象类的使用**

- 继承抽象类的子类**必须重写父类的所有的抽象方法**。否则，该子类也必须声明为抽象类。最终，必须有子类实现该父类的抽象方法，否则，从最初的父类到最后的子类都不能创建，失去意义。

```java
public abstrat class Animal{
	public abstract void run();
}

public Cat extends Animal {
    public void run(){
        System.out.println("小猫正在跑");
    }
}

public class CatTest { 
    public static void main(String[] args) { 
        // 创建子类对象 
        Cat c = new Cat(); 
        // 调用run方法 
        c.run(); 
    } 
}

输出:
小猫正在跑
```

### 3、注意事项（***）

- 抽象类**不能创建对象**，如果创建，编译无法通过而报错。只能创建其非抽象子类的对象。

  >理解：假设创建了抽象类的对象，调用抽象方法，而抽象方法没有具体的方法体，没有意义

- 抽象类中，可以有构造方法，是供子类创建对象时，初始化父类成员使用的。

  >理解：子类的构造方法中，有默认的super()，需要访问父类的构造方法

- 抽象类中，不一定包含抽象方法，但包含抽象方法的类一定时抽象类

  >理解：未包含抽象方法的抽象类，目的就是不想让调用者来创建对象，通常用于某些特殊的类的结构设计

- 抽象类的子类，必须重写抽象父类中的所有抽象方法，否则，编译无法通过而报错。除非该子类也是抽象类。

  >假设不重写抽象方法，则类中可能包含抽象方法。那么创建对象后，调用抽象方法，没有意义

## 四、接口

### 1、概述

- 接口：是Java中的一个引用类型，是方法的集合，如果说类的内部封装了成员变量、构造方法和成员方法，那么接口的内部主要就是**封装了方法**，包含抽象方法（JDK7及以前），默认方法和静态方法（JDK8），私有方法（JDK9）。
- 接口使用`interface`关键字定义。他也会编译成.class文件，但一定要明确他并不是类，而是一种引用数据类型。

>引用数据类型：数组、类和接口

- 接口的使用，它不能创建对象，但是可以被实现（`implemnets`，类似于被继承）。一个实现接口的类（可以看作接口的子类），需要实现接口中所有的抽象方法，创建该类对象，就可以调用方法了，否则它必须是一个抽象类。

### 2、定义格式

```java
public interface 接口名称{
	// 抽象方法
    // 默认方法
    // 静态方法
    // 私有方法
}
```

**含有抽象方法**

- 抽象方法：使用`abstract`关键字修饰，可以省略，没有方法体。该方法供子类实现使用。

```java
public interface InterFaceName {
	public abstract void method();
}
```

**含有默认方法和静态方法**

- 默认方法：使用`default`关键字修饰，不可以省略，供子类方法调用或重写。
- 静态方法：使用`static`修饰，供接口直接调用

```java
public interface InterFaceName {
	public default void method {
		//执行语句
	}
    public static void method2 {
        //执行语句
    }
}
```

**含有私有方法和私有静态方法**

- 私有方法：使用`private`关键字，供接口中默认方法或静态方法调用

```java
public interface InterFaceName {
	private void method{
		//执行语句
	}
}
```

### 3、基本的实现

**实现的概述**

- 类与接口为实现关系，即**类实现结论**，该类可以成为接口的实现类，也可以称为接口的子类。实现的动作类似继承，格式相仿，只是关键字不同，实现使用`implements`关键字

- 非抽象子类实现接口

  >1、必须重写接口的所有抽象方法。
  >
  >2、继承了接口的默认方法，既可以直接调用，也可以重写

- 实现格式

  ```java
  class 类名 implements 接口名 {
  	//重写接口中的抽象方法【必须】
      //重写接口中的默认方法【可选】
  }
  ```

**抽象方法的使用**

- 必须全部实现，代码如下：

接口如下：

```java
public interface LiveAble {
	//定义抽象方法
    public abstract void eat();
    public abstract void sleep();
}
```

定义实现类：

```java
public class Animal implements LiveAble {
	@Override
    public void eat() {
		System.out.println("吃东西");
    }
    @Override
    public void sleep() {
		System.out.println("睡觉");
    }
} 
```

定义测试类

```java
public class Demo {
	public static void main(String[] args){
        //创建子类对象
        Animal a = new Animal();
        //调用实现后的方法
        a.eat();
        a.sleep();
    }
}

//输出结果： 吃东西 晚睡
```

**默认方法的使用**

- 可以继承，可以重写，二选一，但是只能通过实现类的对象来调用

1、继承默认方法，代码如下：

定义接口：

```java
public interface LiveAble {
	public default void fly() {
		System.out.println("天上飞");
	}
}
```

定义实现类：

```java
public class Animal implements LiveAble {
	//继承，什么都不用写，直接调用
}
```

测试类

```java
public class Demo {
	public static void main(String[] args){
        //创建子类对象
        Animal a = new Animal();
        //调用默认方法
        a.fly();
    }
}

//输出结果：天上飞
```

2、重写默认方法，代码如下

定义接口：

```java
public interface LiveAble {
	public default void fly() {
		System.out.println("天上飞");
	}
}
```

定义实现类：

```java
public class Animal implements LiveAble {
	// 重写
    public void fly() {
        System.out.println("自由自在的在天上飞");
    }
}
```

测试类

```java
public class Demo {
	public static void main(String[] args){
        //创建子类对象
        Animal a = new Animal();
        //调用重写方法
        a.fly();
    }
}

//输出结果：自由自在的在天上飞
```

**静态方法的使用**

- 静态与.class文件相关，只能使用接口名调用，不可以通过实现类的类名或者实现类的对象调用，代码如下：

定义接口：

```java
public interface LiveAble {
	public default void run() {
		System.out.println("跑起来");
	}
}
```

定义实现类：

```java
public class Animal implements LiveAble {
    //无法重写静态方法
}
```

测试类

```java
public class Demo {
	public static void main(String[] args){
        //Animal.run //【错误】 无法继承方法，也无法调用
        LiveAble.run()
    }
}

//输出结果：跑起来
```

**私有方法的使用**

- 私有方法：只有默认方法可以调用

- 私有静态方法：默认方法和静态方法可以调用

  如果一个接口有多个默认方法，并且方法中有重复的内容，那么可以抽取出来，封装到私有方法中，供默认方法去调用。从设计的角度讲，私有的方法是对默认方法和静态方法的辅助。

定义接口：

```java
public interface LivaAble {
    //默认方法
	default void func {
        func1;
        func2;
    }
    
    //私有方法1
    private void func1 {
        System.out.println("跑起来~~~");
    }
    
    //私有方法2
    private void func2 {
        System.out.println("跳起来~~~");
    }
    
}
```

### 4、接口的多实现

单继承，但多实现。一个类可以实现多个接口，这叫做接口的**多实现**。并且，一个类能继承一个父类，同时实现多个接口。

```java
class 类名 [extends 类名] implements 接口1，接口2，接口3....{
	//重写接口中的抽象方法 【必须】
	//重写接口中的默认方法 【不重名时可选】
}
```

>[  ]代表可选操作

**抽象方法**

接口中，有多个抽象方法时，实现类必须重写所有的抽象方法。**如果抽象方法有重名的，只需要重写一次**。代码如下：

定义多个接口：

```java
interface A {
	public abstract void showA();
	public abstract void show();
}

interface B {
	public abstract void showB();
	public abstract void show();
}
```

定义实现类：

```java
public class C implements A,B {
	@Override
    public void showA(){
        System.out.println("showA");
    }
    
    @Override
    public void showB(){
        System.out.println("showB");
    }
    
    @Override
    public void show(){
        System.out.println("show");
    }
}
```

**默认方法**

- 接口中，有多个默认方法时，实现类都可以继承使用。**如果默认方法有重名的，必须重写一次**。代码如下：

定义多个接口：

```java
interface A {
	public default void showA();
	public default void show();
}

interface B {
	public default void showB();
	public default void show();
}
```

定义实现类：

```java
public class C implements A,B {
	@Override
    public void show(){
        System.out.println("show");
    }
}
```

**静态方法**

接口中，存在相同的静态方法名并不会冲突，因为只能通过各自的接口名去访问静态方法。

**优先级问题**

当一个类，既继承一个类，又实现若干接口时，父类中的成员方法与接口中的默认方法重名，**子类就近选择执行父类的成员方法**。

定义接口：

```java
interface A {
	public default void methodA {
		System.out.println("AAAAAAAAAAAA");
	}
}
```

定义父类

```java
class D {
	public void methodA {
		System.out.println("DDDDDDDDDD");
	}
}
```

定义子类：

```java
class C extends D implements A {
	//未重写methodA方法
}
```

定义测试类

```java
public class Test { 
	public static void main(String[] args) { 
		C c = new C(); 
		c.methodA(); 
	} 
}

输出结果: DDDDDDDDDDDD
```

### 5、接口的多继承

- 一个接口能继承另一个或者多个接口，这和类之间的继承比较相似，接口的继承使用`extends`关键字，子接口继承父接口的方法。**如果父接口的默认方法有重名的，子接口需要重写一次**

定义父接口：

```java
interface A {
	public default void method {
		System.out.println("AAAAAAAAAAAA");
	}
}
interface B {
	public default void method {
		System.out.println("BBBBBBBBBBBB");
	}
}
```

定义子接口

```java
interface C extends A,B{
    @Override
	public default void method {
		System.out.println("DDDDDDDDDD");
	}
}
```

>小提示：
>
>子接口重写默认方法时，default关键字可以保留
>
>子类重写重写默认方法时，default关键字不可以保留

### 6、其他成员特点

- 接口中，无法定义成员变量，但是可以定义常量，其值不可以改变。默认使用public static final 修饰
- 接口中，没有构造方法，不能创建对象
- 接口中，没有静态代码块。

## 五、多态

### 1、概述

**引入**：多态是继封装、继承之后，面向对象的第三大特性。

生活中，比如跑的动作，，小猫、小狗和大象，跑起来是不一样的。再比如飞的动作，昆虫、鸟类和飞机，飞起来也是不一样的。可见，同一行为，通过不同的事物，可以体现出不同的形态。多态，描述的就是这样的状态。

**定义**：多态：是指同一行为，具有不同的表现形式

**前提**【重点】：

>1、继承或实现【二选一】
>
>2、方法的重写【意义体现：不重写，没意义】
>
>3、父类引用指向子类对象【格式体现】

### 2、多态的体现

多态体现的格式：

```
父类类型 变量名 = new 子类对象；
变量名.方法名();
```

>父类类型：指子类对象继承得父类类型，或者实现得父接口类型。

代码如下：

```java
Fu f = new Zi();
f.method();
```

**当使用多态方式调用方法时，首先检查父类中是否有该方法，如果没有，则编译错译；如果有，执行的是子类重写后的方法**

定义父类：

```java
public abstract class Animal {
    public abstract void eat();
}
```

定义子类：

```java
class Cat extends Animal {
	public void eat() {
		System.out.println("吃鱼");
	}
}
class Dog extends Animal {
    public void eat() {
        System.out.println("吃骨头");
    }
}
```

定义测试类：

```java
public class Test {
	public static void main(String[] args){
        //多态形式，创建对象
        Animal a1 = new Cat();
        //调用的是 Cat 的eat
        a1.eat();
        
        //多态性是，创建对象
        Animal a2 = new Dog();
        //调用的 Dog 的eat
        a2.eat();
    }
}
```

### 3、多态的好处

实际的开发过程中，父类类型作为方法形式的参数，传递子类对象给方法，进行方法的调用，更能体现出多态的扩展性和便利。

定义父类：

```java
public abstract class Animal {
    public abstract void eat();
}
```

定义子类：

```java
class Cat extends Animal {
	public void eat() {
		System.out.println("吃鱼");
	}
}
class Dog extends Animal {
    public void eat() {
        System.out.println("吃骨头");
    }
}
```

定义测试类：

```java
public class Test {
	public static void main(String[] args){
        //多态形式，创建对象
        Cat c = new Cat();
        Dog d = new Dog();
        
        //调用showCatEat
        showCatEat(c);
        //调用showDogEat
        showDogEat(d);
        
        //以上两个方法, 均可以被showAnimalEat(Animal a)方法所替代 而执行效果一致
        showAnimalEat(c);
        showAnimalEat(d);
    }
    
    public static void ShowCatEat(Cat c) {
        c.eat();
    }
    
    public static void ShowDogEat(Dog d) {
        d.eat();
    }
    
    public static void ShowAnimalEat(Animal a) {
        a.eat();
    }
}
```

由于多态特性的支持，showAnimalEat方法是Animal类型，是Cat和Dog的父类类型，父类类型接收子类对象，当然可以把Cat对象和Dog对象，传递给方法。

当eat方法执行时，多态规定，执行的是子类重写的方法，那么效果自然和showCatEat、showDogEat方法一致，所以showAnimalEat完全可以替代以上两种方法

不仅仅是替代，在扩展性方面，无论之后再多的子类出现，我们都不再需要编写showXxxEat方法了，直接使用showAnimalEat都可以完成

所以，多态的好处，体现在，可以使程序编写的更简单，并有良好的扩展性。

### 4、引用类型转换

多态的转型分为向上转型与向下转型两种

**向上转型**

- 多态本身是子类类型向父类类型向上转换的过程，这个过程使默认的。

当父类引用指向一个子类对象，便是向上转型。

```java
父类类型 变量名 = new 子类类型();
Animal a = new Cat();
```

**向下转型**

- 父类类型向子类类型向下转换的过程，这个过程是强制的。

一个已经向上转型的子类对象，将父类引用转为子类引用，可以使用强制类型转换的格式，便是向下转型

```java
子类类型 变量名 = (子类类型) 父类变量名
Cat c = (Cat) a;
```

**为什么要类型转换**

当使用多态方式调用方法时，首先检查父类中是否含有该方法，如果每天，则编译错误。也就是说，**不能调用**子类拥有而父类没有的方法。所以想调用子类特有的方法，必须做向下转型。

定义类：

```java
abstract class Animal {
	abstract void eat();
}

class Cat extends Animal {
    public void eat() {
        System.out.println("吃鱼");
    }
    public void catchMouse(){
        System.out.println("抓老鼠");
    }
}

class Dog extends Animal {
    public void eat() {
        System.out.println("吃骨头");
    }
    public void watchHouse() {
        System.out.println("看家");
    }
}
```

定义测试类：

```java
public class Test {
	//向上转型
    Animal a = new Cat();
    a.eat();       //调用的是Cat 的 eat()

    //向下转型
    Cat c = (Cat) a;
    c.catchMouse(); //调用的是 Cat 的 catchMouse
}
```

**转型的异常**

转型的过程中，一不小心就会遇到这样的问题，

```java
public class Test {
	//向上转型
    Animal a = new Cat();
    a.eat();       //调用的是Cat 的 eat()

    //向下转型
    Dog d = (Dog) a;
    d.catchMouse(); //调用的是 Dog 的 watchMouse 【运行报错】
}
```

这段代码可以通过编译，但是运行时，却报出了`ClassCastException` ，类型转换异常！这是因为，明明创建了 Cat类型对象，运行时，当然不能转换成Dog对象的。这两个类型并没有任何继承关系，不符合类型转换的定义。 

为了避免`ClassCastException`的发生，Java提供了 `instanceof` 关键字，给引用变量做类型的校验，格式如下：

```
变量名 instanceof 数据类型 
如果变量属于该数据类型，返回true。 
如果变量不属于该数据类型，返回false。
```

所以，转换前，最好先做一个判断，代码如下：

```java
public class Test {
	public static void main(String[] args) {
        //向上转型
        Animal a = new Cat();
        a.eat();
        
        //向下转型
        if(a instanceof Cat) {
            Cat c = (Cat) a;
            c.catchMouse();
        } else if (a instanceof Dog) {
            Dog d = (Dog) a;
            d.watchHouse();
        }
    }
}
```

## 六、final

### 1、概述

- final：不可改变。可用于修饰类、方法和变量

  >类：被修饰的类，不可以被继承
  >
  >方法：被修饰的方法，不能被重写
  >
  >变量：被修饰的变量，不能被重新赋值

### 2、使用方式

**修饰类**

```java
final class 类名{
}
```

>`public final class String`、`public final class Math `、`public final class Scanner`

**修饰方法**

```java
修饰符 final 返回值类型 方法名(参数列表) {
	//方法体
}
```

>重写被`final`修饰的方法，编译时就会报错

**修饰变量**

>1、局部变量——基本类型：基本类型的局部变量，被final修饰后，只能赋值一次，不能再更改。
>
>2、局部变量——引用类型：引用类型的局部变量，被final修饰后，只能指向一个对象，地址不能再更改。但												是不影响对象内部的成员变量值的修改
>
>3、成员变量：被final修饰的常量名称，一般都有书写规范，所有字母都**大写**。 

## 七、权限修饰符

包括以下四种：

>- public：公共的
>- protected：受保护的
>- default：默认的
>- private：私有的

|                            | public | protected | default | private |
| -------------------------- | ------ | --------- | ------- | ------- |
| 同一个类中                 | 可以   | 可以      | 可以    | 可以    |
| 同一个包中（子类与无关类） | 可以   | 可以      | 可以    |         |
| 不同包的子类               | 可以   | 可以      |         |         |
| 不同包中的无关类           | 可以   |           |         |         |

编写代码时，如果没有特殊的考虑，建议这样使用权限： 

- 成员变量使用 private ，隐藏细节。 

- 构造方法使用 public ，方便创建对象。 

- 成员方法使用 public ，方便调用方法。 

> 小贴士：不加权限修饰符，其访问能力与default修饰符相同 

## 八、内部类

### 1、概述

- **内部类**：将一个类A定义在另一个类B里面，里面的那个类A就成为**内部类**，B则称为**外部类**

- **成员内部类**：定义在类中方法外的类

  ```java
  class 外部类{
  	class 内部类{
  		
  	}
  }
  ```

  在描述事务时，若一个事务内包含其他事务，就可以使用内部类这种结构。

  ```java
  class Car { //外部类
  	class Engine { 内部类
  		
  	}
  }
  ```

- **访问特点**

  >1、内部类可以直接访问外部类的成员，包括私有的成员
  >
  >2、外部类要访问内部类的成员，必须要建立内部类的对象

  创建内部类对象格式：

  ```java
  外部类名.内部类名 对象名 = new 外部类型().new 内部类型();
  ```

  定义类：

  ```java
  public class Person {
  	private booleab live = true;
      class Heart {
          public void jump() {
              //直接访问外部类成员
              if(live) {
                  System.out.println("心脏在跳动");
              } else {
                  System.out.println("心脏不跳了");
              }
          }
      }
      
      public boolean isLive() {
          return live;
      }
      
      public void setLive(boolean live) {
          this.live = live;
      }
  }
  ```

  定义测试类：

  ```java
  public class Test {
  	public static void main(String[] args){
  		//创建外部类对象
          Person p = new Person();
          //创建内部类对象
          Heart heart = p.new Heart();
          
          //调用内部类方法
          heart.jump();
          //调用外部类方法
          p.setLive(false);
          //调用内部类方法
          heart.jump();
  	}
  }
  
  输出结果: 心脏在跳动 心脏不跳了
  ```

  >内部类依然是一个独立的类，在编译之后内部类会被编译成独立的.class文件，但是前面冠名以外部类的类名和$符合
  >
  >比如：Person$Heart.class

### 2、匿名内部类【重点】

- **匿名内部类**：是内部类的简化写法，它的本质是一个`带具体实现``父类或者父接口``匿名的`**子类对象**。

开发中，最常用到的内部类就是匿名内部类了。以接口举例，当你使用一个接口时，似乎得做如下几个操作：

​	1、定义子类

​	2、重写接口的方法

​	3、创建子类对象

​	4、调用重写后的方法

我们的目的，最终只是为了调用方法，那么能不能简化一下，把以上四步合成一步呢？匿名内部类就是做这样的方式。

**前提**

- 匿名内部类必须**继承一个父类**或者**实现一个父接口**

**格式**

```java
new 父类名或接口名() {
	//方法重写
	@Override
	public void method() {
		//执行语句
	}
}
```

**使用方式**

定义抽象父类：

```java
public abstract class FlyAble{
    public abstract void fly();
} 
```

创建匿名内部类，并调用：

```java
public class InnerDemo {
	public static void main(String[] args) {
        //等号右边：是匿名内部类，定义并创建该接口的子类对象
        //等号左边：是多态赋值，接口类型引用指向子类对象
        FlyAble f = new FlyAble() {
            public void fly() {
                System.out.println("我飞了----");
            }
        }
        
        //调用fly方法，指向重写后方法
        f.fly();
    }
}
```

通常在方法形式参数是接口或者抽象类时，也可以将匿名内部类作为参数传递。

```java
public class InnerDemo2 {
	public static void main(String[] args) {
        //等号右边：是匿名内部类，定义并创建该接口的子类对象
        //等号左边：是多态赋值，接口类型引用指向子类对象
        FlyAble f = new FlyAble() {
            public void fly() {
                System.out.println("我飞了----");
            }
        }
        
        //调用fly方法，指向重写后方法
        showFly(f);
    }
    
    public static void showFly(FlyAble f){
        f.fly();
    } 
}
```

以上两步，也可以合并为一步：

```java
public class InnerDemo2 {
	public static void main(String[] args) {
        //创建匿名内部类，直接传递给showFly(FlyAble f)
        showFly(new FlyAble() {
            public void fly() {
                System.out.println("我飞了----");
            });
    }
    
    public static void showFly(FlyAble f){
        f.fly();
    } 
}
```

## 九、引用类型用法总结

实际的开发中，引用类型的使用非常重要，也是非常普遍的。我们可以在理解基本类型的使用方式基础上，进一步 

去掌握引用类型的使用方式。基本类型可以作为成员变量、作为方法的参数、作为方法的返回值，那么当然引用类 

型也是可以的。

### 1、class作为成员变量

定义武器类：

```java
class Weapon {
	String name; // 武器名称
    int hurt; // 伤害值
}
```

定义盔甲类：

```java
class Armour {
    String name; // 装备名称
    int protect; // 防御值
}
```

定义角色类：

```java
class Role {
	int id;
    int blood;
    String Name;
    
    // 添加武器属性 
    Weapon wp;
    // 添加盔甲属性 
    Armour ar;
       
    //提供get/set方法
 	public Weapon getWp() {
        return wp;
    }
    public void setWp(Weapon wp) {
        this.wp = wp;
    }
    
    public Armour getAr() {
        return ar;
    }
    public void setAr(Armour ar) {
        this.ar = ar;
    }
    
    //攻击方法
    public void attack() {
        System.out.println("使用"+ wp.getName() +", 造成"+wp.getHurt()+"点伤害");
    }
    //穿戴盔甲
    public void wear() {
        this.blood += ar.getProtect();
        System.out.println("穿上"+ar.getName()+", 生命值增加"+ar.getProtect());
    }
}
```

定义测试类



## 十、总结

1、抽象类可以有构造，只不过不能new。

2、接口中可以有变量，但是无论你怎么写，最后都是public static final的。

3、抽象类中可以有静态方法，接口中也可以有。

扩展：

1、接口中可以有非抽象的方法，比如default方法（Java 1.8）。

2、接口中可以有带方法体的方法。（Java 1.8）

3、接口中的方法默认是public的。









