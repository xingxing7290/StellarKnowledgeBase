# BSP和HAL之间的关系

BSP是初始化与运行操作系统的引导程序以及所有主板上面包含的驱动程序

它包含了必要的硬件操作函数

单片机使用BSP可以直接应用开发

但是随着芯片的发展和复杂度的提升，会产生大量的BSP改动，不利于应用程序的维护

因为为了程序的可阅读性和可移植性，提出了硬件抽象层HAL

HAL库的实现是基于BSP的，只是将其进行进一步的封装，实现成标准的接口

这样再有改动硬件部分的电路时，只需要对BSP部分集体的功能函数做改动，而不需要去修改应用层的程序

![Untitled](BSP%E5%92%8CHAL%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB%2086b9f14baa614597b5ff783ad1fcc9ef/Untitled.png)

![Untitled](BSP%E5%92%8CHAL%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB%2086b9f14baa614597b5ff783ad1fcc9ef/Untitled%201.png)

因此一个完整、强健的[嵌入式](https://so.csdn.net/so/search?q=%E5%B5%8C%E5%85%A5%E5%BC%8F&spm=1001.2101.3001.7020)系统的系统hierarchy应该为：

**hardware –> board support package –> hardware abstract layer –> driver –> operating system –> application**

当然嵌入式系统中操作系统并不是必须的，并且在操作系统和应用程序之间可以在有一层中间件Middleware层，用于提供更多的系统功能，这个中间件Middleware层也被称作SDK。