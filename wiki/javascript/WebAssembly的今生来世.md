# WebAssembly的今世来生

> 作者：[Lin Clark](http://code-cartoons.com/)
> 英文原文：[Where is WebAssembly now and what’s next?](https://hacks.mozilla.org/2017/02/where-is-webassembly-now-and-whats-next/)

本文是WebAssembly系列的第六篇文章，如果你没有阅读过之前的文章，建议先从[这里开始阅读](https://hacks.mozilla.org/2017/02/a-cartoon-intro-to-webassembly/)

在2月28日，四大主流浏览器厂商宣布[WebAssembly的核心已经完成](https://lists.w3.org/Archives/Public/public-webassembly/2017Feb/0002.html)，并提供一个浏览器已经开始装载的稳定的初始版本。

![enter image description here](http://7xqj3u.com1.z0.glb.clouddn.com/webassembly601500.png)

它提供了一个浏览器可以运行的稳定的内核。这个内核并不包括社区计划的所有的功能，但它足以使WebAssembly快速和可用。

有了这个，开发者可以开始发布WebAssembly代码了。对于早期的浏览器版本，开发者可以上传一个asm.js版本的代码。因为asm.js是JavaScript的一个子集，任何JS引擎都可以运行它。通过Emscripten，你可以将同一个应用编译为WebAssembly和asm.js。

即使在最初的版本中，WebAssembly也可以运行很快。但是，通过修复和增加新功能可以使WebAssembly变得更快。

## 提升WebAssembly在浏览器中的性能

一些速度上的提升讲来自于浏览器引擎对于WebAssembly支持上的提升。浏览器厂商在独立处理这些问题。

### JS和WebAssembly之间更快的函数调用

目前，在JS代码中调用WebAssembly函数比它需要的要慢，那是因为，它必须做一些所谓的“蹦床”。JIT不知道如何直接处理WebAssembly，所以它必须通过WebAssembly路由到某处。这个引擎本身是一小段代码，它设置为运行优化后的WebAssembly代码。

![enter image description here](http://7xqj3u.com1.z0.glb.clouddn.com/webassembly602500.png)

这可能比如果JIT知道如何直接处理它的速度慢100倍。

如果你将一个独立的大任务传递到WebAssembly模块，你不会注意到这个性能消耗。但是如果你在WebAssembly与JS之间反复调用（就像你处理小任务似的），那么，这个性能消耗是显而易见的。

### 更快的加载时间

JIT必须在更快的加载时间和更快的执行时间之间进行权衡。如果你在编译和提前优化上花费很多时间，这加快了执行速度，但它减慢了启动速度。

有很多正在进行的工作去平衡前期的编译工作（确保在代码执行的时候没有Jank）。事实上，绝大部分代码并不值得花足够的时间去优化它。

由于WebAssembly不需要推测将要使用的类型，因此引擎不必担心在运行时监听类型。这为它们提供了更多的选项，例如将编译与执行并行。

此外，近期添加的JavaScript API将允许WebAssembly的流式编译。 这意味着引擎在字节仍在加载的时候就开始编译。

在火狐浏览器中，我们正在开发一个双编译系统。一个编译器将提前运行，并在代码优化方面做的相当不错。当运行代码时，另一个编译器将在后台进行优化，完全优化后的代码版本将在准备就绪时进行交换。

## 在规范中添加post-MVP功能

WebAssembly的目标之一就是制定小代码块和测试方式，而不是设计前面的一切。

这意味着有很多期望的功能，但并不是100%都经过深思熟虑。它们将必须通过规范的流程，所有的浏览器厂商都已经积极参与了。

这些功能称为未来功能。 我将介绍其中一小部分。

### 直接使用DOM

目前，没有办法与DOM交互。这意味着你不能像element.innerHTML那样在WebAssembly中更新一个节点。

相反，你必须通过JS去设置值，并将值传递回JavaScript的调用者。另一方面，可以在WebAssembly内调用JavaScript函数——JavaScript和WebAssembly函数都可以在WebAssembly模块中的引入。

![enter image description here](http://7xqj3u.com1.z0.glb.clouddn.com/webassembly603500.png)

无论哪种形式，通过JavaScript访问可能会比直接访问要慢。 WebAssembly的某些应用程序可能会被阻止，直到被解决。

### 共享内存并发

加速代码运行的一种方法是使代码的不同部分同时运行。这有时可能会造成Backfire，虽然由于线程之间通信的消耗要比一开始的任务进程更占用时间。

但是如果你可以在线程之间共享内存，它将会降低性能消耗。为此WebAssembly将使用JavaScript的新SharedFrayBuffer。一旦在浏览器中就位，工作组可以开始指定WebAssembly如何使用它们。

### 单指令、多数据（SIMD）

如果你阅读其他帖子或观看有关WebAssembly的讲座，您可能会听到SIMD支持。它代表单指令，多数据。 这是并行运行的另一种方式。

SIMD的使用可以获取大的数据结构，例如不同数目的向量，并且同时将相同的指令应用于不同的部分。这样，它可以大大加快各种复杂计算的游戏或VR的运行速度。

这对于一般的网络应用开发者来说并不是特别重要。但对于使用多媒体的开发人员来说非常重要，例如游戏开发者。

### 异常处理

许多语言像C++一样使用异常处理。但是，异常尚未指定为WebAssembly的一部分。

如果你使用Emscripten编译代码，它将模拟一些编译优化级别的异常处理。这是很慢，但你可以使用[DISABLE_EXCEPTION_CATCHING](https://kripken.github.io/emscripten-site/docs/optimizing/Optimizing-Code.html#c-exceptions)标记来关闭它。

一旦在WebAssembly本地处理了异常，这种仿真就不是必需的。

### 其他改进

未来的某些功能不会影响性能，但会使开发人员更容易使用WebAssembly。

* **源码开发工具**：目前，在浏览器中调试WebAssembly就像调试原始汇编。很少有开发人员可以将其源码映射到程序集。我们正在研究如何改进支持工具，以便开发人员可以调试其源码。
* **垃圾回收**：如果您可以提前定义类型，那么您应该可以将代码转换为WebAssembly。 所以使用类似TypeScript的代码应该可以编译到WebAssembly。目前唯一的困扰是，WebAssembly不知道如何与现有的垃圾收集器（如JS引擎中内置的垃圾收集器）进行交互。这个未来功能的想法是让WebAssembly使用一组低级GC类型和操作对内置GC进行访问。
* **ES6模块集成**：浏览器目前正在添加对使用script标签加载JavaScript模块的支持。 添加此功能后，即使URL指向WebAssembly模块，<script src = url type =“module”>等标签也可以工作。

## 结语

WebAssembly发展非常快，目前浏览器已经实现很多新功能和改进，它应该会发展的更快。

