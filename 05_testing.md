# 第五章 测试

D提供了一个unittest关键字，你可以将测试放在函数后。

    /** Returns: argument times two */
    int twice(int x) { return x+x; }

    /** for example */
    unittest {
        assert (twice(-1) == -2);
        assert (twice(2) == 4);
    }


这些代码块默认被忽略。如果dmd获得-unittest参数，则测试在main之前编译和执行。

>“我需要单位测试，”我告诉自己，“谁能明显有足够的智慧来编写代码并且第一次代码就能运行？”但是他们只是一直盯着我说：“嘿，我们就在这里，用我们！ “直到我感到惭愧，才真正写出了其中的一些。然后我发现我并不是我认为的天才程序员，而且我的代码实际上充满了错误，错别字以及各种其他问题，这些问题只会出现在案例的角落里，当然，我会让第一次测试失败。- [HS Teoh](http://forum.dlang.org/thread/CAJ85NXAu+fKDeq22-=Bjc0jn5KPni5-dpg1EDWA3MqJwKFk+hg@mail.gmail.com#post-mailman.1371.1371919343.13711.digitalmars-d:40puremagic.com)


dmd的另一个测试库的有用参数是 -main。它在程序中插入一个空的main函数，这是执行所必需的，并在main函数之前运行unittest。

你应该保持单元测试在模板外面，否则会多次实例化，这可能不是预期的行为。

# 覆盖

测试的另一个方面是覆盖范围，dmd提供了-cov参数。对于上面的程序，在单元测试期间生成以下文件：

    |/** @returns the argument times two */
    2|int twice(int x) { return x+x; }
    |
    |/** for example */
    |unittest {
    1|  assert (twice(-1) == -2);
    1|  assert (twice(2) == 4);
    |}
    examplecode/unittestexample.d is 100% covered

对于持续测试，您可以为最小覆盖范围提供-cov参数。

# 高级单元测试

虽然D内置了对单元测试的支持，但不支持诸如模型mockup，setup或teardown函数等高级技术。但有先进的测试框架可用。

>参考
[DUnit](http://code.dlang.org/packages/dunit), [Unit-threaded](https://github.com/atilaneves/unit-threaded), [SpecD](https://github.com/jostly/specd)