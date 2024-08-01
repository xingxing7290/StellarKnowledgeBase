# GCC/G++/MDK

gcc

原来表示GUN C Compiler  是C语言编译器，现在表示GUN Compiler Collection 表示一堆编译器的集合。可以编译多种语言

g++

是现在GCC里面的c++编译器。

### GCC、gcc、g++三者关系

gcc（GNU C Compiler）是GCC中的c[编译器](https://so.csdn.net/so/search?q=%E7%BC%96%E8%AF%91%E5%99%A8&spm=1001.2101.3001.7020)，而g++（GNU
 C++ 
Compiler）是GCC中的c++编译器。gcc和g++两者都可以编译c和cpp文件，但存在差异。gcc在编译cpp时语法按照c来编译但默认不能链接到c++的库（gcc默认链接c库，g++默认链接c++库）。g++编译.c和.cpp文件都统一按cpp的语法规则来编译。所以一般编译c用gcc，编译c++用g++。

现在的GCC是一个驱动程序，根据代码的后缀名来判断调用c编译器还是c++编译器 (g++)。比如你的代码后缀是*.c，他会调用c编译器还有linker去链接c的library。如果你的代码后缀是cpp, 他会调用g++编译器，当然library call也是c++版本的。

---

gcc 和 g++ 的区别无非就是调用的编译器不同, 并且传递给链接器的参数不同.

具体而言

**g++** 会把 `.c` 文件当做是 C++ 语言 (在 `.c` 文件前后分别加上 `-xc++` 和 `-xnone`, 强行变成 C++), 从而调用 `cc1plus` 进行编译.

**g++** 遇到 `.cpp` 文件也会当做是 C++, 调用 `cc1plus` 进行编译.

**g++** 还会默认告诉链接器, 让它链接上 C++ 标准库.

**gcc** 会把 `.c` 文件当做是 C 语言. 从而调用 `cc1` 进行编译.

**gcc** 遇到 `.cpp` 文件, 会处理成 C++ 语言. 调用 `cc1plus` 进行编译.

**gcc** 默认不会链接上 C++ 标准库.

Xmake/Cmake/Gmake

gmake 

gmake是GUN make的缩写。[Linux](https://so.csdn.net/so/search?q=Linux&spm=1001.2101.3001.7020)系统环境下的make就是GNU Make。

xmake 

lua语法；多仓库支持，可自己私有包仓库

xmake 是一个基于 [Lua](https://so.csdn.net/so/search?q=Lua&spm=1001.2101.3001.7020) 的轻量级跨平台 C/C++ 构建工具，使用 xmake.lua 维护项目构建

目前，xmake主要用于C/C++项目的构建，但是同时也支持其他native语言的构建，可以实现跟C/C++进行混合编译，同时编译速度也是非常的快

Cmake

DSL；不支持自建包管理集成

它可以根据不同平台、不同的编译器，生成相应的Makefile或者vcproj项目。
通过编写CMakeLists.txt，可以控制生成的Makefile，从而控制编译过程。CMake自动生成的Makefile不仅可以通过make命令构建项目生成目标文件，还支持安装（make install）、测试安装的程序是否能正确执行（make test，或者ctest）、生成当前平台的安装包（make package）、生成源码包（make package_source）、产生Dashboard显示数据并上传等高级功能，只要在CMakeLists.txt中简单配置，就可以完成很多复杂的功能，包括写测试用例。
如果有嵌套目录，子目录下可以有自己的CMakeLists.txt。

但如果源文件太多，一个一个编译时就会特别麻烦，于是人们想到，为什么不设计一种类似批处理的程序，来批处理编译源文件呢，于是就有了make工具，它是一个自动化编译工具，你可以使用一条命令实现完全编译。但是你需要编写一个规则文件，make依据它来批处理编译，这个文件就是makefile，所以编写makefile文件也是一个程序员所必备的技能。

对于一个大工程，编写makefile实在是件复杂的事，于是人们又想，为什么不设计一个工具，读入所有源文件之后，自动生成makefile呢，于是就出现了cmake工具，它能够输出各种各样的makefile或者project文件,从而帮助程序员减轻负担。但是随之而来也就是编写cmakelist文件，它是cmake所依据的规则。所以在编程的世界里没有捷径可走，还是要脚踏实地的。

MinGW/Cygwin

想要在Windows环境下使用Linux的编译工具，也就是gcc/g++，我们需要**一个中间的转换工具或者平台**，这也就是MinGW和cygwin存在的原因。

**MinGW**

- MinGw全称 Minimalistic GNU for Windows，某种程度上可以看做是win版本下的GCC。Mingw有一个Msys的子项目，可以提供一些模拟Linux的shell和基本的Linux工具，Msys是一个辅助环境。
- MinGw 有专门的Win32 API的头文件，来把代码中Linux方式的系统调用替换为对应的Windows下的调用方式，某种程度上可以称之为将Linux调用 翻译为 Windows调用。

**Cygwin**

- Cygwin 则是一个在Windows平台上运行的unix模拟环境，是cygnus solutions 公司开发的自由软件。Cygwin更像一个平台，模拟了Linux的接口，提供了运行在它上面的程序使用，提供了很多Linux环境下的GNU软件。
- Cygwin 通过Cygwin1.dll的文件实现操作系统API的转换，模拟了Linux的调用接口给程序，程序以Linux的方式调用系统API，但这个API的目标库是Cygwin1.dll，**Cygwin1.dll再调用Windows对用的方式实现，再把结果返回给程序**。

总的来说：

- cygwin编译后的exe需要cygwin1.dll作为支持，而mingw不需要就可以直接运行，因为有中间层所以cygwin慢，mingw快。
- cygwin包含的内容更全面，能编译通过的linux源文件更多，mingw的min是minimalist所以能编译通过的更少。但，不是全部，就是说别指望你可以把任何为linux写的源代码在cygwin或mingw编译通过并运行。

# gcc编译过程

用gcc编译*.c文件并非直接生成可执行文件，中间还经历了预处理、编译和汇编几个过程

![https://img-blog.csdnimg.cn/20200412225420983.png](https://img-blog.csdnimg.cn/20200412225420983.png)

- 在预处理阶段，gcc会把需要调用的头文件包含进来，替换宏常量和宏代码段
- 在编译阶段，gcc会检查代码的规范性、是否有语法错误等，在检查无误后，gcc会把文件翻译成 .s 后缀的汇编文件
- 在汇编阶段，gcc会把 .s 后缀的汇编文件 翻译成 .o后缀的目标文件(机器可识别的二进制文件)
- 在链接阶段，gcc会把目标文件链接到库中，生成可执行文件

# 文件类型

在Linux系统中，可执行文件没有统一的后缀，系统从文件的属性来区分可执行文件和不可执行文件。而gcc则通过后缀来区别输入文件的类别，下面介绍gcc所遵循的部分约定规则

> .c为后缀的文件，C语言源代码文件；
> 
> 
> .a为后缀的文件，是由目标文件构成的库文件；
> 
> .C，.cc或.cxx 为后缀的文件，是C++源代码文件；
> 
> .h为后缀的文件，是程序所包含的头文件；
> 
> .i 为后缀的文件，是已经预处理过的C源代码文件；
> 
> .m为后缀的文件，是Objective-C源代码文件；
> 
> .o为后缀的文件，是编译后的目标文件；
> 
> .s为后缀的文件，是汇编语言源代码文件；
> 
> .S为后缀的文件，是经过预编译的汇编语言源代码文件
> 

# gcc编译选项

| 选项 | 作用 |
| --- | --- |
| E | 激活预处理；头文件、宏等展开（.i文件） |
| S | 激活预处理、编译；生成汇编代码（.s文件） |
| c | 激活预处理、编译、汇编；生成目标文件（.o文件） |
| 无 | 激活预处理、编译、汇编、链接；生成可执行文件（.out文件） |
| o | 生成目标 |
| Wall | 打开编译告警（所有） |
| g | 嵌入调试信息，方便gdb调试 |
| llib | 链接 lib 库 （这里是小写 L ） 相当于 C++ #pragma comment(lib, “xxx.lib”) |
| Idir | 增加 include 目录 (这里是大写 i ) 头文件路径 |
| LDir | 增加 lib 目录 （编译静态库和动态库） |