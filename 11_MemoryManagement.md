# 内存管理

D默认使用垃圾收集来分配堆数据。这提供了一个安全的默认设置，您可以安全编程而无需担忧像Java那样的内存问题。

>警告
由于您正在阅读本教程，您显然不是经验丰富的D程序员。 **使用垃圾收集器**！ 本节的其余部分用于展示可用的操作。

# 垃圾收集器调整

但是，（仔细分析后），您可能会发现垃圾收集器成为瓶颈。在这种情况下，D提供了各种方法来解决这个问题。

您可以避免代码的热点在堆栈中进行分配。无论您使用什么样的内存管理，都不用担心，这可能是一个好主意。

*我也避免在D中使用GC，并且从来没有遇到过任何重大问题，只有不便。有策略地使用，在某些情况下它可以很方便，就像在C ++中一样，如果您的特定用例禁止使用GC，则可以避免。我对D的实际经验表明，我总能达到与C ++相同的性能 - [Manu Evans](http://www.reddit.com/r/programming/comments/1nxs2i/the_state_of_rust_08/ccnefe7)*


您也可以暂时禁用垃圾回收。

    std.gc.disable()
    //在这里没有垃圾回收，只有分配
    std.gc.enable()


相反的解决方案是使垃圾收集器适应应用程序。D允许应用程序替换垃圾回收器，并针对特定场景进行优化。当前的垃圾收集器优化不是太好，不如JVM的Hostpot。

# 手动内存管理

如果所有这些方法都无济于事，仍可使用手动内存管理。但是，大多数标准库依赖于垃圾回收。

使用destroy进行手动释放内存。您也可以直接访问malloc，并从libc中释放，从而绕过垃圾收集器。D为C++提供了RAII的所有机制。

没有编译器支持引用计数。但是，您可以使用标准库中的[RefCounted](http://dlang.org/phobos/std_typecons.html#.RefCounted)。它透明地包装了类型和计数引用。通过直接使用malloc和free，来避免垃圾收集器回收。

>参考
[内存管理](http://dlang.org/memory.html)， [std.typecons.RefCounted](http://dlang.org/phobos/std_typecons.html#.RefCounted)