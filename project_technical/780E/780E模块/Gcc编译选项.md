# Gcc编译选项

gcc 时有关调试信息：-g3(调试信息)/-O0(编译优化)/-s(裁剪执行程序信息)

-g /-gdb/-g3 编译调试信息

“-o”是指目标文件 

**gcc 编译选项 -s 可以裁剪执行程序的信息，删除可执行文件中所有符号表和重新定位信息，以压缩可执行文件，导致gdb调试无效，使用命令 file excfilename可以看到有strip**

### 一、gcc -s和[strip](https://so.csdn.net/so/search?q=strip&spm=1001.2101.3001.7020)的区别

`gcc -s`:从可执行文件中删除所有符号表和重定位信息.

`strip`:丢弃目标文件中的符号.

`strip`是可以在已经编译的目标文件上运行的东西.它还具有各种命令行选项,您可以使用它们来配置要删除的信息.例如,`-g`仅删除`gcc -g`添加的调试信息.

请注意,这`strip`不是bash命令,但您可能正在从bash shell运行它.它是一个完全独立于bash的命令,它是GNU二进制实用程序套件的一部分.

### 二、gcc不带-g和strip的区别

1、去掉-g，等于程序做了--strip-debug

2、strip程序，等于程序做了--strip-debug和--strip-symbol

所以strip后程序会更小点

**但对于静态库.a之类的文件，只能用--strip-debug**

**静态编译就不能使用 strip 了**

780E编译使用参数：

- g3: 生成调试信息，级别为 3，包括源代码行号、变量名等，用于后续的调试。
- mcpu=cortex-m3: 指定目标处理器的架构为 ARM Cortex-M3。
- mthumb: 生成 Thumb 指令集的机器码。
- std=gnu99: 使用 GNU C 99 标准。
- nostartfiles: 不使用标准启动文件。
- mapcs-frame: 生成 ARM Procedure Call Standard 框架。
- ffunction-sections: 将每个函数放置在单独的段中，以便在链接时可以进行优化。
- fdata-sections: 将每个全局变量放置在单独的段中，以便在链接时可以进行优化。
- fno-isolate-erroneous-paths-dereference: 禁用错误路径解引用的隔离。
- freorder-blocks-algorithm=stc: 使用 Static Topological Ordering 算法来重新排序代码块。
- Wall: 启用所有常用警告。
- Wno-format: 禁用格式相关的警告。
- gdwarf-2: 生成 Dwarf 2 格式的调试信息。
- fno-inline: 禁用内联函数优化。
- mslow-flash-data: 针对 Flash 存储较慢的情况进行数据访问优化。
- fstack-usage: 生成堆栈使用信息。
- Wstack-usage=4096: 设置堆栈使用的阈值为 4096 字节

Gcc所支持**后缀名解释**

| 后 缀 名 | 所对应的语言 | 后 缀 名 | 所对应的语言 |
| --- | --- | --- | --- |
| .c | C原始程序 | .s/.S | 汇编语言原始程序 |
| .C/.cc/.cxx | C++原始程序 | .h | 预处理文件（头文件） |
| .m | Objective-C原始程序 | .o | 目标文件 |
| .i | 已经过预处理的C原始程序 | .a/.so | 编译后的库文件 |
| .ii | 已经过预处理的C++原始程序 |  |  |

Gcc总体选项列表

| 后缀名 | 所对应的语言 |
| --- | --- |
| -c | 只是编译不链接，生成目标文件“.o” |
| -S | 只是编译不汇编，生成汇编代码 |
| -E | 只进行预编译，不做其他处理 |
| -g | 在可执行程序中包含标准调试信息 |
| -o file | 把输出文件输出到file里 |
| -v | 打印出编译器内部编译各过程的命令行信息和编译器的版本 |
| -I dir | 在头文件的搜索路径列表中添加dir目录 |
| -L dir | 在库文件的搜索路径列表中添加dir目录 |
| -static | 链接静态库 |
| -llibrary | 连接名为library的库文件 |

nist@zq-node2:~/test$ gcc --help
Usage: gcc [options] file...
Options:
-pass-exit-codes         从一个阶段以最高错误代码退出.
--help                   Display this information.
--target-help            显示特定于目标的命令行选项.
--help={common|optimizers|params|target|warnings|[^]{joined|separate|undocumented}}[,...].
显示特定类型的命令行选项.
(使用 '-v --help' 显示子进程的命令行选项).
--version                显示编译器版本信息.

- dumpspecs 显示所有内置规范字符串.
-dumpversion 显示编译器的版本.
-dumpmachine 显示编译器的目标处理器.
- print-search-dirs 显示编译器搜索路径中的目录.
-print-libgcc-file-name 显示编译器配套库的名称.
-print-file-name=<lib> 显示库 <lib> 的完整路径.
-print-prog-name=<prog> 显示编译器组件 <prog> 的完整路径.
-print-multiarch 显示目标的规范化 GNU 三元组，用作库路径中的一个组件.
-print-multi-directory 显示 libgcc 版本的根目录.
-print-multi-lib 显示命令行选项和多个库搜索目录之间的映射.
-print-multi-os-directory 显示操作系统库的相对路径.
-print-sysroot 显示目标库目录.
-print-sysroot-headers-suffix 显示用于查找headers的sysroot后缀.
- Wa,<options> 将逗号分隔的 <options> 传递给汇编器（assembler）.
-Wp,<options> 将逗号分隔的 <options> 传递给预处理器（preprocessor）
-Wl,<options> 将逗号分隔的 <options> 传递给链接器（linker）
-Xassembler <arg> 将 <arg> 传递给汇编器（assembler）.
-Xpreprocessor <arg> 将 <arg> 传递给预处理器（preprocessor）.
-Xlinker <arg> 将 <arg> 传递给链接器（linker）.
- save-temps 不用删除中间文件.
-save-temps=<arg> 不用删除指定的中间文件.
- no-canonical-prefixes 在构建其他 gcc 组件的相对前缀时，不要规范化路径.
-pipe 使用管道而不是中间文件
-time 为每个子流程的执行计时.
-specs=<file> 使用 <file> 的内容覆盖内置规范.
-std=<standard> 假设输入源用于＜standard＞。
--sysroot=<directory> 使用<standard>作为headers和libraries的根目录.
-B <directory> 将 <directory> 添加到编译器的搜索路径.
- v 显示编译器调用的程序.
-### 与 -v 类似，但引用的选项和命令不执行.
-E 仅执行预处理（不要编译、汇编或链接）.
-S 只编译（不汇编或链接）.
-c 编译和汇编，但不链接.
-o <file> 指定输出文件. gcc 编译出来的文件默认是 a.out
-pie 创建一个动态链接、位置无关的可执行文件
-shared 创建共享库/动态库.
-g： 生成调试信息
-w： 不生成任何警告
-Wall： 编译时 显示Warning警告，但只会显示编译器认为会出现错误的警告
-x <language> 指定以下输入文件的语言。允许的语言包括：c c++汇编程序none“none”表示恢复到的默认行为根据文件的扩展名猜测语言。
Options starting with -g, -f, -m, -O, -W, or --param are automatically
passed on to the various sub-processes invoked by gcc. In order to pass
other options on to these processes the -W<letter> options must be used.
For bug reporting instructions, please see:
<file:///usr/share/doc/gcc-9/README.Bugs>.
- shared:
编译动态库时要用到
-pthread:
在Linux中要用到多线程时，需要链接pthread库
-fPIC:
作用于编译阶段，告诉编译器产生与位置无关代码(Position-Independent Code)，
则产生的代码中，没有绝对地址，全部使用相对地址，故而代码可以被加载器加载到内存的任意
位置，都可以正确的执行。这正是共享库所要求的，共享库被加载时，在内存的位置不是固定的。
-fwrapv:
它定义了溢出时候编译器的行为——采用二补码的方式进行操作
-O参数
这是一个程序优化参数，一般用-O2就是，用来优化程序用的
-O2：
会尝试更多的寄存器级的优化以及指令级的优化，它会在编译期间占用更多的内存和编译时间。
-O3： 在O2的基础上进行更多的优化
-Wall:
编译时 显示Warning警告,但只会显示编译器认为会出现错误的警告
-fno-strict-aliasing:
“-fstrict-aliasing”表示启用严格别名规则，“-fno-strict-aliasing”表示禁用严格别名规则，当gcc的编译优化参数为“-O2”、“-O3”和“-Os”时，默认会打开“-fstrict-aliasing”。
-I (大写的i):
是用来指定头文件目录
-I /home/hello/include表示将/home/hello/include目录作为第一个寻找头文件的目录，寻找的顺序是：/home/hello/include-->/usr/include-->/usr/local/include
-l:
-l(小写的 L)参数就是用来指定程序要链接的库，-l参数紧接着就是库名,把库文件名的头lib和尾.so去掉就是库名了,例如我们要用libtest.so库库，编译时加上-ltest参数就能用上了