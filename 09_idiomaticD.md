
# D惯用法

D社区相对年轻，所以对最佳实践没有太多经验。尽管如此，在运行时和标准库的某些程序特征可以被认为是惯用的。

# 常量

D提供了const（常量）和 immutable（不可变）关键字。const意味着我不能改变这个值，immutable不可变的意味着没有人能够改变这个值，这是一个更有力的保证。

你可以将immutable转换为const，但不能反向转换。另外，你可以将mutable转换为const。这意味着函数接受常量参数，并带有可变和不可变的值。

与C相反，const是可传递的，这意味着你从const中获得的任何东西都是const。换句话说，一个const对象不能有可变成员。

### 用在什么时候？

这里有一些指导原则。请注意“应该”，因为在所有情况下没有准则是正确的。

+ 参数应该是const，所以可以给出可变和不可变的值。
  + 但是，请不要复制参数以使它们变为可变或不可变，而是从一开始就声明参数是可变的或不可变的，并让调用者创建副本。
  + 如果函数由于const参数而不是线程安全的，则要么显式记录这个参数，要么使参数不可变。
+ 返回值，这是新构造的，不应该是常量，但可变或不可变。
+ 数据结构不应该将自身限制为可变，常量或不可变。用户应该决定可变性。测试这个。否则，未来的更改可能会破坏用户代码

>参考
[常量FAQ](http://dlang.org/const-faq.html)， [常量和不变量](http://dlang.org/const3.html)，[D的复制和移动语义](http://dconf.org/2013/talks/cehreli.html)，[AliÇehreli演讲](http://dconf.org/2013/talks/cehreli.html)， [准则讨论](http://forum.dlang.org/post/sdefkajobwcfikkelxbr@forum.dlang.org)


# 纯函数
试着让你的函数纯的，不抛出异常和安全的：pure，nothrow 和@safe。

+ 纯粹意味着你的函数不会使用或改变可变的全局状态，除了参数中可用的内容。如果另外参数也不可变，则函数被称为“强纯的”。
+ Nothrow意味着你的函数永远不会抛出异常。
+ 安全意味着您的功能无法通过不安全的转换或内联汇编破坏类型系统。

这些限定符也是纯函数所必需的。例如，一个纯函数只能调用纯函数。

为了记录，大多数人都同意这些参数应该是默认的，并且具有反义的一些“不纯”，“抛出”和“不安全”属性会更好。但是，现在改变它会在破坏太多原来的代码。

>参考
[D中的纯函数](http://klickverbot.at/blog/2012/05/purity-in-d/#pure_member_functions)


# Range 

D用range来代替迭代器，可以描述为一对迭代器。range用于顺序访问，可用于数组，列表，文件和套接字。有多种range，取决于是否能够随机访问。

例如，考虑表达式sort（chain（a，b，c）），其中sort和chain是标准库中的普通函数。就像名称所暗示的那样， 排序是排序功能，链条将多个范围合并为一个。如果a，b和c是随机访问数组，那么排序可以在三个不同的数组上执行。

本教程不会深入探索range，因为该主题非常大。相反，有几个链接。

我认为range真的很酷，而且值得看看书上的处理。有人写它了。- [Andrei Alexandrescu](http://www.reddit.com/r/IAmA/comments/1nl9at/i_am_a_member_of_facebooks_hhvm_team_a_c_and_d/ccjly9n)


>参考
[std.range](http://dlang.org/phobos/std_range.html)， [迭代器](http://www.informit.com/articles/printerfriendly.aspx?p=1407357&rll=1)， [使用Range进行组件编程](http://wiki.dlang.org/Component_programming_with_ranges)， [AliÇehreli文章Ranges](http://ddili.org/ders/d.en/ranges.html)





编译时函数推导
======

由于D允许在编译时调用某些D代码，因此库编写器应该启用其功能以进行编译时推导。这通常意味着根据函数式编程用法来编写代码。


# Scope 范围语句用法

Scope使try-finally（没有catch！）构造并可以进行改善。这通常会出现锁定。

    try {
      l.lock();
      // mutual exclusion stuff
    } finally {
      l.unlock();
    }

变成：

    {
      l.lock();
      scope (exit) l.unlock();
      // mutual exclusion stuff
    }

还可以改善成另外两行语句：
+ scope(failure)：在如果一个异常被抛出时，才会执行的语句
+ scope(success)：如果没有异常被抛出时，才会执行的语句。

>参考
[范围Scope的语言参考](http://dlang.org/statement.html#ScopeGuardStatement)， [安全 Exception](http://dlang.org/exception-safe.html)