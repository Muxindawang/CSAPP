> 第三版在线阅读网站：https://hansimov.gitbook.io/csapp/ch01-a-tour-of-computer-systems/1.1

# 第一章 计算机系统漫游

## 1.1 信息就是位+上下文

系统中所有的信息——包括磁盘文件、内存中的程序、内存中存放的用户数据以及网络上传送的数据，都是由一串比特表示的。区分不同数据对象的唯一方法是我们读到这些数据对象时的上下文。

比如，在不同的上下文中，一个同样的字节序列可能表示一个整数、浮点数、字符串或者机器指令。



## 1.2 程序被其他程序翻译成不同的格式

**预处理器**、**编译器**、**汇编器**和**链接器**一起构成了**编译系统**（compilation system）。对应的文件后缀分别是**.i .s .o**。

## 1.3 了解编译系统如何工作是大有益处的

## 1.4 处理器读并解释储存在内存中的指令

### 1.4.1 系统的硬件组成

1. #### **总线**

贯穿整个系统的是一组电子管道，称作**总线**，它携带信息字节并负责在各个部件间传递。通常总线被设计成传送定长的字节块，也就是字（word）。

2. #### **I/O 设备**

I/O（输入/输出）设备是系统与外部世界的联系通道。我们的示例系统包括四个 I/O 设备∶作为用户输入的键盘和鼠标，作为用户输出的显示器，以及用于长期存储数据和程序的磁盘驱动器（简单地说就是磁盘）。

每个 I/O 设备都通过一个**控制器**或**适配器**与 I/O 总线相连。控制器和适配器之间的区别主要在于它们的封装方式。控制器是 I/O 设备本身或者系统的主印制电路板（通常称作主板）上的芯片组。而适配器则是一块插在主板插槽上的卡。无论如何，它们的功能都是在 I/O 总线和 I/O 设备之间传递信息。

![img](https://hansimov.gitbook.io/csapp/~gitbook/image?url=https%3A%2F%2F4154149387-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MHt_spaxGgCbp2POnfq%252F-MHzautgnqCcwmhp1v0t%252F-MHzbJTPxV8WF5Cpigw-%252F01-04%2520system%2520hardwares.png%3Falt%3Dmedia%26token%3D78949ed7-8d53-4392-b21d-e6123cd2dc50&width=768&dpr=4&quality=100&sign=c44ec80&sv=2)

> 图 1-4 一个典型系统的硬件组成
>
> CPU：中央处理单元；ALU：算术/逻辑单元；PC：程序计数器；USB：通用串行总线

3. #### **主存**

**主存**是一个临时存储设备，在处理器执行程序时，用来存放程序和程序处理的数据。

4. #### 处理器

**中央处理单元**（CPU），简称**处理器**，是解释（或执行）存储在主存中指令的引擎。处理器的核心是一个大小为一个字的存储设备（或**寄存器**），称为**程序计数器**（PC）。

- **加载：**从主存复制一个字节或者一个字到寄存器，以覆盖寄存器原来的内容。
- **存储：**从寄存器复制一个字节或者一个字到主存的某个位置，以覆盖这个位置上原 来的内容。 
- **操作：**把两个寄存器的内容复制到 ALU，ALU 对这两个字做算术运算，并将结果存放到一个寄存器中，以覆盖该寄存器中原来的内容。 
- **跳转：**从指令本身中抽取一个字，并将这个字复制到程序计数器（PC）中，以覆盖 PC 中原来的值。



## 1.5 高速缓存至关重要

这个简单的示例揭示了一个重要的问题，即系统花费了大量的时间把信息从一个地方挪到另一个地方。hello 程序的机器指令最初是存放在磁盘上，当程序加载时，它们被复制到主存；当处理器运行程序时，指令又从主存复制到处理器。相似地，数据串 “hello, world\n” 开始时在磁盘上，然后被复制到主存，最后从主存上复制到显示设备。从程序员的角度来看，这些复制就是开销，减慢了程序“真正”的工作。因此，系统设计者的一个主要目标就是使这些复制操作尽可能快地完成。

**高速缓存存储器**（cache memory，简称为 cache 或高速缓存），作为暂时的集结区域，存放处理器近期可能会需要的信息。

图 1-8 展示了一个典型系统中的高速缓存存储器。位于处理器芯片上的 L1 高速缓存的容量可以达到数万字节，访问速度几乎和访问寄存器文件一样快。一个容量为数十万到数百万字节的更大的 L2 高速缓存通过一条特殊的总线连接到处理器。进程访问 L2 高速缓存的时间要比访问 L1 高速缓存的时间长 5 倍，但是这仍然比访问主存的时间快 5~10 倍。L1 和 L2 高速缓存是用一种叫做**静态随机访问存储器**（SRAM）的硬件技术实现的。比较新的、处理能力更强大的系统甚至有三级高速缓存∶L1、L2 和 L3。系统可以获得一个很大的存储器，同时访问速度也很快，原因是利用了高速缓存的局部性原理，即程序具有访问局部区域里的数据和代码的趋势。通过让高速缓存里存放可能经常访问的数据，大部分的内存操作都能在快速的高速缓存中完成。

![img](https://hansimov.gitbook.io/csapp/~gitbook/image?url=https%3A%2F%2F4154149387-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MHt_spaxGgCbp2POnfq%252F-MHzgjEHwVipe3eiOGOu%252F-MHzhPegzo92o0T7BqNJ%252F01-08%2520%25E9%25AB%2598%25E9%2580%259F%25E7%25BC%2593%25E5%25AD%2598%25E5%25AD%2598%25E5%2582%25A8%25E5%2599%25A8.png%3Falt%3Dmedia%26token%3Dc8cb0d2f-bb3c-4a99-8c16-b9db3835a0c4&width=768&dpr=4&quality=100&sign=58c23aa8&sv=2)

> 图 1-8 高速缓存存储器



## 1.6 存储设备形成层次结构

在处理器和一个较大较慢的设备（例如主存）之间插**入**一个更小更快的存储设备（例如高速缓存）的想法已经成为一个普遍的观念。**存储器层次结构**，如图 1-9 所示。

![img](https://hansimov.gitbook.io/csapp/~gitbook/image?url=https%3A%2F%2F4154149387-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MHt_spaxGgCbp2POnfq%252F-MHzhX2vrq6mFP3tN9EU%252F-MHzi1Edm9hUsnQdAkkL%252F01-09%2520%25E4%25B8%2580%25E4%25B8%25AA%25E5%25AD%2598%25E5%2582%25A8%25E5%2599%25A8%25E5%25B1%2582%25E6%25AC%25A1%25E7%25BB%2593%25E6%259E%2584%25E7%259A%2584%25E7%25A4%25BA%25E4%25BE%258B.png%3Falt%3Dmedia%26token%3Dafb8208e-17dc-475f-9f61-acf0bd0ca891&width=768&dpr=4&quality=100&sign=fa795169&sv=2)

> 图 1-9 一个存储器层次结构的示例

存储器层次结构的主要思想是上一层的存储器作为低一层存储器的高速缓存。因此，寄存器文件就是 L1 的高速缓存，L1 是 L2 的高速缓存，L2 是 L3 的高速缓存，L3 是主存的高速缓存，而主存又是磁盘的高速缓存。

## 1.7 操作系统管理硬件

让我们回到 hello 程序的例子。当 shell 加载和运行 hello 程序时，以及 hello 程序输出自己的消息时，shell  和 hello 程序都没有直接访问键盘、显示器、磁盘或者主存。取而代之的是，它们依靠操作系统提供的服务。我们可以把操作系统看成是应用程序和硬件之间插入的一层软件，如图 1-10 所示。所有应用程序对硬件的操作尝试都必须通过操作系统。

![img](https://hansimov.gitbook.io/csapp/~gitbook/image?url=https%3A%2F%2F4154149387-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MHt_spaxGgCbp2POnfq%252F-MHzi6gd3YrHlZtNiGnn%252F-MHziSfWGuHFYHC0c9kb%252F01-10%2520%25E8%25AE%25A1%25E7%25AE%2597%25E6%259C%25BA%25E7%25B3%25BB%25E7%25BB%259F%25E7%259A%2584%25E5%2588%2586%25E5%25B1%2582%25E8%25A7%2586%25E5%259B%25BE.png%3Falt%3Dmedia%26token%3D4166d09b-61ba-4d06-a185-2484867737fe&width=768&dpr=4&quality=100&sign=851f8e7f&sv=2)

> 图 1-10 计算机系统的分层视图

操作系统有两个基本功能∶（1）防止硬件被失控的应用程序滥用；（2）向应用程序提供简单一致的机制来控制复杂而又通常大不相同的低级硬件设备。操作系统通过几个基本的抽象概念（**进程**、**虚拟内存**和**文件**）来实现这两个功能。如图 1-11 所示，**文件是对 I/O 设备的抽象表示，虚拟内存是对主存和磁盘 I/O 设备的抽象表示，进程则是对处理器、主存和 I/O 设备的抽象表示**。我们将依次讨论每种抽象表示。

![img](https://hansimov.gitbook.io/csapp/~gitbook/image?url=https%3A%2F%2F4154149387-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MHt_spaxGgCbp2POnfq%252F-MHzi6gd3YrHlZtNiGnn%252F-MHzibk5fcFFoAm7QN5W%252F01-11%2520%25E6%2593%258D%25E4%25BD%259C%25E7%25B3%25BB%25E7%25BB%259F%25E6%258F%2590%25E4%25BE%259B%25E7%259A%2584%25E6%258A%25BD%25E8%25B1%25A1%25E8%25A1%25A8%25E7%25A4%25BA.png%3Falt%3Dmedia%26token%3D27d4ecf7-428d-43ca-a754-c841c327f065&width=768&dpr=4&quality=100&sign=18e37188&sv=2)

> 图 1-11 操作系统提供的抽象表示

### 1.7.1 进程

**进程**是操作系统对一个正在运行的程序的一种抽象。在一个系统上可以同时运行多个进程，而每个进程都好像在独占地使用硬件。而**并发运行**，则是说一个进程的指令和另一个进程的指令是交错执行的。在大多数系统中，需要运行的进程数是多于可以运行它们的 CPU 个数的。传统系统在一个时刻只能执行一个程序，而先进的**多核**处理器同时能够执行多个程序。无论是在单核还是多核系统中，一个 CPU 看上去都像是在并发地执行多个进程，这是通过处理器在进程间切换来实现的。操作系统实现这种交错执行的机制称为**上下文切换**。为了简化讨论，我们只考虑包含一个 CPU 的**单处理器系统**的情况。

操作系统保持跟踪进程运行所需的所有状态信息。这种状态，也就是**上下文**，包括许多信息，比如 PC 和寄存器文件的当前值，以及主存的内容。在任何一个时刻，单处理器系统都只能执行一个进程的代码。当操作系统决定要把控制权从当前进程转移到某个新进程时，就会进行上下文切换，即保存当前进程的上下文、恢复新进程的上下文，然后将控制权传递到新进程。新进程就会从它上次停止的地方开始。图 1-12 展示了示例 hello 程序运行场景的基本理念。

示例场景中有两个并发的进程∶shell 进程和 hello 进程。最开始，只有 shell 进程在运行，即等待命令行上的输入。当我们让它运行 hello 程序时，shell 通过调用一个专门的函数，即系统调用，来执行我们的请求，系统调用会将控制权传递给操作系统。操作系统保存 shell 进程的上下文，创建一个新的 hello 进程及其上下文，然后将控制权传给新的 hello 进程。hello 进程终止后，操作系统恢复 shell 进程的上下文，并将控制权传回给它，shell 进程会继续等待下一个命令行输入。

如图 1-12 所示，从一个进程到另一个进程的转换是由操作系统**内核**（kernel）管理的。内核是操作系统代码常驻主存的部分。当应用程序需要操作系统的某些操作时，比如读写文件，它就执行一条特殊的**系统调用**（system call）指令，将控制权传递给内核。然后内核执行被请求的操作并返回应用程序。注意，内核不是一个独立的进程。相反，它是系统管理全部进程所用代码和数据结构的集合。

![img](https://hansimov.gitbook.io/csapp/~gitbook/image?url=https%3A%2F%2F4154149387-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MHt_spaxGgCbp2POnfq%252F-MHzietxOnE2XCBTZHJ7%252F-MHziqrI26zhGJA9_NLL%252F01-12%2520%25E8%25BF%259B%25E7%25A8%258B%25E7%259A%2584%25E4%25B8%258A%25E4%25B8%258B%25E6%2596%2587%25E5%2588%2587%25E6%258D%25A2.png%3Falt%3Dmedia%26token%3D5df4420b-e3c7-46ca-8192-ba207aa49f15&width=768&dpr=4&quality=100&sign=52927021&sv=2)

> 图 1-12 进程的上下文切换

### 1.7.2 线程

一个进程实际上可以由多个称为**线程**的执行单元组成，每个线程都运行在进程的上下文中，并共享同样的代码和全局数据。

### 1.7.3 虚拟内存

虚拟内存是一个抽象概念，它为每个进程提供了一个假象，即每个进程都在独占地使用主存。每个进程看到的内存都是一致的，称为虚拟地址空间。

![img](https://hansimov.gitbook.io/csapp/~gitbook/image?url=https%3A%2F%2F4154149387-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MHt_spaxGgCbp2POnfq%252F-MHzietxOnE2XCBTZHJ7%252F-MHzj3WC193cZbjm8s4A%252F01-13%2520%25E8%25BF%259B%25E7%25A8%258B%25E7%259A%2584%25E8%2599%259A%25E6%258B%259F%25E5%259C%25B0%25E5%259D%2580%25E7%25A9%25BA%25E9%2597%25B4.png%3Falt%3Dmedia%26token%3De75f285a-1895-46f5-83fd-e8857326e5f9&width=768&dpr=4&quality=100&sign=9bb5707b&sv=2)

> 图 1-13 进程的虚拟地址空间

**程序代码和数据。**

**堆。**

**共享库。**

**栈。**

**内核虚拟内存。**

### 1.7.4 文件

**文件**就是字节序列，仅此而已。每个I/O设备，包括磁盘、键盘、显示器，甚至网络，都可以看成是文件。系统中的所有输入输出都是通过使用一小组称为 Unix I/O 的系统函数调用读写文件来实现的。

## 1.8 系统之间利用网络通信

从一个单独的系统来看，网络可视为一个 I/O 设备，如图 1-14 所示。当系统从主存复制一串字节到网络适配器时，数据流经过网络到达另一台机器，而不是比如说到达本地磁盘驱动器。相似地，系统可以读取从其他机器发送来的数据，并把数据复制到自己的主存。

![img](https://hansimov.gitbook.io/csapp/~gitbook/image?url=https%3A%2F%2F4154149387-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MHt_spaxGgCbp2POnfq%252F-MHzkHZkDb2pAz2UFj6L%252F-MHzkTNuWPTixbySY2Yc%252F01-14%2520%25E7%25BD%2591%25E7%25BB%259C%25E4%25B9%259F%25E6%2598%25AF%25E4%25B8%2580%25E7%25A7%258DIO%25E8%25AE%25BE%25E5%25A4%2587.png%3Falt%3Dmedia%26token%3Dd06f41b1-bb97-42e1-ad6d-35f30948cbc7&width=768&dpr=4&quality=100&sign=43c91eea&sv=2)

> 图 1-14 网络也是一种 I/O 设备

![img](https://hansimov.gitbook.io/csapp/~gitbook/image?url=https%3A%2F%2F4154149387-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MHt_spaxGgCbp2POnfq%252F-MHzkHZkDb2pAz2UFj6L%252F-MHzkrKbCqYNGK5oVdPg%252F01-15%2520%25E5%2588%25A9%25E7%2594%25A8telnet%25E9%2580%259A%25E8%25BF%2587%25E7%25BD%2591%25E7%25BB%259C%25E8%25BF%259C%25E7%25A8%258B%25E8%25BF%2590%25E8%25A1%258Chello.png%3Falt%3Dmedia%26token%3D686ab00c-5876-4a23-bb52-6760cdc22591&width=768&dpr=4&quality=100&sign=b4e50bfe&sv=2)

> 图 1-15 利用 telnet 通过网络远程运行 hello

## 1.9 重要主题

**系统不仅仅只是硬件。系统是硬件和系统软件互相交织的集合体，它们必须共同协作以达到运行应用程序的最终目的。**

### 1.9.1 Amdahl 定律

该定律的主要思想是，当我们对系统的某个部分加速时，其对系统整体性能的影响取决于该部分的重要性和加速程度。

### 1.9.2 并发和并行

术语**并发**（concurrency）是一个通用的概念，指一个同时具有多个活动的系统；而术语**并行**（parallelism）指的是用并发来使一个系统运行得更快。

1. 线程级并发

2. 指令级并行

3. 单指令、多数据并行

## 1.10 小结

计算机系统是由硬件和系统软件组成的，它们共同协作以运行应用程序。计算机内部的信息被表示为一组组的位，它们依据上下文有不同的解释方式。程序被其他程序翻译成不同的形式，开始时是 ASCII 文本，然后被编译器和链接器翻译成二进制可执行文件。

处理器读取并解释存放在主存里的二进制指令。因为计算机花费了大量的时间在内存、I/O 设备和 CPU 寄存器之间复制数据，所以将系统中的存储设备划分成层次结构——CPU 寄存器在顶部，接着是多层的硬件高速缓存存储器、DRAM 主存和磁盘存储器。在层次模型中，位于更高层的存储设备比低层的存储设备要更快，单位比特造价也更高。层次结构中较高层次的存储设备可以作为较低层次设备的高速缓存。通过理解和运用这种存储层次结构的知识，程序员可以优化C程序的性能。

操作系统内核是应用程序和硬件之间的媒介。它提供三个基本的抽象∶1）文件是对 I/O 设备的抽象；2）虚拟内存是对主存和磁盘的抽象；3）进程是处理器、主存和 I/O 设备的抽象。

最后，网络提供了计算机系统之间通信的手段。从特殊系统的角度来看，网络就是一种 I/O 设备。
