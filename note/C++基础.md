2. 同步和异步，阻塞与非阻塞
```
同步和异步关注的是消息通信机制 (synchronous communication/ asynchronous communication)
所谓同步，就是在发出一个*调用*时，在没有得到结果之前，该*调用*就不返回。但是一旦调用返回，就得到返回值了。换句话说，就是由*调用者*主动等待这个*调用*的结果。而异步则是相反，*调用*在发出之后，这个调用就直接返回了，所以没有返回结果。换句话说，当一个异步过程调用发出后，调用者不会立刻得到结果。而是在*调用*发出后，*被调用者*通过状态、通知来通知调用者，或通过回调函数处理这个调用。

阻塞和非阻塞关注的是程序在等待调用结果（消息，返回值）时的状态.

阻塞调用是指调用结果返回之前，当前线程会被挂起。调用线程只有在得到结果之后才会返回。
非阻塞调用指在不能立刻得到结果之前，该调用不会阻塞当前线程。
```

3. 
```
```

4. 说一说继承、封装、多态以及多态的实现机制、虚函数、编译时多态
```
一、封装：
封装是实现面向对象程序设计的第一步，封装就是将数据或函数等集合在一个个的单元中（我们称之为类）。
封装的意义在于保护或者防止代码（数据）被我们无意中破坏。
二、继承：
继承主要实现重用代码，节省开发时间。
子类可以继承父类的一些东西。
三、多态
多态：同一操作作用于不同的对象，可以有不同的解释，产生不同的执行结果。在运行时，可以通过指向基类的指针，来调用实现派生类中的方法。

关于多态，简而言之就是用父类型别的指针指向其子类的实例，然后通过父类的指针调用实际子类的成员函数。这种技术可以让父类的指针有“多种形态”，这是一种泛型技术。

多态通过虚函数来实现，而虚函数（Virtual Function）是通过一张虚函数表（Virtual Table）来实现的。简称为V-Table

派生类继承基类时会产生一张与基类相同的表，但是当派生类重载了基类的某个虚函数时，这个表的相应位置的函数就会被替换成重载后的函数。此时如果基类指向的派生类要调用这个方法时，找到的自然就是这个被重载的方法。

编译时多态
1. 对模板参数而言，多态是通过模板具现化和函数重载解析实现的。以不同的模板参数具现化导致调用不同的函数，这就是所谓的编译期多态。
2. 相比较于运行期多态，实现编译期多态的类之间并不需要成为一个继承体系，它们之间可以没有什么关系，但约束是它们都有相同的隐式接口。



虚函数

引入原因：为了方便使用多态特性，我们常常需要在基类中定义虚函数。

纯虚函数
引入原因：为了实现多态性，纯虚函数有点像java中的接口，自己不去实现过程，让继承他的子类去实现。

在很多情况下，基类本身生成对象是不合情理的。例如，动物作为一个基类可以派生出老虎、孔雀等子类，但动物本身生成对象明显不合常理。 这时我们就将动物类定义成抽象类，也就是包含纯虚函数的类
纯虚函数就是基类只定义了函数体，没有实现过程定义方法如下
virtual void Eat() = 0; 直接=0 不要 在cpp中定义就可以了 

虚函数和纯虚函数的区别
1. 虚函数中的函数是实现的哪怕是空实现，它的作用是这个函数在子类里面可以被重载，运行时动态绑定实现动态
纯虚函数是个接口，是个函数声明，在基类中不实现，要等到子类中去实现
2. 虚函数在子类里可以不重载，但是虚函数必须在子类里去实现。

C++类有继承时，析构函数必须为虚函数.不然只释放基类而不会释放子类，导致内存泄漏。
构造函数不能为虚函数，因为虚函数对应一个虚函数表，存在于对象的内存空间中，如果对象还没有被实例化，也就不可能有虚函数表。 [具体参考](https://blog.csdn.net/woyaowenzi/article/details/2310710)

```
6. 设计模式[待补充]
```
单例模式（懒汉、饿汉）

工厂模式（简单工厂，工厂方法，抽象工厂）

模板模式

适配器模式

```

8. select和epoll
```

select本质上是通过设置或者检查存放fd标志位的数据结构来进行下一步处理。这样所带来的缺点是：

1、 单个进程可监视的fd数量被限制，即能监听端口的大小有限。（它没有最大连接数的限制，原因是它是基于链表来存储的）

2、 对socket进行扫描时是线性扫描，即采用轮询的方法，效率较低：(因为epoll内核中实现是根据每个fd上的callback函数来实现的，只有活跃的socket才会主动调用callback)

3、 需要维护一个用来存放大量fd的数据结构，这样会使得用户空间和内核空间在传递该结构时复制开销大（epoll通过内核和用户空间共享一块内存来实现的）

```

9. NIO
```
Buffer
用于和NIO Channel交互。 我们从Channel中读取数据到buffers里，从Buffer把数据写入到Channels；

channel
NIO把它支持的I/O对象抽象为Channel，Channel又称“通道”，类似于原I/O中的流（Stream），但有所区别：
1、流是单向的，通道是双向的，可读可写。
2、流读写是阻塞的，通道可以异步读写。
3、流中的数据可以选择性的先读到缓存中，通道的数据总是要先读到一个缓存中，或从缓存中写入，如下所示：

selector
为了实现Selector管理多个SocketChannel，必须将具体的SocketChannel对象注册到Selector，并声明需要监听的事件（这样Selector才知道需要记录什么数据），一共有4种事件：
1、connect：客户端连接服务端事件，对应值为SelectionKey.OP_CONNECT(8)
2、accept：服务端接收客户端连接事件，对应值为SelectionKey.OP_ACCEPT(16)
3、read：读事件，对应值为SelectionKey.OP_READ(1)
4、write：写事件，对应值为SelectionKey.OP_WRITE(4)

```

11. static关键字
```
四种情况：

1. static全局变量：有static表示作用范围只在当前文件
2. static局部变量：在全局数据区分配内存，只初始化一次
3. static函数：只在当前文件有效，不会影响其它文件的命名
4. static类成员变量：对该类的多个对象来说，静态数据成员只分配一次内存，供所有对象共用
5. 关于静态成员函数，可以总结为以下几点： 
    1、出现在类体外的函数定义不能指定关键字static； 
    2、静态成员之间可以相互访问，包括静态成员函数访问静态数据成员和访问静态成员函数； 
    3、非静态成员函数可以任意地访问静态成员函数和静态数据成员； 
    4、静态成员函数不能访问非静态成员函数和非静态数据成员； 
    5、由于没有this指针的额外开销，因此静态成员函数与类的全局函数相比速度上会有少许的增长； 
    6、调用静态成员函数，可以用成员访问操作符(.)和(->)为一个类的对象或指向类对象的指针调用静态成员函数.
```

16. 堆栈的区别
```
（1）申请方式不同。栈上有系统自动分配和释放；堆上有程序员自己申请并指明大小；
（2）栈是向低地址扩展的数据结构，大小很有限；堆是向高地址扩展，是不连续的内存区域，空间相对大且灵活；
（3）栈由系统分配和释放速度快；堆由程序员控制，一般较慢，且容易产生碎片；
```

18. AVL树,红黑树,B树,B+树原理
```

平衡二叉树
平衡二叉树是基于二分法的策略提高数据的查找速度的二叉树的数据结构；
1. 非叶子节点最多拥有两个子节点；
2. 非叶子节值大于左边子节点、小于右边子节点；
3. 树的左右两边的层级数相差不会大于1;
4. 没有值相等重复的节点;

红黑树
红黑树是一棵二叉排序树，它在每个结点上增加了一个存储位来表示结点的颜色，可以是RED或BLACK。
通过对任一条从根到叶子的简单路径上各个结点的颜色进行约束，红黑树确保没有一条路径会比其它路径长2倍，因此是近乎平衡的。 

一棵红黑树是具有如下性质的二叉排序树：
1. 每个结点的颜色只能是红色或黑色的。
2. 根结点是黑色的。
3. 每个叶子结点（NIL）是黑色的。
3. 如果一个结点是红色的，那么它的两个子结点都是黑色的。
4. 对每个结点，从该结点到其所有后代叶子简单的简单路径上，均包含相同数目的黑色结点。

B树
M阶B树定义： 
1.根结点至少有两个孩子； 
2.每个结点有M-1个key，升序排列； 
3.位于key[i]和key[i+1]之间的孩子结点的值介于key[i]和key[i+1]之间； 
4.其他节点至少有M/2个孩子（M向上取整）。

哈希表
是根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。
解决冲突：
1. 链表法，遇到相同的key时，在对应位置插入链表；
2. 指针探测法，寻找其它空的位置。
    当发生地址冲突时，按照某种方法继续探测哈希表中的其他存储单元，直到找到空位置为止。这个过程可用下式描述： 
    H i ( key ) = ( H ( key )+ d i ) mod m ( i = 1,2,…… ， k ( k ≤ m – 1)) ）


索引的作用及索引的数据结构

mysql索引的用途：
1. 保持数据的完整性；
2. 优化数据的访问性能
3. 改进表的链接（join）操作
4. 对结果进行排序
5. 简化聚合数据操作
　　
索引的数据结构：B-、B+、R-、散列

散列实现对直接查找方式能提供最优的性能，但对一定范围的查找却效率底下。

B-树索引实现是一个专门为范围查询设计的。
1. 通常B树的节点中同时存储了Key和Value，更准确的说应该是Value的引用(存储数据的物理地址)。
2. 我们比较的时候只需要Key而已，但是却把Value也取了出来，从而造成了不必要的I/O，虽然只是取出了value的引用，但是当数据库数据量越来越大的时候，对性能的影响也会随之累积而变得十分可观。
3. 另外，B树进行范围查询时需要回溯，对于硬盘中的数据结构而言，一次回溯意味着一次随机IO。

为了解决这些问题，B+树被发明出来了。

B+树中有几个小规则：
1. 内部的非叶子节点只存储Key，用来索引，不保存Value数据
2. 所有的Value数据都保存在叶子节点中
3. 叶子节点之间用指针串联起来

当然它的分裂方式也有所不同：中间值在分裂的时候不仅会成为父节点，还会在叶子节点中保留一份。

http://blog.codinglabs.org/articles/theory-of-mysql-index.html
```

19. STL容器
```
1. List封装了双向链表,插入和删除快，不需要拷贝和移动数据，只需要改变指针的指向。（随机访问慢，因为要遍历链表）
2. Vector封装了数组,Vector对于随机访问的速度很快，但是对于插入尤其是在头部插入元素速度很慢（中间插入和删除效率低）
3. Map,Set属于标准关联容器，使用了非常高效的平衡检索二叉树：红黑树，他的插入删除直接替换指向节点的指针即可。
    1. Set和Vector的区别在于Set不包含重复的数据，且Set自动排序。
    2. Set和Map的区别在于Set只含有Key，而Map有一个Key和Key所对应的Value两个元素。

使用场景：
1、如果你需要高效的随机存取，而不在乎插入和删除的效率，使用vector
2、如果你需要大量的插入和删除，而不关心随机存取，则应使用list
3、如果你需要随机存取，而且关心两端数据的插入和删除，则应使用deque。
4、如果你要存储一个数据字典，并要求方便地根据key找value，那么map是较好的选择
5、如果你要查找一个元素是否在某集合内存中，则使用set存储这个集合比较好

原文：https://blog.csdn.net/ly52352148/article/details/52471737 
```

20. 
```

```

22. 为什么需要线程池？为什么要设置核心线程数和最大线程数？
```
概念
线程池的基本思想是一种对象池，在程序启动时就开辟一块内存空间，里面存放了众多(未死亡)的线程，池中线程执行调度由池管理器来处理。
当有线程任务时，从池中取一个，执行完成后线程对象归池，这样可以避免反复创建线程对象所带来的性能开销，节省了系统的资源。

优点
1. 降低资源消耗，重复利用已创建的线程从而降低线程创建和销毁带来的损耗；
2. 提升响应速度，当任务到达时可以不等线程创建就直接执行；
3. 提升系统的可管理性，线程本身就是一种稀缺资源，不能无限制的创建，使用线程池可以进行统一的分配，调优和监控。

核心线程数和最大线程数
1. 核心线程会一直存活，即使没有任务需要执行
2. 当线程数小于核心线程数，即使有空闲线程，线程池也会优先创建新线程处理
3. 当前线程数 >= corePoolSize, 且任务对列已满时，线程池会创建新线程来处理任务
4. 当前线程数 = maxPoolSize, 且任务对列已满时, 线程池会拒绝处理任务而抛出异常

```

23. 说一说FPGA的优缺点？
```
缺点：贵，开发周期长
优点：
实时性：处理速度快，流水线并行和数据并行（延迟低，流处理）（而GPU只有数据并行）；
灵活性：可编程逻辑器件，功能可以随时改变（这一特性使得其在深度学习中占据不可或缺的位置）；
功耗：性能功耗比之王。

FPGA不适合做训练，这个主要是训练过程反向传播的算法特点导致，主要表现在3个方面：
1. 不适合浮点运算，而训练过程，基本上都是浮点运算。
2. 训练过程需要计算种类多，FPGA实现某些非线性运算代价大。
3. 算法的反向传播过程的中间结果以及权重相对正向过程需要转置。
```
25. 发生内存泄漏怎么办？
```
1. 显性的 new  malloc delete free 是否匹配
2. 检查是否因为资源未关闭造成的内存泄漏，例如io流、socket
3. 借助第三方的工具

```

26. const与#define的区别
```
1、编译器处理方式：const:编译时确定其zhi；define:预处理时进行替换
2、类型检查：const：有数据类型，编译时进行数据检查；define：无类型，也不做类型检查
3、内存空间：const：在静态存储区储存，仅此一份；define：在代码段区，每做一次替换就会进行一次拷贝
4、define可以用来防止重复定义，const不行
```

26. 如何判断一个数是否为2的幂
```
num & (num - 1) == 0
```

27. 
```

```

29. 函数调用栈的实现过程
```
1. 将被调用函数的参数压栈
2. 将当前指令，即函数调用指令的下一条指令地址作为返回地址压入栈中
3. 跳转到函数体执行
    该函数体的开始指令通常是这样的：
        1）push %ebp ：将调用者的帧指针%ebp压栈，即保存旧栈帧的帧指针以便函数返回时恢复旧栈帧
        2）mov %esp, %ebp：将当前栈顶地址传给%ebp，此时，%ebp既是旧栈帧的结束地址，又是被调用者的新栈帧的起始地址
        3）sub xxx, %esp：将栈顶下移，即为被调用函数开辟栈空间，xxx为立即数且通常为16的整数倍（这会浪费一些空间，但gcc采用该规则来保证数据的严格对齐）
        4）push xxx：该命令为可选项，如有必要，由被调用者负责保存/恢复某些寄存器
    不难推导出函数返回时通常由如下指令序列构成：
        1）pop xxx：可选项，与进入函数时是否push xxx保存寄存器相对应
        2）mov %ebp, %esp：恢复调用者的栈顶指针，即调用者栈帧的结束边界
        3）pop %ebp：调用者的帧指针弹栈，即恢复调用者栈帧的起始边界
        4）ret：从栈中得到返回地址，并跳转到该位置处继续执行
```

30. 

