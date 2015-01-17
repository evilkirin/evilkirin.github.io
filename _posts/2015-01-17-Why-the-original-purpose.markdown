---
layout: post
title:  "'Why', the original purpose"
categories: thoughts
---

# I read a lot books, without knowing why

当初看的那些书（各种语言，各种技术），花了那么多时间，到最后还真的没有留下太多实质性的东西。能够沿用到现在（daily）的应该只有emacs了。Why？

那个时候貌似除了打游戏，就是看书了把。我本身又是一个比较喜欢独处的人，同时又是一个喜欢看书的人。但，仅仅是看，偶尔去做，而且还是做完就忘的人。

# I start to realize what's the knowledge I need

今年是正式工作第一年，也有了女朋友，看书的时间基本是保持一个稳步下降的趋势。

But better than never, 我突然明白了自己现在最需要的知识是什么。其实我在研究生毕业之时，就想到过，我当时面试没有自信的原因是什么，那是因为我写的代码太少了啊。真的是太少了。

开始工作之后，写（or copy from somewhere else）的代码越来越多，总算是发现了正当下最需要的知识，**how to improve the quality of my code**.

前前后后还是看了好几本。"Code Complete","Clean code","The art of readable code","working effective with legacy code","refactor"。也开始关心TDD了，甚至在后来的一个小系统开发里边实践了一下。

但是到后来，我发现做这些事情的动力慢慢消退了，而且能够找来的理由也很多，越来越难说服自己。我这个时候又开始想，我是不是并不真的明白自己为什么要做这些事儿？

![Why?][1]

# Then there comes the 'one' article

具体的来源不太记得了，好像是Hacker News。最开始是一篇名为"[10 questions web developers should ask themselves every day][2]"的文章。里边好几个观点都让我觉得深有同感，同时，asking you yourself questions every day kind of resembles the idea, which I called 'preload context', I was thinking about quite a while ago. 后来，在搜索这个作者（Stephen C. Young）的blog的过程中，bump into另一篇文章，"[Why your code is so hard to understand][3]"，可以这么说，好多东西突然就都串了起来。

这篇文章主要是在回答一个问题，为什么自己精心编写的代码，过了几个星期或几个月自己就完全无法理解了？从某些方面来说，这跟你去看别人的代码也是一个道理。

文章把整个问题解决过程分解为几个阶段：提出问题（problem rises），理解问题（mental model），构建解决方案（semantic model），实际的编码工作（syntax model），最后得到实际的系统（Running System）。

![The entire process][4]

从理论上来说，这三个model所要表达的意思应该是完全一样的。但是，且不去谈最开始形成的mental model有没有真正去解决The inital problem，每个阶段在尝试着将上一阶段形成的model翻译（translate）成当前阶段需要的model时，所需要的上下文是不同的。你需要去整理收集大量的信息（也有来自上一阶段的）放进你的working memory，然后在这些知识或想法的帮助下，你通过努力思考得出了这一阶段的产物，即你构建出来的model，which will then成为下一阶段前进的基础。在解决问题的整个这么一个‘漫长的’阶段里边，你会处理大量的信息，去芜存菁，最后留下了代码。但你真的确定你能够在看到代码的那一瞬间回想起**初心**么？

说实话，也不用初心，对于别人的代码，或者“一个月以前的自己”写出的代码，你能够构建出一个不错的semantic model已经是谢天谢地了。所以，Young给出了他的观点：

> Your task in translating a semantic model into syntax is to try and leave as many clues as possible that will allow you to rebuild the semantic model when you come back at a later time.

至于如何去做到能够留下足够多的线索，那篇文章里边已经写的足够多了，这里就不赘述了。

至于我突然理解到的东西是：

1. 我之前学的那么多语言，全部都是在学习syntax model。其实也是在一种model都没有能够精通的情况下，就急于跳到下一种model。有点类似于今天在读"Tripple Your Reading Speed"里边提到的Progression，其实属快速阅读的一大阻碍。也许，我也可以把不同的语言看做一本变成语言集大成之作中的一个chapter，在浏览了一遍大部分chapters之后，现在应该回过头好好地把java这一章internalize一下了。
2. 了解syntax model，距离具备将semantic model流畅地翻译成syntax model的能力还差的很远。学过英语的人或多或少都能够将一句比较简单的英语翻译为中文，反之亦然，但是如何才能做到读者观罢会心一笑呢？

# OK, what then?

Involve yourself, indulge yourself.

[1]:/pic/why.jpeg
[2]:http://aestheticio.com/10-questions-developers-should-ask-themselves/
[3]:http://aestheticio.com/why-your-code-is-hard-to-understand/
[4]:/pic/processes.png