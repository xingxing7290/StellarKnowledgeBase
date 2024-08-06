# GDBserver 、interrupt指令卡死

使用终端中的arm-none-eabi-gdb  example_copy.elf

使用连接server命令

```bash
(gdb) tar ext:3333
Remote debugging using :3333
0x00000000 in mcuvector ()
```

当你使用 tar ext:3333 命令连接到远程目标设备时，出现 0x00000000 in mcuvector () 这样的输出是正常的。

0x00000000 是指当前程序的执行地址或程序计数器（Program Counter）的值。在程序刚开始执行时或者在断点处停止时，程序计数器通常会指向程序的起始地址。在这种情况下，地址为 0x00000000。

mcuvector 是一个函数或符号的名称，表示目标设备的中断向量表（Interrupt Vector Table）。中断向量表是一个包含中断处理程序地址的数据结构，用于处理来自外部中断的请求。**在 ARM Cortex-M 系列处理器中，中断向量表通常位于地址 0x00000000 处，因此在这里显示了 mcuvector。**

### interrupt停止指令

使用gdb指令 ：”interrupt ” 。这会发送中断信号给目标程序，使其暂停执行。

注意：在某些情况下，interrupt 可能无法正常工作，特别是当目标设备处于某种状态下或运行某些特定类型的代码时。此外，中断程序的执行可能会导致目标设备的状态发生变化。

同时jlink GDBserver卡死提示：Debugger requested to halt target…