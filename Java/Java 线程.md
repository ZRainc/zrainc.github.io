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

程序启动运行main时候，java虚拟机启动一个进程，主线程main在main()调用时被创建。随着调用mt的对象的start方法，另外一个线程也启动了，这样，整个应用就在多线程下运行。

多线程执行时，在栈内存中，其实每一个执行线程都有一片自己所属的栈内存空间。进行方法的压栈和弹栈。

![](D:\mystudy\图片\1606230746(1).jpg)

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

使用线程的匿名内部类的方式，可以方便的实现每个线程执行不同的线程任务操作。

使用匿名内部类的方式实现Runnable接口，重写Runnable接口的run方法

```java
public class NoNameInnerClassThread {
    public static void main(String[] args) {
        Runnable r = new Runnable(){
            public void run(){
                for(int i = 0; i < 20; i++) {
                    System.out.println("张宇：" + i);
                }
            }
        };
        new Thread(r).start();
        for(int i = 0; i < 20; i++) {
            System.out.println("张雨晨：" + i);
        }
    }
}
```



## 3、线程安全

### 3.1、线程安全

如果有多个线程在同时运行，而这些线程可能在同时运行这段代码。程序每次运行的结果和单线程运行的结果是一样的，而且其他的变量的值和预期也是一样的，那么就是线程安全的。

卖票问题不安全，相同的票被卖了多次。

```java
public static class Ticket implements Runnable {
        private int ticket = 100;

        @Override
        public void run() {
            //每个窗口卖票的操作
            //窗口 永远开启
            while (true) {
                if(ticket > 0) { //有票 可以卖
                    //出票操作
                    //使用sleep模拟一下出票时间
                    try{
                        Thread.sleep(1000);
                    }catch (InterruptedException e){
                        e.printStackTrace();
                    }
                    //获取当前线程对象的名字
                    String name = Thread.currentThread().getName();
                    System.out.println(name + "正在卖" + ticket--);
                }
            }
        }

    }

    public static void main(String[] args) {
        //创建线程任务对象
        Ticket ticket = new Ticket();

        //开启三个窗口
        Thread t1 = new Thread(ticket, "窗口1");
        Thread t2 = new Thread(ticket, "窗口2");
        Thread t3 = new Thread(ticket, "窗口3");

        //同时卖票
        t1.start();
        t2.start();
        t3.start();
    }

//输出
窗口2正在卖90
窗口1正在卖90
窗口1正在卖89
窗口3正在卖88
窗口2正在卖88
```

### 3.2、线程同步

当我们使用多个线程访问同一资源时，且多个线程对资源有写的操作，就容易出现线程安全问题。

要解决上述多线程并发访问一个资源的安全性问题：也就是解决重复票和不存在票的问题，Java中提供了同步机制（synchronized）来解决。

有三种方式完成同步操作：

>1、同步代码块
>
>2、同步方法
>
>3、锁机制

### 3.3、同步代码块

- **同步代码块**：`synchronized`关键字可以用于方法中的某个区块中，表示只对这个区块的资源进行互斥访问

  格式：

  ```java
  synchronized(同步锁) {
  	需要同步的代码块
  }
  ```

- **同步锁**：

  对象的同步锁只是一个概念，可以想象为在对象上标记了一个锁。

  1、锁对象：可以是任意类型

  2、多个线程对象，要使用同一把锁

  >注意：在任何时候，最多允许一个线程拥有同步锁，谁拿到锁就进入代码块，其他的线程只能在外面等着（BLOCKED）

使用同步代码块解决代码：

```java
public class Ticket implements Runnable {
    private int ticket = 100;
    
    Object lock = new Object();
    //执行卖票操作
    @Override
    public void run() {
        while(true) {
            synchronized(lock) {
                if()
            }
        }
    }
}
```

### 3.4、同步方法

- **同步方法**：使用`synchronized`修饰的方法，就叫做同步方法，保证A线程执行该方法的时候，其他线程只能在方法外等着

  **格式** ：

  ``` java
  public synchronized void method() {
      可能会产生线程安全问题的代码
  }
  ```

>同步锁是谁？
>
>对于非`static`方法，同步锁就是this
>
>对于`static`方法，我们使用当前方法所在类的字节码对象（类名.class）

使用同步方法代码如下：

```java
   public static class Ticket implements Runnable {
        private int ticket = 100;

        @Override
        public void run() {
            while(true) {
                sellTicket();
            }
        }

        public synchronized void sellTicket() {
            if(ticket > 0) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e){
                    e.printStackTrace();
                }
                //获取当前线程对象的名字
                String name = Thread.currentThread().getName();
                System.out.println(name+"正在卖:"+ticket--);
            }
        }
    }

    public static void main(String[] args) {
        //创建线程任务对象
        Ticket ticket = new Ticket();

        //开启三个窗口
        Thread t1 = new Thread(ticket, "窗口1");
        Thread t2 = new Thread(ticket, "窗口2");
        Thread t3 = new Thread(ticket, "窗口3");

        //同时卖票
        t1.start();
        t2.start();
        t3.start();
    }
```

### 3.5、Lock锁

`java.util.concurrent.locks.Lock`机制提供了比synchronized代码块和synchronized方法更广泛的锁定操作，同步代码块/同步方法具有的功能Lock都有，除此之外更强大，更体现面向对象。

Lock锁也称同步锁，加锁与释放锁方法化了，如下：

- `public void lock()`：加同步锁
- `public void unlock`：释放同步锁

使用如下：

```java
public static void main(String[] args) {
        //创建线程任务对象
        Ticket ticket = new Ticket();

        //开启三个窗口
        Thread t1 = new Thread(ticket, "窗口1");
        Thread t2 = new Thread(ticket, "窗口2");
        Thread t3 = new Thread(ticket, "窗口3");

        //同时卖票
        t1.start();
        t2.start();
        t3.start();
    }

    public static class Ticket implements Runnable {
        private int ticket = 100;

        Lock lock = new ReentrantLock();
        @Override
        public void run() {
            while(true) {
                lock.lock();
                if(ticket > 0) {
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    String name = Thread.currentThread().getName();
                    System.out.println(name + "正在卖" + ticket--);
                }
                lock.unlock();
            }
        }
    }
```

## 4、线程状态

### 4.1、线程状态概述

| 线程状态                | 导致状态发生条件                                             |
| ----------------------- | ------------------------------------------------------------ |
| NEW(新建)               | 线程刚被创建，但是并未启动。还没调用start方法                |
| Runnable(可运行)        | 线程可以在java虚拟机中运行的状态，可能正在运行自己的代码，也可能没有，这取决于操作系统处理器。 |
| Blocked(锁阻塞)         | 当一个线程试图获取一个对象锁，而该对象锁被其他线程所持有，则该线程进入blocked状态；当该线程持有锁时，该线程将变成Runnable状态 |
| Waiting(无限等待）      | 一个线程在等待另一个线程执行一个（唤醒）动作时，该线程进入Waiting状态。进入这个 状态后是不能自动唤醒的，必须等待另一个线程调用notify或者notifyAll方法才能够唤醒。 |
| Timed waiting(计时等待) | 同waiting状态，有几个方法有超时参数，调用他们将进入Timed Waiting状态，这一状态将一直保持到超时期满或者接收到唤醒通知。带有超时参数的常用方法有Thread.sleep、Object.wait |
| Teminated(被终止)       | 因为run方法正常退出而死亡，或者因为没有捕获的异常终止了run方法而死亡。 |

###  4.2、计时等待

一个正在限时等待另一个线程执行一个（唤醒）动作的线程处于这一状态。

**实现一个计数器，计数到100，在每个数字之间暂停1秒，每隔10个数字输出一个字符串**

```java
public static class MyThred implements Runnable {
        @Override
        public void run() {
            for (int i = 0; i < 100; i++) {
                if(i % 10 == 0) {
                    System.out.println("------" + i);
                }
                System.out.println(i);
                try {
                    Thread.sleep(1000);
                    System.out.println("线程睡眠一秒  \n");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public static void main(String[] args) {
        MyThred myThred = new MyThred();
        Thread t = new Thread(myThred);
        t.start();
    }
```

### 4.3、BLOCKED(锁阻塞)

Blocked状态在API中的介绍为：一个正在阻塞等待一个监视器锁（锁对象）的线程处于这一状态。 

我们已经学完同步机制，那么这个状态是非常好理解的了。比如，线程A与线程B代码中使用同一锁，如果线程A获 

取到锁，线程A进入到Runnable状态，那么线程B就进入到Blocked锁阻塞状态。

### 4.4、Waiting

```java
public static class WaitingTest {
        public static Object obj = new Object();

        public static void main(String[] args) {
            //演示waiting
            new Thread(new Runnable() {
                @Override
                public void run() {
                    while (true) {
                        synchronized (obj) {
                            try {
                                System.out.println(Thread.currentThread().getName() + "=== 获取到锁对象，调用wait方法，进入waiting状态，释放锁对象");
                                obj.wait();
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                            System.out.println(Thread.currentThread().getName() + "=== 从waiting状态醒来，获取到锁对象，继续执行了");
                        }
                    }
                }
            }, "等待线程").start();

            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        System.out.println( Thread.currentThread().getName() + "------- 等待3秒钟");
                        Thread.sleep(3000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (obj) {
                        System.out.println(Thread.currentThread().getName() + "获取到锁对象，调用notify方法，释放锁对象");
                        obj.notify();
                    }
                }
            },"唤醒线程").start();
        }
    }

//结果
等待线程=== 获取到锁对象，调用wait方法，进入waiting状态，释放锁对象
唤醒线程------- 等待3秒钟
唤醒线程获取到锁对象，调用notify方法，释放锁对象
等待线程=== 从waiting状态醒来，获取到锁对象，继续执行了
等待线程=== 获取到锁对象，调用wait方法，进入waiting状态，释放锁对象
```

通过上述案例我们会发现，一个调用了某个对象的 Object.wait 方法的线程会等待另一个线程调用此对象的Object.notify()方法 或 Object.notifyAll()方法。其实waiting状态并不是一个线程的操作，它体现的是多个线程间的通信，可以理解为多个线程之间的协作关系，多个线程会争取锁，同时相互之间又存在协作关系。就好比在公司里你和你的同事们，你们可能存在晋升时的竞争，但更多时候你们更多是一起合作以完成某些任务。当多个线程协作时，比如A，B线程，如果A线程在Runnable（可运行）状态中调用了wait()方法那么A线程就进入了Waiting（无限等待）状态，同时失去了同步锁。假如这个时候B线程获取到了同步锁，在运行状态中调用了notify()方法，那么就会将无限等待的A线程唤醒。注意是唤醒，如果获取到锁对象，那么A线程唤醒后就进入Runnable（可运行）状态；如果没有获取锁对象，那么就进入到Blocked（锁阻塞状态）。













### 4.5、补充知识点

![](D:\mystudy\图片\1606369239(1).jpg)

## 5、等待唤醒机制

### 5.1、线程间通信

### 5.2、等待唤醒机制

### 5.3、生产者与消费者问题

包子铺和包子



```java
public static class BaoZi {
        String pier;
        String xianer;
        boolean flag = false; //包子资源是否存在，包子资源状态
    }

    public static class ChiHuo extends Thread {
        private BaoZi bz;

        public ChiHuo(String name, BaoZi bz) {
            super(name);
            this.bz = bz;
        }

        @Override
        public void run() {
            while(true) {
                synchronized (bz) {
                    if( bz.flag == false) {
                        try {
                            bz.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    System.out.println("吃货正在吃" + bz.pier + bz.xianer + "包子");
                    bz.flag = false;
                    bz.notify();

                }
            }
        }
    }

    public static class BaoZiPu extends Thread {
        private BaoZi bz;

        public BaoZiPu(String name , BaoZi bz) {
            super(name);
            this.bz = bz;
        }

        @Override
        public void run() {
            int count = 0;
            while(true) {
                synchronized (bz) {
                    if(bz.flag == true) {
                        try {
                            bz.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }

                    System.out.println("包子铺开始做包子");
                    if(count%2 == 0) {
                        bz.pier = "冰皮";
                        bz.xianer = "五仁";
                    } else {
                        bz.pier = "薄皮";
                        bz.xianer = "牛肉大葱";
                    }
                    count++;

                    bz.flag = true;
                    System.out.println("包子造好了" + bz.pier + bz.xianer);
                    System.out.println("吃货快来吃吧");
                    //唤醒等待线程
                    bz.notify();
                }
            }
        }
    }

    public static void main(String[] args) {
        BaoZi bz = new BaoZi();

        ChiHuo ch = new ChiHuo("吃货", bz);
        BaoZiPu bzp = new BaoZiPu("包子铺", bz);

        ch.start();
        bzp.start();
    }
```

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

使用线程池中线程对象的步骤：

>1、创建线程池对象
>
>2、创建Runnable接口子类对象
>
>3、提交Runnable接口子类对象
>
>4、关闭线程池

```
public static class MyRunnable implements Runnable {

        @Override
        public void run() {
            System.out.println("我要一个教练");
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("教练来了：" + Thread.currentThread().getName());
            System.out.println("教我游泳，交完后，教练回到了游泳池");
        }
    }

    public static void main(String[] args) {
        //创建线程池对象
        ExecutorService service = Executors.newFixedThreadPool(2);//包含两个线程对象
        //创建Runnable实例对象
        MyRunnable r = new MyRunnable();

        //自己创建线程对象的方式
        // Thread t = new Thread(r);
        // t.start(); ‐‐‐> 调用MyRunnable中的run()

        //从线程池中获取线程对象，然后调用MyRunnable中的run
        service.submit(r);
        // 再获取个线程对象，调用MyRunnable中的run()
        service.submit(r);
        service.submit(r);
        //注意：submit方法调用结束后，程序并不终止，是因为线程池控制了线程的关闭
        //将使用完的线程又还回到线程池
        //关闭线程池
        //service.shutdown();
    }
```



































