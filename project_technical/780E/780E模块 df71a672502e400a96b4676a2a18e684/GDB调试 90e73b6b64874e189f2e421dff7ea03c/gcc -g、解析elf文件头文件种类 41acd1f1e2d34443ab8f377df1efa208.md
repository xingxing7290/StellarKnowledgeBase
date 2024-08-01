# gcc -g、解析elf文件头文件种类

-g可执行程序包含调试信息

-g为了调试用的

加个-g 是为了gdb 用，不然gdb用不到

linux 中解析elf文件

使用命令

```bash
readelf -S example_copy.elf | grep debug
```

```bash
[29] .debug_info       PROGBITS        00000000 1732d3 1375a7 00      0   0  1
[30] .debug_abbrev     PROGBITS        00000000 2aa87a 01b6ac 00      0   0  1
[31] .debug_loc        PROGBITS        00000000 2c5f26 08a5c3 00      0   0  1
[32] .debug_aranges    PROGBITS        00000000 3504f0 005ce8 00      0   0  8
[33] .debug_ranges     PROGBITS        00000000 3561d8 009768 00      0   0  1
[34] .debug_macro      PROGBITS        00000000 35f940 086a4c 00      0   0  1
[35] .debug_line       PROGBITS        00000000 3e638c 0a95ec 00      0   0  1
[36] .debug_str        PROGBITS        00000000 48f978 0bd6e8 01  MS  0   0  1
[37] .debug_frame      PROGBITS        00000000 54d060 013cd0 00      0   0  4
```

**readelf -S命令用于显示目标文件或共享目标文件的节表信息。它可以列出目标文件的所有节（section）的信息，包括每个节的名称、类型、大小、偏移量等。其中，节是目标文件中的一些逻辑段，用于存储程序的代码、数据和符号表等信息。通过readelf -S命令，用户可以快速查看目标文件中包含的各个节的信息，有助于了解程序的结构和调试信息。此外，readelf -S命令还可以用于分析和调试程序，例如确定程序的内存映像、调试符号表等。**

![Untitled](gcc%20-g%E3%80%81%E8%A7%A3%E6%9E%90elf%E6%96%87%E4%BB%B6%E5%A4%B4%E6%96%87%E4%BB%B6%E7%A7%8D%E7%B1%BB%2041acd1f1e2d34443ab8f377df1efa208/Untitled.png)