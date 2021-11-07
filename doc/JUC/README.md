## JUC多线程

JUC即java.util.concurrent，是JSR 166 标准规范的一个实现； JSR 166 以及 J.U.C 包的作者是 Doug Lea 。

JUC框架是Java5中引入的，其中包含框架有：

- AbstractQueuedSynchronizer（AQS框架），JUC 中实现锁和同步机制的基础。
- Locks & Condition（锁和条件变量），比 synchronized、wait、notify 更细粒度的锁机制。
- Executor 框架（线程池、Callable、Future），任务的执行和调度框架。
- Synchronizers（同步器），主要用于协助线程同步，有 CountDownLatch、CyclicBarrier、Semaphore、Exchanger。
- Atomic Variables（原子变量），方便程序员在多线程环境下，无锁的进行原子操作，核心操作是 CAS 原子操作，所谓的 CAS 操作，即 compare and swap，指的是将预期值与当前变量的值比较(compare)，如果相等则使用新值替换(swap)当前变量，否则不作操作。
- BlockingQueue（阻塞队列），阻塞队列提供了可阻塞的入队和出对操作，如果队列满了，入队操作将阻塞直到有空间可用，如果队列空了，出队操作将阻塞直到有元素可用。
- Concurrent Collections（并发容器），说到并发容器，不得不提同步容器。在 JDK1.5 之前，为了线程安全，我们一般都是使用同步容器，同步容器主要的缺点是：对所有容器状态的访问都串行化，严重降低了并发性；某些复合操作，仍然需要加锁来保护；迭代期间，若其它线程并发修改该容器，会抛出 ConcurrentModificationException 异常，即快速失败机制。
- Fork/Join 并行计算框架，这块内容是在 JDK1.7 中引入的，可以方便利用多核平台的计算能力，简化并行程序的编写，开发人员仅需关注如何划分任务和组合中间结果。
- TimeUnit 枚举，TimeUnit 是 java.util.concurrent 包下面的一个枚举类，TimeUnit 提供了可读性更好的线程暂停操作，以及方便的时间单位转换方法。





### 一、基础

#### 1、线程和进程

##### 进程

- 是指一个内存中运行的应用程序，每个进程都有一个独立的内存空间，一个应用程序可以同时运行多个进程。

- 进程也是程序的一次执行过程，是系统运行程序的基本单位。

- 系统运行一个程序即是一个进程从创建、运行到消亡的过程。

##### 线程

- 进程内部的一个独立执行单元；
- 一个进程可以同时并发的运行多个线程，可以理解为一个进程便相当于一个单 CPU 操作系统，而线程便是这个系统中运行的多个任务。

##### 区别

- 进程：有独立的内存空间，进程中的数据存放空间（堆空间和栈空间）是独立的，至少有一个线程。
- 线程：堆空间是共享的，栈空间是独立的，线程消耗的资源比进程小的多。

##### 注意

- 因为一个进程中的多个线程是并发运行的，那么从微观角度看也是有先后顺序的，哪个线程执行完全取决于 CPU 的调度，程序员是不能完全控制的（可以设置线程优先级）。而这也就造成的多线程的随机性。
- Java 程序的进程里面至少包含两个线程，主线程也就是 main()方法线程，另外一个是垃圾回收机制线程。每 当使用 java 命令执行一个类时，实际上都会启动一个 JVM，每一个 JVM 实际上就是在操作系统中启动了一个 线程，java 本身具备了垃圾的收集机制，所以在 Java 运行时至少会启动两个线程。
- 由于创建一个线程的开销比创建一个进程的开销小的多，那么我们在开发多任务运行的时候，通常考虑创建 多线程，而不是创建多进程。

#### 2、多线程创建

##### 继承Thread类

~~~java
MyThread myThread = new MyThread();
myThread.start();
class MyThread extends Thread{
    public void run(){}
}
~~~

##### 实现Runnable接口

~~~java
Thread thread1 = new Thread(new MyRunnable()); // 解耦，可复用
class MyRunnable implements Runnable{// 多实现
    private String name = ""; // 当一个任务被多个线程执行时，共享资源
    public void run() {}
}
~~~

- 在Runnable内可以共享同一资源。
- 避免单继承的局限性。
- 解耦，可复用代码。
- 线程池只能放入实现Runnable或callable类线程，不能直接放入继承Thread的类。

##### 匿名内部类实现Runnable

~~~java
new Thread(new Runnable() {}).start();
~~~

- 每个线程可以执行不同的任务代码。

##### 守护线程

- Java中有两种线程，一种是用户线程，另一种是守护线程。

- 用户线程是指用户自定义创建的线程，主线程停止，用户线程不会停止。

- 守护线程当进程不存在或主线程停止，守护线程也会被停止。

~~~java
thread.setDaemon(true); // 设置为守护线程
~~~

#### 3、线程安全

当有多个线程在同时执行同一个任务，对同一资源进行写操作时，就有可能出现线程不安全问题。

##### 线程同步

~~~java
// 创建对象级别的锁
Object lock = new Object();
// 1、同步代码块
synchronized(lock){
    // 可能会产生线程安全问题的代码
}
~~~

```java
// 2、同步方法，使用的是this级别的锁
public synchronized void method(){
   // 可能会产生线程安全问题的代码
}
// 使用this锁的同步代码块
synchronized(this){
    // 可能会产生线程安全问题的代码
}
```

~~~java
// 3、Lock锁
Lock lock = new ReentrantLock(); // 可重入锁
lock.lock();
// 可能会产生线程安全问题的代码
lock.unlock();

// 尝试获取锁，最多等待1秒，还未获得则返回false，不会造成死锁问题
lock.tryLock(1, TimeUnit.SECONDS);
~~~

##### 死锁

- 多线程死锁：同步中嵌套同步,导致锁无法释放。

- 死锁解决办法：不要在同步中嵌套同步。

#### 4、线程操作

##### 6种状态

~~~java
public enum State {
    NEW, // 线程创建，还未启动
    RUNNABLE, // 运行中，而是否在执行任务，则取决于系统处理器
    BLOCKED, // 锁阻塞，试图获取锁，获取到锁后进入Runnable
    WAITING, // 无限等待，无法自动唤醒，需要调用notify或者notifyall。
    TIMED_WAITING, // 计时等待，等待一定时间后自动唤醒，如Thread.sleep/Object.wait。
    TERMINATED; // 被终止，run任务结束，或者没捕获的异常终止。
}
~~~

##### wait/notify

Object方法：

- wait：进入WAITING无限等待。
- notify：唤醒当前线程RUNNABLE运行。
- notifyAll：唤醒所有等待线程RUNNABLE运行。

##### wait/sleep

区别：

- wait是Object的方法，sleep是Thread的方法。
- wait释放锁，进入无限等待。sleep则让出cpu资源，不会释放锁，但仍处于监控状态，等待一段时间后恢复运行状态。

#### 5、线程退出

##### 标志退出

- 设置退出标志，使线程正常退出。（推荐）

##### interrupt

使用interrupt()方法标志中断线程。

- 阻塞状态
  - 如使用了sleep,同步锁的wait,socket中的receiver,accept等方法时，会使线程处于阻塞状态。
  - 当调用线程的interrupt()方法时，会抛出InterruptException异常。
  - 要想正常退出，需通过代码捕获该异常，然后break跳出循环状态。
- 非阻塞状态
  - 使用isInterrupted()判断线程的中断标志来退出循环。
  - 当使用interrupt()方法时，中断标志就会置true，和使用自定义的标志来控制循环是一样的道理。

~~~java
while(true){
    try {
        // 判断线程的中断标志来退出循环
        if (Thread.currentThread().isInterrupted()) {
            break;
        }
        Thread.sleep(100l); // 阻塞状态
    } catch (InterruptedException e) { // 捕获异常退出
        e.printStackTrace();
        break;
    }
}
~~~

##### stop

- 使用stop方法强行终止线程。（极不安全，且该方法已被废弃）

#### 6、线程优先级

现今操作系统基本采用分时的形式调度运行的线程，线程分配得到时间片的多少决定了线程使用处理器资源的多少，也对应了线程优先级这个概念。

##### priority

在JAVA线程中，通过一个int priority来控制优先级，范围为1-10，其中10最高，默认值为5。

```java
// 设置线程优先级
t.setPriority(10);
```

##### join

- join作用是让其他线程变为等待。
- Join把指定的线程加入到当前线程，可以将两个交替执行的线程合并为顺序执行的线程。

- 比如在线程B中调用了线程A的Join()方法，直到线程A执行完毕后，才会继续执行线程B。

##### yield

- yield()方法的作用：暂停当前正在执行的线程，并执行其他线程。（可能没有效果）
- yield()让当前正在运行的线程回到可运行状态，以允许具有相同优先级的其他线程获得运行的机会。
- 因此，使用yield()的目的是：让具有相同优先级的线程之间能够适当的轮换执行。
- 但是，实际中无法保证yield()达到让步的目的，因为，让步的线程可能被线程调度程序再次选中。

#### 7、并发的3个特性

##### 原子性

- 即一个操作或多个操作，要么都执行成功，要么都不执行。
- 例如：银行转账问题。

##### 可见性

- 当一个线程修改了这个变量的值，其他线程能够立即看到修改的值。

~~~java
// 举个反例，线程2在i未赋值之前赋值了j，会导致程序错误。
// 这就是可见性问题，线程1对变量i修改了之后，线程2没有立即看到线程1修改的值。
// 线程1 代码1
int i = 0;
// 线程1 代码2
i = 10;

// 线程2 代码1
j = i;
~~~

##### 有序性

- 有序性：程序执行的顺序按照代码的先后循序执行。
- JVM并不会按照实际编写代码的顺序执行，有可能发生指令重排序（Instruction Reorder）。
- 为什么重排序：处理器为了提高程序运行效率，可能会对输入代码进行优化，它不保证程序中各个语句的执行顺序同代码中的顺序一致。
- as-ifserial：无论如何重排序，程序最终执行结果和代码顺序执行的结果是一致的。（Java编译器、运行时和处理器都会保证java在单线程下遵循as-if-serial语意）

~~~java
// 1、单线程环境下
// 有可能执行的顺序是 2 1 3 4，不可能执行的顺序是 2 1 4 3
int a = 10; // 语句1
int b = 2; // 语句2
a = a + 3; // 语句3
b = a*a; // 语句4
~~~

~~~java
// 2、多线程环境下
// 由于语句1和语句2没有数据依赖性，因此可能会被重排序。
// 如果重排序，会导致线程2在未完成初始化就结束循环，再执行execute(conext)报错
// 线程1:
init = false
context = loadContext(); // 语句1
init = true; // 语句2

// 线程2:
while(!init){ // 如果初始化未完成，等待
  sleep();
}
execute(context); // 初始化完成，执行逻辑
~~~

- 重排序不会影响单个线程的执行，但是会影响到线程并发执行的正确性。

- 要想并发程序正确地执行，必须要保证原子性、可见性以及有序性。只要有一个没有被保证，就有可能会导致程序运行不正确。

### 二、内存模型

#### 1、JVM内存结构

JVM内存结构，由Java虚拟机规范定义。描述的是Java程序执行过程中，由JVM管理的不同数据区域。各个区域有其特定的功能。

PC寄存器、Java虚拟机栈、本地方法栈、Java堆、方法区

#### 2、Java对象模型

HotSpot虚拟机，每一个Java类，在被JVM加载的时候，JVM会给这个类创建一个`instanceKlass`对象，保存在方法区，用来在JVM层表示该Java类

堆、栈、方法区

#### 3、Java内存模型

https://www.hollischuang.com/archives/2550

- JMM（Java Memory Model）
  - 线程对共享变量的所有操作都必须在自己的工作内存（本地内存）中进行，不能直接从主内存中读写。
  - 不同线程之间无法直接访问其他线程本地内存中的变量，线程间变量值的传递需要经过主内存来完成。

- 主内存
  - 主要存储的是Java实例对象，所有线程创建的实例对象都存放在主内存中（成员变量、局部变量、类信息、常量、静态变量）。
  - 多线程操作主内存有可能发生线程安全问题。
- 本地内存
  - 主要存储当前方法的所有本地变量信息（从主内存中拷贝的），每个线程只能访问自己的本地内存。
  - 由于线程间无法相互访问工作内存，因此工作内存的数据不存在线程安全问题。

#### 4、可见性问题

~~~java
public class Demo1Jmm {
    public static void main(String[] args) throws InterruptedException {
        JmmDemo demo = new JmmDemo();
        new Thread().start();
        Thread.sleep(100);
        demo.flag = false;
        System.out.println("flag修改为：" + demo.flag);
    }
    static class JmmDemo implements Runnable {
        public boolean flag = true;
        public void run() {
            System.out.println("线程执行");
            while (flag) {}
            System.out.println("线程结束");
        }
    }
}
~~~

运行结果：没有打印”线程结束“，且程序也没有结束。

原因：线程之间的变量是不可见的，因为读取的是副本，没有及时读取到主内存结果。 

解决办法：强制线程每次读取该值的时候都去“主内存”中取值

#### 5、synchronized

可以保证同一时刻只有一个线程执行代码块，也就是说可以保证并发的原子性、可见性、有序性。

- 保证可见性的两条规则
  - 线程加锁时：清空本地内存，从主内存中重新读取最新的值到本地内存。
  - 线程解锁前：将工作内存中共享变量的值更新到主内存中。
- 同步原理
  - 普通同步方法：对象（this）级别锁。
  - 静态同步方法：类（class）级别锁。
  - 同步代码块：对象（this）级别锁。

### 三、锁优化

- synchronized是重量级锁，效率不高。
- jdk1.6对锁的实现引入了大量的优化，如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。
- 锁的四种状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，他们会随着竞争的激烈而逐渐升级。
- 注意：锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率。

#### 1、自旋锁

情况：线程的阻塞和唤醒需要CPU从用户态转为核心态，频繁的阻塞和唤醒对CPU来说是一件负担很重的工作，势必会给系统的并发性能带来很大的压力。

所谓自旋锁，就是让该线程等待一段时间，不会被立即挂起，看持有锁的线程是否会很快释放锁。怎么等待呢？执行一段无意义的循环即可（自旋）。

- 自旋锁在JDK 1.4.2中引入，默认关闭，使用-XX:+UseSpinning开开启。
- JDK1.6中默认开启。同时自旋的默认次数为10次，可以通过参数-XX:PreBlockSpin来调整。
- JDK1.6引入自适应的自旋锁。

#### 2、适应自旋锁

所谓自适应就意味着自旋的次数不再是固定的，它是由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。成功获取的次数越多，给的自旋数就越多，否则自旋数就少，甚至不自旋。

#### 3、锁消除

情况：为了保证数据的完整性，需要对部分操作进行同步控制，但有些情况下，JVM检测到不可能存在共享数据竞争，这时JVM就会对同步锁进行锁消除。（锁消除的依据是逃逸分析的数据支持）

好处：在使用一些内置API时，如StringBuffer、Vector、HashTable等就会存在隐形的加锁操作，这时JVM会根据逃逸分析清除掉毫无意义的锁。

#### 4、锁粗化

情况：在使用同步锁时，需要让同步块的作用范围尽可能小，仅在共享数据的实际作用域中才进行同步。当存在锁竞争时，能让等待锁的线程也能尽快拿到锁。但如果一系列的连续加锁解锁操作，就有可能导致不必要的性能损耗。

锁粗化，即将多个连续的加锁、解锁操作连接在一起，扩展成一个范围更大的锁。

例如：Vector循环添加元素，JVM检测到有连续的锁，就将加锁操作放到循环外。

#### 5、偏向锁

减少无竞争且只有一个线程使用锁的情况下，使用轻量级锁产生的性能消耗。

轻量级锁的加锁和解锁操作需要依赖多次CAS原子指令。

偏向锁只需要检查是否为偏向锁、锁标识以及ThreadID即可，可以减少不必要的CAS操作。

#### 6、轻量级锁

- 目的是在没有多线程竞争的前提下，减少传统的重量级锁使用操作系统互斥量产生的性能消耗。
- 当关闭偏向锁功能或者多个线程竞争偏向锁导致偏向锁升级为轻量级锁，则会尝试获取轻量级锁。
- 轻量级锁主要使用CAS进行原子操作。

但是对于轻量级锁，其性能提升的依据是“对于绝大部分的锁，在整个生命周期内都是不会存在竞争的”，如果打破这个依据则除了互斥的开销外，还有额外的CAS操作，因此在有多线程竞争的情况下，轻量级锁比重量级锁更慢。

#### 7、重量锁

- 重量级锁通过对象内部的监视器（monitor）实现。
- 其中monitor的本质是依赖于底层操作系统的Mutex Lock（互斥锁）实现。
- 操作系统实现线程之间的切换需要从用户态到内核态的切换，切换成本非常高。

### 四、进阶

#### 1、Volatile

- 比使用synchronized的成本更加低，因为它不会引起线程上下文的切换和调度。

- Java允许线程访问共享变量，为了确保共享变量能被准确和一致地更新，线程应该确保通过排他锁单独获得这个变量。
  - 如果变量被volatile修饰，则java可以确保所有线程看到这个变量的值是一致的。

使用场景：

- 对变量的写入操作不依赖其当前值
  - 例如：count++（count = count + 1）等依赖当前值的情况，有可能会出现线程安全问题。
- 该变量没有包含在其他变量的不变式中
  - 例如：low < up等不变式中有其他变量时，有可能出现线程安全问题。

总结：变量真正独立于其他变量和自己以前的值，在单独使用的时候，适合用volatile

对比synchronized：

- volatile不需要加锁，比synchronized更轻便，不会阻塞线程。
- synchronized既能保证可见性，又能保证原子性，而volatile只能保证可见性。
- volatile变量是一种非常简单但同时又非常脆弱的同步机制。
  - 简单易用，在某些情况下优于锁的性能和伸缩性。
  - volatile在遵循使用条件下可以使用volatile代替synchronized来优化代码提升效率。

#### 2、内存可见性问题

volatile实现内存可见性的过程：

- 写过程
  - 改变本地内存volatile变量的值。
  - 将改变后的副本从本地内存刷新到主内存。
- 读过程
  - 从主内存中读取volatile变量的最新值到线程的本地内存中。
  - 从本地内存中读取volatile变量的副本。

volatile实现内存可见性的原理：

- 写操作时，通过在写操作指令后加入一条store屏障指令，让本地内存中变量的值能够刷新到主内存中。
- 读操作时，通过在读操作前加入一条load屏障指令，及时读取到变量在主内存的值。

内存屏障（Memory Barrier）是一种CPU指令，用于控制特定条件下的重排序和内存可见性问题。Java编译器也会根据内存屏障的规则禁止重排序

volatile的底层实现是通过插入内存屏障，但是对于编译器来说，发现一个最优布置来最小化插入内存屏障的总数几乎是不可能的，所以，JMM采用了保守策略。如下：

- StoreStore屏障可以保证在volatile写之前，其前面的所有普通写操作都已经刷新到主内存中。
- StoreLoad屏障的作用是避免volatile写与后面可能有的volatile读/写操作重排序。
- LoadLoad屏障用来禁止处理器把上面的volatile读与下面的普通读重排序。
- LoadStore屏障用来禁止处理器把上面的volatile读与下面的普通写重排序。

#### 3、原子性问题

多线程情况下进行count++会出错，原因是count++并不是原子操作。

解决方案：

~~~java
// 1、使用synchronized
public synchronized void addCount() {
    for (int i = 0; i < 10000; i++) {
        count++;
    }
}
// 2、使用ReentrantLock可重入锁
private Lock lock = new ReentrantLock();
public void addCount() {
    for (int i = 0; i < 10000; i++) {
        lock.lock();
        count++;
        lock.unlock();
    }
}
// 3、使用AtomicInteger原子操作
public static AtomicInteger count = new AtomicInteger(0);
public void addCount() {
    for (int i = 0; i < 10000; i++) {
        //count++;
        count.incrementAndGet();
    }
}
~~~

### 五、CAS

CAS，Compare And Swap，即比较并交换。

同步组件中大量使用CAS技术实现了Java多线程的并发操作。整个AQS同步组件、Atomic原子类操作等等都是以CAS实现的，甚至ConcurrentHashMap在1.8的版本中也调整为了CAS+Synchronized。可以说CAS是整个JUC的基石。

#### synchronized同步分析

##### 实验

~~~java
public class Demo2Synchronized {
    public void test2() {
        synchronized (this) {
        }
    }
}
~~~

~~~shell
# 编译
javac Demo2Synchronized.java
# 查看字节码
javap -c Demo2Synchronized.class
~~~

~~~text
monitorenter # monitor进入，获取锁
monitorexit # monitor退出，释放锁
~~~

monitorenter和monitorexit这两个jvm指令实现锁的使用，主要是基于 Mark Word和、monitor。

##### Mark Word

Hotspot虚拟机的对象头主要包括两部分数据：

- Mark Word（标记字段）。
  - 用于存储对象自身的运行时数据。
  - 如：哈希码（HashCode）、GC分代年龄、锁状态标志、线程持有的锁、偏向线程 ID、偏向时间戳等。
- Klass Pointer（类型指针）。
  - 是对象指向它的类元数据的指针。虚拟机通过这个指针来确定这个对象是哪个类的实例。

Java对象头一般占有两个机器码（在32位虚拟机中，1个机器码等于4字节，也就是32bit），但是如果对象是数组类型，则需要三个机器码，因为JVM虚拟机可以通过Java对象的元数据信息确定Java对象的大小，但是无法从数组的元数据来确认数组的大小，所以用一块来记录数组长度。

对象头的存储结构（32位虚拟机）：

- 对象hashCode（25bit）、对象的分代年龄（4bit）、是否是偏向锁（1bit）、锁标志位（2bit）。

对象头信息是与对象自身定义的数据无关的额外存储成本，但是考虑到虚拟机的空间效率，Mark Word被设计成一个**非固定的数据结构**以便在极小的空间内存存储尽量多的数据，它会根据对象的状态复用自己的存储空间，也就是说，Mark Word会随着程序的运行发生变化，变化状态如下（32位虚拟机）：

| 锁状态   | 30bit                      | 2bit |
| -------- | -------------------------- | ---- |
| 无锁状态 | 对象hashcode、对象分代年龄 | 01   |
| 轻量级锁 | 指向锁记录的指针           | 00   |
| 重量级锁 | 指向重量级锁的指针         | 10   |
| GC标记   | 空，不需要记录信息         | 11   |

| 锁状态 | 23bit  | 2bit  | 4bit         | 1bit是否是偏向锁 | 2bit锁标志位 |
| ------ | ------ | ----- | ------------ | ---------------- | ------------ |
| 偏向锁 | 线程ID | Epoch | 对象分代年龄 | 1                | 01           |

##### monitor

什么是Monitor？我们可以把它理解为一个同步工具，也可以描述为一种同步机制，它通常被描述为一个对象。与一切皆对象一样，所有的Java对象是天生的Monitor，每一个Java对象都有成为Monitor的潜质，因为在Java的设计中 ，每一个Java对象都带了一把看不见的锁，它叫做内部锁或者Monitor锁。

Monitor 是线程私有的数据结构，每一个线程都有一个可用monitor record列表，同时还有一个全局的可用列表。每一个被锁住的对象都会和一个monitor关联（对象头的MarkWord中的LockWord指向monitor的起始地址），同时monitor中有一个Owner字段存放拥有该锁的线程的唯一标识，表示该锁被这个线程占用。

monitor结构：

- Owner：初始时为NULL表示当前没有任何线程拥有该monitor record，当线程成功拥有该锁后保存线程唯一标识，当锁被释放时又设置为NULL。
- EntryQ：关联一个系统互斥锁（semaphore），阻塞所有试图锁住monitor record失败的线程。
- RcThis：表示blocked或waiting在该monitor record上的所有线程的个数。
- Nest：用来实现重入锁的计数。
- HashCode：保存从对象头拷贝过来的HashCode值（可能还包含GC age）。
- Candidate：用来避免不必要的阻塞或等待线程唤醒，因为每一次只有一个线程能够成功拥有锁，如果每次前一个释放锁的线程唤醒所有正在阻塞或等待的线程，会引起不必要的上下文切换（从阻塞到就绪然后因为竞争锁失败又被阻塞）从而导致性能严重下降。Candidate只有两种可能的值0表示没有需要唤醒的线程1表示要唤醒一个继任线程来竞争锁。

#### CAS原理

CAS的思想：

- 三个参数：当前内存值V、旧的预期值A、即将更新值B。
- 如果预期值A与更新值B相等，则将内存值修改为B，并返回true。
- 如果不相等，则返回false。
- 如果CAS操作失败，通过自旋的方式等待并再次尝试，直到成功。

JUC下的atomic类都是通过CAS来实现的。

- 例如：AtomicInteger，在其中使用了Unsafe unsafe = Unsafe.getUnsafe()。

- Unsafe 是CAS的核心类，它提供了硬件级别的原子操作，操作的值也进行了volatile修饰，保证内存可见性。

~~~java
// AtomicInteger加法
public final int addAndGet(int delta) {
    return unsafe.getAndAddInt(this, valueOffset, delta) + delta;
}
// Unsafe
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));
    return var5;
}
// 对象、对象的地址、预期值、修改值
public final native boolean compareAndSwapInt(Object var1, long var2, int var4, int var5);
~~~

Unsafe 是一个比较危险的类，主要是用于执行低级别、不安全的方法集合。尽管这个类和所有的方法都是公开的（public），但是这个类的使用仍然受限，你无法在自己的java程序中直接使用该类，因为只有授信的代码才能获得该类的实例。

可是为什么Unsafe的native方法就可以保证是原子操作呢？

#### native关键字

native关键字声明为本地方法。Java无法直接访问底层操作系统，但有能力调用其他语言编写的函数or方法，是通过JNI(Java Native Interfface)实现。

使用native告诉JVM这个方法是在外部定义的：

1. javac生成.class文件：`javac NativePeer.java`
2. javah生成.h文件：`javah NativePeer`
3. 编写c语言文件，在其中include进上一步生成的.h文件，然后实现其中声明而未实现的函数
4. 生成dll共享库，然后Java程序load库，调用即可。

native优势：

- 很多层次上用Java去实现是很麻烦的，而且Java解释执行的效率也差了c语言很多，纯Java实现可能会导致效率不达标，或者可读性奇差。
- Java毕竟不是一个完整的系统，它经常需要一些底层的支持，通过JNI和native method我们就可以实现jre与底层的交互，得到强大的底层操作系统的支持，使用一些Java本身没有封装的操作系统的特性。

#### 多CPU的CAS处理

CAS可以保证一次的读、改、写操作是原子操作，在单处理器上该操作容易实现。

CPU提供了两种方法来实现多处理器的原子操作：

- 总线加锁
  - 处理器提供一个LOCK#信号，当一个处理器在总线上输出此信号时，其他处理器的请求将被阻塞，该处理器可以独占使用共享内存。
  - 在锁定期间，其他处理器不能处理内存地址的数据，开销有点大。
- 缓存加锁
  - 在内存区域的数据如果在加锁期间，当执行锁操作写回内存时，处理器不输出LOCK#信号，而是修改内部的内存地址，利用缓存一致性协议来保证原子性。
  - 缓存一致性机制可以保证同一个内存区域的数据仅能被一个处理器修改。

#### CAS的缺陷

- 循环时间太长
  - 如果自旋CAS长时间地不成功，则会给CPU带来非常大的开销。
  - JUC中有些地方就限制了CAS自旋的次数，例如BlockingQueue的SynchronousQueue。
- 只能保证一个共享变量原子操作
- ABA问题
  - CAS需要检查操作值有没有发生改变，如果没有发生改变则更新。
  - 如果一个值原来是A，变成了B，然后又变成了A，那么在CAS检查的时候会发现没有改变。
  - 解决方案是给每个变量都加上一个版本号。例如1A->2B->3A
  - java提供了AtomicStampedReference来解决，其通过包装[E,Integer]的元组来对对象标记版本戳stamp。

### 六、atomic包

AtomicInteger内部维护一个对应基本类型的成员变量value，这个变量是被volatile修饰的，保证多线程下的可见性。在进行原子性操作时，依赖Unsafe类里面的CAS方法，原子操作通过自旋方式，不断地使用CAS函数进行尝试直到达到自己的目的。

#### 1、基本数据类型

- AtomicInteger：整型原子类。
- AtomicLong：长整型原子类。
- AtomicBoolean：布尔型原子类。

~~~java
// AtomicInteger举例
// 获取值
get()
// 获取值，再加法
getAndAdd(int)
// 获取值，再减1
getAndDecrement()
// 获取值，再加1
getAndIncrement()
// 获取值，再设值
getAndSet(int)
// 加法，返回结果
addAndGet(int)
// 减1，返回结果
decrementAndGet()
// 加1，返回结果
incrementAndGet()
// 当get时才会set
lazySet(int)
// 尝试新增后对比，若增加成功则返回true否则返回false
compareAndSet(int, int)
~~~

#### 2、引用类型

- AtomicReference：引用类型原子类。
- AtomicStampedReference：更新引用类型的字段原子类。
  - 对AtomicReference的再一次包装，增加了一层引用和计数器。
- AtomicMarkableReference：更新带有标记位的引用类型原子类。
  - 跟AtomicStampedReference差不多，区别在于更加简单的是和否关系。ABA问题只有两种状态。

~~~java
// AtomicReference 比较值再更新
compareAndSet(u1, u2);
// AtomicStampedReference 比较值和版本再更新
compareAndSet(u1, u2, false, 1);
// AtomicMarkableReference 比较值，版本true则更新
compareAndSet(u1, u2, false, true);
~~~

#### 3、数组类型

- AtomicIntegerArray：整型数组原子类。
- AtomicLongArray：长整型数组原子类。
- AtomicReferenceArray：引用类型数组原子类。

~~~java
// AtomicIntegerArray
// 1索引，2旧值，3新值
compareAndSet(1, 2, 200);
// AtomicReferenceArray 同上
compareAndSet(int, Object, Object);
~~~

#### 4、对象的属性修改类型

- AtomicIntegerFieldUpdater：更新整型字段的更新器。
- AtomicLongFieldUpdater：更新长整型字段的更新器。
- AtomicReferenceFieldUpdater：更新引用类型字段的更新器。

使用限制：

- 不能是static类型。前面说到的unsafe提取的是非static类型的属性偏移量，如果是static类型在获取时如果没有使用对应的方法是会报错的，而这个Updater并没有使用对应的方法。
- 不能是final类型的，因为final根本没法修改。
- 必须是volatile类型的数据，也就是数据本身是读一致的。
- 属性必须对当前的Updater所在的区域是可见的，也就是private如果不是当前类肯定是不可见的，protected如果不存在父子关系也是不可见的，default如果不是在同一个package下也是不可见的。

实现方式：通过反射找到属性，对属性进行操作。

```java
AtomicIntegerFieldUpdater.newUpdater(User.class, "age");
User user = new User("Java", 22);
a.getAndAdd(user,10);
```

#### 5、JDK1.8新增类

- DoubleAdder：双浮点类型原子类
- LongAdder：长整型原子性。
- DoubleAccumulator：类似DoubleAdder，但更加灵活（需要传入一个函数式接口）。
- LongAccumulator：类似LongAdder，但更加灵活（需要传入一个函数式接口）。

LongAdder是jdk1.8提供的累加器，基于Striped64实现，所提供的API基本上可以替换原先的AtomicLong。

LongAdder类似于AtomicLong是原子性递增或者递减类，AtomicLong已经通过CAS提供了非阻塞的原子性操作，相比使用阻塞算法的同步器来说性能已经很好了，但是JDK开发组并不满足，因为在非常高的并发请求下AtomicLong的性能不能让他们接受，虽然AtomicLong使用CAS但是CAS失败后还是通过无限循环的自旋锁不断尝试。

在高并发下N多线程同时去操作一个变量会造成大量线程CAS失败然后处于自旋状态，这大大浪费了cpu资源，降低了并发性。那么既然AtomicLong性能由于过多线程同时去竞争一个变量的更新而降低的，那么如果把一个变量分解为多个变量，让同样多的线程去竞争多个资源那么性能问题不就解决了？是的，JDK8提供的LongAdder就是这个思路。

相当于将资源分片了，查询时通过sum求和即可。

在非常高的并发请求下LongAdder的效率是AtomicLong的6倍左右。

### 七、AQS

AQS(AbstractQueuedSynchronizer），即队列同步器。它是构建锁或者其他同步组件的基础框架（如ReentrantLock、ReentrantReadWriteLock、Semaphore等），是JUC并发包中的核心基础组件。

AQS解决了实现同步器时涉及到的大量细节问题，例如获取同步状态、FIFO同步队列。基于AQS来构建同步器可以带来很多好处。它不仅能够极大地减少实现工作，而且也不必处理在多个位置上发生的竞争问题。

#### 1、state状态

AQS维护了一个volatile int类型的变量state表示当前同步状态。当state>0时表示已经获取了锁，当state = 0时表示释放了锁。

- getState()：返回同步状态的当前值。
- setState()：设置当前同步状态。
- compareAndSetState()：使用CAS设置当前状态，能够保证状态设置的原子性。

#### 2、资源共享方式

- Exclusive：独占，只有一个线程执行，如ReentrantLock。
- Share：共享，多个线程可同时执行，如Semaphore/CountDownLatch。

自定义同步器主要实现一下几种方法：

- isheldExclusively()：是否在独占式模式下被线程占用，一般该方法表示是否被当前线程所独占。
- tryAcquire(int)：独占方式。尝试获取同步状态，成功则返回true，否则false。其他线程需等待该线程释放同步状态才能获取同步状态。
- tryRelease(int)：独占方式。尝试释放同步状态，成功则返回true，否则false。
- tryAcquireShared(int)：共享方式。尝试获取同步状态。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。
- tryReleaseShared(int)：共享方式。尝试释放同步状态，如果释放后允许唤醒后续等待节点，返回true，否则返回false。

#### 3、CLH同步队列

AQS内部维护着一个FIFO队列，该队列就是CLH同步队列，遵循FIFO原则（ First Input First Output先进先出）。CLH同步队列是一个FIFO双向队列，AQS依赖它来完成同步状态的管理。

双向链表结构的队列，同步器通过头尾指针控制队列的出队和入队操作。

### 八、锁

#### 1、互斥锁

为了保证共享数据操作的完整性，每个对象都对应于一个称为“互斥锁”的标记，用来标记任一时刻，只能有一个线程访问该对象。

#### 2、阻塞锁

让线程进入阻塞状态进行等待，当获得相应的信号（唤醒，时间）时，才可以进入线程的准备就绪状态，准备就绪状态的所有线程，通过竞争，进入运行状态。

#### 3、自旋锁

自旋锁就是让当前线程不停地在循环体内执行实现的，当循环的条件被其他线程改变时，才能进入临界区。

自旋锁不停地执行循环体，不进行线程状态的改变，所以相应速度快。当线程数不停增加时，性能下降明显，因为每个线程都需要执行，占用CPU时间。如果线程竞争不激烈，并且保持锁的时间段，适合使用自旋锁。

#### 4、读写锁

一种特殊的自旋锁，它把对共享资源的访问者划分成读者和写者。

读写锁相对于自旋锁而言，能提高并发性，因为在多处理器系统中，它允许同时有多个读者来访问共享资源，最大可能的读者数为实际的逻辑CPU数。写者是排他性的，一个读写锁同时只能有一个写者或多个读者（与CPU数相关），但不能同时既有读者又有写者。

ReentrantReadWriteLock读写锁实现ReadWriteLock接口

```java
public interface ReadWriteLock {
    Lock readLock();
    Lock writeLock();
}
```

只要没有 writer，读取锁可以由多个 reader 线程同时保持。写入锁是独占的。

读写锁的主要特征：

- 公平性：支持公平性和非公平性。
- 重入性：支持重入。读写锁最多支持65535个递归写入锁和65535个递归读取锁。
- 锁降级：写锁能够降级成读锁，遵循获取写锁、获取读锁在释放锁的次序。读锁不能升级为写锁。

读写锁ReentrantReadWriteLock内部维护着一对锁，需要用一个变量维护多种状态。读写锁采用“按位切割使用”的方式来维护这个变量，将其切分为两部分，高16为表示读，低16为表示写。

假如当前同步状态为S，那么写状态等于 S & 0x0000FFFF（将高16位全部抹去），读状态等于S >>> 16(无符号补0右移16位)。代码如下：

```java
static final int SHARED_SHIFT   = 16;
static final int SHARED_UNIT    = (1 << SHARED_SHIFT);
static final int MAX_COUNT      = (1 << SHARED_SHIFT) - 1;
static final int EXCLUSIVE_MASK = (1 << SHARED_SHIFT) - 1;

static int sharedCount(int c)    { return c >>> SHARED_SHIFT; }
static int exclusiveCount(int c) { return c & EXCLUSIVE_MASK; }
```

读锁为一个可重入的共享锁，它能够被多个线程同时持有，在没有其他写线程访问时，读锁总是获取成功。

锁降级：获取写锁->获取读锁->释放写锁。

#### 5、公平锁与非公平锁

- 公平锁（Fair）：加锁前检查是否有排队等待的线程，优先排队等待的线程，先来先得。
- 非公平锁（Nonfair）：加锁时不考虑排队等待问题，直接尝试获取锁，获取不到自动到队尾等待。
- 非公平锁性能比公平锁高，因为公平锁需要在多核情况下维护一个队列。

>公平锁与非公平锁原理：

公平锁与非公平锁的区别在于获取锁的时候是否按照FIFO的顺序来。释放锁不存在公平性和非公平性，比较非公平锁和公平锁获取同步状态的过程，会发现两者唯一的区别就在于公平锁在获取同步状态时多了一个限制条件：hasQueuedPredecessors()，定义如下：

```java
public final boolean hasQueuedPredecessors() {
    Node t = tail;  //尾节点
    Node h = head;  //头节点
    Node s;

    //头节点 != 尾节点
    //同步队列第一个节点不为null
    //当前线程是同步队列第一个节点
    return h != t && ((s = h.next) == null || s.thread != Thread.currentThread());
}
```

该方法主要做一件事情：主要是判断当前线程是否位于CLH同步队列中的第一个。如果是则返回true，否则返回false。

#### 6、可重入锁

ReentrantLock可重入锁，是一种递归无阻塞的同步机制。它可以等同于synchronized使用，且提供了比synchronized更强大、灵活的锁机制，可以减少死锁发生的概率。ReentrantLock是互斥锁，互斥锁在同一时刻仅有一个线程可以进行访问。

提供了公平锁和非公平锁的选择，构造方法接收一个可选的公平的参数（默认false非公平锁），当设置为true时，表示公平锁。公平锁的效率往往没有非公平锁的效率公安，在许多线程访问的情况下，公平锁表现出较低的吞吐量。

可重入锁的构造方法：

~~~java
public ReentrantLock() {
    //非公平锁
    sync = new NonfairSync();
}
public ReentrantLock(boolean fair) {
    //公平锁
    sync = fair ? new FairSync() : new NonfairSync();
}
~~~

Sync为ReentrantLock里面的一个内部类，它继承AQS（AbstractQueuedSynchronizer），它有两个子类：公平锁FairSync和非公平锁NonfairSync。

~~~java
// 非公平锁
ReentrantLock lock = new ReentrantLock();
// 获取锁
lock.lock();
// 释放锁
lock.unlock();
~~~

> 区别synchronized

- 提供了更多更全的功能，具备更强的扩展性。例如：时间锁等候、可中断锁等候、锁投票。
- 提供了条件Condition，对线程的等待、唤醒操作更加详细和灵活，在多个条件变量和高度竞争锁的地方更加适合使用。
- 提供了可轮询的锁请求。它会尝试获取锁，如果成功则继续，否则等到下次运行时处理。而synchronized则一但进入锁请求要么成功要么阻塞。对比synchronized而言，ReentrantLock不会容易产生死锁。
- 支持更加灵活的同步代码块，但是使用synchronized时，只能在同一个synchronized块结构中获取和释放。
  - 注：ReentrantLock的锁释放一定要在finally中处理，否则可能会产生严重的后果。
- 支持中断处理，且性能较synchronized会好些。

### 九、Condition

在没有Lock之前，我们使用synchronized来控制同步，配合Object的wait()、notify()系列方法可以实现等待/通知模式。在JDK5后，Java提供了Lock接口，相对于Synchronized而言，Lock提供了条件Condition，对线程的等待、唤醒操作更加详细和灵活。

#### 1、使用

Condition是一种广义上的条件队列（等待队列）。他为线程提供了一种更为灵活的等待/通知模式，线程在调用await方法后执行挂起操作，直到线程等待的某个条件为真时才会被唤醒。

一个Condition的实例必须与一个Lock绑定，因此Condition一般都是作为Lock的内部实现。

~~~java
private Lock reentrantLock = new ReentrantLock();
private Condition condition = reentrantLock.newCondition();
reentrantLock.lock();
// 等待信号或中断
condition.await();
// 等待信号或中断，时间
condition.await(long time, TimeUnit unit);
// 等待信号或中断，时间，返回剩余时间
condition.awaitNanos(long nanosTimeout);
// 等待信号，对中断不敏感
awaitUninterruptibly();
// 等待信号或中断，截止时间，前true，后false。
awaitUntil(Date deadline);
// 唤醒一个等待线程
condition.signal();
// 唤醒所有线程
condition.signalAll();
~~~

#### 2、实现

Condition为一个接口，其下仅有一个实现类ConditionObject，由于Condition的操作需要获取相关的锁，而AQS则是同步锁的实现基础，所以ConditionObject则定义为AQS的内部类。

每个Condition对象都包含着一个FIFO队列，该队列是Condition对象通知/等待功能的关键。在队列中每一个节点都包含着一个线程引用，该线程就是在该Condition对象上等待的线程。

Node定义与AQS的CLH同步队列的节点使用的都是同一个类（AbstractQueuedSynchronized.Node静态内部类）

调用Condition的await()方法会使当前线程进入等待状态，同时会加入到Condition等待队列同时释放锁。当从await()方法返回时，当前线程一定是获取了Condition相关连的锁。

调用Condition的signal()方法，将会唤醒在等待队列中等待最长时间的节点（条件队列里的首节点），在唤醒节点前，会将节点移到CLH同步队列中。

### 十、并发工具类

#### 1、CyclicBarrier

CyclicBarrier也叫同步屏障，在JDK1.5被引入的一个同步辅助类，在API中是这么介绍的：允许一组线程全部等待彼此达到共同屏障点的同步辅助。 循环阻塞在涉及固定大小的线程方的程序中很有用，这些线程必须偶尔等待彼此。 屏障被称为循环，因为它可以在等待的线程被释放之后重新使用。

CyclicBarrier的内部是使用重入锁ReentrantLock和Condition。它有两个构造方法：

- CyclicBarrier(int parties)：它将在给定数量的参与者（线程）处于等待状态时启动，但它不会在启动屏障时执行预定义的操作。parties表示拦截线程的数量。
- CyclicBarrier(int parties, Runnable barrierAction) ：创建一个新的 CyclicBarrier，它将在给定数量的参与者（线程）处于等待状态时启动，并在启动屏障时执行给定的屏障操作，该操作由最后一个进入屏障的线程执行。

案例：田径运动员。

#### 2、CountDownLatch

CountDownLatch是一个计数的闭锁，作用与CyclicBarrier有点儿相似。在API中是这么介绍的：用给定的计数 初始化 CountDownLatch。由于调用了 countDown() 方法，所以在当前计数到达零之前，await 方法会一直受阻塞。之后，会释放所有等待的线程，await 的所有后续调用都将立即返回。

这种现象只出现一次（计数无法被重置）。如果需要重置计数，请考虑使用 CyclicBarrier。

- CountDownLatch：一个或者多个线程，等待其他多个线程完成某件事情之后才能执行。（依次执行）
- CyclicBarrier：多个线程互相等待，直到到达同一个同步点，再继续一起执行。（同时执行）

CountDownLatch是通过一个计数器来实现的，计数器的初始值为线程的数量。每当一个线程完成了自己的任务后，计数器的值就会减1。当计数器值到达0时，它表示所有的线程已经完成了任务，然后就可以恢复等待的线程继续执行了。

CountDownLatch内部依赖Sync实现，而Sync继承AQS。CountDownLatch仅提供了一个构造方法：

- CountDownLatch(int count) ： 构造一个用给定计数初始化的 CountDownLatch

- await()方法来使当前线程在锁存器倒计数至零之前一直等待，除非线程被中断。内部使用AQS的getState方法获取计数器，如果计数器值不等于0，则会以自旋方式会尝试一直去获取同步状态。

- countDown() 方法递减锁存器的计数，如果计数到达零，则释放所有等待的线程。内部调用AQS的releaseShared(int arg)方法来释放共享锁同步状态。

案例：接力运动员

#### 3、Semaphore

Semaphore是一个控制访问多个共享资源的计数器，和CountDownLatch一样，其本质上是一个“共享锁”。

Semaphore维护了一个信号量许可集。线程可以获取信号量的许可：当信号量中有可用的许可时，线程能获取该许可。否则线程必须等待，直到有可用的许可为止。 线程可以释放它所持有的信号量许可，被释放的许可归还到许可集中，可以被其他线程再次获取。

案例：停车场

Semaphore常用于约束访问一些（物理或逻辑）资源的线程数量。

当信号量初始化为 1 时，可以当作互斥锁使用，因为它只有两个状态：有一个许可能使用，或没有许可能使用。当以这种方式使用时，“锁”可以被其他线程控制和释放，而不是主线程控制释放。

Semaphore内部包含公平锁（FairSync）和非公平锁（NonfairSync），继承内部类Sync，其中Sync继承AQS（再一次阐述AQS的重要性）。

Semaphore提供了两个构造函数：

- Semaphore(int permits) ：创建具有给定的许可数和非公平的 Semaphore。

- Semaphore(int permits, boolean fair) ：创建具有给定的许可数和给定的公平设置的 Semaphore。
- acquire()：获取许可。
- release()：释放许可。

### 十一、ConcurrentHashMap



 















