# 文档

作为程序员，您可能习惯于API文档，如JavaDoc或DoxyGen。D编译器具有内置了-D参数，可以利用注释来生成D文档的功能。

# Ddoc示例
对于Ddoc注释，正常注释必须扩展一个字符。例如 /* */ 变成/ ** * /。正常的注释被文档生成器忽略。由于函数的API文档可能是最重要的用例，下面是一个示例。


    /***********************************
    * computes the foobar coefficient under moon influence
    * Params:
    *  x = the trope factor
    *  but only necessary for edge cases
    *  y = determines the slope
    * Returns: foobar coefficient
    * Throws: IOException if reading moon file fails
    * See_also: bar, http://dlang.org/ddoc.html
    */

    int foo(int x, int y)
    {
      return 42;
    }

为了高级目的，Ddoc还带有一个简单的宏语言。它可以用作更长时间解释的标记语言，它也用于生成大多数D网站。

把上面的例子放到foo.d中，然后dmd -D foo.d将生成一个带有API文档的foo.html文件。尽管如此，这是相当原始的。对于一些漂亮的文档，你必须扩展基本的Ddoc模板。或者用宏可以重写以产生XML或任何其他格式。

美丽的API文档
如果您需要更复杂的工具来为您的项目生成漂亮的API文档，请查看[ddox](https://github.com/rejectedsoftware/ddox)。

# Unittest作为例子

单元测试和注释也出现在文档中。他们可以作为例子。与上面的文档注释中的示例相比，这些示例是作为测试过程的一部分执行的，也就是说它们实际上会检查错误。

    /** Returns: argument times two */
    int twice(int x) { return x+x; }

    /** for example */
    unittest {
      assert (twice(-1) == -2);
      assert (twice(2) == 4);
    }


>参考
[文档生成器](http://dlang.org/ddoc.html)， [标准库文档Ddox变化](http://vibed.org/temp/d-programming-language.org/phobos/index.html)
