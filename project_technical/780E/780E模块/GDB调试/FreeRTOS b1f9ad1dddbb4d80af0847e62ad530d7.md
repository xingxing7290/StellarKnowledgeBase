# FreeRTOS

780E中使用的FreeRTOS版本为FreeRTOS Kernel V9.0.0a 

```html
https://www.freertos.org/zh-cn-cmn-s/FreeRTOS-simulator-for-Linux.html#gdb_debugging_tips
```

在GDB中，使用 `delete` 指令不带参数则可以删除全部的断点和追踪点。

忽略信号量

```html
handle SIGTRAP pass nostop
```

PS D:\SEGGER\JLink_V780> (使用命令行的GDNserver)

```html
.\JLinkGDBServerCL.exe -select USB -device Cortex-M3 -endian little -if SWD -speed 500 -ir -LocalhostOnly -nologtofile -port 3333 -SWOPort 50001 -TelnetPort 50002 -rtos GDBServer\RTOSPlugin_FreeRTOS
```

在FreeRTOS中，线程（Thread）和任务（Task）的概念是相同的。每个任务就是一个线程，有着自己的一个程序。

### 多任务

但info tasks  info thread   info program  info proc 都没有反应

### 多任务中优先级相关想法

考虑可能和freertos中的任务优先级有关

freertos采用轮询调度算法，包含抢占式和合作式

在FreeRTOSConfig.h头文件中由**configUSE_PREEMPTION**这个参数决定，为1时是抢占式模式，为0时是合作式模式。  此模块为抢占式。

FreeRTOS对任务的调度采用基于时间片（time slicing）的方式

模块采用：**抢占式时间片调度    configUSE_TIME_SLICING**（采用时间片） 1

内核调度器在每个时间片结束的时候执行一次，选择处于就绪状态的任务中优先级最高的任务置于下一个时间片执行。如果优先级相同的话则交替执行

### 优先级测试

创建一个新任务，并设置优先级为最高100%  。之后让其while轮询。

发现 在程序中写入的优先级为100% 但实际使用时的priority 为24

20&%—11     50%—16    100%—24

优先级最高的为 UiccDrvTask  为41  推测是手机sim卡的驱动任务

其中包含多个默认任务。且这些任务的优先级都是高于自定义任务的，自定义任务最高24 ，默认任务最低24

UICC通用集成电路卡（Universal Integrated Circuit Card）

UICC卡是一种可移动智能卡，它用于存储用户信息、鉴权密钥、电话簿、短消息等信息。

可能有两个核

如何判断模块有几个核？

在keil →option to target→debug→jlink→setting→swd drice→上面IDCORE有几个，就显示有几个核

freertos的task和线程有什么区别?

在FreeRTOS中，任务（Task）和线程（Thread）的概念是相同的，每个任务就是一个线程，有着自己的一个程序。1 但是，FreeRTOS里面没有任务和线程的概念，任务、线程、进程这三者通通都是同一个东西。3

### IDLE空闲任务

FreeRTOS程序在任意时刻，必须**至少有一个任务处于运行状态**，为了达到这个要求，FreeRTOS使用了Idle任务；

调度器会自动创建idle任务，并且任务始终问就绪态，

idle任务的优先级最低，为0 。所以Idle任务**不会抢占用户任务**的运行。当其他高优先级的任务需要运行时，他们会抢占Idle任务。

Idle任务主要**用于资源回收清理工作**，例如当你在程序中删除一个任务后，就需要Idle任务去清理这个任务占用的资源。因此，不要让Idle任务“饿死”，具体而言，**不要创建一个优先级比Idle任务优先级高，且连续性工作的任务**。