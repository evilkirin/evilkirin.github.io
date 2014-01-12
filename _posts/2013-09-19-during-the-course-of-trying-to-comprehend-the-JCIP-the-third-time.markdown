---
layout: post
category: thoughts
title: Trying to comphrehend the JCIP the third time
---

As I said, this is the third time that I take the book up again and try to digest'em all. Well, I got a feeling that this time I will kick its ass.

When I first got this book, it dates back to when I was junior in my college. After I **finish** it, to be honest, I got no more than a bunch of entangled mess left in my head. And I even thought I've achieved something. Isn't that funny.

And the second time me I pick it up, my english reading is much better and I've become a postgraduate(nothing to be really proud of). Still, the second I close it up, I was quite sure that soon there must be another iteration in this great cause.

好了，回到现在。我是在反复阅读了大量的对我们Java程序员来说相对底层的概念和文章之后，重新拿起的这本书，哦对了，[操作系统概念][1]也让我受益匪浅.

回到这本书。在慢慢中英对比阅读的过程中，我惊讶（或不惊讶？）地发现，我所有这两个月阅读到的东西或多或少其实在这本书里边都有提到，只是我当时看到了也不明白而被大脑选择性过滤了。

在第十六章[Java内存模型][2]的第一个小节有这么一句话：

> **Compilers** may generate instructions in a different order thant the "obvious" one suggested by the source code, or store variables in registers instead of in memory; **processors** may execute instructions in parallel or out of order; **caches** may vary the order in which writes to variables are committed to main memory; and values sotred in **processor-local caches** may not be visible to other processors.

这句话中包含的内容pretty much includes everything I read about in the last two to three month.

先不说什么“温故而知新”，当然，从绝对意义上来说也不可能是我变聪明了，以至于我看懂了。而实际上是因为：I get to know something under the hood which let me see through these convoluted english sentences to really take in these essential concepts and their subtle tangled relationships.

之前有学弟学妹问过我，JCIP怎么样，是否适合入门。当时我应该是处在第二次尝试刚开始的阶段，而且是刚**看完了**[TIJ][5]关于并发的内容。我记得当时也就轻描淡写的说明不合适，其实自己心中也没有太肯定的知道为什么或那些部分不容易看懂。从今天看来，真的是有很大一部分我是没有抓住重点的，因为太多的背景只是我不知道，亦或是理解程度不够。

说到理解程度，必须要提到一本书：“[The 5 elements of effective thinking][3]”。虽然我没有看过这本书（我有去下载这本书的音频版，然后听了半个小时，然后就没有然后了），但是我在google上搜到一篇[review][4]，不得不说，just a glance，其中的睿智已让人折服。

此书重点描绘的五个元素（elements）中的第一个是understand deeply，浅显而深刻的一个概念。下面一句话来自于[review][4]：

> There are degrees to understanding (it's not just a yes-or-no proposition) and you can always heighten yours.

看过了（看完了）多少书其实并不重要，重要的是留下了什么。而想要真正吸收一本书（一本真正的好书是很难被完全吸收的，70~80%已足以向人炫耀），深入的理解是必须的。不断的拆解分析，找到自己不知道的基础部分然后加以学习，才能最终理解构建于这些基础的更高层次的知识。下面这句话还是来自[The 5 elements of effective thinking][3]:

> True experts continually deepen their mastery of the basics. 

中国从小就熟知的温故而知新，真的是被大多数人选择性遗忘了。其中部分人是因为没有理解它的真正含义，而更多的人是没有意识到它的重要性。这篇文章的内容基本都是来自于之前的三篇日记，现在整理一下，依旧是获益良多。

接下来应该是记录我这两个月的阅读收获了。我之前一直想不好怎么去记录。养成了习惯在笔记本上进行摘抄然后反复阅读，然后写点评语的。这个办法在这种情况下不是很行得通。其实我在整个过程中是有很多困惑的，阅读文章实际也是带着疑问的，有时候会突然发现自己的理解并不对，从而刺激了下一次探索。正好结合[The 5 elements of effective thinking][3]中提到的第二个和第三个元素：make mistakes & raise questions，我决定用一路过来心中带着的疑问作为引导来记述。



[1]:http://book.douban.com/subject/4746437/ "Operating System Concepts with Java"
[2]:http://www.cs.umd.edu/~pugh/java/memoryModel/jsr-133-faq.html "JSR 133 (Java Memory Model) FAQ"
[3]:http://book.douban.com/subject/10814727/ "The 5 Elements of Effective Thinking"
[4]:http://casnocha.com/2013/06/book-review-the-5-elements-of-effective-thinking.html "review"
[5]:http://book.douban.com/subject/2130190/ "Java编程思想 （第4版）"
