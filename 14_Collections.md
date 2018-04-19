
# 集合 Collections

几乎每个程序都需要这些集合：Array数组，vector向量，map映射（字典或者哈希表），queues队列，list列表，stack堆栈。


# 切片

D中的基本数组类型称为切片。在C中，数组只是指针的语法糖，这意味着长度检查必须由用户处理。像Java这样的高级语言提供了一个引用的数据结构，这必然意味着双重间接转换。D切片在C和Java之间。他们知道他们的规模，所以边界检查是隐含的。但是，它们是低级的，速度很快。这意味着他们可能会有一些意外的行为。我强烈建议[阅读这篇文章](http://dlang.org/d-array-article.html)，其中涵盖了关于切片的所有知识。正确的容器类是在切片之上构建的。

>参考 
[关于切片的详细文章](http://dlang.org/d-array-article.html)， [语言参考：数组](http://dlang.org/arrays.html)

# Std.container 

标准库提供了[std.container](http://dlang.org/phobos/std_container.html)，它提供了几个泛型的容器。

# 更多

大家都同意，目前标准库应该提供更多的集合类型。但是，在设计集合之前，社区希望完成分配器设计，因为分配是集合中的重要部分。尽管如此，如果你现在需要更多的东西，有几个选择。

+ [Dcollections](https://github.com/schveiguy/dcollections)
+ [Tango](https://github.com/SiegeLord/Tango-D2)

>参考 
[论坛讨论](http://forum.dlang.org/post/xadomzylmuodqoejcgau@forum.dlang.org)