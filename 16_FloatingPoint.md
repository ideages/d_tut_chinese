# 浮点

这并非严格意义上的D标准，但是因为“95％的人对浮点完全不了解”（James Gosling，Keynote 1998），所以谈论[IEEE754](http://en.wikipedia.org/wiki/IEEE_floating_point)可能是个好主意 。D官网上有一篇[Don Clugston](http://dlang.org/d-floating-point.html)写的很好的文章，文章中深入写了的D浮点数的细节。在本教程中，我将向您介绍[Kahan的浮点数概要](http://www.cs.berkeley.edu/~wkahan/JAVAhurt.pdf)（第43页），适用于D：

+ 规则0： 所有经验法则，但这一个是错误的。偶尔会出现违反规则的好理由。
+ 规则1：存储数据和结果不会比您需要和信任的更精确。float够了吗？有疑问，使用double。
+ 规则2：本地或临时变量应该高度精确。如果有疑问，请使用real。
+ 规则3：字段类型应该是简洁的（float？）。计算属性应该精确（double或real？）。

规则0适用于本教程中的所有内容，但为了完整起见，此处明确列出。规则3实际上听起来像是对使用了规则1和2的必然结果。


和C的差异
===

如果你以前用C/C++，那么你（希望）知道以下两个函数之间的区别。

    double a(double x, double y) {
        return y + (x - y);
    }

    double b(double x, double y) {
        double tmp = x-y;
        return y + tmp;
    }


不同之处在于硬件可能会以比double更高的精度计算xy表达式（例如x86上的80bit）。在函数b的情况下，编译器必须在返回之前将tmp表达式四舍五入成为精度，而不同于a中。这可能会导致不同的返回值。但是，在D语义中，这两个版本是等价的。因此从上面的准则应该使用 real类型的tmp。

此外，D可能会在编译时以比硬件更高的精度推导这些函数。

>参考
[D基本数据类型](http://dlang.org/type.html)， [D浮点语义](http://dlang.org/float.html)， [接近机器的real：D中的浮点](http://dlang.org/d-floating-point.html)， [每位计算机科学家应该知道的浮点运算](http://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html)