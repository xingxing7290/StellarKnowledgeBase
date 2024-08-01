# xmake

xmake create hello//创建示例工程
create hello ...
[+]: src\main.cpp
[+]: xmake.lua
[+]: .gitignore
create ok!

xmake//编译

xmake run hallo //运行

xmake f -m debug//编译添加

xmake run -d hello //调试运行

(lldb)进行gdb调试

如果想要使用指定的调试器

$ xmake f --debugger=gdb
$ xmake run -d hello

里面有引用头文件。链接文件。