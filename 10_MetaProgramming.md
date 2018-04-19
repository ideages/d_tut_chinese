# 元编程

从安全到强大。

 模板
 ===

D拥有像C++一样的模板。它们为泛型编程提供了良好的基础。

对于Java程序员来说，语义是完全不同的，因为泛型函数不会编译为二进制文件中的单个函数。相反，它会针对不同的参数多次实例化。这允许编译器专门优化每个实例，但会生成更多代码。

# Traits特征

D为通过称为Traits库为元编程提供了丰富的便利功能。一些来自[编译器](http://dlang.org/traits.html) ，一些来自[标准库](http://dlang.org/phobos/std_traits.html)。


# 字符串混入

编译时编程最强大的机制是字符串混入。由于D可以在编译时推导函数，因此可以在编译时生成字符串。一个字符串mixin，就是生成一个字符串到源代码中，以供进一步编译。

    template GenStruct(string Name, string M1)
    {
      const char[] GenStruct =
        "struct " ~ Name ~ "{ int " ~ M1 ~ "; }";
    }

    mixin(GenStruct!("Foo", "bar"));

这产生（或等同于）以下内容。

    struct Foo { int bar; }

这种元编程的一个高级例子是[Pegged解析器生成器](https://github.com/PhilippeSigaud/Pegged)。您可以提供语法，并在编译时生成高效的分析器。

    mixin(grammar(`
      Arithmetic:
      Term     < Factor (Add / Sub)*
      Add      < "+" Factor
      Sub      < "-" Factor
      Factor   < Primary (Mul / Div)*
      Mul      < "*" Primary
      Div      < "/" Primary
      Primary  < Parens / Neg / Pos / Number / Variable
      Parens   < "(" Term ")"
      Neg      < "-" Primary
      Pos      < "+" Primary
      Number   < ~([0-9]+)

      Variable <- identifier
    `));


语法的小兄弟是正则表达式。D标准库附带了[std.regex](http://dlang.org/phobos/std_regex.html)，这也是元编程的一个很好的例子。它在编译时将简洁的[正则表达式变成代码](http://dlang.org/regular-expression.html)。

元编程的另一个很好的例子是[LuaD](http://jakobovrum.github.io/LuaD/)。它将Lua脚本语言集成到D中。与C接口不同，Lua堆栈操作通过元编程隐藏。实际上，Lua对象可以像D对象一样使用，而不需要显式的push/pop调用。

    auto print = lua.get!LuaFunction("print");
    print("Hello, world!");


>参考
[D中的模板释义](http://nomad.so/2013/07/templates-in-d-explained/)， [正则表达式](http://dlang.org/regular-expression.html)