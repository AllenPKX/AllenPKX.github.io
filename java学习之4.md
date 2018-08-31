# 1.4 java对象模型
### Java对象模型描述了java对象在JVM中的具体表现形式，在HotSpot（一种Java虚拟机）中使用了oop-klass 模型来实现了java对象在JVM中的存储。

### 一、oop
#### Java创建出来的对象存于堆中，而这个对象具体分为三部分：

#### _mark：存储对象的一些标记（如是否加锁，加锁次数）
#### meta_data：存储指向元数据的指针
#### field：对象本身携带的数据
#### 其中_mark和meta_data被称为对象头。

#### 关于锁标记： 
#### 通过同步方法加锁的对象会在对象上添加ACC_SYNCHRONIZED标记，每个线程获得对象的锁时，会在锁计数器上加1，同一线程可对对象重复加锁，释放锁时，锁计数器-1，当锁计数器为0时，其他线程才可执行这个同步方法； 
#### 另外还可通过同步代码块为对象加锁，在同步代码块中，进入同步代码块前会执行monitorenter指令，使锁计数器加1，出代码块后执行monitorexit指令，使锁计数器-1，计数器为0时其他线程可进入该代码块。

### 二、klass
#### klass存于方法区中，代表着类的基本信息，当类A加载时JVM会在方法区创建一个对应的AKlass对象，当a对象被创建对象时在堆中创建aOopInstance对象，oop的对象头中的meta_data就存储着指向aKlass的指针。 
#### 我们知道一个Java类对应有一个Class对象，创建这个对象(如Person.class)同样会在堆中创建一个被称为PersonKlassOop的对象，他的meta_data中的指针会指向方法区中的klassKlass对象。如图所示：
![](https://pan.baidu.com/s/1gFe0S7xrkVsHu72QP-ftmQ)

#### 从图中可以看出，所有的对象都有指针指向代表他们类的基本信息的Klass对象，所有的Class对象的指针都指向同一个Klass对象即KlassKlass。 