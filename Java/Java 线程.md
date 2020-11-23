# Java 线程

## 1、线程基础

### 1.1、并行和并发

- **并发**：指两个或多个事件在**同一时间段内**发生
- **并行**：指两个或多个事件在**同一时刻**发生（同时发生）

### 1.2、线程和进程

- **进程**：是指一个内存中运行的程序，每个进程都有一个独立的运行空间，一个应用程序可以同时运行多个进程；进程是程序的一次执行过程，是系统运行程序的基本单位；系统运行一个程序即是一个进程从创建、运行到消亡的过程。
- **线程**：线程是进程中的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。一个进程中是可以有多个线程的。

>简而言之：一个程序运行后至少有一个进程，一个进程中可以包含多个线程

### 1.3、创建线程类

Java使用`Java.lang.Thread`类代表**线程**，所有的线程对象都必须是Thread类或子类的实例。

Java中通过继承Thread类来**创建**并**启动多线程**的步骤如下：

​	1、定义Thread类的子类，并重写该类的run()方法，该run()方法的方法体就代表了线程需要完成的任务，因此把run()方法称为线程执行体

​	2、创建Thread子类的实例，即创建了线程对象

​	3、调用线程对象的start()方法来启动该线程

代码如下：

测试类：

```java
public class demo {
    public static void main(String[] args){
        //创建自定义线程对象
        MyThread mt = new MyThread("新的线程！");
        //开启线程
        mt.start();
        //在主方法中执行for循环
        for(int i = 0; i < 10; i++ ){
			System.out.println("main线程！"+i);
        }
    }
}
```

自定义线程类

```java
public class MyThread extends Thread {
	//定义执行线程名的构造方法
    public MyThread(String Name){
        //调用父类的String参数的构造方法，
        super(name);
    }
    
    /**
    * 重写run方法，完成该线程执行的逻辑 
    */
    @Override
    public void run(){
        for(int i = 0 ; i < 10 ; i++ ){
			System.out.println(getName()+"：正在执行！"+i);
        }
	}
}
```



## 2、线程

### 2.1、多线程原理

### 2.2、Thread类

- 构造方法：

  >`public Thread()`：分配一个新的线程对象
  >
  >`pblic Thread(String name)`：分配一个指定名字的新的线程对象
  >
  >`public Thread(Runnable target)`：分配一个带有指定目标新的线程对象
  >
  >`public Thread(Runnable target, String name)`：分配一个带有指定目标新的线程对象并指定名字

- 常用方法：

  >`public String getName()`：获取当前线程的名称
  >
  >`public void start()`：导致此线程开始执行；Java虚拟机调用此线程的run方法
  >
  >`public void run()`：此线程要执行的任务在此处定义代码
  >
  >`public static void sleep(long millons)`：是当前正在执行的线程以指定的毫秒数暂停（暂时停止执行）
  >
  >`public static Thread currentThread()`：返回对当前正在执行线程对象的引用

### 2.3、创建线程方式二

采用`java.lang.Runnable`也可以创建线程

步骤如下：

>1、创建Runnable接口的实现类，并重写该接口的run()方法，该run()方法的方法体同样是该线程的线程执行体
>
>2、创建Runnable实现类的实例，并以此实例作为Thread的target来创建Thread对象，改Thread对象才是真正的线程对象
>
>3、调用线程对象的start()方法来启动线程。

代码如下：

```java
public class MyRunnable implements Runnable{
    @Override
    public void run(){
        for (int i = 0; i < 20; i++){
            System.out.println(Thread.currentThread().getName()+" "+ i);
        }
    }
}
```

```java
public class Demo {
	public static void main(String[] args){
        //创建自定义类对象 线程任务对象
        MyRunnable mr = new MyRunnable();
        //创建线程对象
        Thread t = new Thread(mr, '小强');
        t.start();
        for (int i = 0; i < 20; i++) { 
            System.out.println("旺财 " + i); 
        }
    }
}
```

>Runnable对象仅仅作为Thread对象的target，Runnable实现类里包含run()方法仅作为线程的实现体。而实际的对象仍然是Thread实例，只是该Thread线程复制执行其target的run()方法

### 2.4、Thread和Runnable的区别

如果一个类继承Thread，则不适合资源共享。但是如果实现了Runnable接口的话，则很容易实现资源的共享。

**总结**：

实现Runnable接口比继承Thread所具有的优势：

- 适合多个相同的程序代码的线程去共享同一个资源
- 可以避免Java单继承的局限性
- 增加程序的健壮性，实现解耦操作，代码可以被多个线程共享，代码和线程独立
- 线程池只能放入实现Runnable或Callable类线程，不能直接放入继承Thread的类

>扩充：在java中，每次程序运行至少启动2个线程，一个是main线程，一个是垃圾收集线程。因为每当使用 
>
>java命令执行一个类的时候，实际上都会启动一个JVM，每一个JVM其实在就是在操作系统中启动了一个进 
>
>程。 

### 2.5、匿名内部方法实现线程的创建

## 3、线程安全

### 3.1、线程安全

### 3.2、线程同步

### 3.3、同步代码块

### 3.4、同步方法

### 3.5、Lock锁

## 4、线程状态

### 4.1、线程状态概述

### 4.2、Timed Waiting（计时等待）

### 4.3、BLOCKED（锁阻塞）

### 4.4、Waiting（无限等待）

### 4.5、补充知识点

## 5、等待唤醒机制

### 5.1、线程间通信

### 5.2、等待唤醒机制

### 5.3、生产者与消费者问题

## 6、线程池

### 6.1、线程池思想概念

我们使用线程的时候就去创建一个线程，这样实现起来非常方便，但是也会有一个问题：

如果并发的线程数量很多，并且每个线程都是执行一个很短的任务就结束了，这样频繁创建线程就会大大降低系统的效率，因为频繁创建线程和销毁线程需要时间。

那么有没有一种方法能够使线程能够复用，就是执行完一个任务，并不会销毁，而是可以继续执行其他任务呢？

在Java中可以通过线程池来达到这样的效果。

### 6.2、线程池概念

- **线程池**：其实就是一个容纳多个线程的容器，其中的线程可以反复利用，省去了频繁创建线程对象的操作，无需反复创建线程而消耗过多资源

工作原理：

![image-20200915145417967](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20200915145417967.png)

- 合理利用线程池能够带来三个好处：

  >1、降低资源消耗。减少了创建和销毁线程的次数，每个工作线程都可以被重复利用，可执行多个任务
  >
  >2、提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行
  >
  >3、提高线程的客观理性。可以根据系统的承受能力，调整线程池中工作线线程的数目，防止因为消耗过多的内存。而把服务器搞崩溃（每个线程大概需要1MB内存，线程开的越多，消耗的内存也就越大，最后司机。）

### 6.3、线程池的使用







































