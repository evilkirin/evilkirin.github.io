---
layout: post
category: thoughts
title: All about Communication
---

# The trigger

Well, things started when I jump into a book named [What Every Programmer Should Know about Memory][1], be it arranged like a essay I'd rather call it a book. But it's (not just) a little beyond me, with a bunch of concepts and their relationships just blow me off my mind.

Then, I started to search the internet. And I digged up some treasure chests thereafter. Some of them are list below:

 1. [Mechanical Sympathy][2]: this one leads my first dive into this grand sea and full of brand new stuff to rack my brains. The author Martin Thompson is one of the member of the developer group which develops [LMAX][3], a new retail financial trading platform which is build upon the JVM platform. A great programming artist.
 2. [Preshing on Programming][4]: this one clears my head a little bit, but not entirely. Almost all of the articles here talk about things in C++, but I found it not that hard to take in. Again, a lot of hard stuff.
 3. [Bartosz Milewski's Programming Cafe][5]:I don't read many things here, but the ones I read clear much of my confusion at that time.
 4. [Psychosomatic, Lobotomy, Saw][6]:this one is kinda new compared to the above ones, and I found its way of explanation tends to be more acceptable(Eh, may be it's bacause of me being smarter? Unlikely.). I guess the author might be a younger bro.
 
I shall keep this list updated, and blogspot is in the list of sites to be blocked by the GFW. So you might need a goagent or something handy.

这就是我前边两个多月逛的地方，当然还有就是最最伟大的[Wikipedia][7]了。

# Ah ha!

所有我看的这些内容，都是想要让自己的并发编程基础更为扎实。而并发编程中最难以让人理解的部分就是对共享变量的访问，以及对这些访问的管理。单线程的程序不需要这些，因为只有一个人的时候，自己是boss，用不着在乎别人。

在我看了这么多文章之后，我发现，**一个理解并发或者多线程更好的角度是从线程之间的通信入手**：线程们（更确切的说应该是CPU）合作完成任务免不了要交流数据，但这个交流体现在编程语言中并非是通过具体的语言层级的类似于消息收发式的接口（__类似于 actor  model__），而仅仅就是状态的共享（内存共享），这是很奇怪的。

> 通信并非只包含数据通信，还包括状态通知。如Java中，在一个线程调用wait方法阻塞了之后，另一个线程可以通过调用notify或notifyAll来通知其可以回到Ready状态。在java.util.concurrent包中的LockSupport类中使用了Unsafe.park()和unpark()方法来完成相同的功能。不过这里我只想讨论数据通信，如果说得更直观一点，讨论的前提应该是在[Lock-free][23]的程序。

虽然线程这个概念模型在语言级别有Thread类（对象）来表示，但在操作系统层面线程是按照一定的顺序执行指令的一个mental model，在Java中即为方法的执行，底层则是JVM对字节码的解释外加JIT的即时编译执行。如果将线程间的communication与对象的状态本身分离开来理解，应该会有更清晰的认识。

![Memory Communication][21]

刚开始接触多线程的时候，我一直没有真正搞清楚线程之间协作执行是怎么样一种机制（这个理解并非是表层的理解），甚至是完全没有意识到协作这件事情（总是被各种状态的推导折磨）。现在看来，其实是其本身有些counter-intuitive，因为语言层级的状态与操作系统层面上线程间的交互tangle在了一起。了解到了这一点，再加上后来接触到底层那些为了使这个通信正确和及时完成的机制时，我豁然开朗了：**Communication才是根本，其他细节都是在为这个目的服务**。

> 人脑对于为什么做一件事情（cause）总是更为敏感的，而对于怎么做和做什么（how & what）是相对迟钝的。from [How great leaders inspire action][8]

几乎所有我之前看过的书关于多线程编程，强调的都是**线程状态转换，以及线程内部状态的管理**，这是Java，C++这些面向对象语言对于线程间通信和协调的实现手段，也与底层实现方式内存共享直接契合，这无可厚非。同时，在缺少底层的概念的情况下，要把整个通信的流程或细节讲清楚也不太可能。我的理解是，大师们将范围限制在语言层级，至多在包含应用层的内存模型（如JVM的内存模型），能够隐藏其本身一些“不太必要的细节”。这样一来，线程的协作与通信从一定程度上来说，也能比较准确的描述出来。不过当我看到一些描述的时候总会觉得缺少了点什么，这也是最初促使我去探索的地方。

我总觉得将线程间的通信与对象的状态混为一谈让人迷惑，因为线程之间真实想做的事情是通信，以与其他线程协同完成我们赋予的任务。Likewise，底层的内存共享它也就是一种通信机制而已。

如果不是因为最初的面向对象模型被大幅度修改，对象之间变得能够互相访问修改状态，现今的面向对象系统的运作模式应该更类似于[actor model][17]，并发编程也不会那么让人抓狂了。

当然，这里所描述的内容也只能算是hindsight。如果一早有人给我指出来，也许我也不会走那么多弯路。不过话说回来，弯路才是真正磨练人的地方。

# Communication in Memory

接下来，我会尝试将我学到的一些概念串起来，比较粗略地去解释为了完成处理器核心之间的通信，处理器与Cache子系统都做了哪些努力。这些细节，正是我之前在学习并发编程中形成疑惑的根源。因为概念实在太多，我肯定没有办法全部提到。我写下来最主要的原因还是在于促进自己思考，人的思维是比较碎片化的，但是我会努力让他们能够不那么零散。

我们知道，比较常见的[进程间通信][9]有很种，如内存共享，socket，semaphore以及messageQueue。而线程间通信靠的就是内存共享了，确切的说应该是整个内存系统提供的数据共享。这个系统包括了main memory，cache subsystem，buffers and registers.

![Memory Hierachy][11]

* 主存几十纳秒的访问速度早已不能满足CPU的需求。上图来自[Mechanical Sympathy][25].

* Cache子系统是最为核心的部分，这并不仅仅是因为cache比memory更快，**而是因为所有流向CPU的数据都必须经过cache，be it level 1, 2, or 3.**。这里所指经过是指CPU只处理存在在cache中的值，如果cache中没有那么会有一个cache-miss，然后触发数据从主存流向cache并保存其中。当代的处理器极为复杂的cache hierarchy以及其中无时无刻不在运行着的protocol（还包括prefetch）都在为了一件事情努力：减少与memory的交互，而这背后的原因当然是性能。
 
* 大家知道在计算机系统中memory是作为disk的缓存，cache是作为CPU与内存之间的缓存。而实际上在CPU于cache之间还存在着buffer以及register，为了进一步缩短数据交换的延迟。

既然是共享，而且是多个不同组成部分的共享，肯定会存在数据一致性的问题，而且因为多处理器和多核的出现，这不仅仅包括层级间的同步，还包括了同一层级间不同CPU核心上的[on-die Cache][10]之间的数据通信。Kind of like the [fractal (分形)][12]：不同的核心就类似于分布式系统中的不同节点，核心中的cache就是节点的内存（比较粗略的看）。这时，分布式中的一些概念与这里就是相通的了，如一致性和可用性，当然网络的partition是不太可能出现。

一致性体现在不同的核心当前保存的（同一份）数据是否一致，不过数据并不一定存在在cache中，更多的会保存在内存中。可用性体现在当前的读写操作是否能进行。对于读操作，可能因为当前本地的（cache中）数据无效（invalid）而导致读不能立即执行，也可能因为从本地buffer（store buffer）中直接响应读操作而直接拿到脏数据。而对于写操作则会出现由于当前其他核心正在并发写入，而本地对此数据写需要延迟。这部分内容是由[缓存一致性协议][13]来协调的。

这是相似的部分。另一方面，由于cpu issue操作的频率实在太快，程序对数据的变化极其敏感，这导致cache系统对数据通信的时序性要求相对较高。比如相关联的几个数据从一个核心的cache扩散到另一个核心，数据间顺序的不同将可能直接导致程序失败。而这个问题在编译器和处理器in the name of optimization，对指令的[重排序][10]后变得更为复杂。

这也并不奇怪。cache的出现是为了缩短CPU读取和操作内存中数据的时间，重排序也是为了性能，为了在出现cache-miss时能够先执行后边的指令。只不过，这在单线程中比较容易reasoning，而在多线程中却使得通信的合法性和结果变得更难推断。

为了方便对通信（内存同步）的行为进行推导，当然也是为了能够编写出行为正确且健壮的多线程程序，硬件层面（其实就是指处理器）会定义自己的内存模型（memory model）来规范重排序，规定不同的操作对内存施加的影响（还是为了通信）。这其中又根据硬件是否对[Sequential Consistency][14]进行原生支持来界定其[强弱程度][22]。在强弱程度不同的硬件内存模型中，还会定义一些手段（指令）来帮助（强制）实现SC，甚至是[Strict Consistency][15]。

> 这里插一句，在多线程的环境下，共享的数据如果没有采用任何约束手段（也就是说发生了数据竞争[JCIP.Ch16]），对其进行访问将观察到各种可能结果（你可能想象不到的也会包括）。也就是说这样的通信是失败的，即使在极小的概率下你的线程能拿到正确的数据。
> 虽然我们能借助一些‘工具’来规范通信的行为（实际就是让他们有序），我们并不想所有的数据操作都变成这样。我们还是希望我们的代码能够被优化以提升性能，这时程序中的操作间就会出现所谓的偏序（partial order）和全序（total order）[JCIP,Ch16]。
> 除了在通信的时候需要线程能够行为正常一些，其他时候随他们去把。

这个主要的手段就是[内存栅栏][16]（memory barrier）。它本身实际也是一种概念模型而非具体存在（也可能是我理解错了），根据用途能够分为[四种][18]。不过根据硬件内存模型的强弱，[并非所有种类][19]的栅栏都是需要的。有了内存栅栏，我们才能够真正写出结果能够预知的程序。按照我的说法，就是说我们所编写程序中的多个线程能够真正完成有效通信了。

# Conclusion

总算从communication的角度把这些东西串了起来，其实还有不少内容没有提到如：Java中JMM对通信的支持，硬件层面的内容等。我觉得那应该是下一篇文章的内容了。

这篇文章的内容很多很杂，主要还是自己没能真正驾驭住如此多的概念，并将他们通俗易懂的叙述出来。不过在写下这篇文章的过程中，我真正体会到了刘未鹏所说的“[书写是为了更好的思考][24]”，希望现在开始书写还不算晚。

下面这张图简单理了一下相关的一些概念，并分了一下类。部分已经在这里提到过了，其余的应该在下面一篇文章中涉及吧。

![Concepts][20]


[1]:http://people.freebsd.org/~lstewart/articles/cpumemory.pdf
[2]:http://mechanical-sympathy.blogspot.com/
[3]:http://martinfowler.com/articles/lmax.html
[4]:http://preshing.com/
[5]:artoszmilewski.com/
[6]:http://psy-lob-saw.blogspot.com/
[7]:http://www.wikipedia.org/
[8]:http://www.ted.com/talks/simon_sinek_how_great_leaders_inspire_action.html
[9]:http://en.wikipedia.org/wiki/Inter-process_communication#Main_IPC_methodsP
[10]:http://en.wikipedia.org/wiki/Cpu_cache#Multi-level_caches
[11]:/pic/memory_hierachy.png
[12]:http://en.wikipedia.org/wiki/Fractal
[13]:http://en.wikipedia.org/wiki/Cache_coherency
[14]:http://en.wikipedia.org/wiki/Sequential_consistency
[15]:http://en.wikipedia.org/wiki/Strict_consistency
[16]:http://en.wikipedia.org/wiki/Memory_barrier
[17]:http://en.wikipedia.org/wiki/Actor_model
[18]:http://preshing.com/20120710/memory-barriers-are-like-source-control-operations/
[19]:http://www.cs.umd.edu/~pugh/java/memoryModel/jsr-133-faq.html "JSR 133 (Java Memory Model) FAQ"
[20]:/pic/concepts.jpg
[21]:/pic/memory_communication.jpg
[22]:http://preshing.com/20120930/weak-vs-strong-memory-models/
[23]:http://preshing.com/20120612/an-introduction-to-lock-free-programming/
[24]:http://mindhacks.cn/2009/02/09/writing-is-better-thinking/
[25]:http://mechanical-sympathy.blogspot.co.uk/2013/02/cpu-cache-flushing-fallacy.html