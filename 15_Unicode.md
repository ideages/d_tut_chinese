
# Unicode （统一国际编码）

D的源文件默认是UTF-8，这意味着标识符可以包含特殊字符。

    int föö（int 蝴蝶）{return 蝴蝶* 2; }

# 字符串

内置的D类型string 只是一个不可变的char数组。但是，如果迭代它，则会返回dchar代码点。这通常会导致混淆。

>我很好奇，为什么窄类型的dchar类型的.front属性？而不是字符串的底层字符类型。- [飞翔的小孩](http://forum.dlang.org/post/urhhmrnzovxnyufcipwt@forum.dlang.org)

再例如，它会导致以下不同的长度。

    string foo = "bär";
    assert(foo.length == 4);
    assert(walkLength(foo) == 3);

>如果让 <string> .length返回UTF-8字符的数量，它会不会一致？- [尼克尔](http://forum.dlang.org/post/phybgondilwmlwrflawx@forum.dlang.org)


起初这似乎是一个奇怪的设计选择，但是一旦你了解了更多关于Unicode的信息，如果你也关心性能，它是唯一的***理智选择***。为了理解这一点，Unicode的一些背景是必要的。

## 代码点
一个Unicode字符，一个介于0和1,114,112之间的数字。Unicode标准定义了这些数字的含义。

## 编码
编码指定如何以字节表示代码点。

## 代码单元
取决于编码，代码点可能会被分成多个单元。例如，在UTF-8中，一个代码单元是一个字节，上面的代码点ä（U + 00E4）由两个单元（十六进制代码）c3 a4组成。

## 字形
作为人类的感知的就是字形。Unicode允许组合代码点。例如ä（U + 00E4）也可以与产生一个随后（U + 0061） 组合分音符的代码点（U + 0308）。这些组合字符 可以用于疯狂的事情。
人们通常要计算的是字形，但只有使用Unicode表格的所有知识才能实现。计算代码点只需要知道编码。计数字节只是D中的查找，因为数组长度存储在内存中。为了保持字符串与char -array语义一致，length属性给出了char元素的个数。

>目前的设计“赢得”不仅是因为它是现有的设计，而且因为它具有良好的简单性和灵活性优势。在这一点上，改变现有构造的语义是没有问题的。 -- [Andrei Alexandrescu](http://forum.dlang.org/post/l3h49k$b6$1@digitalmars.com)

请注意，char被定义 为“无符号8位UTF-8”，这也是“无符号8位”的Ubyte。“UTF-8”巩固了D采用UTF-8编码的事实。


转码
==
由于你的程序里面用的所有东西不都是UTF-8，所以D还为UTF-16 提供了wstring，为UTF-32 提供了dstring。在dstring的情况下，length属性也反映了代码点的数量。标准库提供std.encoding 模块进行转换。

    wstring ws;
    transcode("hello world",ws);
    // transcode from UTF-8 to UTF-16

    Latin1String ls;
    transcode(ws, ls);
    // transcode from UTF-16 to ISO-8859-1


由于源文件是UTF-8，因此字符串文字也是UTF-8。wstring的类型是UTF-16，transcode从其源参数和目标编码识别类型的编码。

>参考
[每个软件开发人员最低限度，必须了解的Unicode和字符集](http://www.joelonsoftware.com/articles/Unicode.html)