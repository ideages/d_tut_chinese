# 标准库简介

这是对标准库Phobos的简要概述，您可以了解可用的功能：


基础
===

在std.algorithm中可以找到像C++标准模板库中那些基本算法。例如，排序，查找和哈希表（或映射，map）。再就是std.functional 中的curry，memoize和其他的函数式编程助手。内建数组std.array中也包含了其他有用的辅助函数，例如split。

文件格式
===

标准库包含处理各种格式的工具，如 JSON， XML， CSV， Zip， Base64。但对于XML，您可能需要检查 Tango的XML解析器或 找您喜欢的C XML库的绑定。

日期，时间和持续时间
===

基本上，与时间相关的一切都在 std.datetime中：日期，时间，间隔，差异，解析和格式化日期，还有时间测量和基准测试。

# 网络

基础的std.net.curl是libcurl的绑定 。
