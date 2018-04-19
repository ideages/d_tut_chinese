

# 多核编程


由于今天有多核心的计算机是常态，因此让我们讨论如何利用D来利用它。

虽然并发性和并行性[实际上是不同的概念](http://stackoverflow.com/questions/1050222/concurrency-vs-parallelism-what-is-the-difference)，但我们不会单独讨论它们，因为程序员通常会在同一时间遇到它们。我们只在这里提供一个概况。

# 线程 Threads


由操作系统管理的线程的基本机制（PThread）在std.concurrency库中：

    import std.stdio;
    import std.concurrency;

    void spawnedFunc(Tid tid)
    {
      // Receive a message from the owner thread.
      receive(
        (int i) { writeln("Received the number ", i);}
      );

      // Send a message back to the owner thread
      // indicating success.
      send(tid, true);
    }

    void main()
    {
      // Start spawnedFunc in a new thread.
      auto tid = spawn(&spawnedFunc, thisTid);

      // Send the number 42 to this new thread.
      send(tid, 42);

      // Receive the result code.
      auto wasSuccessful = receiveOnly!(bool);
      assert(wasSuccessful);
      writeln("Successfully printed number.");
    }

一旦使用了线程，请记住，在D中全局变量是线程本地的。您可以使用共享限定符将它们声明为整个进程的全局变量。

    shared int countA;   //全局计数器
    int countB;          //线程本地计数器

# 任务 Tasks

这是轻量级线程变体，它由程序本身管理。它也被称为用户级线程。由于任务通常由线程池执行，因此它们也是并行运行的。

    import std.algorithm, std.parallelism, std.range;

    void main() {
        // Parallel reduce can be combined with
        // std.algorithm.map to interesting effect.
        // The following example (thanks to Russel Winder)
        // calculates pi by quadrature  using
        // std.algorithm.map and TaskPool.reduce.
        // getTerm is evaluated in parallel as needed by
        // TaskPool.reduce.
        //
        // Timings on an Athlon 64 X2 dual core machine:
        //
        // TaskPool.reduce:       12.170 s
        // std.algorithm.reduce:  24.065 s

        immutable n = 1_000_000_000;
        immutable delta = 1.0 / n;

        real getTerm(int i)
        {
            immutable x = ( i - 0.5 ) * delta;
            return delta / ( 1.0 + x * x ) ;
        }

        immutable pi = 4.0 * taskPool.reduce!"a + b"(
            std.algorithm.map!getTerm(iota(n))
        );
    }


这与Google Go，Rust，Erlang和其他语言使用的机制相同。这种风格，可以尝试用[Vibe.d](http://vibed.org/) web框架以获得集成框架。但是，D并没有专门为此而犯下一些错误。如果任务调用阻塞函数（例如读取文件），则正在执行的线程会阻塞。由于最大并行度是由底层池中正在运行的线程的数量定义的，因此即使有更多任务要运行，这也可能会使内核空闲。专业语言可以简单地启动另一个线程来弥补阻塞。但在D的任务池大小维护必须手动完成，并且在大多数情况下可能会被遗忘。如果你可以限制自己的异步IO，那就太好了。

# 同步 Synchronization

一旦你有并行性，通常会产生并发，并需要同步。当然，D提供了你可以想到的东西：Lock锁，semaphores信号灯，atomic原子变量。

# 内存模型

目前，D没有指定的内存模型。但是，你可以假设它会遵循C++内存模型。这意味着：数据无竞争程序的顺序一致性。换句话说：正确地将你的程序和标准库中有锁的程序同步，并且所有东西都会按照你的直觉行事。如果您的要求更严格，那么必须使用较低级别的机制，这只会变得更困难。