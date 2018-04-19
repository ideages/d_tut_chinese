

# Hello World

从语法上看，D和C/C++/C#/Java非常像，像if，while，class，struct，int，double和private这样的关键字具有相同的语义。下面是标准的HelloWorld程序：

    import std.stdio;

    void main() {
        writeln("Hello World!");
    }



用这个例子开始的原因是，要测试下编译器安装是否正确。[下载和安装DMD编译器](http://dlang.org/download.html)。将上面的代码放在一个文件中，文件名修改成 hello.d 然后在命令行下编译和执行。


    $ dmd hello.d
    $ ./hello
    Hello World!

如果代码无法运行，请在[D学习论坛](http://forum.dlang.org/group/digitalmars.D.learn)或者Freenode的#d IRC频道寻求帮助.

# 便捷工具


为了方便调用编译器,rdmd命令可以直接编译和执行.

    $ rdmd hello.d
    Hello World!

rdmd会自动查找所有相关文件并将它们编译到执行文件，而dmd需要指定编译的参数。因此rdmd像一个简化的构建工具，而不需要其他的Makefile。

dmd编译器速度很快，你感觉就像使用脚本语言一样。您甚至可以将rdmd放入UNIX的环境下，并用D作为小型脚本。

    ＃！/usr/bin/env rdmd


译者备注：
    Windows的命令行是cmd命令，用法基本类似。
    开始程序无法运行，建议自行度娘。
    