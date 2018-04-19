
# 优化

使用命令行 dmd -release -inline -O。

dmd编译器确实生成了很好的代码，但它与更流行的编译器后端没有结合。因此，对于额外的10-20％的性能提升（或更多的微基准测试microbenchmarks），使用LDC或GDC。所有三个编译器使用相同的前端，但LDC使用LLVM作为后端，GDC使用GCC作为后端。

尽管如此，dmd是参考编译器，LDC和GDC中存在细微差别以及缺陷。另外，dmd速度非常快，可以提高开发效率。

# 性能剖析

由于不是每个优化都可以由编译器执行，程序员通常需要调整他们的代码。由于猜测常常是错误的，因此需要通过分析来查找代码中的热点。使用dmd的参数 -profile来测试代码。当可执行文件运行时，会生成一个trace.log。它包含的数据有点类似于以下内容。

        ------------------
        [... 多行原始信息 ...]

        === Timer Is 3579545 Ticks/Sec, Times are in Microsecs ===

        Num    Tree   Func   Per
        Calls  Time   Time   Call

        2     77773  77742  38871 std.stdio.writeln(immutable(char)[])
        1    231962  75516  75516 std.concurrency.MessageBox.get...
        1     71344  70125  70125 std.concurrency._send(...)
        1     50818  50818  50818 std.stdio.File.LockingTextWriter...

        [... 多行原始信息 ...]


上面显示的列表显示了程序中最耗时的功能。显然，在两次调用中，writeln总共支持77742个msecs。一次调用平均需要38871毫秒。MessageBox.get用了那么一点点的总时间，但是“Tree Time”是231962毫秒，这是被调用函数的时间的总和。

# 基准测试

在优化某个热点时，基准测试是必不可少的。D自带了std.datetime.benchmark和comparingBenchmark，这可用于简单的比较。

    int  a ; 
    void  f0 （） {} 
    void  f1 （） { auto  b  =  a ;} 
    void  f2 （） { auto  b  =  to ！（string ）（a ）;} 
    auto  r  =  benchmark ！（f0 ， f1 ， f2 ）（10_000 ） ; 
    writefln （“Milliseconds to call fun [0] n次：％s” ， r [ 0 ] .msecs ）;


    