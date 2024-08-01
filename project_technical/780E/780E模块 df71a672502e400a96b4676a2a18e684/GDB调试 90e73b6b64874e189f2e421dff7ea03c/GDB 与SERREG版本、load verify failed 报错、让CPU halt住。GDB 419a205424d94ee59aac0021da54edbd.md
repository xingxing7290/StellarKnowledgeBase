# GDB 与SERREG版本、load verify failed 报错、让CPU halt住。GDBserver的多进程模式

### 版本测试

现有GDB版本：10.2.90 ///10.3           SERREG版本：V788h   

### 流程代码

```bash
arm-none-eabi-gdb example_copy.elf
```

```bash
tar ext:3333
```

```bash
monitor reset
```

```bash
load 
```

### GDB 调试中出现的警告

```bash
D:\gitee\luatos-soc-2022\out\example_copy> arm-none-eabi-gdb   example_copy.elf                                 
C:\SysGCC\bin\arm-none-eabi-gdb.exe: warning: **Couldn't determine a path for the index cache directory.**
GNU gdb (GDB) 10.2.90.20210621-git
```

解决办法

这个警告信息表示 GDB 无法确定索引缓存目录的路径。根据我在网络上的搜索，这个警告信息并不会对 GDB 的使用造成任何问题，但可能会让人感到困扰。你可以尝试在 GDB 的配置文件中指定索引缓存目录的路径，或者检查你的环境变量设置是否正确。

在运行之前执行以下操作

在cmd中输入  SET HOME=%USERPROFILE%

在powershell中输入  $Env:HOME = $Env:USERPROFILE

想到了

tar eat:3333连接到远程服务器，但并没有将elf文件加载进去，可以写一个demo ，显示出其中的内存空间，判断是否是将elf文件烧录进去了

### load verify failed 报错

报错：Downloading 128 bytes @ address 0x00824000 - Verify failed

此时在GDB server.exe上勾选这“Verify download”

官方文档中的解释：

Verify download: If checked, the memory on the target will be verified after download.

验证下载:如果勾选此项，将在下载后验证目标上的内存。

日志细节

Downloading 8 bytes @ address 0x0097320C - Verify failed

```bash
01-00000734-00-02982013-0006: $OK#9a
00-00000734-01-02982015-0016: Binary: 24583937333230632c383a04d2ff7f01000000236233 ASCII: $X97320c,8:........#b3
03-00000000-00-02982015-0028: Downloading 8 bytes @ address 0x0097320C
02-00000000-00-02982015-003D: T3CE0 3001:632.256 JLINK_WriteMem(0x0097320C, 0x8 Bytes, ...)
02-00000000-00-02982015-0033: T3CE0 3001:632.256   Data:  04 D2 FF 7F 01 00 00 00
02-00000000-00-02982015-0023: T3CE0 3001:632.256   CPU is running
02-00000000-00-02982015-0037: T3CE0 3001:632.256   CPU_WriteMem(8 bytes @ 0x0097320C)
02-00000000-00-02982019-002F: T3CE0 3001:636.352 - 3.884ms returns 0xFFFFFFFF
02-00000000-00-02982019-003C: T3CE0 3001:636.352 JLINK_ReadMem(0x0097320C, 0x8 Bytes, ...)
02-00000000-00-02982019-0036: T3CE0 3001:636.352   CPU_ReadMem(8 bytes @ 0x0097320C)
02-00000000-00-02982023-001B: T3CE0 3001:640.448   failed
02-00000000-00-02982023-0026: T3CE0 3001:640.448 - 4.078ms returns 1
03-00000000-00-02982023-0011:  - Verify failed
```

观察其中，写进去了数据，但当读的时候却没有读出来，所以失败了。进而验证失败。

解决：

①将SWD评论设低：设为了500kHz  没有解决

②

[关于keil下载时出现Verify Failed! 问题的总结_lwplwp2的博客-CSDN博客](https://blog.csdn.net/lwplwp2/article/details/79606987)

③Verify failed这个问题说明板子的复位电路可能有问题，或装载程序的外部SRAM或SDRAM和CPU 的连接或时序有问题,也有可能是软件偶尔的错误。若是软件偶尔的错误,可以先复位一下CPU,然后再下载程序;或者断电后重新下载程序

④很多时候有些问题是硬件焊接造成的，这个主要针对的是自己焊接的板子，一旦地址数据总线有任何焊接问题，都会出现verify failed错误

⑤chatgpt3.5

![Untitled](GDB%20%E4%B8%8ESERREG%E7%89%88%E6%9C%AC%E3%80%81load%20verify%20failed%20%E6%8A%A5%E9%94%99%E3%80%81%E8%AE%A9CPU%20halt%E4%BD%8F%E3%80%82GDB%20419a205424d94ee59aac0021da54edbd/Untitled.png)

⑥new bing  

这个错误信息表明在使用 GDB 加载程序时，GDB 服务器在验证下载的数据时失败了。这可能是由于多种原因造成的，例如硬件问题、连接问题或者程序本身的问题。你可以尝试检查你的硬件和连接是否正常，或者尝试重新编译你的程序。如果问题仍然存在，你可以查看 GDB 服务器的日志以获取更多信息。希望这些信息对你有所帮助！

源: 与必应的对话， 2023/6/29
(1) UM2576 Introduction STMicroelectronic ST-LINK probe.. [https://www.st.com/resource/en/user_manual/dm00613038-stm32cubeide-stlink-gdb-server-stmicroelectronics.pdf](https://www.st.com/resource/en/user_manual/dm00613038-stm32cubeide-stlink-gdb-server-stmicroelectronics.pdf).
(2) c - Issue with J-Link debugger while working with bootloader on .... [https://stackoverflow.com/questions/44690032/issue-with-j-link-debugger-while-working-with-bootloader-on-stm32f765](https://stackoverflow.com/questions/44690032/issue-with-j-link-debugger-while-working-with-bootloader-on-stm32f765).
(3) Solved: J-Link GDB Server failed XMC1300 - Infineon. [https://community.infineon.com/t5/XMC/J-Link-GDB-Server-failed-XMC1300/td-p/400730](https://community.infineon.com/t5/XMC/J-Link-GDB-Server-failed-XMC1300/td-p/400730).

⑦Verify选项去掉就可以了，程序也能正常运行

### **GDB调试中有关 load和restore的问题**

 1）target remote 10.21.1.242:200000 连目标板

reset -- 复位

load xx.out －－下载我的测试程序。

2） target remote 10.21.1.242:20000

reset

restore xx.bin binary 0x8000 --下载代码。

load 结束后会把PC 指针设定到程序的入口。 restore我没check过pc指针是否会跳到程序入口。

### 让**CPU halt住**

要让 CPU 停止运行并进入停机状态（Halt），可以尝试以下方法：

软件断点：使用调试器在代码中设置断点，当程序执行到该断点时，CPU 将停止运行。在调试器中，可以使用命令（如 break 或 b）设置断点。例如，使用 break main 命令在 main 函数的入口处设置断点。

硬件断点：某些调试接口和芯片支持硬件断点，这些断点是由调试硬件直接处理的，因此对 CPU 的干扰较小。硬件断点可以更快地停止 CPU 运行。在调试器中，可以使用相应的命令（如 hbreak）设置硬件断点。

手动停机指令：在代码中插入停机指令可以强制 CPU 停止运行。停机指令的具体指令码根据目标处理器的体系结构而定。例如，在 ARM Cortex-M3 上，可以使用汇编指令 bkpt（断点指令）将 CPU 停机。在 C/C++ 代码中，可以使用特定的宏定义或内联汇编指令来生成停机指令。例如，使用 __BKPT() 宏定义或 __asm("bkpt") 内联汇编。

硬件复位：如果 CPU 处于无响应状态，可以通过对目标设备进行硬件复位来强制停止 CPU 运行。硬件复位将重置设备的状态，并将 CPU 重新启动到初始状态。

### GDBserver的多进程模式

如果用target remote连接gdbserver，gdbserver只会调试指定的程序。要是程序退出，或者你从程序脱离的话，GDB 会断开连接 ，然后gdbserver就会退出。

如果用 target extended-remote 来连接，gdbserver会进入多进程模式。要是被调试的程序退出

查看进程

info inferiors  

查看线程

info threads

附着到进程

attach process-id 

忽略信号量

```bash
handle SIGTRAP pass nostop
```

持续出现：

Program received signal SIGTRAP, Trace/breakpoint trap.

chatgpt

当在GDB中执行c命令（continue）后，多次出现Program received signal SIGTRAP, Trace/breakpoint trap.的消息，并提示可以按下 <RET> 查看更多信息或按下 q 退出，表示程序连续触发了多个跟踪/断点陷阱。

这种情况可能由以下原因引起：

多个断点：程序中设置了多个断点，每次程序执行到一个断点时都会触发SIGTRAP信号。
代码异常：程序中存在异常情况，导致跟踪/断点陷阱被触发多次。
解决方法：

如果您设置了多个断点，可以考虑检查这些断点的位置，确保它们不会连续触发。
如果代码中存在异常情况，您可能需要仔细检查代码逻辑，查找导致异常的原因，并进行修复。

解决：没有断点了已经

stackoverflow

断点陷阱仅意味着处理器已到达断点。发生这种情况的原因有两种可能。最有可能的是，您的初始化代码被命中是因为您的 CPU 正在重置并再次遇到断点。另一种可能性是设置断点的代码实际上是在初始化以外的地方运行的。有时，通过积极的编译器优化，可能很难准确判断断点映射到哪些代码以及哪些执行路径可以到达那里。

解决load 认证错误问题；调试软硬断点区分与调试并寻找断点寄存器；根据GDB 说明调试gdbserver 的多线程模式