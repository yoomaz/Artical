# Android

## Java 语言

#### Collection 体系

http://blog.csdn.net/haovip123/article/details/45423683



#### LRUCache原理

内部使用 LinkedHashMap 实现，集成 HashMap，HashMap 是单向的链表，LinkedHashMap 是双向的列表。
用这个类有两大好处：
一是它本身已经实现了按照访问顺序的存储，也就是说，最近读取的会放在最前面，最最不常读取的会放在最后（当然，它也可以实现按照插入顺序存储）。
第二，LinkedHashMap 本身有一个方法用于判断是否需要移除最不常读取的数

#### java 1.8 的新特性

lamda表达式

接口可以有方法的实现



#### 不同代码块执行的顺序

先父类，后子类
先静态代码块，再代码块，再构造函数

http://bbs.csdn.net/topics/330127012



### 并发

#### synchronized 关键字

是一种同步锁

1. 锁类

2. 锁方法

3. 锁对象

#### volatile 关键字 

使用 volatile 标记的关键字，每次使用的时候都从内存中获取最新的，保证并发过程中变量准确性

可能是因为cpu缓存的原因导致获取不到最新的



### 设计模式



### 内存相关

#### 四大引用

强，软<SoftReference>，弱<WeakReference>，虚<PhantomReference>，并说明下合适GC
(http://www.th7.cn/Program/java/201602/767785.shtml)
强：任何时候都不会回收, 例如 new 出来的对象
软：只有内存不够的时候才会回收
弱：垃圾回收器线程扫描到的时候回收，但是垃圾回收器线程是优先级比较低的线程，不会很及时的扫描到
虚：任何时候都会回收。虚引用主要用来跟踪对象被垃圾回收的活动。

虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列(ReferenceQueue)联合使用。当垃 圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。程序可以通过判断引用队列中是 否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。

#### JVM内存模型

分为线程私有区和共享数据区
线程私有区：分为程序计数器，虚拟机栈，本地方法栈
程序计数器记录线程正在执行的字节码地址，虚拟机栈是方法执行的内存区，每个执行一个方法就会创建一个栈帧，这个地方会报 StackOverFlow 错误，本地方法栈是执行native方法的内存区
共享数据区：堆，方法区（包含一个常量池）
堆是存储对象的内存区，方法区里存放类信息，编译后的代码，一些常量，静态变量

#### JVM内存回收机制

标记回收，复制，标记压缩，分代

1. 标记回收：扫描整个内存，回收可以回收的对象，效率高，但是会产生碎片
2. 复制：将内存分为两块，扫描一块内存，把不会被gc调的对象放到另一块内存中，然后调换两块内存的角色，结束之后清理掉正在使用的这块内存
3. 标记压缩：扫描整个内存，把不会被会回收的对象做一个标记，然后把这些对象压缩到内存的一端，清理掉边界外的内存，这种做法性价比高，但是比较耗时
4. 分代：把内存分成年轻代和老年代，新分配的对象都在年轻代，当经历多次gc后，如果这个对象还存在，就把它移动到老年代。对年轻代使用复制方式，对老年代使用标记压缩方式

#### 内存优化如何做？

1. bitmap 压缩和使用 WeakRefrence
2. 复用对象，减少创建对象，避免在循环里面创建对象
3. 在Map的 Key 值是 int，long boolean 类型的时候，使用SparseArray代替HashMap

#### 内存泄漏

1. 避免使用非静态 Handler 实例，如果消息没处理完，在MessageQueue 中，Message 持有 Handler，Handler 因为是非静态的实例，内部持有外部类的引用，所以外部类也没办法释放
2. 单例，如果构造方法传入 Context ，因为对象是静态的，所有这个Context对应的对象也没办法释放



## Android

#### 统计启动时长

#### Android 内存泄漏



### 性能优化

#### Android怎么加速启动Activity

1. 第一次打开应用的时候：Application 的 onCreate 方法内不要执行耗时操作，一些初始化可以异步执行
2. activity 的 onPause 方法里面不要执行耗时操作，因为执行完 onPause 才会执行下一个 activity 的 onResume 方法

#### Android中弱引用与软引用的应用场景

1. 软引用：只有系统报 oom 前最后一次 GC 操作时才会回收这块内存，但是GC线程优先级是很低的，所以能保留很长时间，在处理位图的时候或者其他需要大内存的对象的时候使用，防止报 oom 错误。
2. 弱引用：只有被 gc 线程扫描器扫描到了就会被回收。如果有个静态类需要 Context ，可以把这个 context 用若引用，防止内存泄漏

#### 图片加载原理

主要是三级缓存
加载的时候首先看内存有没有，如果没有就去sd卡里找，如果还没有，再请求网络



### 动画

#### 动画的分类

1.  帧动画
2.  补间动画
3.  属性动画



### 四大组件

#### 为什么在Service中创建子线程而不是Activity中

#### Activity 缓存方法：onSaveInstanceState()

1. 只有因为系统的原因导致activity销毁才会调用
2. onSaveInstanceState()如果被调用，这个方法会在onStop()前被触发，但系统并不保证是否在onPause()之前或者之后触发。

#### Service的两种启动方法，有什么区别

1. startService 侧重于计算，bindService 用于其他组件和服务的交互
2. startSercice 启动的服务的生命周期 和 启动他的context生命周期没关系，bindService 启动的服务和启动他的context生命周期共生死

#### IntentService的使用场景与特点

特点：
1. 异步执行：内部使用了 HandlerThread  + Lopper来执行任务，所以是顺序执行的
2. 执行完以后会关闭自己：执行完以后会调用 stopSelf（int id）关闭自己，stopSelf（）会立刻关闭，而 stopSelf（int id） 会在消息处理完以后再关闭

使用场景：
执行后台耗时操作

#### 几种Context区别

1. Activity 是继承 ContextThemeWraper ，Service 和 Application 继承 ContextWrapper
2. 在Dialog中，不能使用 Application的Context
3. Activity 和 Service 中可以调用 getApplication ，BroadCastRceive 用 getApplicationContext
4. 他们在初始化的时候都会实例化一个新的 ContextImpl
5. Context的数量等于Activity的个数 + Service的个数 + 1，这个1为Application

### 并发

#### Android 中的 IPC 机制

跨进程调用的方式:
1.  Intent 或者 Bunddle
2.  文件，SharedPrefrence
3.  Messager
4.  Aidl
5.  ContentProvider

#### Android 消息机制

Android 的消息机制主要就是 Handler 的运行机制和 MessageQueue 和 Looper 的工作过程
1. 概述：
  Handler 调用 post 或者 send 方法时候，会把消息 Message 放入 MessageQueue中，Looper 一旦发现有消息来了，就会把消息取出来，调用 Message 的 callBack（post一个Runable类型的消息）方法或者调用 Hnadler的 callBack 处理消息（创建Handler的时候参数是一个Callback） 方法 或者调用 handleMessage 方法

2. 创建 Handler 的时候，会从本地的 ThreadLocal 里拿到 Looper ，所以的线程必须要有Looper，否则会报错

3. Looper 的 prepare 方法，主线有一个单独的 prepareMainLooper ，会为当前线程创建一个Looper，Looper内部持有一个 MessageQueue，也是这个时候创建的，创建完成后会保存在 ThreadLocal 内

4. ThreadLocal 是一个只存储指定线程数据的对象，也只有在当前指定线程才能获取到存储的数据，Looper就是存储在这个对象里

问题：
1. 如何保证一个线程只有一个Looper？
  创建的 Looper 保存在 ThreadLocal中，参考上面3，4
2. 主线程中的Looper.loop()一直无限循环为什么不会造成ANR？
  (https://www.zhihu.com/question/34652589),(http://blog.csdn.net/ksjay_1943/article/details/54893773)
  虽然是一直循环，但是没有消息的时候是挂起的状态，这个时候并不会消耗cpu的资源，当有消息来得时候，会发送一个信号打断这个种状态。
  造成 ANR 主要是因为在主线程消息处理时间过长

#### 多线程的方式有哪些？

1. new Thread()
2. AsyncTask
3. Handler
4. IntentService
5. ThreadPoolExecutor

### 自定义View

#### 事件处理

分发，拦截，处理。只是View没有拦截处理

#### 自定义View和ViewGroup

说一下自己定义的组件就行

#### 动画

View动画，属性动画，帧动画。再说下View和属性动画区别。
参考：https://developer.android.com/guide/topics/graphics/overview.html

### 异常处理

#### ANR相关

ANR发生条件：1) 5s内没有响应用户输入事件，2) 10s内广播接收器没有处理完毕，3) 20s内服务没有处理完毕

如何查找并分析？1.通过系统 log，可以看到reason， 2.通过 trace.txt,也会记录下来

### 操作系统

#### Dalvik和Art区别

(Just In Time和Ahead Of Time) 字节码 和 机器码
dalvik是执行的时候编译+运行，安装比较快，开启应用比较慢，应用占用空间小
ART是安装的时候就编译好了，执行的时候直接就可以运行的，安装慢，开启应用快，占用空间大.
用个比喻来说就是，骑自行车
dalvik 是已经折叠起来的自行车，每次骑都要先组装自行车才能骑
ART 是已经组装好的自行车，每次骑直接上车就能走人

#### Android GC

已经保存在印象笔记里

1. 垃圾回收算法：
  标记清除(Swipe and Sweep)，复制(Copy)，标记压缩(Swipe and Compact)，分代

2. GC 的类型：
    1. gc for allocate: 没有足够内存分配对象
    2. gc for concurrent：因为内存将要满了
    3. gc for explicit ：用户主动 gc
    4. gc before oom ：报oom之前最后一个gc

3. 内存的一些参数：
  -Xms：最小分配内存
  -Xmx：不受控情况下的最大堆内存大小，起始就是我们在用largeheap属性的时候，可以从系统获取的最大堆大小
  -XX:HeapGrowthLimit：增长上限值，超过就会 oom

4. GC 的过程
  例如当前需要给一个对象分配内存
      1. 查看当前内存够不够，如果不够执行一次 gc for allocate ，这个阶段不回收软引用
      2. 如果还不够，增长内存到增长最大值，再进行分配对象
      3. 如果还不够，执行 gc before oom ，再增长内存到增长最大值，这个阶段回收软引用
      4. 如果还不够，就会 oom

5. Dalvik 和 ART 在GC的不同
    1. 内存区域分配不同：dalvik分为 active 堆 和 zygote 堆，art分为 Image Space（系统类的使用内存区），Zygote Space， Allocation Space，Big Obeject Space，art 分的更细，而且有大对象区域，更利于gc
    2. ART 会在不同的阶段使用不同的gc算法：dalvik一直使用标记清理，而art不同：在前台的时候，因为gc速度很重要，使用标记清理算法，在后台的时候，会使用并发gc，并且会使用标记压缩算法，减少内存碎片
    3. ART 的gc有三个特点: 标记自身（会将新的对象压入一个Allocation Stack中，减少对象遍历范围），预读取（会预先读取Allocation Stack中对象引用的对象），减少 Suspend 时间(在标记的时候就会进行一些清理操作)



#### 看过那些源码？

#### Android ClassLoder机制

http://www.jianshu.com/p/a620e368389a
运行的时候需要把类加载到内存中才能使用，所以需要ClassLoder。
DexClassLoader支持加载APK、DEX和JAR，也可以从SD卡进行加载
PathClassLoader是用来加载Android系统类和应用的类，并且不建议开发者使用

加载的时候需要先对每一个 dex 文件 loadClass 操作，loadClass 过程中会进行 findClass



### 第三方项目

#### OkHttp原理？



#### Retrofit原理？为何用代理？代理的作用是什么？



#### ButterKnife原理？用到反射吗？为什么？













## 计算机网络

### 基于Http追加的协议

#### SPDY

SPDY在应用层和传输层之间增加了SPDY会话层，为了安全还使用了SSL

1. 多路复用流：一个tcp连接，可以无限制处理多个http请求
2. 压缩http首部
3. 赋予请求优先级

### Http/Https

#### http协议的优点



#### http协议的缺点

分 安全性 和 性能 两点来说明

安全性方面

1. 使用明文通信，通信内容可能被窃听
2. 不验证通信双方的身份，无法确认服务器或者客户端是不是伪装的
3. 不能保证报文的完整性，内容可能被篡改

性能方面

1. 一个链接只能发送一个请求
2. 请求只能从客户端开始
3. 每次请求可能都发送相同的首部
4. 首部和报文内容可能没有经过压缩

#### Http和Https的区别？

1. https 是http 直接和 ssl 层 ，比 http 多了一个ssl层，负责与tcp层通信
2. Https是ssl加密传输，Http是明文传输
3. Https是使用端口443，而Http使用80
4. Https协议需要到CA申请证书，这样客户端可以验证服务器身份

#### https 如何验证证书的合法性



#### https中哪里用了对称加密，哪里用了非对称加密



### Tcp/Ip

#### Tcp设计的好处



####  TCP的三次握手？两次行不行？为什么？

http://blog.csdn.net/cmm0401/article/details/68483115



#### 为什么建立连接是三次握手，而关闭连接却是四次挥手呢？

https://www.kancloud.cn/kancloud/android-tutorial/87237
因为服务端在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里 发送给客户端。而关闭连接时，当收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还 能接收数据，己方也未必全部数据都发送给对方了，所以己方可以立即close，也可以发送一些 数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会 分开发送。



#### TCP攻击知道吗？如何进行攻击？



## 安全

### 加解密

#### 加密算法有哪些？对称加密和非对称加密的区别？

MD5，SHA1，Base64，RSA，AES，DES
对称：使用相同密钥，需要在网络传输，安全性不高。（AES，DES）
非对称：使用一对密钥，公钥和私钥，私钥不在网络传输，因此安全性高（RSA）

RSA：http://blog.csdn.net/lemon_tree12138/article/details/50696926



## 数据结构

#### 哈希表会产生冲突吗，如果解决冲突

会产生冲突：哈希函数不同的key，经过散列函数后得到相同的位置
解决冲突：1.再哈希法；2.链地址法；3.公共溢出区；4.开放地址法

https://www.cnblogs.com/jillzhang/archive/2006/11/03/548671.html



## 算法

#### 猫扑素数

#### 1到n，求1的个数

#### 单词反转



## 其他

### 工作习惯

#### 项目中遇到的让你棘手的问题？多久解决，怎么解决？(考的是你发现问题和解决问题的能力)

##### RecycleView 和 EditText 冲突相关

https://blog.csdn.net/longforus/article/details/70231634
https://www.jianshu.com/p/e97c44123630

##### RecycleView4种定位滚动方式演示

https://www.jianshu.com/p/3acc395ae933

##### 自定义EditText，自动加入空格或者其他字符

http://www.jcodecraeer.com/plus/view.php?aid=3163

##### textview 文本空格和分段(自定义View，电话号码或者银行卡)



#### 会对代码进行review吗？何时review？怎么review？



#### 如何学习?何时学习？怎么学习？学习渠道?（考的是学习能力）











#### 