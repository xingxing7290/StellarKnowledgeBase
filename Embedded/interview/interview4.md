# 国信华源嵌入式笔试题

- 1 用预处理指令#define 声明一个常数，用以表明一年中有多少秒(忽略闰年问题)

```C
#define SECONDS_IN_A_YEAR (365 * 24 * 60 * 60)
unsigned long seconds_in_a_year = SECONDS_IN_A_YEAR;

#define SECONDS_PER_YEAR (60 * 60 * 24 * 365)UL
```

- 2 写一个“标准”宏MIN，这个宏输入两个参数并返回一个最小值

```c
#define MIN(A,B) ( (A)<=(B)?(A):(B))
```

要主意宏中的参数用括号括起来，同时最外层也要括起来。因为要有优先级

-3 嵌入式中常使用无线循环，你是如何使用C编写的死循环

```c
while(1){}
for(;;){}
```

- 4 预处理器标识#error的目的是什么？

"#error"是C语言预处理器的一个指令，用于在编译时产生一个错误消息。这通常用于调试或特殊情况处理，当代码中的某个条件不满足程序员预设的要求时，可以使用 #error 指令中断编译过程，输出一个错误消息，从而提醒开发者注意问题。

示例 1: 检查编译器版本
假设我们希望确保代码只在特定的编译器版本或更高版本上编译：

```c
#if __GNUC__ < 5
#error "This code requires GCC 5.0 or higher."
#endif
```

示例 2: 检查必要的宏定义
有时，我们需要确保某些宏在编译之前已经定义：

```c
#ifndef CONFIG_OPTION
#error "CONFIG_OPTION is not defined. Please define it in your configuration."
#endif
```

示例 3: 平台检查
确保代码只在特定平台上编译：

```c
#if !defined(__linux__) && !defined(_WIN32)
#error "This code can only be compiled on Linux or Windows platforms."
#endif
```

- 5 用变量a给出下面定义

(a)一个整型数

```C
int a;
```

(b)一个指向整数型的指针

```C
int *a;
```

(c)一个指向指针的指针，它指向的指针是指向一个整型数

```C
int **a;
```

(d)一个有10个整型数的数组

```C
int a[10];
```

(e)一个有10个指针的数组，该指针是指向一个整数型的

```C
int *a[10];
```

(f)一个指向有10个整型数数组的指针

```C
int (*a)[10];
```

(g)一个指向函数的指针，该函数有一个整型参数并返回一个整型数

```C
int (*a)(int);
```

a 是一个指向函数的指针，这个函数接受一个整型参数并返回一个整型值。

(h)一个有10个指针的数组，该指针指向一个函数，该函数有一个整型参数并返回一个整型数

```C
int (*a[10])(int);
```

- 6 关键字static的作用是什么？

`static` 关键字在C和C++编程语言中有多种用途，具体取决于它的上下文。它可以用于变量和函数，以控制其存储期限和可见性（作用域）。以下是 `static` 关键字的主要作用：

***1. 静态局部变量***

在函数内部使用 `static` 关键字声明的变量在函数的多次调用之间保持其值不变。换句话说，静态局部变量具有静态存储持续时间（在程序的生命周期内分配一次内存），但它们的作用域仅限于函数内部。

```c
#include <stdio.h>

void count() {
    static int counter = 0; // 静态局部变量
    counter++;
    printf("Counter: %d\n", counter);
}

int main() {
    count(); // 输出 Counter: 1
    count(); // 输出 Counter: 2
    count(); // 输出 Counter: 3
    return 0;
}
```

***2. 静态全局变量***

在文件范围（函数外部）使用 `static` 关键字声明的变量的作用域仅限于该源文件。这使得变量不能被其他源文件访问，从而实现内部链接。

```c
// file1.c
static int counter = 0;

void increment_counter() {
    counter++;
}

int get_counter() {
    return counter;
}

// file2.c
extern int counter; // 错误：不能访问 file1.c 中的 static 变量 counter

int main() {
    increment_counter(); // 调用 file1.c 中的函数
    printf("Counter: %d\n", get_counter());
    return 0;
}
```

***3. 静态函数***

在文件范围（函数外部）使用 `static` 关键字声明的函数的作用域仅限于该源文件。其他源文件不能访问或调用这些函数，从而实现内部链接。

```c
// file1.c
static void helper_function() {
    // 这个函数只能在 file1.c 中使用
}

void public_function() {
    helper_function();
}

// file2.c
extern void helper_function(); // 错误：不能访问 file1.c 中的 static 函数

int main() {
    public_function(); // 可以调用 file1.c 中的非静态函数
    return 0;
}
```

***4. 静态类成员（C++）***

在C++中，使用 `static` 关键字可以声明类的静态成员变量和静态成员函数。静态成员变量属于类本身，而不是某个特定对象的成员。静态成员函数只能访问静态成员变量和静态成员函数。

```cpp
#include <iostream>

class Example {
public:
    static int static_variable;

    static void static_function() {
        std::cout << "Static variable: " << static_variable << std::endl;
    }
};

int Example::static_variable = 42;

int main() {
    Example::static_function(); // 输出 Static variable: 42
    Example::static_variable = 100;
    Example::static_function(); // 输出 Static variable: 100
    return 0;
}
```

***总结***

- **静态局部变量**：在函数内部声明，具有静态存储持续时间，函数调用之间保持值不变。
- **静态全局变量**：在文件范围声明，仅在该文件内可见，实现内部链接。
- **静态函数**：在文件范围声明，仅在该文件内可见，实现内部链接。
- **静态类成员（C++）**：属于类本身，而不是某个特定对象，可以通过类名直接访问。

`static` 关键字在不同的上下文中提供了灵活的存储持续时间和作用域控制，有助于实现更好的封装和模块化设计。

- 12 下面的代码输出的是什么？

```c
void foo(void)
{
    unsigned int a=6;
    int b=-20;
    (a+b>6)?puts(">6"):puts("<=6");
}
```

为了回答这个问题，我们需要仔细分析代码中的表达式 `(a + b > 6)`。

代码分析

1. **变量声明和初始化**：
   - `a` 被声明为 `unsigned int`，并初始化为 `6`。
   - `b` 被声明为 `int`，并初始化为 `-20`。

2. **表达式计算**：
   - 表达式 `(a + b > 6)` 需要计算 `a + b` 并比较结果是否大于 `6`。
   - `a` 是无符号整数，而 `b` 是有符号整数。这会引发整数类型转换（类型提升），其中有符号整数 `b` 会被转换为无符号整数进行计算。

 类型转换
当有符号数和无符号数进行运算时，有符号数会被提升为无符号数。这会导致负值的 `b` 被解释为一个非常大的无符号数。

具体来说：

- `b` 的值为 `-20`，如果将其转换为 `unsigned int`，在32位系统中，其值会变为 `4294967276`（因为无符号整数的表示范围是 `0` 到 `4294967295`）。

计算 `a + b`

- `a` 是 `6`。
- 转换后的 `b` 是 `4294967276`。

所以，
\[ a + b = 6 + 4294967276 = 4294967282 \]

比较表达式

- 现在我们比较 `4294967282 > 6`。
- 结果为 `true`，因为 `4294967282` 确实大于 `6`。

 三元操作符结果

- 因此，`puts(">6")` 被执行，输出 `">6"`。

- 13.评价下面的代码片段

```c
unsigned int zero=0;
unsigned int compzero=0XFFFF;
/* 1's complement of zero */
```

1's complement of zero  这一行注释表明，代码的意图是获取 zero 的 1 的补码（1's complement）。

对下面的代码片段进行评价：

```c
unsigned int zero = 0;
unsigned int compzero = 0xFFFF;
/* 1's complement of zero */
```

评价

1. **实现意图不明确**：

- 注释表明需要计算 `zero` 的 1 的补码，但代码中并没有实现这一操作。当前代码仅仅是声明和初始化了两个变量，并没有进行任何操作来计算 `zero` 的 1 的补码。

**1 的补码的计算**：

- 在C语言中，1 的补码通常通过对变量进行按位取反操作来实现。对于一个无符号整数 `zero`，其 1 的补码应该通过 `~zero` 来计算。

1. **改进建议**：

- 将注释与实际代码逻辑一致，并且实际计算 `zero` 的 1 的补码，并将结果存储在 `compzero` 中。

改进后的代码

```c
unsigned int zero = 0;
unsigned int compzero;
/* 1's complement of zero */
compzero = ~zero;
```

这样，`compzero` 就会包含 `zero` 的 1 的补码。

解释

- `~zero` 计算 `zero` 的 1 的补码。
- 由于 `zero` 的值是 `0`，其二进制表示是所有位都为 `0`。
- 按位取反操作 `~0` 将所有位变为 `1`，即 `0xFFFFFFFF`，这取决于 `unsigned int` 的位数。在32位系统中，这将是 `4294967295`（即 `0xFFFFFFFF`）。

总结

当前代码片段缺少计算 1 的补码的实际实现。为了实现注释中的意图，应该在代码中添加计算 1 的补码的逻辑，并将结果存储在 `compzero` 中。改进后的代码如上所示，通过 `compzero = ~zero;` 实现了这一功能。

- C语言中下面的结构合法吗？如果是它做些什么？

```c
int a=5,b=7,c;
c=a+++b;
```

这个 C 语言代码片段是合法的，但可能会让人感到困惑。让我们分解一下它的含义：

```c
int a = 5, b = 7, c;
c = a+++b;
```

在 C 语言中，`a+++b` 其实被解析为 `a++ + b`。这里发生了什么：

1. `a++` 是一个后置递增操作符，表示在表达式计算时使用 `a` 的当前值，然后将 `a` 增加 1。
2. `+ b` 是加法操作符，将 `b` 加到 `a++` 的结果上。

所以 `c = a++ + b;` 实际上是这样的：

- 首先，使用 `a` 的当前值（5），然后将 `a` 递增 1（所以 `a` 变成 6）。
- 然后，将 `a` 的原始值（5）加上 `b`（7），所以 `c` 的值是 `5 + 7 = 12`。

最终的结果是 `a` 的值变成 6，`b` 的值保持 7，`c` 的值是 12。

- AT指令类型

AT 指令（Attention Command）是一组用于控制调制解调器的指令，广泛用于通信设备的配置和管理。AT 指令可以分为几种类型，其中包括设置、读取和自上报（或状态上报）等。下面是这些指令类型的简要介绍：

   **设置指令（Set Commands）**:

- 用于配置或修改设备的参数。
- 示例指令: `AT+CSQ=2`（设置信号质量指示的级别为 2）
- 响应: 通常会返回 `OK` 表示成功，或 `ERROR` 表示失败。

   **读取指令（Read Commands）**:

- 用于查询设备的当前状态或参数。
- 示例指令: `AT+CSQ?`（查询信号质量）
- 响应: 设备会返回当前的状态或参数值，例如 `+CSQ: 15,99`，表示信号质量等级为 15，丢包率为 99。

1. **自上报指令（Event Reporting or Notification Commands）**:
   - 设备主动报告某些事件或状态变化。
   - 示例指令: `AT+CREG?`（查询网络注册状态，设备可能在状态改变时自动上报）
   - 响应: 设备会报告当前的事件或状态，例如 `+CREG: 0,1`，表示设备已注册到网络。

AT 指令在不同设备和模块中可能会有所不同，因此最好参考特定设备的 AT 指令集文档以获取准确的信息。

- const的用法

const 修饰局部变量

```c
int const n = 1;
const int n = 1;
```

两种情况都是正确的，都可以让局部变量n变的不可修改

3.const修饰指针变量

在const修饰局部变量时，变量将变得不可修改。但如果绕过变量，使用局部变量的地址，就可以修改掉局部变量的值，这显然不符合我们的需求，所以想要使变量的值无法通过指针改变时，可以使用const修饰指针变量。它又分为一下几种情况：

```c
int a = 1;
int b = 1;
int c = 1;
//const 在*的左边
int const* p1 = &a;
//const 在*的右边
int* const p2 = &b;
//const 在*的两侧都有
int const* const p3 = &c;
```

当const在*左边时，不能通过解引用指针变量来修改指针所指向的变量。
const在*右边时，不能改变指针变量内存储的地址。
const在*两侧都有时，既不能通过解引用指针变量来修改所指向的变量，也不能改变指针变量所指向的地址。

所以，如果我们想要一个局部变量彻底的无法被修改，需要在定义时使用const修饰变量，同时使用const在*左边修饰指向这个变量的指针变量。

4.const 修饰全局变量

与const修饰局部变量不同，const修饰全局变量时，既不能直接改变变量的值，也不能通过指针间接改变全局变量的值，这与全局变量在内存中的存储方式和const的底层原理有关。

5.const 修饰字符串常量

字符串常量存储在文字常量区，并且本身就不能修改，但为了防止有意无意的修改，使用const修饰字符串常量，这样在编译阶段就能发现代码的问题。

6.const 修饰函数的形式参数

函数的传参分为两种方式：

值传递：仅仅是传递变量中內容的一份临时拷贝，不会影响到原变量，所以一般情况下不需要使用const修饰。

例如：

`void test(int x)`

指针传递：将存有原变量地址的指针变量传递给函数，根据咱们上面const修饰指针变量的情况，同样可以将其分为三种情况：
    修饰指针变量中存储的地址：

`void test(int const* p)`

修饰指针变量中指向的内容：

`void test(int* const p)`

修饰指针变量中指向的内容和存储的地址：

`vodi test(int const* const p)`

通过上面的学习，我们发现使用const修饰某一对象时，产生的效果万变不离其宗，就是修饰谁，谁就不能被修改！ 认识到这一点，相信大家在使用const时可以游刃有余了~