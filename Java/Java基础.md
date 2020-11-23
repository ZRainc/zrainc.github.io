# Java 基础

## 1、java异常

### 1、异常

**异常**：指的是程序在执行过程中，出现的非正常的情况，最终导致JVM的非正常停止。

在Java等面向对象的编程语言中，异常本身就是一个类，产生异常就是创建异常对象并抛出了一个异常对象。Java处理异常的方式是中断处理。

>异常指的并不是语法错误，语法错误，编译不通过，不会产生字节码文件，根本不会运行

**异常体系**：异常的根类是 `java.lang.Throwable`，其下有两个子类：`java.lang.Error`和`java.lang.Exception`，我们通常所说的异常是`java.lang.Exception`。

![image-20200909143836611](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200909143836611.png)

**Throwable体系**：

- Error：严重错误，无法通过处理的错误，只能事先避免
- Exception：表示异常：异常产生后程序员可以通过代码的方式纠正，是程序继续运行，是必须要处理好的。

**Throwable中的常用方法：**

- `public void printStackTrace()`：打印异常的详细信息
- `public String getMessage()`：获取发生异常的原因
- `public String toString()`：获取异常的类型和异常描述信息(不用)

**异常分类**

- 编译时异常：在编译期，就会检查，如果没有检查异常，就不能编译通过
- 运行时异常：在运行时期，检查异常

![image-20200909144659422](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200909144659422.png)

### 2、异常的处理

**抛出异常**

throw**用在方法内**，用来抛出一个异常对象，将这

调用者处，并结束当前方法的执行。

```java
public class ThrowDemo {
    public static void main(String[] args){
        //创建一个数组
        int[] arr = {2,4,52,22};
        //根据索引找对应的元素
        int index = 4;
        int element = getElement(arr, index);
        
		System.out.println(element); 
        System.out.println("over");
    }
    
	   /*
		* 根据 索引找到数组中对应的元素 
		*/
    public static int getElement(int[] arr, int index){
        //判断 索引是否越界
        if(index<0 || index>arr.length-1){
           /*判断条件如果满足，当执行完throw抛出异常对象后，方法已经无法继续运算。 
            *这时就会结束当前方法的执行，并将异常告知给调用者。这时就需要通过异常来解决。 
            */
            throw new ArrayIndexOutOfBoundsException("越界")
        }
        int element = arr[index];
        return element;
    }
}
```

>对于调用者来说，该怎么处理？一种是进行捕获处理，另一种就是继续将问题声明出去，使用throws声明处理。

**声明异常throws**

关键字**throws**运用于方法声明之上，用于表示当前方法不处理异常，而是提醒该方法的调用者来处理异常（抛出异常）

```java
public class ThrowsDemo{
    public static void main(String[] args) throws FileNotFoundException{
        read("a.txt");
    }
    
    //如果定义功能时有问题发生需要报告给调用者。可以通过在方法上使用throws关键字进行声明
    public static void read(String path) throws FileNotFoundException{
        if (!path.equals("a.txt")) {
            //如果不是 a.txt这个文件 
            // 我假设 如果不是 a.txt 认为 该文件不存在 是一个错误 也就是异常 
            throw throw new FileNotFoundException("文件不存在"); 
        }
    }
}
```

**捕获异常try...catch**

如果异常出现的话，会立刻终止程序，所以我们要处理异常：

>1、该方法不处理，而是声明抛出，由该方法得调用者来处理（throws）
>
>2、在方法中使用try-catch的语句块来处理异常

- **try**：该代码块中编写可能产生异常的代码
- **catch**：用来进行某种异常的捕获，实现对捕获到的异常进行处理

```java
public class TryCatchDemo{
    public static void main(String[] args){
        try{ // 当产生异常时，必须有处理方式。要么声明，要么捕获
            read("a.txt");
        } catch(FileNotFoundException e) { //括号中需要定义什么呢？
            //try中抛出的是什么异常，在括号中就定义什么异常类型
            System.out.println(e);
        }
        System.out.println("over");
    }
    /*
    * 我们 当前这个方法中 有异常 有编译期异常
    */
    public static void read(String path) throws FileNotFoundException{
        if(!path.equals("a.txt")){ //如果不是a.txt这个文件
            throw new FileNotFoundException("文件不存在");
        }
    }
}
```

>`printStackTrace`：打印异常的跟踪栈信息并输出到控制台。在开发和调试阶段，都要使用。

**finally代码块**

- **finally**：有一些特定的代码无论异常是否发生，都需要执行。在finally代码块中的代码一定会被执行。

> finally不能单独使用

```java
public class TryCatchDemo4{
	public static void main(String[] args){
		try{
            read("a.txt");
        } catch(FileNotFoundException e){
            // 抓取到的是编译器异常 抛出去的是运行期
            throw new RuntimeException(e);
        } finally {
            System.out.println("不管程序怎么样，这里都将会被执行");
		}
        System.out.println("over");
	}
    
   /*
    * 我们 当前这个方法中 有异常 有编译期异常
    */
    public static void read(String path) throws FileNotFoundException{
        if(!path.equals("a.txt")){ //如果不是a.txt这个文件
            throw new FileNotFoundException("文件不存在");
        }
    }
}
```

>当只有在try或者catch中调用退出JVM的相关方法，此时finally才不会执行，否则finally永远会执行

**异常注意事项**

- 运行时异常被抛出可以不处理，既不捕获也不声明抛出。

- **如果finally有return语句，永远返回finally中的结果，避免该情况。**

- 多个异常使用捕获又该如何处理呢？ 

  1. 多个异常分别处理。 

  2. 多个异常一次捕获，多次处理。 

  3. 多个异常一次捕获一次处理。 

  一般我们是使用一次捕获多次处理方式，格式如下： 

  ```java
  try{ 
  	编写可能会出现异常的代码 
  }catch(异常类型A e){ //当try中出现A类型异常,就用该catch来捕获.
  	 处理异常的代码 
  	//记录日志/打印异常信息/继续抛出异常 
  }catch(异常类型B e){ //当try中出现B类型异常,就用该catch来捕获. 
  	处理异常的代码 
  	//记录日志/打印异常信息/继续抛出异常 
  }
  ```

  注意:这种异常处理方式，要求多个catch中的异常不能相同，并且若catch中的多个异常之间有子父类异 

  常的关系，那么子类异常要求在上面的catch处理，父类异常在下面的catch处理。

- 如果父类抛出了多个异常,子类重写父类方法时,抛出和父类相同的异常或者是父类异常的子类或者不抛出异 

  常。

- 父类方法没有抛出异常，子类重写父类该方法时也不可抛出异常。此时子类产生该异常，只能捕获处理，不 

  能声明抛出 

### 3、自定义异常

**自定义异常** ：Java中不同的异常类，分别表示一种不同的异常情况。自定义异常是根据我们开发业务的异常情况来定义异常类

**实例**：

我们模拟注册操作，如果用户名已存在，则抛出异常并提示：亲，该用户名已经被注册。 

首先定义一个登陆异常类：

```java
//业务逻辑异常
public class RegisterException extends Exception{
    //空参构造
    public RegisterException(){
    }
    
    //表示异常提示
    public RegisterException(String message){
        super(message);
    }
}
```

模拟登陆操作，使用数组模拟数据库中的存储的数据，并提供当前注册账户是否存在用于判断

```java
public class Demo{
	//模拟数据库中已经存在的账户
    public static String[] names = {"bill", "hill", "jill"};
    
    public static void main(String[] args){
        //调用方法
        try{
            //可能出现的代码异常
            checkUsername("nill");
            System.out.println("登陆成功")；
        } catch(RegisterException e) {
            //处理异常
            e.printStackTrace();
        }
	}
    
    //判断当前注册账户是否存在
    //因为是编译期异常，又想调用者去处理，所以声明该异常
    public static void checkUsername(String uname) throws LoginException{
        for(String name : names){
            if(name.equals(uname)){ //如果名字在这里面，就抛出登陆异常
                throw new RegisterException("亲"+name+"已经被注册了！");
            }
        } 
        return ture;
    }
}
```

## 2、Java Static关键字

　1、 Java中静态变量只能在类主体中定义，不能在方法中定义。 静态变量属于类所有而不属于方法。

   2、 静态块：用static申明，JVM加载类时执行，仅执行一次

　　   构造块：类中直接用{}定义，每一次创建对象时执行

​      **执行顺序优先级：静态成员变量或静态代码块>main方法>非静态成员变量或非静态代码块>构造方法**

   3、类的加载顺序

　　   (1) 父类静态对象和静态代码块

　　   (2) 子类静态对象和静态代码块

　　   (3) 父类非静态对象和非静态代码块

　　   (4) 父类构造函数

　　   (5) 子类 非静态对象和非静态代码块

　　   (6) 子类构造函数





















































