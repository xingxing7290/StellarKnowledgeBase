# M3的ARMv7-M架构和CoreSight架构

### ARMv7-M架构

Cortex-M3内核使用的架构是ARMv7-M。在ARM处理器的架构中，Cortex-M系列是专为嵌入式系统设计的一组处理器，它们采用了ARMv7-M架构。

ARMv7-M架构主要特点如下：

精简指令集 (RISC)：ARMv7-M架构采用精简指令集，提供了一组简洁而高效的指令，以适应嵌入式系统的资源限制和功耗要求。
Thumb指令集：Cortex-M处理器支持Thumb指令集，其中大多数指令是16位的，可以在节省存储空间的同时提高代码密度。
支持硬件浮点运算：Cortex-M3是支持硬件浮点运算的Cortex-M系列处理器，有一个单精度浮点运算单元 (FPU)，能够加速浮点计算。
中断和异常处理：ARMv7-M架构提供了一套中断和异常处理机制，使得处理器能够响应外部事件和异常情况，并能够有效地切换上下文进行处理。
特权级别：Cortex-M3处理器支持两个特权级别，即特权态 (Privileged) 和非特权态 (Unprivileged)，以便实现简单的操作系统和安全控制。
低功耗设计：Cortex-M3处理器针对嵌入式系统的低功耗要求进行了优化，可以在高性能和低功耗之间进行平衡。
总的来说，Cortex-M3内核使用ARMv7-M架构，旨在为嵌入式系统提供高效、低功耗和可靠的处理解决方案。

### CoreSight 架构

CoreSight是一种由ARM提供的调试和跟踪技术，用于嵌入式处理器的调试、性能分析和跟踪。它提供了一系列硬件调试和跟踪组件，帮助开发人员进行嵌入式系统的调试和优化。

核心特点和组件：

调试接口：CoreSight提供了一种标准的调试接口，称为Debug Access Port (DAP)，允许外部调试器与处理器内部通信。DAP支持多种调试协议，如JTAG (Joint Test Action Group) 和SWD (Serial Wire Debug)。

调试组件：CoreSight内部集成了多个调试组件，包括Debug Access Port (DAP)、Debug Control Block (DCB)、Debug ROM、Embedded Trace Macrocell (ETM)、Data Watchpoint and Trace (DWT)等。

跟踪组件：CoreSight还支持跟踪功能，用于记录处理器的指令执行流和数据访问情况。其中，Embedded Trace Macrocell (ETM) 可以用于指令跟踪，Data Watchpoint and Trace (DWT) 可以用于数据跟踪。

跟踪缓冲区：CoreSight提供了一种跟踪缓冲区，用于存储跟踪数据。这些跟踪数据可以在系统运行时被读取，以进行性能分析和优化。

调试和跟踪软件工具：ARM提供了一系列调试和跟踪软件工具，如ARM DS-5、Keil MDK等，用于与CoreSight技术配合使用，实现嵌入式系统的调试和优化。

通过使用CoreSight技术，开发人员可以实时监测处理器的运行状态，收集和分析调试信息，帮助快速定位和解决软件中的问题，同时对系统性能进行优化。CoreSight在ARM架构的处理器中得到广泛应用，特别是在Cortex-M和Cortex-A系列处理器中，帮助开发者更高效地开发和调试嵌入式系统。

[ARM Coresight: Debug and Trace in Embedded System - gettobyte](https://gettobyte.com/arm-coresight/)

“CoreSight 是 ARM 的调试架构，用于复杂 SoC 设计（单核和多核）中的调试和跟踪解决方案”

CoreSight 提供调试、跟踪、监控和优化完整片上系统 （SoC） 设计性能所需的所有基础设施。

ARM Cortex M 处理器（M3/M4/M33/M7/M0 等）的调试和跟踪功能基于 CoreSight 调试架构设计。该架构涵盖广泛的领域，包括调试接口协议、用于调试访问的片上总线、调试组件控制、安全功能、跟踪数据接口等。

JTAG是一个行业标准协议（IEEE 1149.1），用于调试和边界扫描测试。它是事实上的串行协议，几乎存在于除ARM以外的每个处理器系列中，例如AVR 8内核，MIPS，PowerPC等。

需要注意的是：

JTAG需要4个引脚：TCK，TDI，TMS，TDO;最近的信号 TRST 是可选的。

ARM CoreSight Technology 推出了 2 线协议 SWD（串行线调试）：SWCK 和 SWDIO，用于调试所有基于 ARM 的处理器。

串行线调试（SWD）协议提供相同的调试访问功能，并支持奇偶校验错误检测，从而在具有较高电噪声的系统中实现更高的可靠性。因此，串行线调试协议比JTAG接口更有利。此外，SWD和JTAG调试协议共享相同的连接：TCK和SWCL使用相同的引脚，TMS和STDIO使用相同的引脚。

### 调试和跟踪：ARM 核心视线架构

[Debug and Trace: ARM CORESIGHT ARCHITECTURE - gettobyte](https://gettobyte.com/debug-and-trace-arm-coresight-architecture/)

跟踪和调试就像我们核心处理器中的外设。它们有自己的电路、组件、通信协议、通信引脚、通信总线和自己的可编程寄存器，用于配置要使用的调试和跟踪器功能并进行设置。

对于ARM处理器，该外设由CoreSight架构的名称设计。现在它有许多组件，如ITM，DWT，STM，DAP等。它共同提供了调试，跟踪，时间戳，多产计数器等功能。

![Untitled](M3%E7%9A%84ARMv7-M%E6%9E%B6%E6%9E%84%E5%92%8CCoreSight%E6%9E%B6%E6%9E%84%20851698d9a2c34270bdbb52dd9e76b5ca/Untitled.png)

**调试访问端口 （DAP）**

启用 SoC 和主机调试器之间的调试访问。调试端口用于访问外部调试器，访问端口用于片上系统资源。

**调试端口 （DP）**

JTAG和SWD（+SWO引脚 - 用于跟踪）是可用于调试/跟踪的通信协议。现在，为了将MCU内核连接到主机调试器（Stlinkv2）并进行通信，我们需要MCU上的特殊端口（I / O引脚），因为这些协议在非常高的带宽下工作，并且直接与处理器一起发挥作用。此端口称为调试端口。

所有 ARM cortex M 处理器中都提供 3 个调试端口模块：

SWJ-DP （串行线 JTAG 调试端口） à 支持串行线和 JTAG 协议
SW-DP （串行线调试端口） à 仅支持串行线协议。
JTAG-DP （JTAG 调试端口）à 仅支持 JTAG 协议，该协议在老一代 ARM 处理器和几乎所有处理器中可用。

**调试和跟踪总线 & 接入端口 （AP）**

接入端口是连接DP和CoreSight架构组件的端口。

CoreSight 系统使用以下总线协议将组件连接在一起，并实现在 SoC 中的集成。

AMBA 跟踪总线 （ATB-AP）
AMBA 3 高级外围总线 （AMBA 3 APB）
高级高性能总线 （AHB-AP）
AMBA 高级可扩展接口 （AXI-AP）
ARM Cortex M3/M4 高级高性能总线 （AHB-AP） 用于内部调试总线协议，将不同的调试组件连接在一起。

![Untitled](M3%E7%9A%84ARMv7-M%E6%9E%B6%E6%9E%84%E5%92%8CCoreSight%E6%9E%B6%E6%9E%84%20851698d9a2c34270bdbb52dd9e76b5ca/Untitled%201.png)

### **debug 和trace的区别**

Debug仅在debug版本的应用程序输出结果,而Trace在debug和release版本的程序中都起作用。

Debug和Trace都是用于记录程序运行过程中的信息，但它们之间有一些区别。Debug类提供了一组帮助调试代码的方法和属性，而Trace类提供了一组帮助跟踪代码执行的方法和属性。通俗地说，Debug用于在不打断程序的调试或跟踪下，用来记录程序执行的过程。Debug只在debug状态下会输出，而Trace在Release下也会输出，在Release下Debug的内容会消失。

简而言之，Debug和Trace都可以用来记录程序运行过程中的信息，但它们的使用场景不同。Debug主要用于调试代码，而Trace主要用于跟踪代码执行。希望这些信息对您有所帮助！

### **Debug和Release的对比**

![Untitled](M3%E7%9A%84ARMv7-M%E6%9E%B6%E6%9E%84%E5%92%8CCoreSight%E6%9E%B6%E6%9E%84%20851698d9a2c34270bdbb52dd9e76b5ca/Untitled%202.png)