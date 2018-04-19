# 与Ç互操作性

## 生成绑定

D可以很容易地与C库进行链接。但是，头文件必须转换为D绑定。

理想情况下，我们可以自动从C头文件生成绑定。实际上，您可以使用 [DStep](https://github.com/jacob-carlborg/dstep) 或手动进行转换。如果DStep成功，您可以快速开展工作。使用手动方法，您还可以将诸如预处理器宏之类的东西移植到各自的D等价物中。

从结构上看，建议采用两级绑定方法。在第一级尝试复制 C API并尽可能接近。在第二个层次上，您可以选择进行便捷的包装，这可以利用D特定的功能。

官方绑定的存储库是 [Deimos](https://github.com/D-Programming-Deimos)。请在创建时后提交绑定。

参考
[头文件绑定](http://dlang.org/htomodule.html)， [DStep](https://github.com/jacob-carlborg/dstep)

# C ++ 集成

比C interop更有限，但可能会有一定的延伸。

参考
[与C++接口](http://dlang.org/cpp_interface.html)， [在D中使用游戏引擎](http://dconf.org/2013/talks/evans_1.html)，[Manu Evans在DConf 2013 演讲](http://dconf.org/2013/talks/evans_1.html)

# 移植代码

将C/C++代码移植到D是相对容易的，因为语言非常相似。

参考
[将C转换为D](http://dlang.org/ctod.html)，[将C++转换为D](http://dlang.org/cpptod.html)，[将C预处理器转换为D](http://dlang.org/pretod.html)