# 浅谈 WebAssembly

> 作者：Lin Clark
> 英文原文：[A cartoon intro to WebAssembly](https://hacks.mozilla.org/2017/02/a-cartoon-intro-to-webassembly/)

WebAssembly的发展很快，你可能已经听说过它了。但是什么让它发展的如此之快？

在这个系列中，我会向你解释WebAssembly为什么会发展的如此之快。

## 那么，WebAssembly是什么呢？

WebAssembly是一种用JavaScript以外的编程语言编写代码，并在浏览器中运行该代码的一种方法。所以在与JavaScript比较下，人们认为WebAssembly相对发展更快速。

我不想表明它是一个只能使用WebAssembly或JavaScript其中之一的情况。事实上，我们期望的是开发人员可以再在同一个应用程序中使用WebAssembly和JavaScript。

但是比较这两者是有用的，因为你可以了解WebAssembly的潜在影响。

## 一些性能历史

JavaScript创建于1995年，他的设计不是为了性能更快速。所以在被设计出的前十年里，它的性能和发展并不快速。

之后浏览器开始变得更具有竞争力。

2008年的时候，被人们称为的性能大战开始了。许多浏览器增加了即时编译器，也被称为JITs。当JavaScript运行时，JIT可以看到模式并使代码在基于这些模式里运行更快。

这些JIT的引入导致了JavaScript的性能拐点。JS的执行速度快了10倍。

![webassembly-01](http://7xqj3u.com1.z0.glb.clouddn.com/webassembly-01.png)

通过这种性能的改进，JavaScript开始被用于期初没有被期望用于的场景。例如用于服务端编程的Node.js。这种性能的提高使得在一个全新的类的问题上使用JavaScript变成了可能。

现在我们可能有面临另一个WebAssembly的拐点。

![webassembly-02](http://7xqj3u.com1.z0.glb.clouddn.com/webassembly-02.png)

因此，让我们深入了解细节，了解什么使WebAssembly更快。

### 背景相关知识
* [JavaScript Just-in-time (JIT) 工作原理](https://zhuanlan.zhihu.com/p/25669120)
* [编译器如何生成汇编](https://zhuanlan.zhihu.com/p/25718411)

### 当前WebAssembly 相关进展
* [Creating and working with WebAssembly modules](https://hacks.mozilla.org/2017/02/creating-and-working-with-webassembly-modules/)
* [What makes WebAssembly fast?](https://hacks.mozilla.org/2017/02/what-makes-webassembly-fast/)

### WebAssembly的未来发展
* [Where is WebAssembly now and what’s next?(翻译中)](https://hacks.mozilla.org/2017/02/where-is-webassembly-now-and-whats-next/)