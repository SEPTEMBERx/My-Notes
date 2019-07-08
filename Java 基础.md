## Java 基础

### 基本数据类型：

![img](https://pic2.zhimg.com/80/v2-c43cf4e49414eedeeeeaee8921fd1e85_hd.jpg)

### 包装类型：

包装类型是对基本数据类型不足之处的补充。

基本数据类型的传递方式是值传递，

而包装类型是引用传递，同时提供了很多数据类型间转换的方法。

Java1.5 以后可以自动装箱和拆箱。

### 集合

![img](https://pic4.zhimg.com/80/v2-349e87506add2a7eb0ab07df20d8700f_hd.jpg)



List：有序、可重复。可以通过索引快速查找，但进行增删操作时后续的数据需要移动，所以增删速度慢。

Set：无序、不可重复。

Map：键值对、键唯一、值不唯一。Map 集合中存储的是键值对，键不能重复，值可以重复。根据键得到值，对 map 集合遍历时先得到键的 set 集合，对 set 集合进行遍历，得到相应的值。

### 多线程

![img](https://pic4.zhimg.com/80/v2-2193aebf710bfa63beb6fb0e258c86fb_hd.jpg)

新建状态: 一个新产生的线程从新状态开始了它的生命周期。它保持这个状态直到程序 start 这个线程。

运行状态:当一个新状态的线程被 start 以后，线程就变成可运行状态，一个线程在此状态下被认为是开始执行其任务

就绪状态:当一个线程等待另外一个线程执行一个任务的时候，该线程就进入就绪状态。当另一个线程给就绪状态的线程发送信号时，该线程才重新切换到运行状态。

休眠状态: 由于一个线程的时间片用完了，该线程从运行状态进入休眠状态。当时间间隔到期或者等待的时间发生了，该状态的线程切换到运行状态。

终止状态: 一个运行状态的线程完成任务或者其他终止条件发生，该线程就切换到终止状态。

------

# Java程序执行过程

- .java ——编译——> .class  （编译期）

- 类加载器(Classloader)负责加载各个字节码文件（.class）  （运行期）

- 加载完.class后，由执行引擎执行，在执行过程中，需要运行时数据区提供数据  （运行期）

  ![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20170817162053998?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDQyOTQyNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![img](http://dl.iteye.com/upload/attachment/0084/2313/9df89177-114a-343a-bfe0-672739b33ed6.png)

![img](https://images2015.cnblogs.com/blog/866881/201602/866881-20160217143147706-1305825333.jpg)

- 整个白框部分表示JVM管理的内存
- 包含栈帧的地方称作JVM方法栈，一个栈帧就是一个方法
- 在Hotspot JVM中，JVM方法栈和本地方法栈是同一个
- java方法是通过出栈操作来执行的（在类加载后入栈），所以执行引擎直接操作的是栈帧（即一个方法）

**代码编译**

使用命令javac Main.java编译，编译后能看到一个.class文件，里面长这样

cafe babe：前四个字节称为魔数，作用是标识不同版本的虚拟机
0000 0034：是java的版本号，0000是次版本号，0034是主版本号

0x0034 = 3 * 16 + 4 = 52 表示JDK1。8版本

0013：是常量池入口，0x0013 = 19，表示常量池有18项常量（1~18，第0项空出来）

```
cafe babe 0000 0034 001d 0a00 0600 0f09
0010 0011 0800 120a 0013 0014 0700 1507
0016 0100 063c 696e 6974 3e01 0003 2829
5601 0004 436f 6465 0100 0f4c 696e 654e
756d 6265 7254 6162 6c65 0100 046d 6169
6e01 0016 285b 4c6a 6176 612f 6c61 6e67
2f53 7472 696e 673b 2956 0100 0a53 6f75
7263 6546 696c 6501 0009 4d61 696e 2e6a
...
```

**类加载**

主要是指将*.class文件加载到JVM，并形成Class对象的机制，之后就可以对Class对象实例化并调用了。

特点：

- 类加载机制可以在运行时动态加载外部类

- 类加载的过程

- 类加载的双亲委托机制

- 类加载器的层次关系及源码

  

  ### 类加载器工作机制：
  
  1.装载：将Java二进制代码导入jvm中，生成Class文件。 
  2.连接：a）校验：检查载入Class文件数据的正确性 b）准备：给类的静态变量分配存储空间 c）解析：将符号引用转成直接引用 
3：初始化：对类的静态变量，静态方法和静态代码块执行初始化工作。
  

双亲委派模型：类加载器收到类加载请求，首先将请求委派给父类加载器完成 

用户自定义加载器->应用程序加载器->扩展类加载器->启动类加载器。

  

**执行代码**

两种执行方式：

- 解释执行（运行期解释字节码并执行）

  - 速度慢，效率低
  - 但是要比编译为机器码执行省内存

- 编译为机器码执行（将字节码编译为机器码并执行，这个编译过程发生在运行期，称为

  JIT

  编译），下面是两种模式

  - client（即C1）：只做少量性能开销比高的优化，占用内存少，适用于桌面程序。
  - server（即C2）：进行了大量优化，占用内存多，适用于服务端程序。会收集大量的运行时信息。

![img](http://dl.iteye.com/upload/attachment/0084/2346/aa51b456-7476-3d3b-a125-8b00eb19098c.png)



### 什么是 GC？为什么要有 GC？

GC（Garbage Collection）是垃圾收集的意思，负责清除对象并释放内存。Java 提供的 GC 功能可以自动检测对象是否超过作用域从而达到自动回收内存的目的，从而防止内存泄漏。

**GC的主要任务：**

1.分配内存；
2.确保被引用对象的内存不被错误的回收；
3.回收不再被引用的对象的内存空间

**垃圾回收机制的主要解决问题**

---

1.哪些内存需要回收？

**[1]引用计数算法 :**  Reference Counting

每当一个地方引用它时，计数器+1；引用失效时，计数器-1；计数值=0——不可能再被	引用  引用计数算法很难解决**对象之间相互矛盾循环引用**的问题

**[2]可达性分析算法：**Reachability Analysis  **JVM采用**

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20170817171138449?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDQyOTQyNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

把一系列“GC Roots”作为起始点，从节点向下搜索，路径称为引用链，当一个对象到GC 	Roots没有任何引用链相连，即不可达时，则证明此对象时不可用的

在Java中可作为GCRoots的对象：
1).虚拟机栈（栈帧中的本地变量表）中引用的对象；
2).方法区中类静态属性引用的对象；
3).方法区中常量引用的对象；
4).本地方法栈中JNI引用的对象

---

2.什么时候回收？

即使是被判断不可达的对象，也要再进行筛选

当对象没有覆盖finalize()方法，或者finalize方法已经被虚拟机调用过，则没有必要执行

finalize()是Object的protected方法，子类可以覆盖该方法以实现资源清理工作，GC在回收对象之前调用该方法。

如果有必要执行——放置在F-Queue的队列中——Finalizer线程执行

---

3.如何回收？

垃圾收集算法：

- 标记—清除算法	Mark-Sweep    **老生代**

用在老生代中， 遍历所有的GC Root，分别标记处可达的对象和不可达的对象，然后清除

两个阶段：标记，清除；

不足：效率问题；空间问题(会产生大量不连续的内存碎片)

- 复制算法	Copying   **新生代**

用在新生代中，它将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活的对象复制到另外一块上，然后再把已使用过的内存空间一次清理掉.
**不足：**将内存缩小为了原来的一半

- 标记—整理算法 Mark-Compact

  标记阶段和Mark-Sweep一样，但是在完成标记之后，它不是直接清理可回收对象，而是将存活对象都向一端移动，然后清理掉端边界以外的内存

---

Java内存（堆内存）的分配与回收由JVM垃圾收集器自动完成

JVM内存结构

![img](https://01.imgmini.eastday.com/mobile/20180323/20180323081720_870da702adf964c7abcbdafaab2ad7db_1.jpeg)

| 线程私有                                                     | 线程共享                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 程序计数器、虚拟机栈、本地方法栈                             | **java堆、方法区**                                           |
| 随线程生而生，随线程去而去。线程分配多少内存都是有数的，当线程销毁时，内存就被释放了 | 堆和方法区的内存都是动态分配的（使用new关键字），所以也需要动态回收。 **这部分内存的回收依赖GC完成** |

栈：存放局部变量

堆：存放所有new出来的东西     堆内存就是GC管理的主要区域（知识点，重点）

所有线程共享

方法区：被虚拟机加载的类信息、常量、静态常量等

程序计数器(和系统相关) : 指示当前程序执行到了哪一行，执行Java方法时记录正在执行的虚拟机字节码指令地址；执行本地方法时，计数器值为null

本地方法栈

VM又把堆内存分三代，新生代，老年代，持久代

![img](https://01.imgmini.eastday.com/mobile/20180323/20180323081720_870da702adf964c7abcbdafaab2ad7db_2.jpeg)

新生代：分为eden , From Survivor, TO Survivor

绝大多数刚创建的对象会被分配在Eden区，其中的大多数对象很快就会消亡

Eden区是连续的内存空间，因此在其上分配内存极快

---

# GC过程

为了分代垃圾回收，Java堆内存分为3代：新生代，老年代和永久代

新的对象实例会优先分配在新生代，在经历几次Minor GC后(默认15次)，还存活的会被移至老年代(某些大对象会直接在老年代分配)。

Minor GC发生在新生代，当Eden区没有足够空间时，会发起一次Minor GC，将Eden区中的存活对象移至Survivor区。

Major GC发生在老年代，当升到老年代的对象大于老年代剩余空间时会发生Major GC。

发生Major GC时用户线程会暂停，会降低系统性能和吞吐量。

JVM的参数-Xmx和-Xms用来设置Java堆内存的初始大小和最大值。依据个人经验这个值的比例最好是1:1或者1:1.5



## 新生代

**1**		每次新生代的垃圾回收（又称Minor GC）采用copy算法

最初一次，当Eden区满的时候，执行Minor GC，将消亡的对象清理掉，并将剩余的对象复制到一个存活区From 

此时，TO 是空白的，两个Survivor总有一个是空白的

---

**2**		下次Eden区满了，再执行一次Minor GC，将消亡的对象清理掉，将存活的对象复制到TO 中

然后清空Eden区同时也 将From 中消亡的对象清理掉 将From 存活的对象也复制到TO 区

然后清空From区之后，“From”和“To”会交换他们的角色  新的“To”是上次GC前的“From”，新的“From”是上次GC前的“To”

保证名为To的Survivor区域是空的

---

**3**		Minor GC会一直重复这样的过程  直到“To”区被填满

“To”区被填满之后，会将所有对象移动到年老代中

如果没有填满，当两个存活区切换了几次（每进行一次MinorGC，都会在存活的对象做一个标记，加1，当标记的值大于15，HotSpot虚拟机默认15次，用-XX:MaxTenuringThreshold控制，进入老年代）之后，仍然存活的对象（其实只有一小部分，比如，我们自己定义的对象），将被复制到老年代

---

## 老年代

在新生代中经历了N次垃圾回收后仍然存活的对象，就会被放到年老代 该区域中对象存活率高

老年代的垃圾回收（又称Major GC）通常使用“标记-清理”或“标记-整理”算法

整堆包括新生代和老年代的垃圾回收称为Full GC

当老年代的空间不足时，会触发Major GC/Full GC 速度一般比Minor GC慢10倍以上

---

## 持久代

GC不会在主程序运行期对永久区域进行清理

这也导致了永久代的区域会随着加载的Class的增多而胀满，最终抛出OOM(out of memory)异常。

在JDK8的HotSpot中，把永久代从Java堆中移除了，并把类的元数据直接保存在本地内存区域（堆外内存），称之为元空间

元空间并不在虚拟机中，而是使用本地内存

# Java 网络编程

- **TCP**：TCP 是传输控制协议的缩写，它保障了两个应用程序之间的可靠通信。通常用于互联网协议，被称 TCP / IP。

  

- **UDP**：UDP 是用户数据报协议的缩写，一个无连接的协议。提供了应用程序之间要发送的数据的数据包。

  

**1.GET 和 POST 的区别？**

GET 请求可被缓存

GET 请求保留在浏览器历史记录中

GET 请求可被收藏为书签

GET 请求不应在处理敏感数据时使用

GET 请求有长度限制

---

POST 请求不会被缓存

POST 请求不会保留在浏览器历史记录中

POST 不能被收藏为书签

POST 请求对数据长度没有要求



**2.DNS使用的协议**

**既使用TCP又使用UDP**，首先了解一下TCP与UDP传送字节的长度限制：

UDP报文的最大长度为512字节，而TCP则允许报文长度超过512字节。当DNS查询超过512字节时，协议的TC标志出现删除标志，这时则使用TCP发送。通常传统的UDP报文一般不会大于512字节。

**区域传送时使用TCP，主要有一下两点考虑：**

**辅域名服务器会定时（一般时3小时）向主域名服务器进行查询以便了解数据是否有变动。如有变动，则会执行一次区域传送，进行数据同步**。区域传送将使用TCP而不是UDP，因为数据同步传送的数据量比一个请求和应答的数据量要多得多。

TCP是一种可靠的连接，保证了数据的准确性。

**域名解析时使用UDP协议：**

***客户端向DNS服务器查询域名，一般返回的内容都不超过512字节，用UDP传输即可***。不用经过TCP三次握手，这样DNS服务器负载更低，响应更快。虽然从理论上说，客户端也可以指定向DNS服务器查询的时候使用TCP，但事实上，很多DNS服务器进行配置的时候，仅支持UDP查询包。

**3.COOKIES和SESSION区别**

***Cookies是一种能够让网站服务器把少量数据储存到客户端的硬盘或内存，或是从客户端的硬盘读取数据的一种技术。***

Cookies是当你浏览某网站时，由Web服务器置于你硬盘上的一个非常小的文本文件，它可以记录你的用户ID、密码、浏览过的网页、停留的时间等信息。

**session: 当用户请求来自应用程序的 Web 页时，如果该用户还没有会话，则 Web 服务器将自动创建一个 Session 对象。当会话过期或被放弃后，服务器将终止该会话。**

cookie机制：采用的是在客户端保持状态的方案，而session机制采用的是在服务端保持状态的方案。同时我们看到由于服务器端保持状态的方案在客户端也需要保存一个标识，所以session机制可能需要借助cookie机制来达到保存标识的目的。

**Session是服务器用来跟踪用户的一种手段，每个Session都有一个唯一标识：session ID。当服务器创建了Session时，给客户端发送的响应报文包含了Set-cookie字段，其中有一个名为sid的键值对，这个键值Session ID。**

客户端收到后就把Cookie保存浏览器，并且之后发送的请求报表都包含SessionID。HTTP就是通过Session和Cookie这两个发送一起合作来实现跟踪用户状态，Session用于服务端，Cookie用于客户端

**4.三次握手**

第一次握手：建立连接时，客户端发送syn包(syn=j)到服务器，并进入SYN_SEND状态，等待服务器确认；

第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；

第三次握手：客户端收到服务器的SYN＋ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。

完成三次握手，客户端与服务器开始传送数据



**5.一次完整的HTTP请求过程**

域名解析 --> 发起TCP的3次握手 --> 建立TCP连接后发起http请求 --> 服务器响应http请求，浏览器得到html代码 --> 浏览器解析html代码，并请求html代码中的资源（如js、css、图片等） --> 浏览器对页面进行渲染呈现给用户

 

**6.TCP如何保证可靠传输？**

**三次握手**。

**将数据截断为合理的长度**。应用数据被分割成 TCP 认为最适合发送的数据块（按字节编号，合理分片）

**超时重发**。当 TCP 发出一个段后，它启动一个定时器，如果不能及时收到一个确认就重发

**对于收到的请求，给出确认响应**

**校验出包有错，丢弃报文段，不给出响应**

**对失序数据进行重新排序，然后才交给应用层**

**对于重复数据 ， 能够丢弃重复数据**

**流量控制**。TCP 连接的每一方都有固定大小的缓冲空间。TCP 的接收端只允许另一端发送接收端缓冲区所能接纳的数据。这将防止较快主机致使较慢主机的缓冲区溢出。

**拥塞控制**。当网络拥塞时，减少数据的发送。

 

**7.详细介绍HTTP**

HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议。

特点

简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。

灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。

无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。

无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

支持B/S及C/S模式。

 

**8.URI和URL的区别**

URI，是uniform resource identifier，统一资源标识符，用来唯一的标识一个资源。Web上可用的每种资源如HTML文档、图像、视频片段、程序等都是一个来URI来定位的

URI一般由三部组成：

访问资源的命名机制

存放资源的主机名

资源自身的名称，由路径表示，着重强调于资源。

URL是uniform resource locator，统一资源定位器，它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何locate这个资源。URL是Internet上用来描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上，特别是著名的Mosaic。采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。

URL一般由三部组成：

协议(或称为服务方式)

存有该资源的主机IP地址(有时也包括端口号)

主机资源的具体地址。如目录和文件名等

 

**9.HTTPS和HTTP的区别**

 

https协议需要到CA申请证书，一般免费证书很少，需要交费。

http是超文本传输协议，信息是明文传输；https 则是具有安全性的ssl加密传输协 议。

http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

http默认使用80端口，https默认使用443端口

### **1.网络编程时的同步、异步、阻塞、非阻塞？**

同步：函数调用在没得到结果之前，没有调用结果，不返回任何结果。
异步：函数调用在没得到结果之前，没有调用结果，返回状态信息。
阻塞：函数调用在没得到结果之前，当前线程挂起。得到结果后才返回。
非阻塞：函数调用在没得到结果之前，当前线程不会挂起，立即返回结果。

### **2.Java如何实现无阻塞方式的Socket编程？**

NIO有效解决了多线程服务器存在的线程开销问题。

在NIO中使用多线程主要目的不是为了应对每个客户端请求而分配独立的服务线程，

而是通过多线程充分利用多个CPU的处理能力和处理中的等待时间，达到提高服务能力的目的。

### **3.什么是java 的序列化(串行化)？**

**简单说就是为了保存在内存中的各种对象的状态（也就是实例变量，不是方法），**

**并且可以把保存的对象状态再读出来。虽然你可以用你自己的各种各样的方法来保存object states，但是Java给你提供一种应该比你自己好的保存对象状态的机制，那就是序列化。**

### **4.什么情况下需要序列化？序列化的注意事项，如何实现java 序列化(串行化)？**

· 当你想把的内存中的对象状态保存到一个文件中或者数据库中时候；

· 当你想用套接字在网络上传送对象的时候；

· 当你想通过RMI传输对象的时候；

**序列化注意事项**

1、如果子类实现Serializable接口而父类未实现时，父类不会被序列化，但此时父类必须有个无参构造方法，否则会抛InvalidClassException异常。

2、静态变量不会被序列化，那是类的“菜”，不是对象的。串行化保存的是对象的状态，即非静态的属性，即实例变量。不能保存类变量。

3、transient关键字修饰变量可以限制序列化。对于不需要或不应该保存的属性，应加上transient修饰符。要串行化的对象的类必须是公开的（public）。

4、虚拟机是否允许反序列化，不仅取决于类路径和功能代码是否一致，一个非常重要的一点是两个类的序列化 ID是否一致，就是 private static final long serialVersionUID = 1L。

5、Java 序列化机制为了节省磁盘空间，具有特定的存储规则，当写入文件的为同一对象时，并不会再将对象的内容进行存储，而只是再次存储一份引用。反序列化时，恢复引用关系。

6、序列化到同一个文件时，如第二次修改了相同对象属性值再次保存时候，虚拟机根据引用关系知道已经有一个相同对象已经写入文件，因此只保存第二次写的引用，所以读取时，都是第一次保存的对象。

### **5.java中有几种类型的流？JDK为每种类型的流提供了一些抽象类以供继承，请说出他们分别是哪些类？**

JDK提供的流继承了四大类：

**InputStream(字节输入流)，OutputStream（字节输出流），Reader（字符输入流），Writer（字符输出流）。**

**按流向分类：**

输入流: 程序可以从中读取数据的流。
输出流: 程序能向其中写入数据的流。

**按数据传输单位分类：**

字节流：以字节（8位二进制）为单位进行处理。主要用于读写诸如图像或声音的二进制数据。
字符流：以字符（16位二进制）为单位进行处理。
都是通过字节流的方式实现的。字符流是对字节流进行了封装，方便操作。在最底层，所有的输入输出都是字节形式的。
后缀是Stream是字节流，而后缀是Reader，Writer是字符流。

**按功能分类：**

节点流：从特定的地方读写的流类，如磁盘或者一块内存区域。
过滤流：使用节点流作为输入或输出。过滤流是使用一个已经存在的输入流或者输出流连接创建的。

### **6.用JAVA SOCKET 编程，读服务器几个 字符，再写入本地显示。**

客户端向服务器端发送连接请求后，就被动地等待服务器的响应。

**典型的TCP客户端要经过下面三步操作：**

1、创建一个Socket实例：构造函数向指定的远程主机和端口建立一个TCP连接；
2、通过套接字的I/O流与服务端通信；
3、使用Socket类的close方法关闭连接。

###  

服务端的工作是建立一个通信终端，并被动地等待客户端的连接。

典型的TCP服务端执行如下两步操作：

1、创建一个ServerSocket实例并指定本地端口，用来监听客户端在该端口发送的TCP连接请求；
2、重复执行：
1）调用ServerSocket的accept（）方法以获取客户端连接，并通过其返回值创建一个Socket实例；
2）为返回的Socket实例开启新的线程，并使用返回的Socket实例的I/O流与客户端通信；
3）通信完成后，使用Socket类的close（）方法关闭该客户端的套接字连接。



























2、final, finally 和 finalize 的区别？



final 用于声明属性，方法和类，表示属性不可变，方法不可被重写，类不可被继承。

finally 是异常处理语句结构的一部分，表示总是执行。

finalize 是 object 类的一个方法，在垃圾收集器执行的时候会调用这个对象回收的方法，工垃圾收集时其他资源的回收，比如关闭文件。

### 3、什么是单例模式？实现步骤？

单例模式保证了对象唯一。分为懒汉式（在类加载时不初始化）和饿汉式（在类加载时就完成了初始化，所以类加载比较慢，但获取对象的速度快）。

实现步骤：私有化构造函数、创建一个静态的私有对象、提供公共的访问方法。

### 4、ArrayList 和 LinkedList 有何区别？

ArrayList 是基于动态数组的数据结构，LinkedList 是基于链表的数据结构；对于随机访问 get 和 set，ArrayList 较优，因为 LinkedList 要移动指针；对于新增和删除操作 add 和 remove，LinedList 较优，因为ArrayList 要移动数据。

#### 数组（Array）和列表（ArrayList）有什么区别?

1、存储内容比较：
Array 数组可以包含基本类型和对象类型，
ArrayList 却只能包含对象类型。
Array 数组在存放的时候一定是同种类型的元素。ArrayList 就不一定了 。

2、空间大小比较：
Array 数组的空间大小是固定的,所以需要事前确定合适的空间大小。
ArrayList 的空间是动态增长的,而且，每次添加新的元素的时候都会检查内部数组的空间是否足够。

3.方法上的比较：

ArrayList 方法上比 Array 更多样化，比如添加全部 addAll()、删除全部 removeAll()、返回迭代器 iterator() 等。

应用场景

如果想要保存一些在整个程序运行期间都会存在而且不变的数据，我们可以将它们放进一个全局数组里

单纯只是想要以数组的形式保存数据，而不对数据进行增加等操作，只是方便我们进行查找的话，那么，我们就选择 ArrayList

需要对元素进行频繁的移动或删除，或者是处理的是超大量的数据 可以考虑选择 LinkedList

### 5、HashMap 和 Hashtable 的区别？

HashMap 允许空键值，Hashtable 不允许；

HashMap 继承自 AbstractMap，Hashtable 继承自 Dictionary 类，两者都实现了 Map 接口； HashMap 的方法不是同步的，Hashtable 的方法是同步的。

### 6、Iterater 和 ListIterator 之间有什么区别？

Iterator 用来遍历 Set 和 List 集合，而 ListIterator 只能遍历 List； Iterator 只可以向前遍历，而 LIstIterator 可以双向遍历；ListIterator 从 Iterator 接口继承，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。

### 7、创建线程的方式？

继承 Thread 类

实现 Runnable 接口

使用 Executor 框架

### 8、什么是死锁？

两个线程或两个以上线程都在等待对方执行完毕才能继续往下执行的时候就发生了死锁。结果就是这些线程都陷入了无限的等待中。

### 9、wait()与 sleep()的区别？

sleep()来自 Thread 类，wait()来自 Object 类；

调用 sleep()方法，线程不会释放对象锁。而调用 wait 方法线程会释放对象锁；

sleep()睡眠后不出让系统资源，wait 让其他线程可以占用 CPU；

sleep(milliseconds)需要指定一个睡眠时间，时间一到会自动唤醒。而 wait()需要配合 notify()

或者 notifyAll()使用。

### 10、什么是 ThreadLocal？ThreadLocal 和 Synchonized 的区别？

线程局部变量。是局限于线程内部的变量，属于线程自身所有，不在多个线程间共享。Java提供 ThreadLocal 类来支持线程局部变量，是一种实现线程安全的方式。

synchronized 是利用锁的机制，使变量或代码块在某一时该只能被一个线程访问。而 ThreadLocal 为每一个线程都提供了变量的副本，使得每个线程在某一时间访问到的并不是同一个对象，这样就隔离了多个线程对数据的数据共享。

------

## Java Web

### Ajax

AJAX = Asynchronous JavaScript and XML（异步 JavaScript 和 XML）。

Ajax 的原理简单来说通过 XmlHttpRequest 对象来向服务器发异步请求，从服务器获得数据，然后用 Javascript 来操作 DOM 而更新页面。这其中最关键的一步就是从服务器获得请求数据。

XmlHttpRequest 是 ajax 的核心机制，它是在 IE5 中首先引入的，是一种支持异步请求的技术。简单的说，也就是 Javascript 可以及时向服务器提出请求和处理响应，而不阻塞用户。达到无刷新的效果。

![img](https://pic3.zhimg.com/80/v2-99287cefef5d3f3e9523228066e6cd4a_hd.jpg)

### JQuery

JQuery 是一个 JavaScript 库。功能包括 HTML 元素选取和操作、CSS 操作、HTML 事件函数、 JavaScript 特效和动画、HTML DOM 遍历和修改、AJAX 和 Utilities。除此之外，JQuery 还提供了大量插件。

基础语法： $(selector).action()。

选择器：主要分四大选择器，分别是基本选择器、层次选择器、过滤选择器、属性过滤选择器。

事件：例如 click()、dblclick()、mouseenter()、mouseleave()、mousedown()等。

## Cookie

在 web 程序中是使用 HTTP 协议来传输数据的，因为 http 是无状态协议，一旦数据交换完毕，客户端和服务器端的连接就会关闭，再次交换数据需要建立新的连接，所以无法实现会话跟踪，cookie 技术则弥补了这一缺陷。

cookie 实际上一段的文本信息，客户端请求服务器。

如果服务器需要记录该用户的状态，就使用 response 向客户端浏览器颁发一个 cookie。

客户端浏览器会把 cookie 保存起来。

当浏览器再请求该网站时，浏览器把请求的网址连同该 cookie 一同提交给服务器。

服务器检查该 cookie，以此来辨认用户的状态。

服务器还可以根据需要修改 cookie 的内容。

### 过程

1. 用户使用浏览器访问一个支持Cookie的网站的时候，用户会提供包括用户名在内的个人信息并且提交至服务器；

   

2. 服务器在向客户端回传相应的超文本的同时也会发回这些个人信息，当然这些信息并不是存放在HTTP响应体 （Response Body)中的，而是存放于HTTP响应头(Response Header)

   

3. 客户端浏览器接收到来自服务器的响应之后，浏览器会将这些信息存放在一个统一的位置。 对于Windows操作系统而言，我们可以从： [系统盘]:\Documents and Settings[用户名]\Cookies目录中找到存储的Cookie；

   

4. 客户端再次向服务器发送请求的时候，都会把相应的Cookie再次发回至服务器。 而这次，Cookie信息则存放在HTTP请求头(equest Header)了。

![img](https://pic2.zhimg.com/80/v2-ad05c5fa4ff99375d4cc109806faa5d5_hd.jpg)

### cookie 生命周期:

cookie 的 maxAge 决定 cookie 的生命周期，单位为秒（second）。

cookie 通过 getMaxAge() 方法和 setMaxAge()方法来获得 maxAge 属性，

如果 maxAhe 属性为正，则表示 cookie 会在 maxAge 秒之后自动失效。

如果 maxAge 属性为负，则说明 cookie 仅在本浏览器窗口和本窗口打开的子窗口下有效，关闭窗口 cookie 则失效。

maxAge 的默认值是-1 当 maxAge 的值为 0 时，表示删除 cookie。

### Session

session 也是一种记录客户状态的机制，不同的是 cookie 保存在客户端浏览器中，而 session 保存在服务器上。客户端浏览器访问服务器是时候把客户端信息以某种形式记录在服务器上，这就是 session 中查找该客户的状态。

session 生命周期：

session 保存在服务器端，为了获得更高的存取速度，服务器一般把 session 放在内存。每个用户都会有一个独立的 session,如果 session 内容过于复杂，当大量客户访问服务器时可能会导致内存溢出。

session 在用户第一次访问服务器的时候自动创建，需要注意只有访问 JSP，Servlet 等程序时才会创建 session；只要访问 HTML、IMAGE 等静态资源不会创建 session。如果尚未生成session,可以使用 request.getSession(true)强制生成 session。

session 生成后，只要用户访问，服务器就会更新 session 的最后访问时间，并维护该 session。用户每访问服务器一次，无论是否续写 session 服务器都认为该用户的 session 活跃（active）了一次。

Session 对应的类是 javax.servlet.http.HttpSession，每一个访问者都对应一个 session 对象，并将其状态信息保存在这个 session 对象中，session 对象的创建是在用户第一次访问服务器时产生的。

------

## 热门面试问题：

### 1、原生态 Ajax 执行流程？

创建 XMLHttpRequest 对象；

注册回调函数；

设置连接信息；

发送数据，与服务器开始交互；

接受服务器返回数据。

### 2、转发（forward）和重定向（redirect）的区别？

forward 是容器中控制权的转向，是服务器请求资源，服务器直接访问目标地址的 URL，把那个 URL 的响应内容读取过来，然后把这些内容再发给浏览器，浏览器根本不知道服务器发送的内容是从哪儿来的，所以它的地址栏中还是原来的地址。

redirect 就是服务器端根据逻辑，发送一个状态码，告诉浏览器重新去请求那个地址，因此从浏览器的地址栏中可以看到跳转后的链接地址，很明显 redirect 无法访问到服务器保护起来资源，但是可以从一个网站 redirect 到其他网站。

### 3、怎么防止表单重复提交？

i.禁掉提交按钮。表单提交后使用 Javascript 使提交按钮 disable。

ii．Post/Redirect/Get 模式。在提交后执行页面重定向，这就是所谓的 Post-Redirect-Get (PRG) 模式。简言之，当用户提交了表单后，你去执行一个客户端的重定向，转到提交成功信息页面。

iii.在 session 中存放一个特殊标志。当表单页面被请求时，生成一个特殊的字符标志串，存在 session 中，同时放在表单的隐藏域里。接受处理表单数据时，检查标识字串是否存在，并立即从 session 中删除它，然后正常处理数据。

### 4、web.xml 文件中可以配置哪些内容？

web.xml 用于配置 Web 应用的相关信息，如：监听器（listener）、过滤器（filter）、 Servlet、相关参数、会话超时时间、安全验证方式、错误页面等。

------

## 数据库（MySQL）

### 连接查询

分类：内连接、外连接、自然连接（略）、交叉连接（略）。

### 内连接

基本语法：左表 [inner] join 右表 on 左表.字段 = 右表.字段;

从左表中取出每一条记录，去右表中与所有的记录进行匹配：匹配必须是某个条件在左表中与右表中相同最终才会保留结果，否则不保留。

### 外连接

基本语法: 左表 left/right join 右表 on 左表.字段 = 右表.字段;

left join: 左外连接(左连接), 以左表为主表

right join: 右外连接(右连接), 以右表为主表

以某张表为主，取出里面的所有记录，然后每条与另外一张表进行连接：不管能不能匹配上条件，最终都会保留。能匹配，正确保留；不能匹配，其他表的字段都置空 NULL。

![img](https://pic1.zhimg.com/80/v2-9cc11281696ccb5d9da6c7c3a1af5634_hd.jpg)


左外连接

![img](https://pic4.zhimg.com/80/v2-08315c8f364746512b536dd8156fc517_hd.jpg)


右外连接

![img](https://pic4.zhimg.com/80/v2-4915bc85816895455192a91a1fcc9767_hd.jpg)

### 联合查询

基本语法：

```java
		Select 语句 1
		
		Union [union 选项]
		
		Select 语句 2
		
		……
```

将多次查询(多条 select 语句), 在记录上进行拼接(字段不会增加)，每一条 select 语句获取的字段数必须严格一致(但是字段类型无关)。

其中 union 选项有 2 个。ALL：保留所有；Distinct（默认）：去重。

应用：查询同一张表，但是有不同的需求；查询多张表，多张表的结构完全一致，保存的数据也是一样的。

在联合查询中，order by 不能直接使用。需要对查询语句使用括号才行。另外需要配合 limit 使用。

### 索引

如果说数据库表中的数据是一本书，那么索引就是书的目录。索引能够让我们快速的定位想要查询的数据。

索引的结构：BTree 索引和 Hash 索引。

MyISAM 和 InnoDB 存储引擎：只支持 BTREE 索引， 也就是说默认使用 BTREE，不能够更换。

MEMORY/HEAP 存储引擎：支持 HASH 和 BTREE 索引。

索引的分类：单列索引(普通索引，唯一索引，主键索引)、组合索引、全文索引、空间索引。

### 数据库引擎

InnoDB：支持事务处理，支持外键，支持崩溃修复能力和并发控制。如果需要对事务的完整

性要求比较高（比如银行），要求实现并发控制（比如售票），那选择 InnoDB 有很大的优势。

如果需要频繁的更新、删除操作的数据库，也可以选择 InnoDB，因为支持事务的提交（commit）

和回滚（rollback）。

MyISAM：插入数据快，空间和内存使用比较低。如果表主要是用于插入新记录和读出记录，

那么选择 MyISAM 能实现处理高效率。如果应用的完整性、并发性要求比较低，也可以使

用。

MEMORY：所有的数据都在内存中，数据的处理速度快，但是安全性不高。如果需要很快的

读写速度，对数据的安全性要求较低，可以选择 MEMOEY。它对表的大小有要求，不能建

立太大的表。所以，这类数据库只使用在相对较小的数据库表。

### 存储过程

SQL 语句需要先编译然后执行，而存储过程（Stored Procedure）是一组为了完成特定功能的 SQL 语句集，经编译后存储在数据库中，用户通过指定存储过程的名字并给定参数（如果该存储过程带有参数）来调用执行它。

存储过程是可编程的函数，在数据库中创建并保存，可以由 SQL 语句和控制结构组成。当想要在不同的应用程序或平台上执行相同的函数，或者封装特定功能时，存储过程是非常有用的。数据库中的存储过程可以看做是对编程中面向对象方法的模拟，它允许控制数据的访问方式。

存储过程的优点：
增强 SQL 语言的功能和灵活性；
标准组件式编程；
较快的执行速度；
减少网络流量；
作为一种安全机制来充分利用。

------

## 热门面试问题：

### 1、JDBC 编程的步骤？

（1）	注册驱动；

（2）	获取连接对象 Connection；

（3）	创建 Statement 对象；

（4）	运行 SQL 语句；

（5）	处理结果；

（6）	关闭连接释放资源。

### 2、事务的 ACID 是什么？事务并发会产生哪些问题？

ACID 表示事务的特性：原子性、一致性、隔离性和持久性。

- **原子性(Atomic)：**事务中各项操作，要么全做要么全不做，任何一项操作的失败都会导致整个事务的失败；
- **一致性(Consistent)：**事务结束后系统状态是一致的；
- **隔离性(Isolated)：**并发执行的事务彼此无法看到对方的中间状态；
- **持久性(Durable)：**事务完成后所做的改动都会被持久化，即使发生灾难性的失败。通过日志和同步备份可以在故障发生后重建数据。

事务并发产生的问题：脏读、幻读、不可重复读。

**脏读（Dirty Read）：**A 事务读取 B 事务尚未提交的数据并在此基础上操作，而 B 事务执行回滚，那么 A 读取到的数据就是脏数据。

**幻读（Phantom Read）：**事务 A 重新执行一个查询，返回一系列符合查询条件的行，发现其中插入了被事务 B 提交的行。

**不可重复读（Unrepeatable Read）：**事务 A 重新读取前面读取过的数据，发现该数据已经被另一个已提交的事务 B 修改过了。

### 3、数据库性能优化有哪些方式？

```text
SQL 优化：
		尽量避免使用 SELECT	*；
		只查询一条记录时使用 limit 1；
		使用连接查询代替子查询；
		尽量使用一些能通过索引查询的关键字。
表结构优化：

		尽量使用数字类型字段，提高比对效率；
		长度不变且对查询速度要求高的数据可以考虑使用 char，否则使用 varchar；表中字段过多时可以适当的进行垂直分割，将部分字段移动到另外一张表；表中数据量过大可以适当的进行水平分割，将部分数据移动到另外一张表。

其它优化：

		对查询频率高的字段适当的建立索引，提高效率；根据表的用途使用合适的数据库引擎；读写分离。		
```

## 框架部分

## Spring

### Spring 的理解

spring 是一个开源框架，Spring 为简化企业级应用开发而生，使用 Spring 可以使简单的 JavaBean 实现以前只有 EJB 才能实现的功能。Spring 是一个 IOC 和 AOP 容器框架。 Spring 主要核心是：

(1)控制反转(IOC)：传统的 java 开发模式中，当需要一个对象时，我们会自己创建一个对象，而在 Spring 开发模式中，Spring 容器使用了工厂模式为我们创建了所需要的对象，我们直接调用 Spring 为我们提供的对象即可，这就是控制反转的思想。实例化一个 java 对象有三种方式：使用类构造器，使用静态工厂方法，使用实例工厂方法。当使用 spring 时我们不需要关心通过何种方式实例化一个对象，spring 通过控制反转机制自动为我们实例化一个对象。

(2)依赖注入(DI)：Spring 使用 Java Bean 对象的 Set 方法或者带参数的构造方法为我们在创建所需对象时将其属性自动设置所需要的值的过程就是依赖注入的基本思想。

(3)面向切面编程(AOP)：在面向对象编程(OOP)思想中，我们将事物纵向抽象成一个个的对象。而在面向切面编程中，我们将一个个对象某些类似的方面横向抽象成一个切面，对这个切面进行一些如权限验证，事物管理，记录日志等公用操作处理的过程就是面向切面编程的思想。

在 Spring 中，所有管理的对象都是 JavaBean 对象，而 BeanFactory 和 ApplicationContext 就是 spring 框架的两个 IOC 容器，现在一般使用 ApplicationContext，其不但包含了 BeanFactory 的作用，同时还进行更多的扩展。

### Spring Bean 生命周期

1.Spring 容器 从 XML 文件中读取 Bean 的定义，并实例化 Bean。

2.Spring 根据 Bean 的定义填充所有的属性。

3.如果 Bean 实现了 BeanNameAware 接口，Spring 传递 bean 的 ID 到 setBeanName 方法。

4.如果 Bean 实现了 BeanFactoryAware 接口， Spring 传递 beanfactory 给 setBeanFactory 方法。

5.如 果 有 任 何 与 bean 相 关 联 的 BeanPostProcessors ， Spring 会 在postProcesserBeforeInitialization()方法内调用它们。

6.如果 bean 实现 IntializingBean 了，调用它的 afterPropertySet 方法，如果 bean 声明了初始化方法，调用此初始化方法。

7.如果有 BeanPostProcessors 和 bean 关联，这些 bean 的 postProcessAfterInitialization()

方法将被调用。

8.如果 bean 实现了 DisposableBean，它将调用 destroy()方法。注意：

有两个重要的 bean 生命周期方法，第一个是 setup() ， 它是在容器加载 bean 的时候被调用。第二个方法是 teardown() 它是在容器卸载类的时候被调用。

The bean 标签有两个重要的属性 init-method 和 destroy-method。使用它们你可以自己定制初始化和注销方法。它们也有相应的注解@PostConstruct 和@PreDestroy。

### Spring 中的设计模式

代理模式—Spring 中两种代理方式，若目标对象实现了若干接口，spring 使用 JDK 的 java.lang.reflect.Proxy 类代理，若目标对象没有实现任何接口，spring 使用 CGLIB 库生成目标对象的子类。

单例模式—在 spring 配置文件中定义的 bean 默认为单例模式。

模板方法模式—用来解决代码重复的问题。比如： RestTemplate, JmsTemplate, JpaTemplate。

前端控制器模式—Srping 提供了 DispatcherServlet 来对请求进行分发。

视图帮助(View Helper )—Spring 提供了一系列的 JSP 标签，高效宏来辅助将分散的代码整合在视图里。

依赖注入—贯穿于 BeanFactory/ApplicationContext 接口的核心理念。

工厂模式—在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。Spring 中使用 BeanFactory 用来创建对象的实例。

### Spring 注解

Spring 在 2.5 版本以后开始支持用注解的方式来配置依赖注入。可以用注解的方式来替代 XML 方式的 bean 描述，可以将 bean 描述转移到组件类的内部，只需要在相关类上、方法上或者字段声明上使用注解即可。注解注入将会被容器在 XML 注入之前被处理，所以后者会覆盖掉前者对于同一个属性的处理结果。

注解装配在 Spring 中是默认关闭的。所以需要在 Spring 文件中配置一下才能使用基于注解的装配模式。如果你想要在你的应用程序中使用关于注解的方法的话，请参考如下的配置。

```text
<beans>

<context:annotation-config/>

<!-- bean definitions go here -->

</beans>
```

在 context:annotation-config/标签配置完成以后，就可以用注解的方式在 Spring 中向属

性、方法和构造方法中自动装配变量。

几种比较重要的注解类型：

1.@Required：该注解应用于设值方法。

2.@Autowired：该注解应用于有值设值方法、非设值方法、构造方法和变量。

3.@Qualifier：该注解和@Autowired 注解搭配使用，用于消除特定 bean 自动装配的歧义。

4.JSR-250 Annotations： Spring 支持基于 JSR-250 注解的以下注解，@Resource、 @PostConstruct 和 @PreDestroy。

### Spring 事务

Spring 支持两种类型的事务管理：

1.编程式事务管理：这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。

2.声明式事务管理：这意味着你可以将业务代码和事务管理分离，你只需用注解和 XML 配置来管理事务。

Spring 事务配置示例（使用 tx 标签配置的拦截器）

```text
<!-- 定义事务管理器（声明式的事务） -->

<bean id="transactionManager"

class="org.springframework.orm.hibernate3.HibernateTransactionManager">

<property name="sessionFactory" ref="sessionFactory" />

</bean>

<!-- 配置 Advice 通知 -->

<tx:advice id="txAdvice" transaction-manager="transactionManager"> <tx:attributes>

<tx:method name="*" propagation="REQUIRED" /> </tx:attributes>

</tx:advice>

<!-- 配置切点切面 -->

<aop:config>

<aop:pointcut id="interceptorPointCuts" expression="execution(* com.bluesky.spring.dao.*.*(..))" />

<aop:advisor advice-ref="txAdvice"

pointcut-ref="interceptorPointCuts" />

</aop:config>
```

## SpringMVC

### SpringMVC 执行流程



![img](https://pic2.zhimg.com/80/v2-c7dd0eeb5a4f3de320c1a47c7d763f31_hd.jpg)


1、用户发送请求至 DispatcherServlet（前端控制器）；

2、DispatcherServlet 收到请求调用 HandlerMapping（处理器映射器）；

3、HandlerMapping 找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给 DispatcherServlet；

4、DispatcherServlet 调用 HandlerAdapter（处理器适配器）；

5、HandlerAdapter 经过适配调用具体的 Controller (处理器，也叫后端控制器)；

6、Controller 执行完成返回 ModelAndView 对象；

7、HandlerAdapter 将 controller 执行结果 ModelAndView 返回给 DispatcherServlet；

8、DispatcherServlet 将 ModelAndView 传给 ViewReslover（视图解析器）；

9、ViewReslover 解析后返回具体 View；

10、DispatcherServlet 根据 View 进行渲染视图（即将模型数据填充至视图中）；

11、DispatcherServlet 响应用户。

### springmvc 常用注解

@RequestMapping：是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

@PathVariable：用于将请求 URL 中的模板变量映射到功能处理方法的参数上，即取出 uri 模板中的变量作为参数。

@requestParam ： 主 要 用 于 在 SpringMVC 后 台 控 制 层 获 取 参 数 ， 类 似 一 种 是request.getParameter(“name”)，它有三个常用参数：defaultValue = “0”, required = false, value = “isApp”；defaultValue 表示设置默认值，required 铜过 boolean 设置是否是必须要传入的参数，value 值表示接受的传入的参数类型。

@ResponseBody ： 该 注 解 用 于 将	Controller	的 方 法 返 回 的 对 象 ， 通 过 适 当 的HttpMessageConverter 转换为指定格式后，写入到 Response 对象的 body 数据区。使用时机：返回的数据不是 html 标签的页面，而是其他某种格式的数据时（如 json、xml 等）使用 @RequestBody ： 该 注 解 常 用 来 处 理 Content-Type: 不 是 application/x-www-form-urlencoded 编码的内容，例如 application/json, application/xml 等； @RequestHeader ：可以把 Request 请求 header 部分的值绑定到方法的参数上。

@CookieValue ：可以把 Request header 中关于 cookie 的值绑定到方法的参数上。

### SpringMVC 和 Struts2 对比

机制：spring mvc 的入口是 servlet，而 struts2 是 filter（这里要指出，filter 和 servlet 是不同的。以前认为 filter 是 servlet 的一种特殊），这样就导致了二者的机制不同，这里就牵涉到 servlet 和 filter 的区别了。

性能：spring 会稍微比 struts 快。spring mvc 是基于方法的设计，而 sturts 是基于类，每次发一次请求都会实例一个 action，每个 action 都会被注入属性，而 spring 基于方法，粒度更细，但要小心把握像在 servlet 控制数据一样。spring3 mvc 是方法级别的拦截，拦截到方法后根据参数上的注解，把 request 数据注入进去，在 spring3 mvc 中，一个方法对应一个 request 上下文。而 struts2 框架是类级别的拦截，每次来了请求就创建一个 Action，然后调用 setter getter 方法把 request 中的数据注入；struts2 实际上是通过 setter getter 方法与 request 打交道的；struts2 中，一个 Action 对象对应一个 request 上下文。

参数传递：struts 是在接受参数的时候，可以用属性来接受参数，这就说明参数是让多个方法共享的。

设计思想上：struts 更加符合 oop 的编程思想，spring 就比较谨慎，在 servlet 上扩展。

## Mybatis

### Mybatis 的理解

MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手工设置参数以及抽取结果集。MyBatis 使用简单的 XML 或注解来配置和映射基本体，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java 对象)映射成数据库中的记录。

Mybatis 的优点：

1、简单易学。mybatis 本身就很小且简单。没有任何第三方依赖，最简单安装只要两个 jar 加配置几个 sql 映射文件，易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现；

2、灵活。mybatis 不会对应用程序或者数据库的现有设计强加任何影响。 sql 写在 xml 里，便于统一管理和优化。通过 sql 基本上实现不使用数据访问框架可以实现的所有功能；

3、解除 sql 与程序代码的耦合。通过提供 DAO 层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql 和代码的分离，提高了可维护性；4、提供映射标签，支持对象与数据库的 orm 字段关系映射；5、提供对象关系映射标签，支持对象关系组建维护；

6、提供 xml 标签，支持编写动态 sql。

### Mybatis 缓存

一级缓存：Mybatis 的一级缓存的作用域是 session，当 openSession()后，如果执行相同的SQL（相同语句和参数），Mybatis 不进行执行 SQL，而是从缓存中命中返回。

二级缓存：Mybatis 的二级缓存的作用域是一个 mapper 的 namespace，同一个 namespace 中查询 sql 可以从缓存中命中。二级缓存是可以跨 session 的。

## SpringBoot

### SpringBoot 简介

Spring Boot(英文中是“引导”的意思)，是用来简化 Spring 应用的搭建到开发的过程。应用开箱即用，只要通过 “just run”（可能是 java -jar 或 tomcat 或 maven 插件 run 或 shell 脚本），就可以启动项目。二者，Spring Boot 只要很少的 Spring 配置文件（例如那些 xml，property）。因为“习惯优先于配置”的原则，使得 Spring Boot 在快速开发应用和微服务架构实践中得到广泛应用。

### SpringBoot 特性

自动配置：针对很多 Spring 应用程序常见的应用功能，Spring Boot 能自动提供相关配置；起步依赖：告诉 Spring Boot 需要什么功能，它就能引入需要的库；

命令行界面：这是 Spring Boot 的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建；

Actuator：让你能够深入运行中的 Spring Boot 应用程序，一探究竟。

### SpringBoot 核心

@SpringBootApplication 这个 Spring Boot 核心注解是由其它三个重要的注解组合，分别是：

@SpringBootConfiguration 、 @EnableAutoConfiguration 和 @ComponentScan。

@ SpringBootConfiguration

点开查看发现里面还是应用了@Configuration。任何一个标注了@Configuration 的 Java 类定义的都是一个 JavaConfig 配置类。SpringBoot 社区推荐使用基于 JavaConfig 的配置形式，所以，这里的启动类标注了@Configuration 之后，本身其实也是一个 IoC 容器的配置类。

@EnableAutoConfiguration

是一个复合注解。最重要的是@Import(EnableAutoConfigurationImportSelector.class)，借助EnableAutoConfigurationImportSelector，@EnableAutoConfiguration 可以帮助 SpringBoot 应用将所有符合条件的@Configuration 配置都加载到当前 SpringBoot 使用的 IoC 容器。 @ComponentScan

@ComponentScan 这个注解在 Spring 中很重要，它对应 XML 配置中的元素， @ComponentScan 的功能其实就是自动扫描并加载符合条件的组件（比如@Component 和 @Repository 等）或者 bean 定义，最终将这些 bean 定义加载到 IoC 容器中。

## SpringCloud

### SpringCloud 简介

spring Cloud 是一个基于 Spring Boot 实现的云应用开发工具，它为基于 JVM 的云应用开发中的配置管理、服务发现、断路器、智能路由、微代理、控制总线、全局锁、决策竞选、分布式会话和集群状态管理等操作提供了一种简单的开发方式。

### SpringCloud 核心组件

服务注册发现 - Netflix Eureka

配置中心 - spring cloud config

负载均衡-Netflix Ribbon

断路器 - Netflix Hystrix

路由(网关) - Netflix Zuul

### 微服务

微服务是一种可以让软件职责单一、松耦合、自包含、可以独立运行和部署的架构思想。关键思想就是：拆分、单一、独立、组件化。把原本一个庞大、复杂的项目按业务边界拆分一个一个独立运行的小项目，通过接口的方式组装成一个大的项目。

## Docker

### Docker 简介

Docker 项目的目标是实现轻量级的操作系统虚拟化解决方案。 Docker 的基础是 Linux 容器（LXC）等技术。在 LXC 的基础上 Docker 进行了进一步的封装，让用户不需要去关心容器的管理，使得操作更为简便。用户操作 Docker 的容器就像操作一个快速轻量级的虚拟机一样简单。

### Docker 理解

Docker 其实和虚拟机的目的差不多，都是虚拟化技术，但是 docker 比虚拟机更加轻量级，更快，更加易于移植。

镜像: 创建虚拟机和 docker 都必不可少的东西。创建一个虚拟机，就先得下载操作系统的 ISO 镜像文件，然后通过镜像文件安装操作系统，和实体机类似，然后能在虚拟机中去安装各种软件。

容器: 通俗拿 VM 虚拟机和 Docker 来举例，一个容器就类似于一个虚拟机，只不过在 Docker 技术的术语上称为容器。这个容器里装的就是我们部署的应用在运行，和虚拟机一样可以开机，关机，重启。Docker 称为容器的运行，关闭，重启。而且这个容器可以打包为镜像文件，类似虚拟机快照的文件，放在其它虚拟机上又可以保持原样能运行，Docker 也是如此，把容器打包为镜像文件，然后在新的服务器安装好的 Docker 环境下导入进去，保持原来的状态能够运行。

## Redis

### Redis 简介

Remote Dictionary Server（Redis）是一个基于 key-value 键值对的持久化数据库存储系统。支持多种数据结构，这些数据类型都支持 push/pop、add/remove 及取交集并集和差集及更丰富的操作，而且这些操作都是原子性的。

### Redis 支持的数据类型

字符串（strings）
散列（hashes）
列表（lists）
集合（sets）
有序集合（sorted sets）

### Redis 应用场景

缓存
计数器
发布订阅构建消息系统
排行榜

### Redis 持久化

RDB 持久化可以在指定的时间间隔内生成数据集的时间点快照（point-in-time snapshot）。 AOF 持久化记录服务器执行的所有写操作命令，并在服务器启动时，通过重新执行这些命令来还原数据集。 AOF 文件中的命令全部以 Redis 协议的格式来保存，新命令会被追加到文件的末尾。 Redis 还可以在后台对 AOF 文件进行重写（rewrite），使得 AOF 文件的体积不会超出保存数据集状态所需的实际大小。

### Redis 的优势

性能极高 – Redis 能读的速度是 110000 次/s,写的速度是 81000 次/s 。

丰富的数据类型 – Redis 支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
原子 – Redis 的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过 MULTI 和 EXEC 指令包起来。丰富的特性 – Redis 还支持 publish/subscribe, 通知, key 过期等等特性。

## Solr

### Solr 简介

Solr 是一个基于 Lucene 的 Java 搜索引擎服务器。Solr 提供了层面搜索、命中醒目显示并且支持多种输出格式（包括 XML/XSLT 和 JSON 格式）。它易于安装和配置，而且附带了一个基于 HTTP 的管理界面。Solr 已经在众多大型的网站中使用，较为成熟和稳定。Solr 包装并扩展了 Lucene，所以 Solr 的基本上沿用了 Lucene 的相关术语。更重要的是，Solr 创建的索引与 Lucene 搜索引擎库完全兼容。通过对 Solr 进行适当的配置，某些情况下可能需要进行编码，Solr 可以阅读和使用构建到其他 Lucene 应用程序中的索引。此外，很多 Lucene 工具（如 Nutch、 Luke）也可以使用 Solr 创建的索引。

### Solr 配置

Schema.xml：

在下载 solr 包的安装解压目录的\solr\example\solr\collection1\conf 中找到，它就是 solr 模式关联的文件。

fieldtype 节点主要用来定义数据类型；

field 节点指定建立索引和查询数据的字段；

solrQueryParser 指定搜索时多个词之间的关系，可以是 or 或 and。

solrconfig.xml：

配置文件主要定义了 SOLR 的一些处理规则，包括索引数据的存放位置，更新，删除，查询的一些规则配置。

datadir 节点定义了索引数据和日志文件的存放位置； lib 节点表示 solr 引用包的位置。

### 倒排索引

正排表（索引）是以文档的 ID 为关键字，表中记录文档中每个字的位置信息，查找时扫描表中每个文档中字的信息直到找出所有包含查询关键字的文档。

![img](https://pic4.zhimg.com/80/v2-ae7d374c8a5f7e1653dc80f0dccbcadb_hd.jpg)


倒排表（索引）以字或词为关键字进行索引，表中关键字所对应的记录表项记录了出现这个字或词的所有文档，一个表项就是一个字表段，它记录该文档的 ID 和字符在该文档中出现的位置情况。

![img](https://pic1.zhimg.com/80/v2-87d4403c5d5aa2f0a45deb41791b79cc_hd.jpg)



## RabbitMQ/ActiveMQ

### RabbitMQ 简介

RabbitMQ 是一个由 Erlang 语言开发的 AMQP 的开源实现。

AMQP：Advanced Message Queue，高级消息队列协议。它是应用层协议的一个开放标准，为面向消息的中间件设计，基于此协议的客户端与消息中间件可传递消息，并不受产品、开发语言等条件的限制。

### RabbitMQ 特点

可靠性（Reliability）： 使用持久化、传输确认和发布确认机制来保证可靠性。

灵活的路由（Flexible Routing）：在消息进入队列之前，通过 Exchange 来路由消息的。对于典型的路由功能，RabbitMQ 已经提供了一些内置的 Exchange 来实现。针对更复杂的路由功能，可以将多个 Exchange 绑定在一起，也通过插件机制实现自己的 Exchange 。

高可用（Highly Available Queues）：队列可以在集群中的机器上进行镜像，使得在部分节点出问题的情况下队列仍然可用。

多种协议（Multi-protocol）：RabbitMQ 支持多种消息队列协议，比如 STOMP、MQTT 等。多语言客户端（Many Clients）：RabbitMQ 几乎支持所有常用语言，比如 Java、.NET、Ruby等。

管理界面（Management UI）：RabbitMQ 提供了一个易用的用户界面，使得用户可以监控和管理消息 Broker 的许多方面。

跟踪机制（Tracing）：如果消息异常，RabbitMQ 提供了消息跟踪机制，使用者可以找出发生了什么。

插件机制（Plugin System）：RabbitMQ 提供了许多插件，来从多方面进行扩展，也可以编写自己的插件。

### RabbitMQ 工作模式

简单模式：一个生产者发送消息到队列，一个消费者接收。

工作队列模式：一个生产者，多个消费者，每个消费者获取到的消息唯一，多个消费者只有一个队列。

发布/订阅模式：一个生产者发送的消息会被多个消费者获取，每个消费者只能从自己订阅的队列中获取。

路由模式：生产者发布消息的时候添加路由键，消费者绑定队列到交换机时添加键值，这样就可以接收到需要接收的消息。

通配符模式：基本思想和路由模式是一样的，只不过路由键支持模糊匹配，符号“#”匹配一个或多个词，符号“*”只匹配一个词。

### ActiveMQ 简介

ActiveMQ 是 Apache 推出的一款开源的，完全支持 JMS1.1 和 J2EE1.4 规范的 JMS Provider 实现的消息中间件。

### ActiveMQ 工作模式

点对点模式：一个消息只有一个消费者消费。

发布/订阅模式：订阅一个主题的消费者只能消费自它订阅之后发布的消息。JMS 规范允许客户创建持久订阅，这在一定程度上放松了时间上的相关性要求。持久订阅允许消费者消费它在未处于激活状态时发送的消息。

### MQ 对比



![img](https://pic4.zhimg.com/80/v2-973fe9aafbd917b0ab6f16917ef365b7_hd.jpg)



## Dubbo

### Dubbo 简介

Dubbo 是一个分布式服务框架，致力于提供高性能和透明化的 RPC 远程服务调用方案，以及 SOA 服务治理方案。

其核心部分包括：

远程通讯：提供对多种基于长连接的 NIO 框架抽象封装，包括多种线程模型、序列化、"请求-响应"模式的信息交换方案；

集群容错：提供基于借口方法的透明远程过程调用，包括多协议支持、软负载均衡、失败容错、地址路由、动态配置等集群支持；

自动发现：基于注册中心目录服务，使服务消费方能动态地查找服务提供方，使地址透明，使服务提供方可以平滑增加或减少机器。

### Dubbo 开发流程

第一步：要在系统中使用 dubbo 应该先搭建一个注册中心，一般推荐使用 zookeeper；第二步：有了注册中心然后是发布服务，发布服务需要使用 spring 容器和 dubbo 标签来发布服务。并且发布服务时需要指定注册中心的位置；

第三步：服务发布之后就是调用服务。一般调用服务也是使用 spring 容器和 dubbo 标签来引用服务，这样就可以在客户端的容器中生成一个服务的代理对象，在 action 或者 Controller 中直接调用 service 的方法即可。

Zookeeper 注册中心的作用主要就是注册和发现服务的作用。类似于房产中介的作用，在系统中并不参与服务的调用及数据的传输。

## FastDFS

### FastDFS 简介

FastDFS 是一个开源的高性能分布式文件系统（DFS）。 它的主要功能包括：文件存储，文件同步和文件访问，以及高容量和负载平衡。主要解决了海量数据存储问题，特别适合以中小文件（建议范围：4KB < file_size <500MB）为载体的在线服务。

FastDFS 系统有三个角色：跟踪服务器(Tracker Server)、存储服务器(Storage Server)和客户端(Client)。

Tracker Server：跟踪服务器，主要做调度工作，起到均衡的作用；负责管理所有的 storage server 和 group，每个 storage 在启动后会连接 Tracker，告知自己所属 group 等信息，并保持周期性心跳。

Storage Server：存储服务器，主要提供容量和备份服务；以 group 为单位，每个 group 内

可以有多台 storage server，数据互为备份。

Client：客户端，上传下载数据的服务器，也就是我们自己的项目所部署在的服务器。

![img](https://pic1.zhimg.com/80/v2-26a203580040734f8dfe3c99a1dcfff8_hd.jpg)



## Nginx

### Nginx 简介

Nginx 是一款高性能的 http 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器。由俄罗斯的程序设计师 Igor Sysoev 所开发，官方测试 nginx 能够支支撑 5 万并发链接，并且 cpu、内存等资源消耗却非常低，运行非常稳定。

### Nginx 功能

静态 HTTP 服务器：Nginx 是一个 HTTP 服务器，可以将服务器上的静态文件（如 HTML、图片）通过 HTTP 协议展现给客户端。

反向代理服务器：客户端本来可以直接通过 HTTP 协议访问某网站应用服务器，但如果单台服务器承受不住压力需要使用多台服务器共同处理请求，这时可以在中间加上一个 Nginx，客户端请求 Nginx，Nginx 请求应用服务器，然后将结果返回给客户端，此时 Nginx 就是反向代理服务器。

负载均衡：当客户端访问量很大，通过反向代理的方式，使用轮询、加权轮询和 IP Hash 的策略将请求分配给多台服务器。

## Quartz

### Quartz 简介

Quartz 是一个任务调度框架。它具有以下特点：

强大的调度功能，例如支持丰富多样的调度方法，可以满足各种常规及特殊需求；

灵活的应用方式，例如支持任务和调度的多种组合方式，支持调度数据的多种存储方式；分布式和集群能力，Terracotta 收购后在原来功能基础上作了进一步提升；

作为 Spring 默认的调度框架，Quartz 很容易与 Spring 集成实现灵活可配置的调度功能。

### Quartz 核心元素

Scheduler：任务调度器，实际执行任务调度的控制器。在 spring 中通过 SchedulerFactoryBean 封装起来；

Trigger ：触发器，用于定义任务调度的时间规则，有 SimpleTrigger 、 CronTrigger 、

DateIntervalTrigger 和 NthIncludedDayTrigger，其中 CronTrigger 用的比较多，在 spring 中

封装在 CronTriggerFactoryBean 中；

Calendar：一些日历特定时间点的集合。一个 trigger 可以包含多个 Calendar，以便排除或

包含某些时间点；

JobDetail：用来描述 Job 实现类及其它相关的静态信息。如 Job 名字、关联监听器等信息。在 spring 中有 JobDetailFactoryBean 和 MethodInvokingJobDetailFactoryBean 两种实现，如果任务调度只需要执行某个类的某个方法，可以通过 MethodInvokingJobDetailFactoryBean 来调用；

Job：是一个接口，只有一个方法 void execute(JobExecutionContext context)，开发者实现该接口定义运行任务，JobExecutionContext 类提供了调度上下文的各种信息。Job 运行时的信息保存在 JobDataMap 实例中。实现 Job 接口的任务，默认是无状态的，若要将 Job 设置成有状态的，在 quartz 中是给实现的 Job 添加@DisallowConcurrentExecution 注解，在与 spring 结合中可以在 spring 配置文件的 job detail 中配置 concurrent 参数。

------

## 数据库笔试题

### 用一条 SQL 语句查询出每门课都大于 80 分的学生姓名



![img](https://pic4.zhimg.com/80/v2-816a306879e73c10bf6a7922c9df11f7_hd.jpg)

```text
# 准备数据的 sql 代码：

create	table	score(id	int	primary	key	auto_increment,namevarchar(20),subject

varchar(20),score int);

insert into score values

(null,'张三','语文',81),

(null,'张三','数学',75),

(null,'李四','语文',76),

(null,'李四','数学',90),

(null,'王五','语文',81),

(null,'王五','数学',100),

(null,'王五 ','英语',90);

#答案：

#A：select distinct name from score where name not in (select distinct name from score where score<=80)

#B：select distinct name t1 from score where 80< all (select score from score where name=t1)
```

### 所有球队之间的比赛组合

一张叫 team 的表，里面只有一个字段 name，一共有 4 条纪录，分别是 a、b、c、d，对应四个球队，现在四个球队进行比赛，用一条 sql 语句显示所有可能的比赛组合。

```text
#答案：select a.name，b.name	from team a, team b where a.name < b.name
```

### 显示文章标题，发帖人、最后回复时间

表：id，title，postuser，postdate，parentid

准备 sql 语句：

```text
drop table if exists articles;
create table articles(id int auto_increment primary key,titlevarchar(50), postuser varchar(10), postdate datetime,parentid int referencesarticles(id)); insert into articles values

(null,'第一条','张三','1998-10-10 12:32:32',null), (null,'第二条','张三','1998-10-10 12:34:32',null), (null,'第一条回复 1','李四','1998-10-10 12:35:32',1), (null,'第二条回复 1','李四','1998-10-10 12:36:32',2), (null,'第一条回复 2','王五','1998-10-10 12:37:32',1), (null,'第一条回复 3','李四','1998-10-10 12:38:32',1), (null,'第二条回复 2','李四','1998-10-10 12:39:32',2), (null,'第一条回复 4','王五','1998-10-10 12:39:40',1);

#答案：

select a.title，a.postuser， (select max(postdate) from articles where parentid=a.id ) reply from articles a where a.parentid is null;

#注释：子查询可以用在选择列中，也可用于 where 的比较条件中，还可以用于 from 从句中。
```

### 航空网的几个航班查询题

![img](https://pic4.zhimg.com/80/v2-36368acaeef3cfb0cf030c72903e926f_hd.jpg)



### 1、查询起飞城市是北京的所有航班，按到达城市的名字排序

参与运算的列是我起码能够显示出来的那些列，但最终我不一定把它们显示出来。各个表组合出来的中间结果字段中必须包含所有运算的字段。

```sql
select * from flight f,city c

where f.endcityid =c.cityid and startcityid =

(select c1.cityidfrom city c1 where c1.cityname = "北京")

order by c.citynameasc;

select flight.flightid,'北京' startcity, e.cityname from flight,city e

where flight.endcityid=e.cityid and flight.startcityid=( selectcityid from city where cityname='北京');

select flight.flightid,s.cityname,e.cityname from flight,city s,city e 

where flight.startcityid=s.cityid and s.cityname='北京' andflight.endCityId=e.cit yID order by e.cityName desc;
```

2、查询北京到上海的所有航班纪录（起飞城市，到达城市，起飞时间，航班号）

```sql
select c1.CityName,c2.CityName,f.StartTime,f.flightID

from city c1,city c2,flight f

where f.StartCityID=c1.cityID

and f.endCityID=c2.cityID

and c1.cityName='北京'

and c2.cityName='上海'
```

3、查询具体某一天（2005-5-8）的北京到上海的的航班次数

```sql
select count(*) from

(select c1.CityName,c2.CityName,f.StartTime,f.flightID

from city c1,city c2,flight f

where f.StartCityID=c1.cityID

and f.endCityID=c2.cityID

and c1.cityName='北京'

and c2.cityName='上海'

and 查帮助获得的某个日期处理函数(startTime) like '2005-5-8%' mysql 中提取日期部分进行比较的示例代码如下：

select * from flight wheredate_format(starttime,'%Y-%m-%d')='1998-01-02'
```



## **1、什么是Servlet？**

可以从两个方面去看Servlet：

a、API：有一个接口servlet，它是servlet规范中定义的用来处理客户端请求的程序需要实现的顶级接口。

b、组件：服务器端用来处理客户端请求的组件，需要在web.xml文件中进行配置。

## **2、Servlet的生命周期，并说出Servlet和CGI的区别？**

servlet有良好的生存期的定义，包括加载和实例化、初始化、处理请求以及服务结束。

这个生存期由javax.servlet.servlet接口中的init、service、destroy方法表达。

web容器加载servlet，生命周期开始。通过调用servlet的init()方法进行servlet的初始化。通过调用service()方法实现根据请求的不同调用不同的do**()方法。结束服务，web容器调用servlet的destroy()方法。

![img](https://pic3.zhimg.com/80/v2-78c62738d745587d82ca22982f3a957a_hd.jpg)

**区别：**

Servlet处于服务器进程中，它通过多线程运行service()方法，一个实例可以服务于多个请求，并且实例一般不会被销毁；而CGI对每个请求都产生一个新的进程，服务完成后就销毁，所有效率低于Servlet

## **3、Servlet接口中有哪些方法？**

Servlet接口定义了5个方法：

1. void init(ServletConfig config) throws ServletException
2. void service(ServletRequest req, ServletResponse resp) throws ServletException, java.io.IOException
3. void destory()
4. java.lang.String getServletInfo()
5. ServletConfig getServletConfig()

## **4、get和post请求的区别？**

a、get是用来从服务器上获取数据，而post是用来向服务器传递数据；

b、get将表单中数据按照variable=value的形式，添加到action所指向的URL后面，并且两者用"？"连接，变量之间用"&"连接；而post是将表单中的数据放在form的数据体中，按照变量与值对应的方式，传递到action所指定的URL。

c、get是不安全的，因为在传输过程中，数据是被放在请求的URL中;而post的所有操作对用户来说都是不可见的。

d、get传输的数据量小，这主要应为受url长度限制;而post可以传输大量的数据，所有上传文件只能用post提交。

e、get限制form表单的数据集必须为ASCII字符；而post支持整个IS01 0646字符集。

f、get是form表单的默认方法。

## **5、什么情况下调用doGet()和doPost()？**

默认情况是调用doGet()方法，JSP页面中的Form表单的method属性设置为post的时候，调用的为doPost()方法；为get的时候，调用deGet()方法。

## **6、转发（Forward）和重定向（Redirect）的区别？**

转发是服务器行为，重定向是客户端行为。

转发（Forword）
通过RequestDispatcher对象的forward（HttpServletRequest request,HttpServletResponse response）方法实现的。RequestDispatcher可以通过HttpServletRequest 的getRequestDispatcher()方法获得。例如下面的代码就是跳转到login_success.jsp页面。

```java
request.getRequestDispatcher("login_success.jsp").forward(request, response);
```

重定向（Redirect） 是利用服务器返回的状态吗来实现的。客户端浏览器请求服务器的时候，服务器会返回一个状态码。服务器通过HttpServletRequestResponse的setStatus(int status)方法设置状态码。如果服务器返回301或者302，则浏览器会到新的网址重新请求该资源。

\1. 从地址栏显示来说

forward是服务器请求资源，服务器直接访问目标地址的URL，把那个URL的响应内容读取过来，然后把这些内容再发给浏览器。浏览器根本不知道服务器发送的内容从哪里来的，所以它的地址栏还是原来的地址。
redirect是服务端根据逻辑，发送一个状态码,告诉浏览器重新去请求那个地址，所以地址栏显示的是新的URL。

\2. 从数据共享来说

forward：转发页面和转发到的页面可以共享request里面的数据.
redirect：不能共享数据.

\3. 从运用地方来说

forward：一般用于用户登陆的时候,根据角色转发到相应的模块.
redirect：一般用于用户注销登陆时返回主页面和跳转到其它的网站等

\4. 从效率来说

forward：高.
redirect：低.

## **7、自动刷新（Refresh）**

自动刷新不仅可以实现一段时间之后自动跳转到另一个页面，还可以实现一段时间之后自动刷新本页面。Servlet中通过HttpServletResponse对象设置Header属性实现自动刷新例如：

```java
Response.setHeader("Refresh","1000;URL=http://localhost:8080/servlet/example.htm");
```

其中1000为时间，单位为毫秒。URL指定就是要跳转的页面（如果设置自己的路径，就会实现没过一秒自动刷新本页面一次）

## **8、Servlet与线程安全**

Servlet不是线程安全的，多线程并发的读写会导致数据不同步的问题。 解决的办法是尽量不要定义name属性，而是要把name变量分别定义在doGet()和doPost()方法内。虽然使用synchronized(name){}语句块可以解决问题，但是会造成线程的等待，不是很科学的办法。
注意：多线程的并发的读写Servlet类属性会导致数据不同步。但是如果只是并发地读取属性而不写入，则不存在数据不同步的问题。因此Servlet里的只读属性最好定义为final类型的。

## **9、JSP和Servlet有哪些相同点和不同点，他们之间的联系是什么？**

JSP是Servlet的扩展，本质上是Servlet的简易方式，更强调应用的外表表达。JSP编译后是"类Servlet"。Servlet和JSP最主要的不同点在于，Servlet的应用逻辑是在java文件中，并且完全从表示层中的HTML里分离开来。而JSP的情况是java和HTML可以组合成一个扩展名为.jsp的文件。JSP偏重于视图，Servlet偏重于业务逻辑。

## **10、JSP工作原理：**

JSP是一种Servlet，但是与HttpServlet的工作方式不太一样。HttpServlet是先由源代码编译为class文件后部署到服务器下，为先编译后部署。而JSP则是先部署后编译。JSP会在客户端第一次请求JSP文件时被编译为HttpJspPage类（接口Servlet的一个子类）。该类会被服务器临时存放在服务器工作目录里面。下面通过实例给大家介绍。


工程JspLoginDemo下有一个名为login.jsp的Jsp文件，把工程第一次部署到服务器上后访问这个Jsp文件，我们发现这个目录下多了下图这两个东东。
.class文件便是JSP对应的Servlet。编译完毕后再运行class文件来响应客户端请求。以后客户端访问login.jsp的时候，Tomcat将不再重新编译JSP文件，而是直接调用class文件来响应客户端请求。

![img](https://pic1.zhimg.com/80/v2-fdeb8f39d09113a9b18c8cd010c47ef8_hd.jpg)

由于JSP只会在客户端第一次请求的时候被编译 ，因此第一次请求JSP时会感觉比较慢，之后就会感觉快很多。如果把服务器保存的class文件删除，服务器也会重新编译JSP。

开发Web程序时经常需要修改JSP。Tomcat能够自动检测到JSP程序的改动。如果检测到JSP源代码发生了改动。Tomcat会在下次客户端请求JSP时重新编译JSP，而不需要重启Tomcat。这种自动检测功能是默认开启的，检测改动会消耗少量的时间，在部署Web应用的时候可以在web.xml中将它关掉。

## **11、Request对象的主要方法有哪些？**

- setAttribute(String name,Object)：设置名字为name的request 的参数值
- getAttribute(String name)：返回由name指定的属性值
- getAttributeNames()：返回request 对象所有属性的名字集合，结果是一个枚举的实例
- getCookies()：返回客户端的所有 Cookie 对象，结果是一个Cookie 数组
- getCharacterEncoding() ：返回请求中的字符编码方式 = getContentLength() ：返回请求的 Body的长度
- getHeader(String name) ：获得HTTP协议定义的文件头信息
- getHeaders(String name) ：返回指定名字的request Header 的所有值，结果是一个枚举的实例
- getHeaderNames() ：返回所以request Header 的名字，结果是一个枚举的实例
- getInputStream() ：返回请求的输入流，用于获得请求中的数据
- getMethod() ：获得客户端向服务器端传送数据的方法
- getParameter(String name) ：获得客户端传送给服务器端的有 name指定的参数值
- getParameterNames() ：获得客户端传送给服务器端的所有参数的名字，结果是一个枚举的实例
- getParameterValues(String name)：获得有name指定的参数的所有值
- getProtocol()：获取客户端向服务器端传送数据所依据的协议名称
- getQueryString() ：获得查询字符串
- getRequestURI() ：获取发出请求字符串的客户端地址
- getRemoteAddr()：获取客户端的 IP 地址
- getRemoteHost() ：获取客户端的名字
- getSession([Boolean create]) ：返回和请求相关 Session
- getServerName() ：获取服务器的名字
- getServletPath()：获取客户端所请求的脚本文件的路径
- getServerPort()：获取服务器的端口号
- removeAttribute(String name)：删除请求中的一个属性

## **12、request.getAttribute()和 request.getParameter()有何区别？**

a、request.getParameter()获取的类型是String；request.getAttribute()获取的类型是Object。

b、request.getPrameter()获取的是POST/GET传递的参数值和URL中的参数；request.getAttribute()获取的是对象容器中的数据值/对象。

c、request.setAttribute()和request.getAttribute()可以发送、接收对象；request.getParamter()只能接收字符串，官方不开放request.setParamter()（也就是没有这个方法）。

setAttribute()和getAttribute()的传参原理：

setAttribute()是应用服务器把这个对象放在该页面所对应的一块内存中去，当你的页面服务器重定向到另外一个页面时，应用服务器会把这块内存拷贝到另一个页面所对应的那块内存中。这个就可以通过getAttribute()获取到相应的参数值或者对象。

## **13、JSP中动态include和静态include的区别？**

a、静态include：语法：<%@ include file="文件名" %>，相当于复制，编辑时将对应的文件包含进来，当内容变化时，不会再一次对其编译，不易维护。

b、动态include：语法：<jsp:include page="文件名">,能够自动检查被包含文件，当客户端对JSP文件进行请求时，会重新将对应的文件包含进来，进行实时的更新。

## **14、JSP有哪些内置对象？作用分别是什么？**

JSP一共有9个内置对象：

1. request：负责得到客户端请求的信息，对应类型：javax.servlet.http.HttpServletRequest
2. response:负责向客户端发出响应，对应类型：javax.servlet.http.HttpServletResponse
3. session:负责保存同一客户端一次会话过程中的一些信息，对应类型：javax.servlet.http.httpsession
4. out：负责管理对客户端的输出，对应类型：javax.serlvet.jsp.jspwriter
5. application:表示整个应用环境的信息，对应类型：javax.servlet.servletcontext
6. config:表示ServletConfig，对应类型：javax.servlet.servletconfig
7. exception:表示页面中发生的异常，可以通过它获得页面异常信息，对应类型：java.lang.exception
8. pagecontext:表示这个JSP页面上下文，对应类型：javax.servlet.jsp.pagecontext
9. page:表示当前JSP页面本身。

## **15、JSP有哪些动作？作用分别是什么？**

JSP一共有以下6中基本动作：

1. JSP：include （当页面被请求的时候引入一个文件）
2. JSP：forward （将请求转到另一个页面）
3. JSP：useBean （获得JavaBean的一个实例）
4. JSP：setProperty （设置JavaBean的属性）
5. JSP：getProperty （获得JavaBean的属性）
6. JSP：plugin （根据浏览器类型为Java插件生成object或者embed两种标记）

## **16、JSP常用指令有哪些？**

**page、include、taglib**

a、page指令：定义页面的一些属性。

常用属性：

contentType="text/html;charset=utf-8"; 向浏览器端输出数据的编码

pageEncoding="utf-8"; JSP编译成java文件时所用的编码

session="true" 是否自动创建session

b、include指令：引入一个静态的JSP页面

c、taglib指令：引入一个标签库

## **17、讲解JSP中的四种作用域**

JSP中的四种作用域包括page、request、session和application，具体来说：

a、page是代表一个页面相关的对象和属性。一个页面由一个编译好的java servlet类（可以带有include指令，但不可以带有include动作）表示。这既包括servlet又包括编译成servlet的jsp页面。

b、request是代表与web客户机发出的一个请求相关的对象和属性。一个请求可能跨越多个页面，涉及多个web组件（由于forware指令和include动作的关系）

c、session是代表与用于某个web客户机的一个用户体验相关的对象和属性。一个web回话也可以经常跨域多个客户机请求。

d、application是代表与整个web应用程序相关的对象和属性。这实质上是跨域整个web应用程序，包括多个页面、请求和回话的一个全局作用域。

## **18、如何实现JSP或Servlet的单线程模式？**

在JSP中使用page指令进行设置，具体代码如下：

```java
<%@page isThreadSafe="false"%>
```

对于Servlet，可以让自定义的Servlet实现SingleThreadModel标识接口。

说明：如果将JSP或Servlet设置成单线程工作模式，会导致每个请求创建一个Servlet实例，这种实践将导致严重的性能问题（服务器的内存压力很大，还会导致频繁的垃圾回收），所以通常情况下并不会这么做。

## **19、JSP乱码如何解决？**

a、JSP页面乱码：<%@page contentType="text/html;charset=utf-8" %>

b、表单提交时出现乱码：request.setCharacterEncoding("utf-8");

c、数据库出现乱码：jdbc:mysql://localhost:3306:/user?useSSL=false&useUnicode=true&characterEncoding=utf-8;

其实我一般的处理的方法就是配置一个过滤器对每个JSP页面进行字符集处理。

## **20、实现会话跟踪的技术有哪些？**

**1. 使用Cookie**

向客户端发送Cookie

```java
Cookie c =new Cookie("name","value"); //创建Cookie 
c.setMaxAge(60*60*24); //设置最大时效，此处设置的最大时效为一天
response.addCookie(c); //把Cookie放入到HTTP响应中
```

从客户端读取Cookie

```java
String name ="name"; 
Cookie[]cookies =request.getCookies(); 
if(cookies !=null){ 
  for(int i= 0;i<cookies.length;i++){ 
   Cookie cookie =cookies[i]; 
   if(name.equals(cookis.getName())) 
   //something is here. 
   //you can get the value 
   cookie.getValue(); 
      
  }
}
```

优点: 数据可以持久保存，不需要服务器资源，简单，基于文本的Key-Value

缺点: 大小受到限制，用户可以禁用Cookie功能，由于保存在本地，有一定的安全风险。

**2. URL 重写**

在URL中添加用户会话的信息作为请求的参数，或者将唯一的会话ID添加到URL结尾以标识一个会话。

优点： 在Cookie被禁用的时候依然可以使用

缺点： 必须对网站的URL进行编码，所有页面必须动态生成，不能用预先记录下来的URL进行访问。

**3. 隐藏的表单域**

```java
<input type="hidden" name ="session" value="..."/>
```

优点： Cookie被禁时可以使用

缺点： 所有页面必须是表单提交之后的结果。

**4. HttpSession**

在所有会话跟踪技术中，HttpSession对象是最强大也是功能最多的。当一个用户第一次访问某个网站时会自动创建 HttpSession，每个用户可以访问他自己的HttpSession。可以通过HttpServletRequest对象的getSession方 法获得HttpSession，通过HttpSession的setAttribute方法可以将一个值放在HttpSession中，通过调用 HttpSession对象的getAttribute方法，同时传入属性名就可以获取保存在HttpSession中的对象。与上面三种方式不同的 是，HttpSession放在服务器的内存中，因此不要将过大的对象放在里面，即使目前的Servlet容器可以在内存将满时将HttpSession 中的对象移到其他存储设备中，但是这样势必影响性能。添加到HttpSession中的值可以是任意Java对象，这个对象最好实现了 Serializable接口，这样Servlet容器在必要的时候可以将其序列化到文件中，否则在序列化时就会出现异常。

## **21、Cookie和Session的的区别？**

1. 由于HTTP协议是无状态的协议，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户，这个机制就是Session.典型的场景比如购物车，当你点击下单按钮时，由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用用于标识这个用户，并且跟踪用户，这样才知道购物车里面有几本书。这个Session是保存在服务端的，有一个唯一标识。在服务端保存Session的方法很多，内存、数据库、文件都有。集群的时候也要考虑Session的转移，在大型的网站，一般会有专门的Session服务器集群，用来保存用户会话，这个时候 Session 信息都是放在内存的，使用一些缓存服务比如Memcached之类的来放 Session。
2. 思考一下服务端如何识别特定的客户？这个时候Cookie就登场了。每次HTTP请求的时候，客户端都会发送相应的Cookie信息到服务端。实际上大多数的应用都是用 Cookie 来实现Session跟踪的，第一次创建Session的时候，服务端会在HTTP协议中告诉客户端，需要在 Cookie 里面记录一个Session ID，以后每次请求把这个会话ID发送到服务器，我就知道你是谁了。有人问，如果客户端的浏览器禁用了 Cookie 怎么办？一般这种情况下，会使用一种叫做URL重写的技术来进行会话跟踪，即每次HTTP交互，URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户。
3. Cookie其实还可以用在一些方便用户的场景下，设想你某次登陆过一个网站，下次登录的时候不想再次输入账号了，怎么办？这个信息可以写到Cookie里面，访问网站的时候，网站页面的脚本可以读取这个信息，就自动帮你把用户名给填了，能够方便一下用户。这也是Cookie名称的由来，给用户的一点甜头。所以，总结一下：Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式。

## **22、什么是Tomcat？**

Tomcat是一种web服务器，java编写的web项目可以部署在上面，用户在客户端请求时，都是将请求发到Tomcat上，Tomcat在将请求发到对应的项目上。

## **23、 详细描述MVC**

基于java的web应用系统采用MVC设计模型，即用Model（模型）、View（视图）和Controller（控制）分离设计，这是目前web应用服务系统的主流设置方向。

Model：处理业务逻辑的模块。

View：负责页面显示，显示Model的处理结果给用户，主要实现数据到页面的转换过程。

Controller：负责每个请求的分发，把Form数据传递给Model进行处理，处理完成后，把处理结果返回给相应的View显示给用户。

## 目录

- Spring 概述
- 依赖注入
- Spring beans
- Spring注解
- Spring数据访问
- Spring面向切面编程（AOP）
- Spring MVC

## **Spring 概述**

### **1. 什么是spring?**

Spring 是个java企业级应用的开源开发框架。Spring主要用来开发Java应用，但是有些扩展是针对构建J2EE平台的web应用。Spring 框架目标是简化Java企业级应用开发，并通过POJO为基础的编程模型促进良好的编程习惯。



### **2. 使用Spring框架的好处是什么？**

- **轻量：**Spring 是轻量的，基本的版本大约2MB。
- **控制反转：**Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
- **面向切面的编程(AOP)：**Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
- **容器：**Spring 包含并管理应用中对象的生命周期和配置。
- **MVC框架**：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
- **事务管理：**Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。
- **异常处理：**Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。

### **3.  Spring由哪些模块组成****?**

以下是Spring 框架的基本模块：

- Core module
- Bean module
- Context module
- Expression Language module
- JDBC module
- ORM module
- OXM module
- Java Messaging Service(JMS) module
- Transaction module
- Web module
- Web-Servlet module
- Web-Struts module
- Web-Portlet module

### **4. 核心容器（应用上下文) 模块****。**

这是基本的Spring模块，提供spring 框架的基础功能，BeanFactory 是 任何以spring为基础的应用的核心。Spring 框架建立在此模块之上，它使Spring成为一个容器。

### **5. BeanFactory – BeanFactory 实现举例。**

Bean 工厂是工厂模式的一个实现，提供了控制反转功能，用来把应用的配置和依赖从正真的应用代码中分离。

最常用的BeanFactory 实现是XmlBeanFactory 类。

### **6. XMLBeanFactory** 

最常用的就是org.springframework.beans.factory.xml.XmlBeanFactory ，它根据XML文件中的定义加载beans。该容器从XML 文件读取配置元数据并用它去创建一个完全配置的系统或应用。

### **7. 解释AOP模块**

AOP模块用于发给我们的Spring应用做面向切面的开发， 很多支持由AOP联盟提供，这样就确保了Spring和其他AOP框架的共通性。这个模块将元数据编程引入Spring。

### **8. 解释JDBC抽象和DAO模块****。**

通过使用JDBC抽象和DAO模块，保证数据库代码的简洁，并能避免数据库资源错误关闭导致的问题，它在各种不同的数据库的错误信息之上，提供了一个统一的异常访问层。它还利用Spring的AOP 模块给Spring应用中的对象提供事务管理服务。

### **9. 解释对象/关系映射集成模块****。**

Spring 通过提供ORM模块，支持我们在直接JDBC之上使用一个对象/关系映射映射(ORM)工具，Spring 支持集成主流的ORM框架，如Hiberate,JDO和 iBATIS SQL Maps。Spring的事务管理同样支持以上所有ORM框架及JDBC。

### **10.  解释WEB 模块****。**

Spring的WEB模块是构建在application context 模块基础之上，提供一个适合web应用的上下文。这个模块也包括支持多种面向web的任务，如透明地处理多个文件上传请求和程序级请求参数的绑定到你的业务对象。它也有对Jakarta Struts的支持。

### **12.  Spring配置文件**

Spring配置文件是个XML 文件，这个文件包含了类信息，描述了如何配置它们，以及如何相互调用。

### **13.  什么是Spring IOC 容器？**

Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

### **14.  IOC的优点是什么？**

IOC 或 依赖注入把应用的代码量降到最低。它使应用容易测试，单元测试不再需要单例和JNDI查找机制。最小的代价和最小的侵入性使松散耦合得以实现。IOC容器支持加载服务时的饿汉式初始化和懒加载。

### **15. ApplicationContext通常的实现是什么?**

- **FileSystemXmlApplicationContext ：**此容器从一个XML文件中加载beans的定义，XML Bean 配置文件的全路径名必须提供给它的构造函数。
- **ClassPathXmlApplicationContext：**此容器也从一个XML文件中加载beans的定义，这里，你需要正确设置classpath因为这个容器将在classpath里找bean配置。
- **WebXmlApplicationContext：**此容器加载一个XML文件，此文件定义了一个WEB应用的所有bean。

### **16. Bean 工厂和 Application contexts  有什么区别？**

Application contexts提供一种方法处理文本消息，一个通常的做法是加载文件资源（比如镜像），它们可以向注册为监听器的bean发布事件。另外，在容器或容器内的对象上执行的那些不得不由bean工厂以程序化方式处理的操作，可以在Application contexts中以声明的方式处理。Application contexts实现了MessageSource接口，该接口的实现以可插拔的方式提供获取本地化消息的方法。

### **17. 一个Spring的应用看起来象什么？**

- 一个定义了一些功能的接口。
- 这实现包括属性，它的Setter ， getter 方法和函数等。
- Spring AOP。
- Spring 的XML 配置文件。
- 使用以上功能的客户端程序。

## **依赖注入**

### **18. 什么是Spring的依赖注入****？**

依赖注入，是IOC的一个方面，是个通常的概念，它有多种解释。这概念是说你不用创建对象，而只需要描述它如何被创建。你不在代码里直接组装你的组件和服务，但是要在配置文件里描述哪些组件需要哪些服务，之后一个容器（IOC容器）负责把他们组装起来。

### **19.  有哪些不同类型的IOC（依赖注入）方式？**

- **构造器依赖注入：**构造器依赖注入通过容器触发一个类的构造器来实现的，该类有一系列参数，每个参数代表一个对其他类的依赖。
- **Setter方法注入：**Setter方法注入是容器通过调用无参构造器或无参static工厂 方法实例化bean之后，调用该bean的setter方法，即实现了基于setter的依赖注入。

### **20. 哪种依赖注入方式你建议使用，构造器注入，还是 Setter方法注入？**

你两种依赖方式都可以使用，构造器注入和Setter方法注入。最好的解决方案是用构造器参数实现强制依赖，setter方法实现可选依赖。

## **Spring Beans**

### **21.什么是Spring beans?**

Spring beans 是那些形成Spring应用的主干的java对象。它们被Spring IOC容器初始化，装配，和管理。这些beans通过容器中配置的元数据创建。比如，以XML文件中<bean/> 的形式定义。

Spring 框架定义的beans都是单件beans。在bean tag中有个属性”singleton”，如果它被赋为TRUE，bean 就是单件，否则就是一个 prototype bean。默认是TRUE，所以所有在Spring框架中的beans 缺省都是单件。

### **22. 一个 Spring Bean 定义 包含什么？**

一个Spring Bean 的定义包含容器必知的所有配置元数据，包括如何创建一个bean，它的生命周期详情及它的依赖。

### **23. 如何给Spring 容器提供配置元数据?**

这里有三种重要的方法给Spring 容器提供配置元数据。

XML配置文件。

基于注解的配置。

基于java的配置。

### **24. 你怎样定义类的作用域?** 

当定义一个<bean> 在Spring里，我们还能给这个bean声明一个作用域。它可以通过bean 定义中的scope属性来定义。如，当Spring要在需要的时候每次生产一个新的bean实例，bean的scope属性被指定为prototype。另一方面，一个bean每次使用的时候必须返回同一个实例，这个bean的scope 属性 必须设为 singleton。

### **25. 解释Spring支持的几种bean的作用域。**

Spring框架支持以下五种bean的作用域：

- **singleton :** bean在每个Spring ioc 容器中只有一个实例。
- **prototype**：一个bean的定义可以有多个实例。
- **request**：每次http请求都会创建一个bean，该作用域仅在基于web的Spring ApplicationContext情形下有效。
- **session**：在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。
- **global-session**：在一个全局的HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。

缺省的Spring bean 的作用域是Singleton.

### **26. Spring框架中的单例bean是线程安全的吗?**

不，Spring框架中的单例bean不是线程安全的。

### **27. 解释Spring框架中bean的生命周期。**

- Spring容器 从XML 文件中读取bean的定义，并实例化bean。
- Spring根据bean的定义填充所有的属性。
- 如果bean实现了BeanNameAware 接口，Spring 传递bean 的ID 到 setBeanName方法。
- 如果Bean 实现了 BeanFactoryAware 接口， Spring传递beanfactory 给setBeanFactory 方法。
- 如果有任何与bean相关联的BeanPostProcessors，Spring会在postProcesserBeforeInitialization()方法内调用它们。
- 如果bean实现IntializingBean了，调用它的afterPropertySet方法，如果bean声明了初始化方法，调用此初始化方法。
- 如果有BeanPostProcessors 和bean 关联，这些bean的postProcessAfterInitialization() 方法将被调用。
- 如果bean实现了 DisposableBean，它将调用destroy()方法。

### **28.  哪些是重要的bean生命周期方法？ 你能重载它们吗？**

有两个重要的bean 生命周期方法，第一个是setup ， 它是在容器加载bean的时候被调用。第二个方法是 teardown  它是在容器卸载类的时候被调用。

The bean 标签有两个重要的属性（init-method和destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct和@PreDestroy）。

### **29. 什么是Spring的内部bean？**

当一个bean仅被用作另一个bean的属性时，它能被声明为一个内部bean，为了定义inner bean，在Spring 的 基于XML的 配置元数据中，可以在 <property/>或 <constructor-arg/> 元素内使用<bean/> 元素，内部bean通常是匿名的，它们的Scope一般是prototype。

### **30. 在 Spring中如何注入一个java集合？**

Spring提供以下几种集合的配置元素：

- <list>类型用于注入一列值，允许有相同的值。
- <set> 类型用于注入一组值，不允许有相同的值。
- <map> 类型用于注入一组键值对，键和值都可以为任意类型。
- <props>类型用于注入一组键值对，键和值都只能为String类型。

### **31. 什么是bean装配?** 

装配，或bean 装配是指在Spring 容器中把bean组装到一起，前提是容器需要知道bean的依赖关系，如何通过依赖注入来把它们装配到一起。

### **32. 什么是bean的自动装配？**

Spring 容器能够自动装配相互合作的bean，这意味着容器不需要<constructor-arg>和<property>配置，能通过Bean工厂自动处理bean之间的协作。

### **33. 解释不同方式的自动装配 。**

有五种自动装配的方式，可以用来指导Spring容器用自动装配方式来进行依赖注入。

- **no**：默认的方式是不进行自动装配，通过显式设置ref 属性来进行装配。
- **byName：**通过参数名 自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byname，之后容器试图匹配、装配和该bean的属性具有相同名字的bean。
- **byType:：**通过参数类型自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byType，之后容器试图匹配、装配和该bean的属性具有相同类型的bean。如果有多个bean符合条件，则抛出错误。
- **constructor：这个方式类似于**byType， 但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常。
- **autodetect：**首先尝试使用constructor来自动装配，如果无法工作，则使用byType方式。

### **34.****自动装配有哪些局限性 ?**

自动装配的局限性是：

- **重写**： 你仍需用 <constructor-arg>和 <property> 配置来定义依赖，意味着总要重写自动装配。
- **基本数据类型**：你不能自动装配简单的属性，如基本数据类型，String字符串，和类。
- **模糊特性：**自动装配不如显式装配精确，如果有可能，建议使用显式装配。

### **35. 你可以在Spring中注入一个null 和一个空字符串吗？**

**可以。**

## **Spring注解**

### **36. 什么是基于Java的Spring注解配置? 给一些注解的例子.**

基于Java的配置，允许你在少量的Java注解的帮助下，进行你的大部分Spring配置而非通过XML文件。

以@Configuration 注解为例，它用来标记类可以当做一个bean的定义，被Spring IOC容器使用。另一个例子是@Bean注解，它表示此方法将要返回一个对象，作为一个bean注册进Spring应用上下文。

### **37. 什么是基于注解的容器配置?**

相对于XML文件，注解型的配置依赖于通过字节码元数据装配组件，而非尖括号的声明。

开发者通过在相应的类，方法或属性上使用注解的方式，直接组件类中进行配置，而不是使用xml表述bean的装配关系。

### **38. 怎样开启注解装配？**

注解装配在默认情况下是不开启的，为了使用注解装配，我们必须在Spring配置文件中配置 <context:annotation-config/>元素。

### **39. @Required  注解**

这个注解表明bean的属性必须在配置的时候设置，通过一个bean定义的显式的属性值或通过自动装配，若@Required注解的bean属性未被设置，容器将抛出BeanInitializationException。

### **40. @Autowired 注解**

@Autowired 注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required一样，修饰setter方法、构造器、属性或者具有任意名称和/或多个参数的PN方法。

### **41. @Qualifier 注解**

当有多个相同类型的bean却只有一个需要自动装配时，将@Qualifier 注解和@Autowire 注解结合使用以消除这种混淆，指定需要装配的确切的bean。

## **Spring数据访问**

### **42.在Spring框架中如何更有效地使用JDBC?** 

使用SpringJDBC 框架，资源管理和错误处理的代价都会被减轻。所以开发者只需写statements 和 queries从数据存取数据，JDBC也可以在Spring框架提供的模板类的帮助下更有效地被使用，这个模板叫JdbcTemplate （例子见这里[here](http://examples.javacodegeeks.com/enterprise-java/spring/jdbc/spring-jdbctemplate-example/)）

### **43. JdbcTemplate**

JdbcTemplate 类提供了很多便利的方法解决诸如把数据库数据转变成基本数据类型或对象，执行写好的或可调用的数据库操作语句，提供自定义的数据错误处理。

### **44. Spring对DAO的支持**

Spring对数据访问对象（DAO）的支持旨在简化它和数据访问技术如JDBC，Hibernate or JDO 结合使用。这使我们可以方便切换持久层。编码时也不用担心会捕获每种技术特有的异常。

### **45. 使用Spring通过什么方式访问Hibernate?** 

在Spring中有两种方式访问Hibernate：

- 控制反转  Hibernate Template和 Callback。
- 继承 HibernateDAOSupport提供一个AOP 拦截器。

### **46. Spring支持的ORM**

Spring支持以下ORM：

- Hibernate
- iBatis
- JPA (Java Persistence API)
- TopLink
- JDO (Java Data Objects)
- OJB

### **47.如何通过HibernateDaoSupport将Spring和Hibernate结合起来？**

用Spring的 SessionFactory 调用 LocalSessionFactory。集成过程分三步：

- 配置the Hibernate SessionFactory。
- 继承HibernateDaoSupport实现一个DAO。
- 在AOP支持的事务中装配。

### **48. Spring支持的事务管理类型**

Spring支持两种类型的事务管理：

- **编程式事务管理**：这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。
- **声明式事务管理：**这意味着你可以将业务代码和事务管理分离，你只需用注解和XML配置来管理事务。

### **49. Spring框架的事务管理有哪些优点？**

- 它为不同的事务API  如 JTA，JDBC，Hibernate，JPA 和JDO，提供一个不变的编程模式。
- 它为编程式事务管理提供了一套简单的API而不是一些复杂的事务API如
- 它支持声明式事务管理。
- 它和Spring各种数据访问抽象层很好得集成。

### **50. 你更倾向用那种事务管理类型？**

大多数Spring框架的用户选择声明式事务管理，因为它对应用代码的影响最小，因此更符合一个无侵入的轻量级容器的思想。声明式事务管理要优于编程式事务管理，虽然比编程式事务管理（这种方式允许你通过代码控制事务）少了一点灵活性。

## **Spring面向切面编程（AOP）**

### **51.  解释AOP**

面向切面的编程，或AOP， 是一种编程技术，允许程序模块化横向切割关注点，或横切典型的责任划分，如日志和事务管理。

### **52. Aspect 切面**

AOP核心就是切面，它将多个类的通用行为封装成可重用的模块，该模块含有一组API提供横切功能。比如，一个日志模块可以被称作日志的AOP切面。根据需求的不同，一个应用程序可以有若干切面。在Spring AOP中，切面通过带有@Aspect注解的类实现。

### **52. 在Spring AOP 中，关注点和横切关注的区别是什么？**

关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。
横切关注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。

### **54. 连接点**

连接点代表一个应用程序的某个位置，在这个位置我们可以插入一个AOP切面，它实际上是个应用程序执行Spring AOP的位置。

### **55. 通知**

通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过SpringAOP框架触发的代码段。

Spring切面可以应用五种类型的通知：

- **before**：前置通知，在一个方法执行前被调用。
- **after:** 在方法执行之后调用的通知，无论方法执行是否成功。
- **after-returning:** 仅当方法成功完成后执行的通知。
- **after-throwing:** 在方法抛出异常退出时执行的通知。
- **around:** 在方法执行之前和之后调用的通知。

### **56. 切点**

切入点是一个或一组连接点，通知将在这些位置执行。可以通过表达式或匹配的方式指明切入点。

### **57. 什么是引入?** 

引入允许我们在已存在的类中增加新的方法和属性。

### **58. 什么是目标对象?** 

被一个或者多个切面所通知的对象。它通常是一个代理对象。也指被通知（advised）对象。

### **59. 什么是代理?**

代理是通知目标对象后创建的对象。从客户端的角度看，代理对象和目标对象是一样的。

### **60. 有几种不同类型的自动代理？**

BeanNameAutoProxyCreator

DefaultAdvisorAutoProxyCreator

Metadata autoproxying

### **61. 什么是织入。什么是织入应用的不同点？**

织入是将切面和到其他应用类型或对象连接或创建一个被通知对象的过程。

织入可以在编译时，加载时，或运行时完成。

### **62. 解释基于XML Schema方式的切面实现。**

在这种情况下，切面由常规类以及基于XML的配置实现。

### **63. 解释基于注解的切面实现**

在这种情况下(基于@AspectJ的实现)，涉及到的切面声明的风格与带有java5标注的普通java类一致。

## **Spring 的MVC**

### **64. 什么是Spring的MVC框架？**

Spring 配备构建Web 应用的全功能MVC框架。Spring可以很便捷地和其他MVC框架集成，如Struts，Spring 的MVC框架用控制反转把业务对象和控制逻辑清晰地隔离。它也允许以声明的方式把请求参数和业务对象绑定。

### **65. DispatcherServlet**

Spring的MVC框架是围绕DispatcherServlet来设计的，它用来处理所有的HTTP请求和响应。

### **66. WebApplicationContext**

WebApplicationContext 继承了ApplicationContext  并增加了一些WEB应用必备的特有功能，它不同于一般的ApplicationContext ，因为它能处理主题，并找到被关联的servlet。

### **67. 什么是Spring MVC框架的控制器？**

控制器提供一个访问应用程序的行为，此行为通常通过服务接口实现。控制器解析用户输入并将其转换为一个由视图呈现给用户的模型。Spring用一个非常抽象的方式实现了一个控制层，允许用户创建多种用途的控制器。

### **68. @Controller 注解**

该注解表明该类扮演控制器的角色，Spring不需要你继承任何其他控制器基类或引用Servlet API。

### **69. @RequestMapping 注解**

该注解是用来映射一个URL到一个类或一个特定的方处理法上。

1、Spring是什么?

        Spring是一个轻量级的IoC和AOP容器框架。是为Java应用程序提供基础性服务的一套框架，目的是用于简化企业应用程序的开发，它使得开发者只需要关心业务需求。常见的配置方式有三种：基于XML的配置、基于注解的配置、基于Java的配置。

主要由以下几个模块组成：

Spring Core：核心类库，提供IOC服务；

Spring Context：提供框架式的Bean访问方式，以及企业级功能（JNDI、定时任务等）；

Spring AOP：AOP服务；

Spring DAO：对JDBC的抽象，简化了数据访问异常的处理；

Spring ORM：对现有的ORM框架的支持；

Spring Web：提供了基本的面向Web的综合特性，例如多方文件上传；

Spring MVC：提供面向Web应用的Model-View-Controller实现。

 

2、Spring 的优点？

（1）spring属于低侵入式设计，代码的污染极低；

（2）spring的DI机制将对象之间的依赖关系交由框架处理，减低组件的耦合性；

（3）Spring提供了AOP技术，支持将一些通用任务，如安全、事务、日志、权限等进行集中式管理，从而提供更好的复用。

（4）spring对于主流的应用框架提供了集成支持。

 

3、Spring的AOP理解：

OOP面向对象，允许开发者定义纵向的关系，但并适用于定义横向的关系，导致了大量代码的重复，而不利于各个模块的重用。

AOP，一般称为面向切面，作为面向对象的一种补充，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为“切面”（Aspect），减少系统中的重复代码，降低了模块间的耦合度，同时提高了系统的可维护性。可用于权限认证、日志、事务处理。

AOP实现的关键在于 代理模式，AOP代理主要分为静态代理和动态代理。静态代理的代表为AspectJ；动态代理则以Spring AOP为代表。

（1）AspectJ是静态代理的增强，所谓静态代理，就是AOP框架会在编译阶段生成AOP代理类，因此也称为编译时增强，他会在编译阶段将AspectJ(切面)织入到Java字节码中，运行的时候就是增强之后的AOP对象。

（2）Spring AOP使用的动态代理，所谓的动态代理就是说AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。

Spring AOP中的动态代理主要有两种方式，JDK动态代理和CGLIB动态代理：

        ①JDK动态代理只提供接口的代理，不支持类的代理。核心InvocationHandler接口和Proxy类，InvocationHandler 通过invoke()方法反射来调用目标类中的代码，动态地将横切逻辑和业务编织在一起；接着，Proxy利用 InvocationHandler动态创建一个符合某一接口的的实例,  生成目标类的代理对象。
    
        ②如果代理类没有实现 InvocationHandler 接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法并添加增强代码，从而实现AOP。CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。

（3）静态代理与动态代理区别在于生成AOP代理对象的时机不同，相对来说AspectJ的静态代理方式具有更好的性能，但是AspectJ需要特定的编译器进行处理，而Spring AOP则无需特定的编译器处理。

 InvocationHandler 的 invoke(Object  proxy,Method  method,Object[] args)：proxy是最终生成的代理实例;  method 是被代理目标实例的某个具体方法;  args 是被代理目标实例某个方法的具体入参, 在方法反射调用时使用。

 

4、Spring的IoC理解：

（1）IOC就是控制反转，是指创建对象的控制权的转移，以前创建对象的主动权和时机是由自己把控的，而现在这种权力转移到Spring容器中，并由容器根据配置文件去创建实例和管理各个实例之间的依赖关系，对象与对象之间松散耦合，也利于功能的复用。DI依赖注入，和控制反转是同一个概念的不同角度的描述，即 应用程序在运行时依赖IoC容器来动态注入对象需要的外部资源。

（2）最直观的表达就是，IOC让对象的创建不用去new了，可以由spring自动生产，使用java的反射机制，根据配置文件在运行时动态的去创建对象以及管理对象，并调用对象的方法的。

（3）Spring的IOC有三种注入方式 ：构造器注入、setter方法注入、根据注解注入。

IoC让相互协作的组件保持松散的耦合，而AOP编程允许你把遍布于应用各层的功能分离出来形成可重用的功能组件。

 

5、BeanFactory和ApplicationContext有什么区别？

        BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。其中ApplicationContext是BeanFactory的子接口。

（1）BeanFactory：是Spring里面最底层的接口，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，控制bean的生命周期，维护bean之间的依赖关系。ApplicationContext接口作为BeanFactory的派生，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：

①继承MessageSource，因此支持国际化。

②统一的资源文件访问方式。

③提供在监听器中注册bean的事件。

④同时加载多个配置文件。

⑤载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层。

（2）①BeanFactroy采用的是延迟加载形式来注入Bean的，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。这样，我们就不能发现一些存在的Spring的配置问题。如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常。

        ②ApplicationContext，它是在容器启动时，一次性创建了所有的Bean。这样，在容器启动时，我们就可以发现Spring中存在的配置错误，这样有利于检查所依赖属性是否注入。 ApplicationContext启动后预载入所有的单实例Bean，通过预载入单实例bean ,确保当你需要的时候，你就不用等待，因为它们已经创建好了。
    
        ③相对于基本的BeanFactory，ApplicationContext 唯一的不足是占用内存空间。当应用程序配置Bean较多时，程序启动较慢。

（3）BeanFactory通常以编程的方式被创建，ApplicationContext还能以声明的方式创建，如使用ContextLoader。

（4）BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册。

 

6、请解释Spring Bean的生命周期？

 首先说一下Servlet的生命周期：实例化，初始init，接收请求service，销毁destroy；

 Spring上下文中的Bean生命周期也类似，如下：

（1）实例化Bean：

对于BeanFactory容器，当客户向容器请求一个尚未初始化的bean时，或初始化bean的时候需要注入另一个尚未初始化的依赖时，容器就会调用createBean进行实例化。对于ApplicationContext容器，当容器启动结束后，通过获取BeanDefinition对象中的信息，实例化所有的bean。

（2）设置对象属性（依赖注入）：

实例化后的对象被封装在BeanWrapper对象中，紧接着，Spring根据BeanDefinition中的信息 以及 通过BeanWrapper提供的设置属性的接口完成依赖注入。

（3）处理Aware接口：

接着，Spring会检测该对象是否实现了xxxAware接口，并将相关的xxxAware实例注入给Bean：

①如果这个Bean已经实现了BeanNameAware接口，会调用它实现的setBeanName(String beanId)方法，此处传递的就是Spring配置文件中Bean的id值；

②如果这个Bean已经实现了BeanFactoryAware接口，会调用它实现的setBeanFactory()方法，传递的是Spring工厂自身。

③如果这个Bean已经实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入Spring上下文；

（4）BeanPostProcessor：

如果想对Bean进行一些自定义的处理，那么可以让Bean实现了BeanPostProcessor接口，那将会调用postProcessBeforeInitialization(Object obj, String s)方法。由于这个方法是在Bean初始化结束时调用的，所以可以被应用于内存或缓存技术；

（5）InitializingBean 与 init-method：

如果Bean在Spring配置文件中配置了 init-method 属性，则会自动调用其配置的初始化方法。

（6）如果这个Bean实现了BeanPostProcessor接口，将会调用postProcessAfterInitialization(Object obj, String s)方法；

以上几个步骤完成后，Bean就已经被正确创建了，之后就可以使用这个Bean了。

（7）DisposableBean：

当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用其实现的destroy()方法；

（8）destroy-method：

最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。

 

7、 解释Spring支持的几种bean的作用域。

Spring容器中的bean可以分为5个范围：

（1）singleton：默认，每个容器中只有一个bean的实例，单例的模式由BeanFactory自身来维护。

（2）prototype：为每一个bean请求提供一个实例。

（3）request：为每一个网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收。

（4）session：与request范围类似，确保每个session中有一个bean的实例，在session过期后，bean会随之失效。

（5）global-session：全局作用域，global-session和Portlet应用相关。当你的应用部署在Portlet容器中工作时，它包含很多portlet。如果你想要声明让所有的portlet共用全局的存储变量的话，那么这全局变量需要存储在global-session中。全局作用域与Servlet中的session作用域效果相同。

 

8、Spring框架中的单例Beans是线程安全的么？

        Spring框架并没有对单例bean进行任何多线程的封装处理。关于单例bean的线程安全和并发问题需要开发者自行去搞定。但实际上，大部分的Spring bean并没有可变的状态(比如Serview类和DAO类)，所以在某种程度上说Spring的单例bean是线程安全的。如果你的bean有多种状态的话（比如 View Model 对象），就需要自行保证线程安全。最浅显的解决办法就是将多态bean的作用域由“singleton”变更为“prototype”。

9、Spring如何处理线程并发问题？

在一般情况下，只有无状态的Bean才可以在多线程环境下共享，在Spring中，绝大部分Bean都可以声明为singleton作用域，因为Spring对一些Bean中非线程安全状态采用ThreadLocal进行处理，解决线程安全问题。

ThreadLocal和线程同步机制都是为了解决多线程中相同变量的访问冲突问题。同步机制采用了“时间换空间”的方式，仅提供一份变量，不同的线程在访问前需要获取锁，没获得锁的线程则需要排队。而ThreadLocal采用了“空间换时间”的方式。

ThreadLocal会为每一个线程提供一个独立的变量副本，从而隔离了多个线程对数据的访问冲突。因为每一个线程都拥有自己的变量副本，从而也就没有必要对该变量进行同步了。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。

 

10-1、Spring基于xml注入bean的几种方式：

（1）Set方法注入；

（2）构造器注入：①通过index设置参数的位置；②通过type设置参数类型；

（3）静态工厂注入；

（4）实例工厂；

详细内容可以阅读：https://blog.csdn.net/a745233700/article/details/89307518

10-2、Spring的自动装配：

在spring中，对象无需自己查找或创建与其关联的其他对象，由容器负责把需要相互协作的对象引用赋予各个对象，使用autowire来配置自动装载模式。

在Spring框架xml配置中共有5种自动装配：

（1）no：默认的方式是不进行自动装配的，通过手工设置ref属性来进行装配bean。

（2）byName：通过bean的名称进行自动装配，如果一个bean的 property 与另一bean 的name 相同，就进行自动装配。 

（3）byType：通过参数的数据类型进行自动装配。

（4）constructor：利用构造函数进行装配，并且构造函数的参数通过byType进行装配。

（5）autodetect：自动探测，如果有构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。

基于注解的方式：

使用@Autowired注解来自动装配指定的bean。在使用@Autowired注解之前需要在Spring配置文件进行配置，<context:annotation-config />。在启动spring IoC时，容器自动装载了一个AutowiredAnnotationBeanPostProcessor后置处理器，当容器扫描到@Autowied、@Resource或@Inject时，就会在IoC容器自动查找需要的bean，并装配给该对象的属性。在使用@Autowired时，首先在容器中查询对应类型的bean：

如果查询结果刚好为一个，就将该bean装配给@Autowired指定的数据；

如果查询的结果不止一个，那么@Autowired会根据名称来查找；

如果上述查找的结果为空，那么会抛出异常。解决方法时，使用required=false。

@Autowired可用于：构造函数、成员变量、Setter方法

注：@Autowired和@Resource之间的区别

(1) @Autowired默认是按照类型装配注入的，默认情况下它要求依赖对象必须存在（可以设置它required属性为false）。

(2) @Resource默认是按照名称来装配注入的，只有当找不到与名称匹配的bean才会按照类型来装配注入。

 

11、Spring 框架中都用到了哪些设计模式？

（1）工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；

（2）单例模式：Bean默认为单例模式。

（3）代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术；

（4）模板方法：用来解决代码重复的问题。比如. RestTemplate, JmsTemplate, JpaTemplate。

（5）观察者模式：定义对象键一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现--ApplicationListener。

 

12、Spring事务的实现方式和实现原理：

Spring事务的本质其实就是数据库对事务的支持，没有数据库的事务支持，spring是无法提供事务功能的。真正的数据库层的事务提交和回滚是通过binlog或者redo log实现的。

（1）Spring事务的种类：

spring支持编程式事务管理和声明式事务管理两种方式：

①编程式事务管理使用TransactionTemplate。

②声明式事务管理建立在AOP之上的。其本质是通过AOP功能，对方法前后进行拦截，将事务处理的功能编织到拦截的方法中，也就是在目标方法开始之前加入一个事务，在执行完目标方法之后根据执行情况提交或者回滚事务。

声明式事务最大的优点就是不需要在业务逻辑代码中掺杂事务管理的代码，只需在配置文件中做相关的事务规则声明或通过@Transactional注解的方式，便可以将事务规则应用到业务逻辑中。

声明式事务管理要优于编程式事务管理，这正是spring倡导的非侵入式的开发方式，使业务代码不受污染，只要加上注解就可以获得完全的事务支持。唯一不足地方是，最细粒度只能作用到方法级别，无法做到像编程式事务那样可以作用到代码块级别。

（2）spring的事务传播行为：

spring事务的传播行为说的是，当多个事务同时存在的时候，spring如何处理这些事务的行为。

① PROPAGATION_REQUIRED：如果当前没有事务，就创建一个新事务，如果当前存在事务，就加入该事务，该设置是最常用的设置。

② PROPAGATION_SUPPORTS：支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就以非事务执行。‘

③ PROPAGATION_MANDATORY：支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就抛出异常。

④ PROPAGATION_REQUIRES_NEW：创建新事务，无论当前存不存在事务，都创建新事务。

⑤ PROPAGATION_NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。

⑥ PROPAGATION_NEVER：以非事务方式执行，如果当前存在事务，则抛出异常。

⑦ PROPAGATION_NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则按REQUIRED属性执行。

（3）Spring中的隔离级别：

① ISOLATION_DEFAULT：这是个 PlatfromTransactionManager 默认的隔离级别，使用数据库默认的事务隔离级别。

② ISOLATION_READ_UNCOMMITTED：读未提交，允许另外一个事务可以看到这个事务未提交的数据。

③ ISOLATION_READ_COMMITTED：读已提交，保证一个事务修改的数据提交后才能被另一事务读取，而且能看到该事务对已有记录的更新。

④ ISOLATION_REPEATABLE_READ：可重复读，保证一个事务修改的数据提交后才能被另一事务读取，但是不能看到该事务对已有记录的更新。

⑤ ISOLATION_SERIALIZABLE：一个事务在执行的过程中完全看不到其他事务对数据库所做的更新。

 

13、Spring框架中有哪些不同类型的事件？

Spring 提供了以下5种标准的事件：

（1）上下文更新事件（ContextRefreshedEvent）：在调用ConfigurableApplicationContext 接口中的refresh()方法时被触发。

（2）上下文开始事件（ContextStartedEvent）：当容器调用ConfigurableApplicationContext的Start()方法开始/重新开始容器时触发该事件。

（3）上下文停止事件（ContextStoppedEvent）：当容器调用ConfigurableApplicationContext的Stop()方法停止容器时触发该事件。

（4）上下文关闭事件（ContextClosedEvent）：当ApplicationContext被关闭时触发该事件。容器被关闭时，其管理的所有单例Bean都被销毁。

（5）请求处理事件（RequestHandledEvent）：在Web应用中，当一个http请求（request）结束触发该事件。

如果一个bean实现了ApplicationListener接口，当一个ApplicationEvent 被发布以后，bean会自动被通知。

 

14、解释一下Spring AOP里面的几个名词：

（1）切面（Aspect）：被抽取的公共模块，可能会横切多个对象。 在Spring AOP中，切面可以使用通用类（基于模式的风格） 或者在普通类中以 @AspectJ 注解来实现。

（2）连接点（Join point）：指方法，在Spring AOP中，一个连接点 总是 代表一个方法的执行。 

（3）通知（Advice）：在切面的某个特定的连接点（Join point）上执行的动作。通知有各种类型，其中包括“around”、“before”和“after”等通知。许多AOP框架，包括Spring，都是以拦截器做通知模型， 并维护一个以连接点为中心的拦截器链。

（4）切入点（Pointcut）：切入点是指 我们要对哪些Join point进行拦截的定义。通过切入点表达式，指定拦截的方法，比如指定拦截add*、search*。

（5）引入（Introduction）：（也被称为内部类型声明（inter-type declaration））。声明额外的方法或者某个类型的字段。Spring允许引入新的接口（以及一个对应的实现）到任何被代理的对象。例如，你可以使用一个引入来使bean实现 IsModified 接口，以便简化缓存机制。

（6）目标对象（Target Object）： 被一个或者多个切面（aspect）所通知（advise）的对象。也有人把它叫做 被通知（adviced） 对象。 既然Spring AOP是通过运行时代理实现的，这个对象永远是一个 被代理（proxied） 对象。

（7）织入（Weaving）：指把增强应用到目标对象来创建新的代理对象的过程。Spring是在运行时完成织入。

切入点（pointcut）和连接点（join point）匹配的概念是AOP的关键，这使得AOP不同于其它仅仅提供拦截功能的旧技术。 切入点使得定位通知（advice）可独立于OO层次。 例如，一个提供声明式事务管理的around通知可以被应用到一组横跨多个对象中的方法上（例如服务层的所有业务操作）。



15、Spring通知有哪些类型？

https://blog.csdn.net/qq_32331073/article/details/80596084

（1）前置通知（Before advice）：在某连接点（join point）之前执行的通知，但这个通知不能阻止连接点前的执行（除非它抛出一个异常）。

（2）返回后通知（After returning advice）：在某连接点（join point）正常完成后执行的通知：例如，一个方法没有抛出任何异常，正常返回。 

（3）抛出异常后通知（After throwing advice）：在方法抛出异常退出时执行的通知。 

（4）后通知（After (finally) advice）：当某连接点退出的时候执行的通知（不论是正常返回还是异常退出）。 

（5）环绕通知（Around Advice）：包围一个连接点（join point）的通知，如方法调用。这是最强大的一种通知类型。 环绕通知可以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它们自己的返回值或抛出异常来结束执行。 环绕通知是最常用的一种通知类型。大部分基于拦截的AOP框架，例如Nanning和JBoss4，都只提供环绕通知。 

同一个aspect，不同advice的执行顺序：

①没有异常情况下的执行顺序：

around before advice
before advice
target method 执行
around after advice
after advice
afterReturning

②有异常情况下的执行顺序：

around before advice
before advice
target method 执行
around after advice
after advice
afterThrowing:异常发生

java.lang.RuntimeException: 异常发生

# Mybatis常见面试题总结

1、什么是Mybatis？

（1）Mybatis是一个半ORM（对象关系映射）框架，它内部封装了JDBC，开发时只需要关注SQL语句本身，不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。程序员直接编写原生态sql，可以严格控制sql执行性能，灵活度高。

（2）MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

（3）通过xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过java对象和 statement中sql的动态参数进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射为java对象并返回。（从执行sql到返回result的过程）。

2、Mybaits的优点：

（1）基于SQL语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成任何影响，SQL写在XML里，解除sql与程序代码的耦合，便于统一管理；提供XML标签，支持编写动态SQL语句，并可重用。

（2）与JDBC相比，减少了50%以上的代码量，消除了JDBC大量冗余的代码，不需要手动开关连接；

（3）很好的与各种数据库兼容（因为MyBatis使用JDBC来连接数据库，所以只要JDBC支持的数据库MyBatis都支持）。

（4）能够与Spring很好的集成；

（5）提供映射标签，支持对象与数据库的ORM字段关系映射；提供对象关系映射标签，支持对象关系组件维护。

3、MyBatis框架的缺点：

（1）SQL语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写SQL语句的功底有一定要求。

（2）SQL语句依赖于数据库，导致数据库移植性差，不能随意更换数据库。

4、MyBatis框架适用场合：

（1）MyBatis专注于SQL本身，是一个足够灵活的DAO层解决方案。

（2）对性能的要求很高，或者需求变化较多的项目，如互联网项目，MyBatis将是不错的选择。

 

5、MyBatis与Hibernate有哪些不同？

（1）Mybatis和hibernate不同，它不完全是一个ORM框架，因为MyBatis需要程序员自己编写Sql语句。

（2）Mybatis直接编写原生态sql，可以严格控制sql执行性能，灵活度高，非常适合对关系数据模型要求不高的软件开发，因为这类软件需求变化频繁，一但需求变化要求迅速输出成果。但是灵活的前提是mybatis无法做到数据库无关性，如果需要实现支持多种数据库的软件，则需要自定义多套sql映射文件，工作量大。 

（3）Hibernate对象/关系映射能力强，数据库无关性好，对于关系模型要求高的软件，如果用hibernate开发可以节省很多代码，提高效率。 

 

6、#{}和${}的区别是什么？

#{}是预编译处理，${}是字符串替换。

Mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；

Mybatis在处理${}时，就是把${}替换成变量的值。

使用#{}可以有效的防止SQL注入，提高系统安全性。

 

7、当实体类中的属性名和表中的字段名不一样 ，怎么办 ？

第1种： 通过在查询的sql语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。

    <select id=”selectorder” parametertype=”int” resultetype=”me.gacl.domain.order”>
       select order_id id, order_no orderno ,order_price price form orders where order_id=#{id};
    </select>
第2种： 通过<resultMap>来映射字段名和实体类属性名的一一对应的关系。

 <select id="getOrder" parameterType="int" resultMap="orderresultmap">
        select * from orders where order_id=#{id}
    </select>

   <resultMap type=”me.gacl.domain.order” id=”orderresultmap”>
        <!–用id属性来映射主键字段–>
        <id property=”id” column=”order_id”>

        <!–用result属性来映射非主键字段，property为实体类属性名，column为数据表中的属性–>
        <result property = “orderno” column =”order_no”/>
        <result property=”price” column=”order_price” />
    </reslutMap>


8、 模糊查询like语句该怎么写?

第1种：在Java代码中添加sql通配符。

    string wildcardname = “%smi%”;
    list<name> names = mapper.selectlike(wildcardname);
     
    <select id=”selectlike”>
     select * from foo where bar like #{value}
    </select>
第2种：在sql语句中拼接通配符，会引起sql注入

    string wildcardname = “smi”;
    list<name> names = mapper.selectlike(wildcardname);
     
    <select id=”selectlike”>
         select * from foo where bar like "%"#{value}"%"
    </select>


9、通常一个Xml映射文件，都会写一个Dao接口与之对应，请问，这个Dao接口的工作原理是什么？Dao接口里的方法，参数不同时，方法能重载吗？

Dao接口即Mapper接口。接口的全限名，就是映射文件中的namespace的值；接口的方法名，就是映射文件中Mapper的Statement的id值；接口方法内的参数，就是传递给sql的参数。

Mapper接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为key值，可唯一定位一个MapperStatement。在Mybatis中，每一个<select>、<insert>、<update>、<delete>标签，都会被解析为一个MapperStatement对象。

举例：com.mybatis3.mappers.StudentDao.findStudentById，可以唯一找到namespace为com.mybatis3.mappers.StudentDao下面 id 为 findStudentById 的 MapperStatement。

Mapper接口里的方法，是不能重载的，因为是使用 全限名+方法名 的保存和寻找策略。Mapper 接口的工作原理是JDK动态代理，Mybatis运行时会使用JDK动态代理为Mapper接口生成代理对象proxy，代理对象会拦截接口方法，转而执行MapperStatement所代表的sql，然后将sql执行结果返回。

 

10、Mybatis是如何进行分页的？分页插件的原理是什么？

        Mybatis使用RowBounds对象进行分页，它是针对ResultSet结果集执行的内存分页，而非物理分页。可以在sql内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。
    
       分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，然后重写sql，根据dialect方言，添加对应的物理分页语句和物理分页参数。

 








11、Mybatis是如何将sql执行结果封装为目标对象并返回的？都有哪些映射形式？

第一种是使用<resultMap>标签，逐一定义数据库列名和对象属性名之间的映射关系。

第二种是使用sql列的别名功能，将列的别名书写为对象属性名。

有了列名与属性名的映射关系后，Mybatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。

 

12、如何执行批量插入?

首先,创建一个简单的insert语句:

    <insert id=”insertname”>
         insert into names (name) values (#{value})
    </insert>
然后在java代码中像下面这样执行批处理插入:

  list<string> names = new arraylist();
    names.add(“fred”);
    names.add(“barney”);
    names.add(“betty”);
    names.add(“wilma”);

    // 注意这里 executortype.batch
    sqlsession sqlsession = sqlsessionfactory.opensession(executortype.batch);
    try {
     namemapper mapper = sqlsession.getmapper(namemapper.class);
     for (string name : names) {
         mapper.insertname(name);
     }
     sqlsession.commit();
    }catch(Exception e){
     e.printStackTrace();
     sqlSession.rollback(); 
     throw e; 
    }
     finally {
         sqlsession.close();
    }


13、如何获取自动生成的(主)键值?

insert 方法总是返回一个int值 ，这个值代表的是插入的行数。

如果采用自增长策略，自动生成的键值在 insert 方法执行完后可以被设置到传入的参数对象中。

示例：

<insert id=”insertname” usegeneratedkeys=”true” keyproperty=”id”>
     insert into names (name) values (#{name})
</insert>
    name name = new name();
    name.setname(“fred”);

    int rows = mapper.insertname(name);
    // 完成后,id已经被设置到对象中
    system.out.println(“rows inserted = ” + rows);
    system.out.println(“generated key value = ” + name.getid());


14、在mapper中如何传递多个参数?

（1）第一种：
//DAO层的函数
Public UserselectUser(String name,String area);  
//对应的xml,#{0}代表接收的是dao层中的第一个参数，#{1}代表dao层中第二参数，更多参数一致往后加即可。
<select id="selectUser"resultMap="BaseResultMap">  
    select *  fromuser_user_t   whereuser_name = #{0} anduser_area=#{1}  
</select>  

（2）第二种： 使用 @param 注解:
public interface usermapper {
   user selectuser(@param(“username”) string username,@param(“hashedpassword”) string hashedpassword);
}
然后,就可以在xml像下面这样使用(推荐封装为一个map,作为单个参数传递给mapper):
<select id=”selectuser” resulttype=”user”>
         select id, username, hashedpassword
         from some_table
         where username = #{username}
         and hashedpassword = #{hashedpassword}
</select>

（3）第三种：多个参数封装成map
try{
//映射文件的命名空间.SQL片段的ID，就可以调用对应的映射文件中的SQL
//由于我们的参数超过了两个，而方法中只有一个Object参数收集，因此我们使用Map集合来装载我们的参数
Map<String, Object> map = new HashMap();
     map.put("start", start);
     map.put("end", end);
     return sqlSession.selectList("StudentID.pagination", map);
 }catch(Exception e){
     e.printStackTrace();
     sqlSession.rollback();
    throw e; }
finally{
 MybatisUtil.closeSqlSession();
 }


15、Mybatis动态sql有什么用？执行原理？有哪些动态sql？

Mybatis动态sql可以在Xml映射文件内，以标签的形式编写动态sql，执行原理是根据表达式的值 完成逻辑判断并动态拼接sql的功能。

Mybatis提供了9种动态sql标签：trim | where | set | foreach | if | choose | when | otherwise | bind。

 

16、Xml映射文件中，除了常见的select|insert|updae|delete标签之外，还有哪些标签？

答：<resultMap>、<parameterMap>、<sql>、<include>、<selectKey>，加上动态sql的9个标签，其中<sql>为sql片段标签，通过<include>标签引入sql片段，<selectKey>为不支持自增的主键生成策略标签。

 

17、Mybatis的Xml映射文件中，不同的Xml映射文件，id是否可以重复？

不同的Xml映射文件，如果配置了namespace，那么id可以重复；如果没有配置namespace，那么id不能重复；

原因就是namespace+id是作为Map<String, MapperStatement>的key使用的，如果没有namespace，就剩下id，那么，id重复会导致数据互相覆盖。有了namespace，自然id就可以重复，namespace不同，namespace+id自然也就不同。

 

18、为什么说Mybatis是半自动ORM映射工具？它与全自动的区别在哪里？

Hibernate属于全自动ORM映射工具，使用Hibernate查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取，所以它是全自动的。而Mybatis在查询关联对象或关联集合对象时，需要手动编写sql来完成，所以，称之为半自动ORM映射工具。

 

19、 一对一、一对多的关联查询 ？ 

<mapper namespace="com.lcb.mapping.userMapper">  
    <!--association  一对一关联查询 -->  
    <select id="getClass" parameterType="int" resultMap="ClassesResultMap">  
        select * from class c,teacher t where c.teacher_id=t.t_id and c.c_id=#{id}  
    </select>  

    <resultMap type="com.lcb.user.Classes" id="ClassesResultMap">  
        <!-- 实体类的字段名和数据表的字段名映射 -->  
        <id property="id" column="c_id"/>  
        <result property="name" column="c_name"/>  
        <association property="teacher" javaType="com.lcb.user.Teacher">  
            <id property="id" column="t_id"/>  
            <result property="name" column="t_name"/>  
        </association>  
    </resultMap>  

 







    <!--collection  一对多关联查询 -->  
    <select id="getClass2" parameterType="int" resultMap="ClassesResultMap2">  
        select * from class c,teacher t,student s where c.teacher_id=t.t_id and c.c_id=s.class_id and c.c_id=#{id}  
    </select>  
     
    <resultMap type="com.lcb.user.Classes" id="ClassesResultMap2">  
        <id property="id" column="c_id"/>  
        <result property="name" column="c_name"/>  
        <association property="teacher" javaType="com.lcb.user.Teacher">  
            <id property="id" column="t_id"/>  
            <result property="name" column="t_name"/>  
        </association>  
     
        <collection property="student" ofType="com.lcb.user.Student">  
            <id property="id" column="s_id"/>  
            <result property="name" column="s_name"/>  
        </collection>  
    </resultMap>  
</mapper> 


20、MyBatis实现一对一有几种方式?具体怎么操作的？

有联合查询和嵌套查询,联合查询是几个表联合查询,只查询一次, 通过在resultMap里面配置association节点配置一对一的类就可以完成；

嵌套查询是先查一个表，根据这个表里面的结果的 外键id，去再另外一个表里面查询数据,也是通过association配置，但另外一个表的查询通过select属性配置。

 

21、MyBatis实现一对多有几种方式,怎么操作的？

        有联合查询和嵌套查询。联合查询是几个表联合查询,只查询一次,通过在resultMap里面的collection节点配置一对多的类就可以完成；嵌套查询是先查一个表,根据这个表里面的 结果的外键id,去再另外一个表里面查询数据,也是通过配置collection,但另外一个表的查询通过select节点配置。

 








22、Mybatis是否支持延迟加载？如果支持，它的实现原理是什么？

答：Mybatis仅支持association关联对象和collection关联集合对象的延迟加载，association指的就是一对一，collection指的就是一对多查询。在Mybatis配置文件中，可以配置是否启用延迟加载lazyLoadingEnabled=true|false。

它的原理是，使用CGLIB创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用a.getB().getName()，拦截器invoke()方法发现a.getB()是null值，那么就会单独发送事先保存好的查询关联B对象的sql，把B查询上来，然后调用a.setB(b)，于是a的对象b属性就有值了，接着完成a.getB().getName()方法的调用。这就是延迟加载的基本原理。

当然了，不光是Mybatis，几乎所有的包括Hibernate，支持延迟加载的原理都是一样的。

 

 23、Mybatis的一级、二级缓存:

1）一级缓存: 基于 PerpetualCache 的 HashMap 本地缓存，其存储作用域为 Session，当 Session flush 或 close 之后，该 Session 中的所有 Cache 就将清空，默认打开一级缓存。

2）二级缓存与一级缓存其机制相同，默认也是采用 PerpetualCache，HashMap 存储，不同在于其存储作用域为 Mapper(Namespace)，并且可自定义存储源，如 Ehcache。默认不打开二级缓存，要开启二级缓存，使用二级缓存属性类需要实现Serializable序列化接口(可用来保存对象的状态),可在它的映射文件中配置<cache/> ；

3）对于缓存数据更新机制，当某一个作用域(一级缓存 Session/二级缓存Namespaces)的进行了C/U/D 操作后，默认该作用域下所有 select 中的缓存将被 clear。

 

24、什么是MyBatis的接口绑定？有哪些实现方式？

接口绑定，就是在MyBatis中任意定义接口,然后把接口里面的方法和SQL语句绑定, 我们直接调用接口方法就可以,这样比起原来了SqlSession提供的方法我们可以有更加灵活的选择和设置。

接口绑定有两种实现方式,一种是通过注解绑定，就是在接口的方法上面加上 @Select、@Update等注解，里面包含Sql语句来绑定；另外一种就是通过xml里面写SQL来绑定, 在这种情况下,要指定xml映射文件里面的namespace必须为接口的全路径名。当Sql语句比较简单时候,用注解绑定, 当SQL语句比较复杂时候,用xml绑定,一般用xml绑定的比较多。

25、使用MyBatis的mapper接口调用时有哪些要求？

①  Mapper接口方法名和mapper.xml中定义的每个sql的id相同；
②  Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同；
③  Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同；
④  Mapper.xml文件中的namespace即是mapper接口的类路径。

26、Mapper编写有哪几种方式？

第一种：接口实现类继承SqlSessionDaoSupport：使用此种方法需要编写mapper接口，mapper接口实现类、mapper.xml文件。
（1）在sqlMapConfig.xml中配置mapper.xml的位置
<mappers>
    <mapper resource="mapper.xml文件的地址" />
    <mapper resource="mapper.xml文件的地址" />
</mappers>
（2）定义mapper接口
（3）实现类集成SqlSessionDaoSupport
mapper方法中可以this.getSqlSession()进行数据增删改查。
（4）spring 配置
<bean id=" " class="mapper接口的实现">
    <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
</bean>


第二种：使用org.mybatis.spring.mapper.MapperFactoryBean：
（1）在sqlMapConfig.xml中配置mapper.xml的位置，如果mapper.xml和mappre接口的名称相同且在同一个目录，这里可以不用配置
<mappers>
    <mapper resource="mapper.xml文件的地址" />
    <mapper resource="mapper.xml文件的地址" />
</mappers>
（2）定义mapper接口：
①mapper.xml中的namespace为mapper接口的地址
②mapper接口中的方法名和mapper.xml中的定义的statement的id保持一致
③Spring中定义
<bean id="" class="org.mybatis.spring.mapper.MapperFactoryBean">
    <property name="mapperInterface"   value="mapper接口地址" /> 
    <property name="sqlSessionFactory" ref="sqlSessionFactory" /> 
</bean>


第三种：使用mapper扫描器：
（1）mapper.xml文件编写：
mapper.xml中的namespace为mapper接口的地址；
mapper接口中的方法名和mapper.xml中的定义的statement的id保持一致；
如果将mapper.xml和mapper接口的名称保持一致则不用在sqlMapConfig.xml中进行配置。 
（2）定义mapper接口：
注意mapper.xml的文件名和mapper的接口名称保持一致，且放在同一个目录
（3）配置mapper扫描器：
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="mapper接口包地址"></property>
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/> 
</bean>
（4）使用扫描器后从spring容器中获取mapper的实现对象。

 

27、简述Mybatis的插件运行原理，以及如何编写一个插件。

答：Mybatis仅可以编写针对ParameterHandler、ResultSetHandler、StatementHandler、Executor这4种接口的插件，Mybatis使用JDK的动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行这4种接口对象的方法时，就会进入拦截方法，具体就是InvocationHandler的invoke()方法，当然，只会拦截那些你指定需要拦截的方法。

编写插件：实现Mybatis的Interceptor接口并复写intercept()方法，然后在给插件编写注解，指定要拦截哪一个接口的哪些方法即可，记住，别忘了在配置文件中配置你编写的插件。
--------------------- 
# SpringMVC常见面试题总结

1、什么是Spring MVC ？简单介绍下你对springMVC的理解?

Spring MVC是一个基于Java的实现了MVC设计模式的请求驱动类型的轻量级Web框架，通过把Model，View，Controller分离，将web层进行职责解耦，把复杂的web应用分成逻辑清晰的几部分，简化开发，减少出错，方便组内开发人员之间的配合。

 

2、SpringMVC的流程？

（1）用户发送请求至前端控制器DispatcherServlet；
（2） DispatcherServlet收到请求后，调用HandlerMapping处理器映射器，请求获取Handle；
（3）处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet；
（4）DispatcherServlet 调用 HandlerAdapter处理器适配器；
（5）HandlerAdapter 经过适配调用 具体处理器(Handler，也叫后端控制器)；
（6）Handler执行完成返回ModelAndView；
（7）HandlerAdapter将Handler执行结果ModelAndView返回给DispatcherServlet；
（8）DispatcherServlet将ModelAndView传给ViewResolver视图解析器进行解析；
（9）ViewResolver解析后返回具体View；
（10）DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）
（11）DispatcherServlet响应用户。



3、Springmvc的优点:

（1）可以支持各种视图技术,而不仅仅局限于JSP；

（2）与Spring框架集成（如IoC容器、AOP等）；

（3）清晰的角色分配：前端控制器(dispatcherServlet) , 请求到处理器映射（handlerMapping), 处理器适配器（HandlerAdapter), 视图解析器（ViewResolver）。

（4） 支持各种请求资源的映射策略。

 

4、Spring MVC的主要组件？

（1）前端控制器 DispatcherServlet（不需要程序员开发）

作用：接收请求、响应结果，相当于转发器，有了DispatcherServlet 就减少了其它组件之间的耦合度。

（2）处理器映射器HandlerMapping（不需要程序员开发）

作用：根据请求的URL来查找Handler

（3）处理器适配器HandlerAdapter

注意：在编写Handler的时候要按照HandlerAdapter要求的规则去编写，这样适配器HandlerAdapter才可以正确的去执行Handler。

（4）处理器Handler（需要程序员开发）

（5）视图解析器 ViewResolver（不需要程序员开发）

作用：进行视图的解析，根据视图逻辑名解析成真正的视图（view）

（6）视图View（需要程序员开发jsp）

View是一个接口， 它的实现类支持不同的视图类型（jsp，freemarker，pdf等等）

 

5、springMVC和struts2的区别有哪些?

（1）springmvc的入口是一个servlet即前端控制器（DispatchServlet），而struts2入口是一个filter过虑器（StrutsPrepareAndExecuteFilter）。

（2）springmvc是基于方法开发(一个url对应一个方法)，请求参数传递到方法的形参，可以设计为单例或多例(建议单例)，struts2是基于类开发，传递参数是通过类的属性，只能设计为多例。

（3）Struts采用值栈存储请求和响应的数据，通过OGNL存取数据，springmvc通过参数解析器是将request请求内容解析，并给方法形参赋值，将数据和视图封装成ModelAndView对象，最后又将ModelAndView中的模型数据通过reques域传输到页面。Jsp视图解析器默认使用jstl。

 

6、SpringMVC怎么样设定重定向和转发的？

（1）转发：在返回值前面加"forward:"，譬如"forward:user.do?name=method4"

（2）重定向：在返回值前面加"redirect:"，譬如"redirect:http://www.baidu.com"

 

7、SpringMvc怎么和AJAX相互调用的？

通过Jackson框架就可以把Java里面的对象直接转化成Js可以识别的Json对象。具体步骤如下 ：

（1）加入Jackson.jar

（2）在配置文件中配置json的映射

（3）在接受Ajax方法里面可以直接返回Object,List等,但方法前面要加上@ResponseBody注解。

 

8、如何解决POST请求中文乱码问题，GET的又如何处理呢？

（1）解决post请求乱码问题：

在web.xml中配置一个CharacterEncodingFilter过滤器，设置成utf-8；

<filter>

    <filter-name>CharacterEncodingFilter</filter-name>
    
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    
    <init-param>
    
        <param-name>encoding</param-name>
    
        <param-value>utf-8</param-value>
    
    </init-param>

</filter>

<filter-mapping>

    <filter-name>CharacterEncodingFilter</filter-name>
    
    <url-pattern>/*</url-pattern>

</filter-mapping>

（2）get请求中文参数出现乱码解决方法有两个：

①修改tomcat配置文件添加编码与工程编码一致，如下：

<ConnectorURIEncoding="utf-8" connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>

 ②另外一种方法对参数进行重新编码：

String userName = new String(request.getParamter("userName").getBytes("ISO8859-1"),"utf-8")

ISO8859-1是tomcat默认编码，需要将tomcat编码后的内容按utf-8编码。

 

9、Spring MVC的异常处理 ？

答：可以将异常抛给Spring框架，由Spring框架来处理；我们只需要配置简单的异常处理器，在异常处理器中添视图页面即可。

10、SpringMvc的控制器是不是单例模式,如果是,有什么问题,怎么解决？

答：是单例模式,所以在多线程访问的时候有线程安全问题,不要用同步,会影响性能的,解决方案是在控制器里面不能写字段。

11、 SpringMVC常用的注解有哪些？

@RequestMapping：用于处理请求 url 映射的注解，可用于类或方法上。用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径。

@RequestBody：注解实现接收http请求的json数据，将json转换为java对象。

@ResponseBody：注解实现将conreoller方法返回对象转化为json对象响应给客户。

12、SpingMvc中的控制器的注解一般用那个,有没有别的注解可以替代？

答：一般用@Conntroller注解,表示是表现层,不能用别的注解代替。

13、如果在拦截请求中，我想拦截get方式提交的方法,怎么配置？

答：可以在@RequestMapping注解里面加上method=RequestMethod.GET。

14、怎样在方法里面得到Request,或者Session？

答：直接在方法的形参中声明request,SpringMvc就自动把request对象传入。

15、如果想在拦截的方法里面得到从前台传入的参数,怎么得到？

答：直接在形参里面声明这个参数就可以,但必须名字和传过来的参数一样。

16、如果前台有很多个参数传入,并且这些参数都是一个对象的,那么怎么样快速得到这个对象？

答：直接在方法中声明这个对象,SpringMvc就自动会把属性赋值到这个对象里面。

17、SpringMvc中函数的返回值是什么？

答：返回值可以有很多类型,有String, ModelAndView。ModelAndView类把视图和数据都合并的一起的，但一般用String比较好。

18、SpringMvc用什么对象从后台向前台传递数据的？

答：通过ModelMap对象,可以在这个对象里面调用put方法,把对象加到里面,前台就可以通过el表达式拿到。

19、怎么样把ModelMap里面的数据放入Session里面？

答：可以在类上面加上@SessionAttributes注解,里面包含的字符串就是要放入session里面的key。

 

20、SpringMvc里面拦截器是怎么写的：

有两种写法,一种是实现HandlerInterceptor接口，另外一种是继承适配器类，接着在接口方法当中，实现处理逻辑；然后在SpringMvc的配置文件中配置拦截器即可：

  <!-- 配置SpringMvc的拦截器 -->

<mvc:interceptors>

    <!-- 配置一个拦截器的Bean就可以了 默认是对所有请求都拦截 -->
     
    <bean id="myInterceptor" class="com.zwp.action.MyHandlerInterceptor"></bean>
     
    <!-- 只针对部分请求拦截 -->
     
    <mvc:interceptor>
     
       <mvc:mapping path="/modelMap.do" />
     
       <bean class="com.zwp.action.MyHandlerInterceptorAdapter" />
     
    </mvc:interceptor>

</mvc:interceptors>

21、注解原理：

注解本质是一个继承了Annotation的特殊接口，其具体实现类是Java运行时生成的动态代理类。我们通过反射获取注解时，返回的是Java运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池。
--------------------- 
