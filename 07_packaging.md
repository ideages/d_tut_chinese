# 包

dub是经典包管理工具。它需要一个简单的JSON文档package.json来描述项目，并且需要一定的目录结构。

+ 源代码位于源或src子目录中
+ main函数在app.d或<package name> .d中
+ 字符串导入位于views子目录中

主代码库位于code.dlang.org上。这是D官方代码库，相当于CPAN，PyPI，RubyGems，NPM等。在那里你也可以找到[下载](http://code.dlang.org/download)。

# DUB示例
举个有用的例子，让我们看看DDox，一个不错的文档生成器，它比内建的版本更复杂一点。假设你已经安装了dub,git，使用git克隆到本地的ddox：

    git clone https://github.com/rejectedsoftware/ddox.git
    cd ddox
    dub build

从Github获得源代码后，dub build命令获取所有dub依赖项（vibe.d，libevent，libev，openssl）并构建应用程序。但是，在Linux上，lib* Dub软件包仅包含绑定，这意味着它假定依赖库已安装在系统上，否则会出现链接器错误。

参考
[主要的D包索引](http://code.dlang.org/)