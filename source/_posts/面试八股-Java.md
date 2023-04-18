---
title: 面试八股--Java
top: false
cover: false
author: DULULU oO
date: 2023-04-11 17:05:27
password:
summary:
tags: Java
categories: 面试
---

## Java的特性

**封装**：把一个对象的属性和方法封装起来，为了提高安全性
**继承**：子类继承父类的方法和熟悉，Java不支持多继承
**多态**：同一个类型的对象在指向同一个方法，可能出现多种行为 如 Animal cat = new cat

实现多态：先继承，再重写父类中的方法，最后向上转型

## 接口与抽象类

接口中可以有 default 方法、静态方法，静态方法可通过接口直接调用， default 方法必须通过对象调用。实现接口的类不能继承接口静态方法，接口中可以声明 abstract 方法，此时，abstract 方法跟接口中的普通方法具有相同效果。

什么时候使用抽象类和接口

**抽象类**：如果你拥有一些方法并且想让它们中的一些有默认实现
**接口**：如果你想实现多重继承，那么你必须使用接口。由于Java不支持多继承，子类不能够继承多个类，可以实现多个接口。因此你就可以使用接口来解决它。
如果基本功能在不断改变，那么就需要使用抽象类。如果不断改变基本功能并且使用接口，那么就要改变所有实现了该接口的类


### 双亲委派机制
如果一个类加载器收到了类加载请求，它不会自己先去加载，而是把请求委托给父类的加载器，父类加载器再委托给父类的父类加载器，直到到达顶层或者某一层的父类加载器无法完成加载任务

作用：
1. 通过类加载器之间的优先级关系，避免重复加载
2. 提高安全性，避免修改java的核心api

## 基本数据类型

byte，boolean: 一个字节 8 bits
short，char: 两个字节 16 bits
int，float：四个字节 32 bits
long，double：八个字节 64 bits

### int和Integer的区别
Integer是int的包装类，int只是一种数据类型
所以Integer必须经过实例化才能使用，Integer实际上是对象的的引用，每次new一个Integer时，是生成一个指向该对象的指针，而int是直接存储数据的值
Integer默认null，int 默认0

### String，StringBuilder，StringBuffer的区别
[参考](https://blog.csdn.net/qq_47183158/article/details/123729517)
#### String：
String底层是**通过char类型**的数据实现的，是被 **final** 修饰的类，不能被继承；String实现了 Serializable 和 Comparable 接口，表示String支持序列化和可以比较大小；所以字符串的值创建之后就不可以被修改，具有不可变性。
```java
String a = "123";
a = "456";

System.out.println(a)	// 打印出来的a为456

```
因为第二次赋值时会生成一个新的对象，a指向新的实例对象，而之前的实例对象如果不再引用会被当作垃圾回收。

实例化一个String对象时：string a = "123";此时的a存在字符串**常量池**中，而new String("123")会存在**堆中**

#### StringBuffer
StringBuffer代表一个**字符序列可变**的字符串，当一个StringBuffer被创建以后，通过StringBuffer提供的append()、insert()、reverse()、setCharAt()、setLength()等方法可以改变这个字符串对象的字符序列，但都不会产生新的对象。通过StringBuffer生成的字符串，可以调用toString()方法将其转换为一个String对象。

```java
StringBuffer b = new StringBuffer("123");
b.append("456");

System.out.println(b);	// b打印结果为：123456
```

且**StringBuffer是线程安全的**：
1. StringBuffer类中的方法都添加了synchronized关键字，用来保证线程安全。

2. StringBuffer 每次**获取 toString** 都会**直接使用缓存区的 toStringCache 值来构造一个字符串**。StringBuffer 的这个toString 方法仍然是同步的。

而 StringBuilder 则每次都需要**复制一次字符数组**，再构造一个字符串。
3. 性能：StringBuffer 是线程安全的，它的所有公开方法都是同步的，StringBuilder 是没有对方法加锁同步的，**所以StringBuilder 的性能要大于 StringBuffer。**

StringBuffer 适用于用在多线程操作同一个 StringBuffer 的场景，而 StringBuilder 更适合单线程场合。

##### StringBuffer的扩容机制



StringBuffer和StringBuilder都是继承自AbstractStringBuilder，它们两个的区别在于buffer是线程安全的，builder是线程不安全的，前者安全效率低，后者高效不安全。它们的扩容机制也是这样的区别，所以我们只需要分析一个的扩容就可以了，分析buffer，另一个只用把synchronized关键字去掉就是一样的。

1. 初始容量

既然是容器，那么是一定会有个初始容量的，目的在于避免在内存中过度占用内存。容器的初始容量有默认和使用构造函数申明两种。

StringBuffer类可以创建可修改的字符串序列，该类有以下三个改造方法。

StringBuffer()的初始容量可以容纳16个字符，当该对象的体存放的字符的长度大于16时，实体容量就自动增加StringBuffer对象可以通过length()方法获取实体中存放的符序列长度，通过 capacity()方法来获取当前实体的实际量。

StringBuffer(int size)可以指定分配给该对象的实体的初容量参数为参数size指定的字符个数。当该对象的实体存放的符序列的长度大于size个字符时，实体的容量就自动的增加。便存放所增加的字符。
StringBuffer(String s)可以指定给对象的实体的初始容量参数字符串s的长度额外再加16个字符。当该对象的实体存放字符序列长度大于size个字符时，实体的容量自动的增加，以存放所增加的字符。

2. 扩容实现

在父类AbstractStringBuilder中,底层是一个字符数组来保存字符串的
```java
AbstractStringBuilder(int capacity) {
        value = new char[capacity];
    }
```

StringBuffer:
```java
public StringBuffer(String str) {
        super(str.length() + 16);
        append(str);
    }

```
因此扩容其实是，使用append()方法在字符串后面追加值的时候，如果长度超过了该字符串存储空间大小了就就会先进性扩容。**构建新的并且存储空间更大的字符串**，将旧的复制过去。
> 扩容规则：
    先原始容量 * 2 + 2（加2是因为拼接字符串通常末尾都会有个多余的字符）
    如果扩容了之后，容量够用，新的容量就为扩容之后的容量。
    如果扩容了之后，容量不够用，新的容量就是所需要的容量，即原始字符串长度加上新添加的字符串长度。
    扩容完成之后，将原始数组复制到新的容量中，然后将新的字符串添加进去


#### StringBuilder
**StringBuilder**: 也代表可变字符串对象，这点和StringBuffer很相似，但StringBuilder不是线程安全的

### HashMap

1. JDK8以后hashmap的底层数据结构由数据+链表+红黑树实现
2、jdk8以后插入数据采用尾插法。因为引入了树形结构，总是要遍历

**红黑树是一种自平衡的二叉查找树，是一种高效的查找树。**

首先，红黑树是一个二叉搜索树，它在每个节点增加了一个存储位记录节点的颜色，可以是RED,也可以是BLACK；通过任意一条从根到叶子简单路径上颜色的约束，红黑树保证最长路径不超过最短路径的二倍，因而近似平衡（最短路径就是全黑节点，最长路径就是一个红节点一个黑节点，当从根节点到叶子节点的路径上黑色节点相同时，最长路径刚好是最短路径的两倍）。它同时满足以下特性：

1. 节点是红色或黑色
2. 根是黑色
3. 叶子节点（外部节点，空节点）都是黑色，这里的叶子节点指的是最底层的空节点（外部节点），下中的那些null节点才是叶子节点，null节点的父节点在红黑树里不将其看作叶子节点
4. 红色节点的子节点都是黑色，红色节点的父节点都是黑色，从根节点到叶子节点的所有路径上不能有 2 个连续的红色节点
5. 从任一节点到叶子节点的所有路径都包含相同数目的黑色节点

**为什么用红黑树不用平衡二叉树：**平衡二叉追求完全平衡，在实际应用中的空间开销较大，读取方面表现优异，但维护代价大。而红黑树不追求完全平衡，性能更优。

**为什么不选择B树和B+树**：B+树比B树更加矮胖，且由于B+树的非叶子节点上不存储数据，在HashMap中数据率较少的情况下，数据会挤在一个节点中，退化成为链表。且B树和B+树主要用于数据存储在磁盘上的场景，而红黑树用于内存排序。



## 泛型

参数化类型--把一种明确的工作推迟到**实际创建对象**时才确定名曲的数据类型
如：ArrayList<Object> arr = new Arraylist<>()
各种泛型类：List，Set，Map

## JVM
JVM就是java虚拟机，JVM运行时数据区有**方法区**，**Java堆**、**程序计数器**，**本地方法栈**，**虚拟机栈**
[参考博客](https://blog.csdn.net/weixin_43122090/article/details/105093777?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168142794216782425193986%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168142794216782425193986&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-105093777-null-null.142^v83^insert_down1,239^v2^insert_chatgpt&utm_term=jvm&spm=1018.2226.3001.4187)

### 运行时数据区

#### 方法区
方法区是所有线程共享的内存区域，它用于存储已被Java虚拟机加载的**类信息、常量、静态变量时编译器编译后的代码**等数据。
它有个别命叫Non-Heap（非堆）。当方法区无法满足内存分配需求时，抛出OutOfMemoryErro常。

#### Java堆

java堆是java虚拟机所管理的内存中最大的一块，是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是**存放对象实例**。**所有的对象实例以及数组**都要在堆上分配。

java堆是垃圾收集器管理的主要区域，因此也被成为“GC堆”。从内存回收角度来看java堆可分为：新生代和老生代。

无论怎么划分，都与存放内容无关，无论哪个区域，存储的都是对象实例，进一步的划分都是为了更好的回收内存，或者更快的分配内存。
根据Java虚拟机规范的规定，**java堆可以处于物理上不连续的内存空间中**。当前主流的虚拟机都是可扩展的（通过 -Xmx 和 -Xms 控制）。如果堆中没有内存可以完成实例分配，并且**堆也无法再扩展时，将会抛出OutOfMemoryError异常。**

#### 程序计数器

程序计数器是一块较小的内存空间，它可以看作是：保存当前线程所正在执行的字节码指令的地址(行号)

由于Java虚拟机的多线程是通过**线程轮流切换**并分配处理器执行时间的方式来实现的，一个处理器都只会执行一条线程中的指令。因此，为了线程切换后能恢复到正确的执行位置，每条线程都有一个独立的程序计数器，各个线程之间计数器互不影响，独立存储。称之为“线程私有”的内存。**程序计数器内存区域是虚拟机中唯一没有规定OutOfMemoryError情况的区域。**

可以把它看作线程计数器，由于线程时最小的执行单元，不具备记忆功能，所以当线程A被线程B打断后它会挂起，然后等到线程B执行完成后，从程序计数器中恢复A之前的状态。

#### Java虚拟机栈（Java Virtual Machine Stacks）
java虚拟机是线程私有的，它的生命周期和线程相同。虚拟机栈描述的是Java方法执行的内存模型：**每个方法在执行的同时都会创建一个栈帧**（StaFrame）用于存储**局部变量表、操作数栈、动态链接、方法出口**等信息。

**局部变量表**：是用来存储我们临时8个基本数据类型、对象引用地址、returnAddress类型。（returnAddress中保存的是return后要执行的字节码的指令地址。）

**操作数栈**：操作数栈就是用来操作的，例如代码中有个 i = 6*6，他在一开始的时候就会进行操作，读取我们的代码，进行计算后再放入局部变量表中去

**动态链接**：假如我方法中，有个 service.add()方法，要链接到别的方法中去，这就是动态链接，存储链接的地方

**出口**：出口正常的话就是return 不正常的话就是抛出异常落

**一个方法调用另一个方法，会创建很多栈帧吗？**
答：会创建。如果一个栈中有动态链接调用别的方法，就会去创建新的栈帧，栈中是由顺序的，一个栈帧调用另一个栈帧，另一个栈帧就会排在调用者下面

**本地方法栈（Native Method Stack）**

本地方法栈很好理解，他很栈很像，只不过方法上带了 native 关键字的栈字
它是虚拟机栈为虚拟机执行Java方法（也就是字节码）的服务
native关键字的方法是看不到的，必须要去oracle官网去下载才可以看的到，而且native关键字饰的大部分源码都是C和C++的代码。

### Java内存结构

![](img/posts/interview/jvm2.jpg)

**直接内存与堆内存的区别**：
直接内存申请空间耗费很高的性能，**堆内存申请空间耗费比较低**
**直接内存的IO读写的性能要优于堆内存**，在多次读写操作的情况相差非常明显

虚拟机核心的组件就是执行引擎，它负责执行虚拟机的字节码，一般户先进行编译成机器码后执行。

“虚拟机”是一个相对于“物理机”的概念，**虚拟机的字节码是不能直接在物理机上运行的，需要JVM字节码执行引擎编译成机器码后才可在物理机上执行**。

### JVM垃圾回收机制

GC主要用于Java**堆**的管理。Java 中的堆是 JVM 所管理的最大的一块内存空间，主要用于存放各种类的实例对象。

> 什么是内存垃圾？

程序在运行过程中，会产生大量的内存垃圾（**一些没有引用指向的内存对象**都属于内存垃圾，因为这些对象已经无法访问，程序用不了它们了，对程序而言它们已经死亡），为了确保程序运行时的性能，java虚拟机在程序运行的过程中不断地进行自动的垃圾回收（GC）


> 怎么清理内存垃圾？
GC是不定时去堆内存中清理不可达对象。不可达的对象并不会马上就会直接回收， 垃圾收集器在一个Java程序中的执行是自动的，不能强制执行清楚那个对象，即使程序员能明确地判断出有一块内存已经无用了，是应该回收的，程序员也不能强制垃圾收集器回收该内存块。程序员唯一能做的就是通过调用System.gc 方法来"建议"执行垃圾收集器，但是他是否执行，什么时候执行却都是不可知的。这也是垃圾收集器的最主要的缺点。当然相对于它给程序员带来的巨大方便性而言，这个缺点是瑕不掩瑜的。

```java
package com.lijie;

public class Test {
	public static void main(String[] args) {
		Test test = new Test();
		test = null;
		System.gc(); // 手动回收垃圾
	}

	@Override
	protected void finalize() throws Throwable {
		// gc回收垃圾之前调用
		System.out.println("gc回收垃圾之前调用的方法");
	}
}

```

### JVM参数设置

```java
#常用的设置
-Xms：初始堆大小，JVM 启动的时候，给定堆空间大小。 

-Xmx：最大堆大小，JVM 运行过程中，如果初始堆空间不足的时候，最大可以扩展到多少。 

-Xmn：设置堆中年轻代大小。整个堆大小=年轻代大小+年老代大小+持久代大小。 

-XX:NewSize=n 设置年轻代初始化大小大小 

-XX:MaxNewSize=n 设置年轻代最大值

-XX:NewRatio=n 设置年轻代和年老代的比值。如: -XX:NewRatio=3，表示年轻代与年老代比值为 1：3，年轻代占整个年轻代+年老代和的 1/4 

-XX:SurvivorRatio=n 年轻代中 Eden 区与两个 Survivor 区的比值。注意 Survivor 区有两个。8表示两个Survivor :eden=2:8 ,即一个Survivor占年轻代的1/10，默认就为8

-Xss：设置每个线程的堆栈大小。JDK5后每个线程 Java 栈大小为 1M，以前每个线程堆栈大小为 256K。

-XX:ThreadStackSize=n 线程堆栈大小

-XX:PermSize=n 设置持久代初始值	

-XX:MaxPermSize=n 设置持久代大小
 
-XX:MaxTenuringThreshold=n 设置年轻带垃圾对象最大年龄。如果设置为 0 的话，则年轻代对象不经过 Survivor 区，直接进入年老代。

#下面是一些不常用的

-XX:LargePageSizeInBytes=n 设置堆内存的内存页大小

-XX:+UseFastAccessorMethods 优化原始类型的getter方法性能

-XX:+DisableExplicitGC 禁止在运行期显式地调用System.gc()，默认启用	

-XX:+AggressiveOpts 是否启用JVM开发团队最新的调优成果。例如编译优化，偏向锁，并行年老代收集等，jdk6纸之后默认启动

-XX:+UseBiasedLocking 是否启用偏向锁，JDK6默认启用	

-Xnoclassgc 是否禁用垃圾回收

-XX:+UseThreadPriorities 使用本地线程的优先级，默认启用	

等等等......

```
### JVM调优总结

1. 在实际工作中，我们可以直接将初始的堆大小与最大堆大小相等，这样的好处是可以减少程序运行时垃圾回收次数，从而提高效率。
2. 初始堆值和最大堆内存内存越大，吞吐量就越高，但是也要根据自己电脑(服务器)的实际内存来比较。
3. 最好使用并行收集器,因为并行收集器速度比串行吞吐量高，速度快。当然，服务器一定要是多线程的
4. 设置堆内存新生代的比例和老年代的比例最好为1:2或者1:3。默认的就是1:2
5.减少GC对老年代的回收。设置生代带垃圾对象最大年龄，进量不要有大量连续内存空间的java对象，因为会直接到老年代，内存不够就会执行GC

## java的四种引用类型

强引用: 在程序内存不足(OOM)的时候也不会被回收 -> 是创建一个对象，并把这个对象赋给一个引用遍历
软引用：只有在OOM的时候，要迫切释放空间的时候，JVM才会释放软引用的对象 -> 软引用可实现内存敏感的告诉缓存，比如网页缓存与图片缓存，使用软引用能防止内存泄漏，增强程序的鲁棒性
弱引用：JVM发现了弱引用就会回收。
虚引用：和弱引用差不多，但在被回收前胡放入referenceQueue中

## 深浅拷贝

**深拷贝**: 深拷贝是一个独立的对象拷贝，拷贝所有的属性的同时，对其他对象的引用仍然指向原来的对象

**浅拷贝**: 被复制的对象的所有变量都与原来对象的值相同，但所有对其它对象的引用仍然指向原来的对象

## 重载和重写
**重写**：子类重写父类的方法
**重载**：同名方法，不同的参数类型、参数数量、参数顺序

## 关键字

### final
final修饰的类不可以被继承，但可以继承其他类
final修改的方法不可以被覆盖，但可以被继承
final修饰的基本数据类型是常量

### static

static方法：静态方法不能访问类的非静态成员变量和非静态成员方法
static变量：静态变量被所有的对象所共享，在内存中只有一个父辈，在创建对象时被初始化

### volatile
[示例](https://blog.csdn.net/didi1663478999/article/details/98523122)
在多线程中，volatile和synchronized都起到非常重要的作用，synchronized是通过加锁来实现线程的安全性。而volatile的主要作用是在多处理器开发中保证共享变量对于多线程的可见性。
可见性的意思是，**当一个线程修改一个共享变量时，另外一个线程能读取到修改以后的值**
volatile时乐观锁（认为自己使用数据时不会有别的线程修改数据，因此不会添加锁），适合读取操作频繁的场景


### synchronized

synchronied主要用于解决多线程的资源同步性，对于普通同步方法--锁是当前实例对象，对于静态同步方法，锁是当前类的class对象。对于同步方法块，所是synchronized括号里配置的对象。
synchronized是所著当前变量，只有当前线程可以访问该变量，其他线程会block
voliate仅仅能在变量级别使用，而synchronzed可以在变量、方法和类级别使用
volatile进能实现变量的修改可见性，不能保证原子性，而synchronized可以保证变量的修改可见性和原子性
synchronized和lock都是悲观锁，认为在自己写入数据时会有别的线程修改数据，适合写入操作频繁的场景

synchronized 在字节码层被映射成两个指令：monitorenter 和 monitorexit，当一个线程遇到 monitorenter 指令时，会尝试去获取锁，如果获取成功，锁的数量 +1，（因为synchronized是一个可重入锁，需要使用锁计数来判断锁的情况），如果没有获取到锁，就会阻塞；当线程遇到 monitorexit 指令时，锁计数 -1，当计数器为 0 时，线程释放锁；如果线程遇到异常，也会释放锁。

```java
public class APP {
    void test() {
        synchronized (this) {
            System.out.println("hello world");
        }
    }
}

```

## 创建线程池

### 线程与进程

线程是操作系统能够进行运算调度的最小单位，它被包含在进程之中，是进程中的实际运作单位。
线程是进程的子集，一个进程可以有很多线程，每条线程并行执行不同的任务。不同的进程使用不同的内存空间，而所有的线程共享一片相同的内存空间。别把它和栈内存搞混，每个线程都拥有单独的栈内存用来存储本地数据。

### 如何在Java中实现线程？
在语言层面有两种方式。java.lang.Thread 类的实例就是一个线程但是它需要调用java.lang.Runnable接口来执行，由于线程类本身就是调用的Runnable接口所以你可以继承java.lang.Thread 类或者直接调用Runnable接口来重写run()方法实现线程
调用runnable接口更好

### Thread 类中的start() 和 run() 方法有什么区别？

这个问题经常被问到，但还是能从此区分出面试者对Java线程模型的理解程度。start()方法被用来启动新创建的线程，而且start()内部调用了run()方法，这和直接调用run()方法的效果不一样。当你调用run()方法的时候，只会是在原来的线程中调用，没有新的线程启动，start()方法才会启动新线程

### 创建线程池

#### ThreadPoolExecutor

**原理**：  提交一个任务到线程池中，线程池的处理流程如下：

1. 判断线程池里的**核心线程是否都在执行任务**，如果不是（核心线程空闲或者还有核心线程没有被建）则创建一个新的工作线程来执行任务。如果核心线程都在执行任务，则进入下个流程。
2. 线程池判断**工作队列是否已满**，如果工作队列没有满，则将新提交的任务存储在这个工作队列里。果工作队列满了，则进入下个流程。
3. 判断线程池里的线程**是否都处于工作状态**，如果没有，则创建一个新的工作线程来执行任务。如果经满了，则交给饱和策略来处理这个任务。
采用ThreadPoolExecutor创建的优点：
可以实时获取线程池内线程的各种状态
可以动态调整线程池大小

ThreadPoolExecutor参数：

corePoolSize 核心线程数大小，当线程数 < corePoolSize ，会创建线程执行 runnable
maximumPoolSize 最大线程数， 当线程数 >= corePoolSize的时候，会把 runnable 放入workQueue中
keepAliveTime 保持存活时间，当线程数大于corePoolSize的空闲线程能保持的最大时间。
unit 时间单位
workQueue 保存任务的阻塞队列
threadFactory 创建线程的工厂

```java
package com.example.demo.pool;
 
import java.io.Serializable;
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
 
public class ThreadPool {
 
    public static void main(String[] args) {
 
        long currentTimeMillis = System.currentTimeMillis();
 
        // 构造一个线程池
        ThreadPoolExecutor threadPool = new ThreadPoolExecutor(
                corePoolSize = 5,
                maximumPoolSize = 6,
                keepAliveTime = 3,
                TimeUnit.SECONDS, new ArrayBlockingQueue<Runnable>(3)
        );
 
        for (int i = 1; i <= 10; i++) {
            try {
                String task = "task=" + i;
                System.out.println("创建任务并提交到线程池中：" + task);
                threadPool.execute(new ThreadPoolTask(task));
                Thread.sleep(100);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
 
        try {
            //等待所有线程执行完毕当前任务。
            threadPool.shutdown();
            boolean loop = true;
            do {
                //等待所有线程执行完毕当前任务结束，等待2秒
                loop = !threadPool.awaitTermination(2, TimeUnit.SECONDS);
            } while (loop);
            if (!loop) {
                System.out.println("所有线程执行完毕");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            System.out.println("耗时：" + (System.currentTimeMillis() - currentTimeMillis));
        }
 
    }
 
}
 
 
class ThreadPoolTask implements Runnable, Serializable {
 
    private Object attachData;
    ThreadPoolTask(Object tasks) {
        this.attachData = tasks;
    }
    @Override
    public void run() {
        try {
            System.out.println("开始执行任务：" + attachData + "任务，使用的线程池，线程名称：" + Thread.currentThread().getName());
        } catch (Exception e) {
            e.printStackTrace();
        }
        attachData = null;
    }
}
 
 
```