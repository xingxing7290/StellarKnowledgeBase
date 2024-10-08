1.    说明X宏的作用，并给出一个实例

预处理时进行替换，提高编程效率

`#define MIN(A,B) ((A)<=(B)?(A):(B))`

条件编译

    #if: 开始一个条件编译块。
    #ifdef: 如果宏被定义，则执行其后的代码。
    #ifndef: 如果宏未被定义，则执行其后的代码。
    #elif: 否则，如果条件满足，则执行其后的代码。
    #else: 否则，执行其后的代码。
    #endif: 结束一个条件编译块。

```C
#include <stdio.h>
#ifdef WINDOWS
#include <windows.h>  // 包含 Windows 头文件
#else
#include <time.h>     // 包含 POSIX 头文件
#endif
```

2.    弹性结构体怎么定义和使用，举例说明

在结构体最后定义一个数组成员，使结构体数组可以根据需要动态分配

```c
typedef struct {
    int count;  // 数组元素的数量
    char items[];  // 灵活数组成员
} MyStruct;
```

3.    STM32F103型号的MCU，什么情况会进入HardFault，进入HardFault后怎么排查是哪条语句导致的异常，给出具体的排查方法

Jlink进行GDB调试，逐步确定报错函数，然后多打几个断点进行调试，确定异常语句范围，然后确定异常语句下面的函数定义逐层向下或者是单个语句分析语句错误原因。

4.    for循环条件语句，i++，i--，++i，--i，编译为汇编代码后的主要区别是什么？哪一个的执行效率最高？F103在armccV5编译器基于C99标准-O0优化，分别有多少CPU时钟周期的差距

i++，i--  先使用再加减
++i，--i  先加减再使用

    i++：
        汇编代码：ldr r1, [r0] (加载 i 的值到寄存器 r1)，add r1, r1, #1 (将 r1 的值加 1)，str r1, [r0] (将 r1 的值存回 i)。
        时钟周期：大约需要 3 个时钟周期（加载、加法、存储）。

    ++i：
        汇编代码：ldr r1, [r0] (加载 i 的值到寄存器 r1)，add r1, r1, #1 (将 r1 的值加 1)，str r1, [r0] (将 r1 的值存回 i)。
        时钟周期：同样需要大约 3 个时钟周期（加载、加法、存储）。

    i--：
        汇编代码：ldr r1, [r0] (加载 i 的值到寄存器 r1)，sub r1, r1, #1 (将 r1 的值减 1)，str r1, [r0] (将 r1 的值存回 i)。
        时钟周期：大约需要 3 个时钟周期（加载、减法、存储）。

    --i：
        汇编代码：ldr r1, [r0] (加载 i 的值到寄存器 r1)，sub r1, r1, #1 (将 r1 的值减 1)，str r1, [r0] (将 r1 的值存回 i)。
        时钟周期：同样需要大约 3 个时钟周期（加载、减法、存储）。

从上述汇编代码可以看出，无论是前缀还是后缀形式的自增/自减操作，它们在 ARM 架构下的汇编表示基本上是一样的，都需要执行加载、算术操作和存储三个步骤。因此，在 ARM 架构下，这些操作的执行效率相当，都需要大约 3 个时钟周期

5.    用变量a给出下面的定义：

a)    一个整型数  int a；

b)    一个指向整型数的指针 int *a;

c)    一个指向指针的的指针，它指向的指针是指向一个整型数 int **a;

d)    一个有10个整型数的数组 int a[10];

e)    一个有10个指针的数组，该指针是指向一个整型数  int *a[10];

f)    一个指向有10个整型数数组的指针 int (*a)[10];

g)    一个指向函数的指针，该函数有一个整型参数并返回一个整型数 int (*a)(int);

h)    一个有10个指针的数组，该指针指向一个函数，该函数有一个整型参数并返回一个整型数 int (*a[10])(int);

6.    在F103中，要求设置一RAM相对偏移地址为0x67a9的整型变量的值为0xaa66。编译器是armccV5基于C99标准-O0优化，写代码完成，

uint32_t *addr=(uint32_t *)0x67A9
*addr=0xaa66;

7.    针对跨平台场景，请指出下面代码的问题，并给出改进代码
unsigned int zero = 0;
unsigned int compzero = 0xFFFF; /*1's complement of zero */

对于一个int型不为16为的处理器来说，上面的代码时错误的
正确的编写应该是 unsigned int compzero =~0;

8.    嵌入式C语言的标准库中动态分配内存有哪几种接口函数，主要区别是什么？怎么保证超长内存拷贝语句执行时，乱序执行处理器的数据可靠性和鲁棒性？

malloc();申请size的空间
free();释放内存
calloc();申请，并将申请的内存清零

使用malloc标准库进行拷贝
多线程时乱序执行使用互斥锁

9.    常用的ADC芯片采样架构有哪些，区别是什么？STM32F103内部采用的是什么ADC架构？最大采样率和采样深度是多少？要实现最大采样率，外设时钟应该怎么设置？

逐次逼近型:低成本和低功耗,‌适用于中速或较低速、‌中等精度的数据采集、传感器接口.
积分型:高分辨率，适合音频和传感器应用。
流水线型:高速度，中等到较高的分辨率。视频处理、通信
Flash型：极高的转换速度，低分辨率，成本高，功耗大，高速数字信号处理

STM32F103采用的逐次逼近型。
STM32F103的最大采样速率为1MHz，16位的采样深度
STM32F103的ADC的最大时钟频率为36MHz。
配置时钟：
配置APB2时钟：确保APB2总线的时钟频率足够高，通常建议设置为72 MHz。
ADC时钟分频：ADC时钟频率应在1 MHz到14 MHz之间，通常通过设置ADC的预分频器来实现。
采样时间配置：选择较短的采样时间，以减少采样延迟。

10.   假设使用STM32F103的内部ADC，采样2.5V的外部直流信号，采样保持时钟设置为13.5Cycles，理论的ENOB是多少？采样Vpp=1.0V的正弦信号，理论最小不失真原始波形频率不应大于多少Hz？若该正弦信号是856Hz，但是信号混杂了511Hz的系统噪声，FFT的参数应该怎么设置？

3.3v/2^12=3.3/4096=0.000805V

ENOB==12-log2(3.3V/1.0v)=12-1.736=10.26

对于Vpp=1.0V的正弦信号，其频率最大为1MHz f Hz：
理论最小不失真的频率为： 500K Hz

Δf=1 MHz/1024​≈976.56 Hz
使用1MHz的采样率和1024点的FFT，可以准确地分析856Hz和511Hz的频率成分。
