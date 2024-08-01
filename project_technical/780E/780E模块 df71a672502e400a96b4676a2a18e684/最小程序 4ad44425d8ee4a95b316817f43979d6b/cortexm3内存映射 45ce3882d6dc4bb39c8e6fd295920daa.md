# cortexm3内存映射

![Untitled](cortexm3%E5%86%85%E5%AD%98%E6%98%A0%E5%B0%84%2045ce3882d6dc4bb39c8e6fd295920daa/Untitled.png)

![Untitled](cortexm3%E5%86%85%E5%AD%98%E6%98%A0%E5%B0%84%2045ce3882d6dc4bb39c8e6fd295920daa/Untitled%201.png)

主存储器(图中Flash区)起始起止为0x08000000，终止地址依Flash大小而定，图中0x0807 FFFF为512k的终止地址。

然后经过一段保留区，从0x1FFFF000 – 0x1FFF F7FF为系统存储器，是不可擦除的ROM区，存储ISP程序，

最后option bytes这个区域是16个字节，是控制flash区域的寄存器

首先要讲一下STM32F1的三种启动模式，如下图所示。

- 1).主闪存存储器启动：从STM32内置的Flash启动(0x0800 0000-0x0807 FFFF)，一般我们使用JTAG或者SWD模式下载程序时，就是下载到这个里面，重启后也直接从这启动程序。
- 2).系统存储器启动：从系统存储器启动(0x1FFFF000– 0x1FFF F7FF)，这种模式启动的程序功能是由厂家设置的。一般来说，我们选用这种启动模式时，是为了从串口下载程序，因为在厂家提供的ISP程序中，提供了串口下载程序的固件，可以通过这个ISP程序将用户程序下载到系统的Flash中。
- 3).片上SRAM启动：从内置SRAM启动(0x2000 0000-0x3FFFFFFF)，既然是SRAM，自然也就没有程序存储的能力了，这个模式一般用于程序调试。

Boot MemorySpace（Aliased to Flash or systen memory depending onBOOT pins）。其实这块空间是预留的，不存数据，或者它压根不存在。在不同的启动方式下，这块区域会被映射到其他区域：

1).从Main Flash 启动:Boot Space 是Main Flash 的别名。以0x08000000 对应的内存为例，则该块内存既可以通过0x00000000 操作也可以通过0x08000000 操作，且都是操作的同一块内存

2).从System Memory启动：Boot Space 是System Memory的别名。以0x1FFFFFF0对应的内存为例，则该块内存既可以通过0x00000000 操作也可以通过0x1FFFFFF0操作，且都是操作的同一块内存

3).从SRAM 启动：SRAM 只能通过0x20000000进行操作，与上述两者不同。从SRAM 启动时，需要在应用程序初始化代码中重新设置向量表的位置。

![Untitled](cortexm3%E5%86%85%E5%AD%98%E6%98%A0%E5%B0%84%2045ce3882d6dc4bb39c8e6fd295920daa/Untitled%202.png)

STM32F1存储器映射

![Untitled](cortexm3%E5%86%85%E5%AD%98%E6%98%A0%E5%B0%84%2045ce3882d6dc4bb39c8e6fd295920daa/Untitled%203.png)

![Untitled](cortexm3%E5%86%85%E5%AD%98%E6%98%A0%E5%B0%84%2045ce3882d6dc4bb39c8e6fd295920daa/Untitled.webp)