---
layout: post
title:  "Dynamic proxies once again"
categories: readsomecode
---

动态代理玩出花

这次看的代码来自Dominic Fox的[New Tricks with Dynamic Proxies in Java 8 ](https://opencredo.com/dynamic-proxies-java/)，作者围绕动态代理构建起了一个能够快速创建mock对象和matcher对象的小框架。有点想不起来为啥会去找这个主题的文章，应该是跟提升测试效率有关系。不过我也好久没有去关注这块儿内容了，之前也为编写代码创建测试数据的效率问题搜索了好久（找到，还实际用过[npryce/make-it-easy: A tiny framework that makes it easy to write Test Data Builders in Java](https://github.com/npryce/make-it-easy)），与这个倒也不是一个方向上事情。

除了重新熟悉动态代理，这篇文章里边值得学习的点很多：

1. responsibility chain，结合函数式的编程方式，通过方法参数来组装chain。当然，也有别的方法，可以去探究一下。
2. 关于Java 8中default method和static method for interface的应用
3. 作者对函数的使用是真的很6，函数是参数，也是返回值，不停地嵌套。说实话，虽然之前看了那么多关于函数式的文章和书（全忘了），javascript也学了也用了，什么Generator、Promise、Continuation这些东西也都有理解（是真的理解），The little schemer也看了一遍了，还是看得我一愣一愣的。很有趣是真的。
4. 作者很擅长将一个流程拆分成两个，然后分别封装，以提升灵活性，满足不同的需求。当然，易读性也会差一些。。
    - 将InvocationHandler所做的事情分成了两部分，判断和处理。然后基于此进行抽象，判断逻辑是一个接口，返回的处理逻辑也是一个接口。在将判断逻辑组装成chain时，还能再插入拦截器，这都是套路啊。
    - 在“using dynamic proxies to create bean-like value objects to represent records”时，将bean的解析与这部分信息的使用解开，这样解析bean得到的信息就能存下来反复使用。这里边也用到了一些函数式编程的东西，我现在虽然能理解，但是理解程度并不高。我觉得应该在真的需要的时候再回来看看。

里边还提到:

> Remoting has fallen somewhat out of favour in recent years, as developers have come to understand that method call dispatch and sending a request over the network have fundamentally different semantics and failure modes, but dynamic proxies are still in the language.

之前看的一篇讲解REST的文章，[理解本真的REST架构风格](http://www.infoq.com/cn/articles/understanding-restful-style)，正好讲到了不同类型的分布式架构风格，分别是DO、RPC和REST。这个地方讲的removing应该主要指的是RPC，整合HSF就是用动态代理来做的。难道作者的意思我们应该从RPC切换到REST模式？

文章中用到了较多Java特色的函数式编程的技巧：

1. 将方法引用传来传去，还作为方法返回值从方法中返回（实际的返回值是一个接口）
2. curry，虽然在JS中用过，但还是花了些时间来理解
3. 最后一个，让我理解了好久，感受一下：

```java
@FunctionalInterface
public interface ClassInterpreter<T> {

    static <T> ClassInterpreter<T> cached(ClassInterpreter<T> interpreter) {
        return Memoizer.memoize(interpreter::interpret)::apply;
    }

    static <T> ClassInterpreter<T> mappingWith(UnboundMethodInterpreter<T> interpreter) {
        return iface -> TypeInfo.forType(iface)
                .streamNonDefaultPublicInstanceMethods()
                .map(MethodInfo::getMethod)
                .collect(Collectors.toMap(
                        Function.identity(),
                        interpreter::interpret
                ))::get;
    }

    UnboundMethodInterpreter<T> interpret(Class<?> iface);

}

@FunctionalInterface
public interface UnboundMethodInterpreter<S> {

    UnboundMethodCallHandler<S> interpret(Method method);

    default MethodInterpreter bind(S state) {
        return method -> interpret(method).bind(state);
    }
}
```

为啥ClassInterpreter#mappingWith返回的lambda的最后一句是返回的Map的get方法？

文章的第二三部分：

- [New Tricks with Dynamic Proxies in Java 8 (part 2) - OpenCredo](https://opencredo.com/dynamic-proxies-java-part-2/)
- [New Tricks With Dynamic Proxies In Java 8 (part 3) - OpenCredo](https://opencredo.com/new-tricks-with-dynamic-proxies-in-java-8-part-3/)