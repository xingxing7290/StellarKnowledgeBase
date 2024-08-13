# 嵌入式常见经典笔试题

## 预处理器（Preprocessor）

>用预处理指令#define 声明一个常数，用以表明1年中有多少秒（忽略闰年问题）

`#define SECONDS_PER_YEAR (60 * 60 * 24 * 365)UL`

我在这想看到几件事情：
(1) #define 语法的基本知识（例如：不能以分号结束，括号的使用，等等）
(2) 懂得预处理器将为你计算常数表达式的值，因此，直接写出你是如何计算一年中有多少秒而不是计算出实际的值，是更清晰而没有代价的。
(3) 意识到这个表达式将使一个16位机的整型数溢出-因此要用到长整型符号L,告诉编译器这个常数是的长整型数。
(4) 如果你在你的表达式中用到UL（表示无符号长整型），那么你有了一个好的起点。记住，第一印象很重要。

>写一个"标准"宏MIN ，这个宏输入两个参数并返回较小的一个。

`#define MIN(A,B) ((A) <= (B) ? (A) : (B))`

这个测试是为下面的目的而设的：
(1) 标识#define在宏中应用的基本知识。这是很重要的。因为在 嵌入(inline)操作符 变为标准C的一部分之前，宏是方便产生嵌入代码的唯一方法，对于嵌入式系统来说，为了能达到要求的性能，嵌入代码经常是必须的方法。
(2)三重条件操作符的知识。这个操作符存在C语言中的原因是它使得编译器能产生比if-then-else更优化的代码，了解这个用法是很重要的。
(3) 懂得在宏中小心地把参数用括号括起来
(4) 我也用这个问题开始讨论宏的副作用，例如：当你写下面的代码时会发生什么事？
least = MIN(*p++, b);

>预处理器标识#error的目的是什么？

如果你不知道答案，请看参考文献1。这问题对区分一个正常的伙计和一个书呆子是很有用的。只有书呆子才会读C语言课本的附录去找出象这种问题的答案。当然如果你不是在找一个书呆子，那么应试者最好希望自己不要知道答案。

 #error命令是C/C++语言的预处理命令之一，当预处理器预处理到#error命令时将停止编译并输出用户自定义的错误消息。

## 死循环（Infinite loops）

>嵌入式系统中经常要用到无限循环，你怎么样用C编写死循环呢？

这个问题用几个解决方案。我首选的方案是：

```C
while(1)
{

}
```

一些程序员更喜欢如下方案：

```c
for(;;)
{

}
```

这个实现方式让我为难，因为这个语法没有确切表达到底怎么回事。如果一个应试者给出这个作为方案，我将用这个作为一个机会去探究他们这样做的基本原理。如果他们的基本答案是："我被教着这样做，但从没有想到过为什么。"这会给我留下一个坏印象。

第三个方案是用 goto
Loop:
...
goto Loop;
应试者如给出上面的方案，这说明或者他是一个汇编语言程序员（这也许是好事）或者他是一个想进入新领域的BASIC/FORTRAN程序员。

## 数据声明（Data declarations）

> 用变量a给出下面的定义

a) 一个整型数（An integer）
b)一个指向整型数的指针（ A pointer to an integer）
c)一个指向指针的的指针，它指向的指针是指向一个整型数（ A pointer to a pointer to an integer）
d)一个有10个整型数的数组（ An array of 10 integers）
e) 一个有10个指针的数组，该指针是指向一个整型数的。（An array of 10 pointers to integers）
f) 一个指向有10个整型数数组的指针（ A pointer to an array of 10 integers）
g) 一个指向函数的指针，该函数有一个整型参数并返回一个整型数（A pointer to a function that takes an integer as an argument and returns an integer）
h) 一个有10个指针的数组，该指针指向一个函数，该函数有一个整型参数并返回一个整型数（ An array of ten pointers to functions that take an integer argument and return an integer ）

答案是：
a) int a; // An integer
b) int *a; // A pointer to an integer
c) int **a; // A pointer to a pointer to an integer
d) int a[10]; // An array of 10 integers
e) int *a[10]; // An array of 10 pointers to integers
f) int (*a)[10]; // A pointer to an array of 10 integers
g) int (*a)(int); // A pointer to a function a that takes an integer argument and returns an integer
h) int (*a[10])(int); // An array of 10 pointers to functions that take an integer argument and return an integer

## Static

1. 关键字static的作用是什么？

这个简单的问题很少有人能回答完全。在C语言中，关键字static有三个明显的作用：
(1) 在函数体，一个被声明为静态的变量在这一函数被调用过程中维持其值不变。
(2) 在模块内（但在函数体外），一个被声明为静态的变量可以被模块内所有函数访问，但不能被模块外其它函数访问。它是一个本地的全局变量。
(3) 在模块内，一个被声明为静态的函数只可被这一模块内的其它函数调用。那就是，这个函数被限制在声明它的模块的本地范围内使用。

大多数应试者能正确回答第一部分，一部分能正确回答第二部分，同是很少的人能懂得第三部分。这是一个应试者的严重的缺点，因为他显然不懂得本地化数据和代码范围的好处和重要性。

## Const

> 关键字const有什么含意？

我只要一听到被面试者说："const意味着常数"，我就知道我正在和一个业余者打交道。去年Dan Saks已经在他的文章里完全概括了const的所有用法，因此ESP(译者：Embedded Systems Programming)的每一位读者应该非常熟悉const能做什么和不能做什么.如果你从没有读到那篇文章，只要能说出const意味着"只读"就可以了。尽管这个答案不是完全的答案，但我接受它作为一个正确的答案。（如果你想知道更详细的答案，仔细读一下Saks的文章吧。）
如果应试者能正确回答这个问题，我将问他一个附加的问题：
下面的声明都是什么意思？

const int a;
int const a;
const int *a; 常整数
int * const a; 常指针
int const * a const;

/******/
前两个的作用是一样，a是一个常整型数。
第三个意味着a是一个指向常整型数的指针（也就是，整型数是不可修改的，但指针可以）。
第四个意思a是一个指向整型数的常指针（也就是说，指针指向的整型数是可以修改的，但指针是不可修改的）。
最后一个意味着a是一个指向常整型数的常指针（也就是说，指针指向的整型数是不可修改的，同时指针也是不可修改的）。
如果应试者能正确回答这些问题，那么他就给我留下了一个好印象。顺带提一句，也许你可能会问，即使不用关键字 const，也还是能很容易写出功能正确的程序，那么我为什么还要如此看重关键字const呢？我也如下的几下理由：
1) 关键字const的作用是为给读你代码的人传达非常有用的信息，实际上，声明一个参数为常量是为了告诉了用户这个参数的应用目的。如果你曾花很多时间清理其它人留下的垃圾，你就会很快学会感谢这点多余的信息。（当然，懂得用const的程序员很少会留下的垃圾让别人来清理的。）
2) 通过给优化器一些附加的信息，使用关键字const也许能产生更紧凑的代码。
3) 合理地使用关键字const可以使编译器很自然地保护那些不希望被改变的参数，防止其被无意的代码修改。简而言之，这样可以减少bug的出现。

## Volatile

>关键字volatile有什么含意?并给出三个不同的例子。

一个定义为volatile的变量是说这变量可能会被意想不到地改变，这样，编译器就不会去假设这个变量的值了。精确地说就是，优化器在用到这个变量时必须每次都小心地重新读取这个变量的值，而不是使用保存在寄存器里的备份。下面是volatile变量的几个例子：

1) 并行设备的硬件寄存器（如：状态寄存器）
2) 一个中断服务子程序中会访问到的非自动变量(Non-automatic variables)
3) 多线程应用中被几个任务共享的变量

回答不出这个问题的人是不会被雇佣的。我认为这是区分C程序员和嵌入式系统程序员的最基本的问题。搞嵌入式的家伙们经常同硬件、中断、RTOS等等打交道，所有这些都要求用到volatile变量。不懂得volatile的内容将会带来灾难。

假设被面试者正确地回答了这是问题（嗯，怀疑是否会是这样），我将稍微深究一下，看一下这家伙是不是直正懂得volatile完全的重要性。

1) 一个参数既可以是const还可以是volatile吗？解释为什么。
2) 一个指针可以是volatile 吗？解释为什么。
3) 下面的函数有什么错误：

```c
int square(volatile int *ptr)
{
        return *ptr * *ptr;
}
```

下面是答案：

1) 是的。一个例子是只读的状态寄存器。它是volatile因为它可能被意想不到地改变。它是const因为程序不应该试图去修改它。
2) 是的。尽管这并不很常见。一个例子是当一个中服务子程序修该一个指向一个buffer的指针时。
3) 这段代码有点变态。这段代码的目的是用来返指针*ptr指向值的平方，但是，由于*ptr指向一个volatile型参数，编译器将产生类似下面的代码：

```c
int square(volatile int *ptr) 
{
    int a,b;
    a = *ptr;
    b = *ptr;
    return a * b;
}
```

由于*ptr的值可能被意想不到地该变，因此a和b可能是不同的。结果，这段代码可能返不是你所期望的平方值！正确的代码如下：

```c
long square(volatile int *ptr) 
{
    int a;
    a = *ptr;
    return a * a;
}
```

## 位操作（Bit manipulation）

>嵌入式系统总是要用户对变量或寄存器进行位操作。给定一个整型变量a，写两段代码，第一个设置a的bit 3，第二个清除a 的bit 3。在以上两个操作中，要保持其它位不变。

对这个问题有三种基本的反应

1) 不知道如何下手。该被面者从没做过任何嵌入式系统的工作。
2) 用bit fields。Bit fields是被扔到C语言死角的东西，它保证你的代码在不同编译器之间是不可移植的，同时也保证了的你的代码是不可重用的。我最近不幸看到 Infineon为其较复杂的通信芯片写的驱动程序，它用到了bit fields因此完全对我无用，因为我的编译器用其它的方式来实现bit fields的。从道德讲：永远不要让一个非嵌入式的家伙粘实际硬件的边。
3) 用 #defines 和 bit masks 操作。这是一个有极高可移植性的方法，是应该被用到的方法。最佳的解决方案如下：

```c
#define BIT3 (0x1 << 3)
static int a;

void set_bit3(void) 
{
    a |= BIT3;
}
void clear_bit3(void) 
{
    a &= ~BIT3;
}
```

一些人喜欢为设置和清除值而定义一个掩码同时定义一些说明常数，这也是可以接受的。我希望看到几个要点：说明常数、|=和&=~操作。

访问固定的内存位置（Accessing fixed memory locations）

>嵌入式系统经常具有要求程序员去访问某特定的内存位置的特点。在某工程中，要求设置一绝对地址为0x67a9的整型变量的值为0xaa66。编译器是一个纯粹的ANSI编译器。写代码去完成这一任务。

这一问题测试你是否知道为了访问一绝对地址把一个整型数强制转换（typecast）为一指针是合法的。这一问题的实现方式随着个人风格不同而不同。典型的类似代码如下：

```c
    int *ptr;
    ptr = (int *)0x67a9;
    *ptr = 0xaa55;
```

A more obscure approach is:
一个较晦涩的方法是：

`*(int * const)(0x67a9) = 0xaa55;`

即使你的品味更接近第二种方案，但我建议你在面试时使用第一种方案。

## 中断（Interrupts）

> 中断是嵌入式系统中重要的组成部分，这导致了很多编译开发商提供一种扩展—让标准C支持中断。具代表事实是，产生了一个新的关键字 __interrupt。下面的代码就使用了__interrupt关键字去定义了一个中断服务子程序(ISR)，请评论一下这段代码的。

```c
__interrupt double compute_area (double radius)
{
    double area = PI * radius * radius;
    printf("\nArea = %f", area);
    return area;
}
```

这个函数有太多的错误了，以至让人不知从何说起了：

1) ISR 不能返回一个值。如果你不懂这个，那么你不会被雇用的。
2) ISR 不能传递参数。如果你没有看到这一点，你被雇用的机会等同第一项。
3) 在许多的处理器/编译器中，浮点一般都是不可重入的。有些处理器/编译器需要让额处的寄存器入栈，有些处理器/编译器就是不允许在ISR中做浮点运算。此外，ISR应该是短而有效率的，在ISR中做浮点运算是不明智的。
4) 与第三点一脉相承，printf()经常有重入和性能上的问题。如果你丢掉了第三和第四点，我不会太为难你的。不用说，如果你能得到后两点，那么你的被雇用前景越来越光明了。

## 代码例子（Code examples）

>12 . 下面的代码输出是什么，为什么？

```c
void foo(void)
{
    unsigned int a = 6;
    int b = -20;
    (a+b > 6) ? puts("> 6") : puts("<= 6");
}
```

这个问题测试你是否懂得C语言中的整数自动转换原则，我发现有些开发者懂得极少这些东西。不管如何，这无符号整型问题的答案是输出是 ">6"。原因是当表达式中存在有符号类型和无符号类型时所有的操作数都自动转换为无符号类型。因此-20变成了一个非常大的正整数，所以该表达式计算出的结果大于6。这一点对于应当频繁用到无符号数据类型的嵌入式系统来说是丰常重要的。如果你答错了这个问题，你也就到了得不到这份工作的边缘。

>13 . 评价下面的代码片断：

```c
unsigned int zero = 0;
unsigned int compzero = 0xFFFF; 
/*1's complement of zero */
```

对于一个int型不是16位的处理器为说，上面的代码是不正确的。应编写如下：

unsigned int compzero = ~0;

这一问题真正能揭露出应试者是否懂得处理器字长的重要性。在我的经验里，好的嵌入式程序员非常准确地明白硬件的细节和它的局限，然而PC机程序往往把硬件作为一个无法避免的烦恼。

到了这个阶段，应试者或者完全垂头丧气了或者信心满满志在必得。如果显然应试者不是很好，那么这个测试就在这里结束了。但如果显然应试者做得不错，那么我就扔出下面的追加问题，这些问题是比较难的，我想仅仅非常优秀的应试者能做得不错。提出这些问题，我希望更多看到应试者应付问题的方法，而不是答案。不管如何，你就当是这个娱乐吧...

## 动态内存分配（Dynamic memory allocation）

>14 . 尽管不像非嵌入式计算机那么常见，嵌入式系统还是有从堆（heap）中动态分配内存的过程的。
>那么嵌入式系统中，动态分配内存可能发生的问题是什么？

这里，我期望应试者能提到内存碎片，碎片收集的问题，变量的持行时间等等。这个主题已经在ESP杂志中被广泛地讨论过了（主要是 P.J. Plauger, 他的解释远远超过我这里能提到的任何解释），所有回过头看一下这些杂志吧！让应试者进入一种虚假的安全感觉后，我拿出这么一个小节目：
下面的代码片段的输出是什么，为什么？

```c
char *ptr;
if ((ptr = (char *)malloc(0)) == NULL) 
    puts("Got a null pointer");
else
    puts("Got a valid pointer");//有效指针
```

这是一个有趣的问题。最近在我的一个同事不经意把0值传给了函数malloc，得到了一个合法的指针之后，我才想到这个问题。这就是上面的代码，该代码的输出是"Got a valid pointer"。我用这个来开始讨论这样的一问题，看看被面试者是否想到库例程这样做是正确。得到正确的答案固然重要，但解决问题的方法和你做决定的基本原理更重要些。

## Typedef

>15 Typedef 在C语言中频繁用以声明一个已经存在的数据类型的同义字。也可以用预处理器做类似的事。例如，思考一下下面的例子：

```c
#define dPS struct s *
typedef struct s * tPS;
```

以上两种情况的意图都是要定义dPS 和 tPS 作为一个指向结构s指针。哪种方法更好呢？（如果有的话）为什么？
这是一个非常微妙的问题，任何人答对这个问题（正当的原因）是应当被恭喜的。

答案是：typedef更好。思考下面的例子：

dPS p1,p2;
tPS p3,p4;

第一个扩展为

struct s * p1, p2;
.
上面的代码定义p1为一个指向结构的指，p2为一个实际的结构，这也许不是你想要的。第二个例子正确地定义了p3 和p4 两个指针。

## 晦涩的语法

> 16 . C语言同意一些令人震惊的结构,下面的结构是合法的吗，如果是它做些什么？

int a = 5, b = 7, c;
c = a+++b;

这个问题将做为这个测验的一个愉快的结尾。不管你相不相信，上面的例子是完全合乎语法的。问题是编译器如何处理它？水平不高的编译作者实际上会争论这个问题，根据最处理原则，编译器应当能处理尽可能所有合法的用法。因此，上面的代码被处理成：

c = a++ + b;

因此, 这段代码持行后a = 6, b = 7, c = 12。
如果你知道答案，或猜出正确答案，做得好。如果你不知道答案，我也不把这个当作问题。我发现这个问题的最大好处是这是一个关于代码编写风格，代码的可读性，代码的可修改性的好的话题。

## 上海某全球五百强面试题（嵌入式）

>1.static变量和static 函数各有什么特点？

 Static 变量特点：

1. **作用域限制**：仅在声明它的函数或代码块内可见。
2. **生命周期**：在整个程序运行期间都存在，不会随着函数调用结束而销毁。
3. **初始化**：只初始化一次，之后每次函数调用时保持上次的值。
4. **存储位置**：位于静态存储区。

Static 函数特点：

1. **作用域限制**：仅在其声明的文件内可见，不能被其他文件中的代码访问。
2. **隐藏性**：防止命名冲突，可以在多个文件中使用相同的名字而不会产生冲突。
3. **调用**：可以在声明它的文件内的任何地方被调用。
4. **存储位置**：与普通函数相同，但具有文件局部可见性。

>3.描述一下嵌入式基于ROM的运行方式基于RAM的运行方式有什么区别。

基于ROM的运行方式：

1. **存储位置**：程序代码存储在非易失性存储器（如ROM、Flash）中。
2. **持久性**：即使断电后，程序代码仍然保留在存储器中。
3. **更新难度**：由于ROM是非易失性的，因此更新程序代码较为困难。
4. **性能**：直接从ROM执行程序通常比从RAM执行要慢一些，因为ROM的读取速度一般低于RAM。
5. **成本与可靠性**：ROM/Flash成本相对较低且更加可靠，因为它们没有活动部件。

 基于RAM的运行方式：

1. **存储位置**：程序代码加载到易失性存储器（如RAM）中。
2. **持久性**：一旦断电，RAM中的数据会丢失。
3. **更新便捷性**：可以很容易地更新或修改程序代码。
4. **性能**：从RAM执行程序通常比从ROM快，因为RAM提供更快的数据存取速度。
5. **成本与可靠性**：RAM的成本相对较高，且可能不如ROM/Flash可靠，因为它有活动部件并且容易受到电源影响。

简单来说，基于ROM的运行方式适用于不需要频繁更新程序且对成本敏感的应用场景；而基于RAM的运行方式更适合需要快速执行代码或经常更新程序的应用。

>4.task 有几种状态？

在嵌入式系统或操作系统中，任务（有时也称为线程或进程）的状态可以分为几种基本类型。这些状态反映了任务在其生命周期中的不同阶段。以下是常见的几种状态：

1. **就绪（Ready）**：
   - 任务已经准备好运行，等待CPU分配时间片。

2. **运行（Running）**：
   - 任务正在使用CPU执行。

3. **阻塞（Blocked）** 或 **等待（Waiting）**：
   - 任务因等待某个事件发生（例如等待资源可用、等待I/O操作完成等）而暂时无法继续执行。

4. **挂起（Suspended）** 或 **休眠（Sleeping）**：
   - 任务被暂停，但不是因为等待特定事件，而是因为调度决策或其他原因（例如低优先级导致被更高优先级的任务抢占）。

5. **终止（Terminated）** 或 **完成（Completed）**：
   - 任务已完成其执行并退出，或者被明确地终止。

有些操作系统或实时操作系统（RTOS）可能会有一些额外的状态或更详细的划分，但以上是最常见的几种状态。这些状态可以帮助我们理解任务是如何被管理和调度的。

>5.task 有几种通讯方式？

在嵌入式系统或操作系统中，任务（或线程、进程）之间的通信是实现多任务协同工作的重要手段。任务间的通信方式多种多样，根据不同的需求和系统特性，常见的几种通信方式包括：

1. **信号量（Semaphores）**：
   - 用于同步多个任务或保护共享资源，通过信号量可以实现互斥访问和资源计数。

2. **消息队列（Message Queues）**：
   - 允许任务间发送消息，每个消息都有一个标识符，可以用来传递信息或命令。

3. **共享内存（Shared Memory）**：
   - 多个任务可以访问同一段内存区域来交换数据，通常需要结合其他同步机制（如信号量）使用以避免竞态条件。

4. **事件标志组（Event Flags）**：
   - 用于通知一个或多个任务某些事件的发生，通常用于简单的同步目的。

5. **邮箱（Mailboxes）**：
   - 类似于消息队列，但通常针对特定类型的数据结构进行优化，适合于传递复杂的数据结构。

6. **管道（Pipes）**：
   - 提供了一种单向或双向的数据传输通道，适用于父子进程之间的通信，在某些嵌入式系统中可能不常用。

7. **互斥锁（Mutexes）**：
   - 用于保护共享资源免受并发访问的影响，确保一次只有一个任务能够访问资源。

8. **条件变量（Condition Variables）**：
   - 与互斥锁配合使用，允许任务等待特定条件成立。

9. **远程过程调用（Remote Procedure Calls, RPCs）**：
   - 在分布式系统中，允许一个任务调用另一个任务的功能，如同本地调用一样。

每种通信方式都有其适用场景和优缺点，选择合适的通信机制取决于具体的应用需求、系统的实时性和响应时间要求等因素。

>6.C函数允许重入吗？

C语言本身并不直接规定函数是否必须是可重入的（reentrant），这主要取决于函数的设计和实现。可重入性是指函数能够在多个执行环境中同时运行而不干扰彼此的能力。为了使一个函数成为可重入的，它必须满足以下条件：

1. **没有全局变量依赖**：函数不应依赖于任何全局变量或静态变量，因为这些变量会被所有调用所共享。

2. **没有静态局部变量**：函数不应使用静态局部变量，因为静态局部变量在函数多次调用之间保持不变，这可能导致意外的结果。

3. **避免使用不可重入的库函数**：如果函数使用了不可重入的库函数，则整个函数也将变得不可重入。

4. **使用可重入版本的库函数**：如果可能，应使用可重入版本的库函数，例如使用`strncpy_s`代替`strcpy`。

5. **使用互斥锁等同步机制**：对于必须共享的数据结构，可以通过互斥锁等同步机制来保证函数的可重入性。

6. **避免使用标准输入输出**：标准输入输出通常是不可重入的，因为它们依赖于全局状态。

7. **避免使用堆栈溢出的风险**：递归函数如果处理不当也可能导致不可重入。

 示例说明：

- **不可重入的例子**：

```c
  int global_var = 0;

  void not_reentrant() {
      global_var++; // 使用全局变量
  }
```

- **可重入的例子**：

```c
  void reentrant(int *var) {
      (*var)++; // 使用传入的指针，不使用全局变量
  }
```

总结：

- C语言中的函数默认情况下可能是不可重入的，特别是当它们依赖于全局状态时。
- 如果需要函数具备可重入性，则需要在设计时遵循上述原则。
- 实际应用中，特别是在多线程或多任务环境中，确保函数可重入是非常重要的，以避免数据竞争和同步问题。

>7.嵌入式操作系统和通用操作系统有什么差别？

 嵌入式操作系统与通用操作系统的差别

嵌入式操作系统（Embedded Operating System, EOS）和通用操作系统（General Purpose Operating System, GPOS）在设计目标、功能特性和应用场景上有所不同。下面是一些主要的区别：

 设计目标与应用场景：

- **嵌入式操作系统**：
  - 主要用于嵌入式设备，如微控制器、智能家电、汽车电子系统等。
  - 强调实时性、资源效率和可靠性。
  - 通常针对特定硬件平台优化。

- **通用操作系统**：
  - 适用于个人电脑、服务器和其他通用计算平台。
  - 强调用户友好性、多任务处理能力和广泛的软件支持。

功能特性：

- **实时性**：
  - **EOS**：通常具有硬实时或软实时能力，能够满足严格的时限要求。
  - **GPOS**：不总是具备实时特性，更侧重于公平调度和用户交互体验。

- **资源管理**：
  - **EOS**：资源有限，需要高效利用内存、CPU时间和外设接口。
  - **GPOS**：资源相对丰富，支持更多的进程和线程，并具有高级的虚拟内存管理机制。

- **启动时间**：
  - **EOS**：启动速度快，以便快速响应用户或外部事件。
  - **GPOS**：启动时间较长，因为需要加载更多的服务和驱动程序。

- **定制化**：
  - **EOS**：高度可定制，可以根据具体应用需求裁剪系统。
  - **GPOS**：提供广泛的服务和工具，但通常不易裁剪。

- **用户界面**：
  - **EOS**：通常没有图形用户界面（GUI），或使用轻量级GUI。
  - **GPOS**：通常提供丰富的GUI环境，便于用户操作。

- **安全性和可靠性**：
  - **EOS**：特别强调安全性与可靠性，因为很多嵌入式系统涉及到关键任务应用。
  - **GPOS**：虽然也很重视安全性和可靠性，但通常不如EOS严格。

- **开发和维护**：
  - **EOS**：通常由专门的团队开发和维护，面向特定应用领域。
  - **GPOS**：由大型社区或公司维护，支持广泛的开发者和用户群体。

- **更新周期**：
  - **EOS**：更新周期可能较长，因为更新通常涉及现场部署设备。
  - **GPOS**：更新更为频繁，以适应新技术和用户需求的变化。

总结：

- 嵌入式操作系统通常是为了满足特定的应用需求而设计的，强调实时性、资源效率和可靠性。
- 通用操作系统则更加注重用户体验、广泛的软件兼容性和强大的功能集。

选择哪种操作系统取决于具体的使用场景和要求。

## 嵌入式软件工程师笔试题

1、将一个字符串逆序
2、将一个链表逆序
3、计算一个字节里（byte）里面有多少bit被置1
4、搜索给定的字节(byte)
5、在一个字符串中找到可能的最长的子字符串
6、字符串转换为整数
7、整数转换为字符串

```c
/*
* 题目：将一个字符串逆序
*/
#include <stdio.h>
#include <string.h>

//使用临时变量进行字符交换
void reverString(char *str)
{
    int len=strlen(str);
    for (int i=0;i<len/2;i++)
    {
        char temp=str[i];
        str[i]=str[len-i-1];
        str[len-i-1]=temp;
    }
}

//使用辅助数组
char* reverseString(const char *str)
{
    int len =strlen(str);
    char *revStr=malloc((len+1)*sizeof(char))
    if(revStr==NULL)
        return NULL;
    for(int i=0; i<len;i++)
    {
        revStr[i]=str[len-i-1];
    }
    revStr[len]='\0';
}

```

```c
/*
* 题目：将一个字符串逆序
*/
#include <stdio.h>
#include <stdlib.h>

//定义链表
typdef struct Node{
    int data;
    struct Node *next
}Node;

// 创建新节点的函数
Node* createNode(int data) {
    Node *newNode = (Node*)malloc(sizeof(Node));
    if (newNode == NULL) {
        printf("Memory allocation failed.\n");
        exit(1);
    }
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// 逆序链表的函数
Node* reverseList(Node *head) {
    Node *prev = NULL;
    Node *current = head;
    Node *next = NULL;
    while (current != NULL) {
        next = current->next; // 保存下一个节点
        current->next = prev; // 反转当前节点的指针
        prev = current;       // 移动prev
        current = next;       // 移动current
    }
    head = prev;             // 新的头节点
    return head;
}

// 打印链表的函数

void printList(Node *head) {
    Node *temp = head;
    while (temp != NULL) {
        printf("%d -> ", temp->data);
        temp = temp->next;
    }
    printf("NULL\n");
}


int main() {
    // 创建链表
    Node *head = createNode(1);
    head->next = createNode(2);
    head->next->next = createNode(3);
    head->next->next->next = createNode(4);
    head->next->next->next->next = createNode(5);
    // 打印原始链表
    printf("Original list: ");
    printList(head);
    // 逆序链表
    head = reverseList(head);
    // 打印逆序后的链表
    printf("Reversed list: ");
    printList(head);
    // 清理内存
    Node *temp;
    while (head != NULL) {
        temp = head;
        head = head->next;
        free(temp);
    }
    return 0;
}
```

```c
/*
*计算一个字节里（byte）里面有多少bit被置1
*/

#include <stdio.h>

int countSetBitsA(unsigned char byte) {
    int count = 0;
    unsigned char mask = 1; // 初始掩码为最低位1
    for (int i = 0; i < 8; i++) { // 一个字节有8位
        if ((byte & mask) != 0) {
            count++;
        }
        mask <<= 1; // 将掩码左移一位
    }

    return count;
}


int countSetBitsB(unsigned char byte) {
    int count = 0;
    while (byte) {
        byte &= byte - 1; // 清除最低位的1
        count++;
    }
    return count;
}

int main() {
    unsigned char byte = 0xAA; // 10101010
    int numSetBits = countSetBitsA(byte);
    printf("Number of set bits in 0x%02X: %d\n", byte, numSetBits);
    int numSetBits = countSetBitsB(byte);
    printf("Number of set bits in 0x%02X: %d\n", byte, numSetBits);
    return 0;
}

```

1。编写一个 C 函数，该函数在一个字符串中找到可能的最长的子字符串，且该字符串是由同一字符组成的。

试题1：请写一个C函数，若处理器是Big_endian的，则返回0；若是Little_endian的，则返回1
解答：

```c
int checkCPU( )
{ 
    { 
        union w 
        {   
            int  a; 
            char b; 
        } c; 
        c.a = 1; 
           return(c.b ==1);
    s} 
} 
```

剖析：

嵌入式系统开发者应该对Little-endian和Big-endian模式非常了解。采用Little-endian模式的CPU对操作数的存放方式是从低字节到高字节，而Big-endian模式对操作数的存放方式是从高字节到低字节。例如，16bit宽的数0x1234在Little-endian模式CPU内存中的存放方式（假设从地址0x4000开始存放）为：

内存地址    0x4000	 0x4001
存放内容	0x34	 0x12

而在Big-endian模式CPU内存中的存放方式则为：
内存地址	0x4000	0x4001
存放内容	0x12	0x34
32bit宽的数0x12345678在Little-endian模式CPU内存中的存放方式（假设从地址0x4000开始存放）为：
内存地址	0x4000	0x4001	0x4002	0x4003
存放内容	0x78	0x56	0x34	0x12
而在Big-endian模式CPU内存中的存放方式则为：
内存地址	0x4000	0x4001	0x4002	0x4003
存放内容	0x12	0x34	0x56	0x78

联合体union的存放顺序是所有成员都从低地址开始存放，面试者的解答利用该特性，轻松地获得了CPU对内存采用Little-endian还是Big-endian模式读写。如果谁能当场给出这个解答，那简直就一个天才的程序员。

>2.

```c
char str[] = “Hello” ;
char *p = str ;
int n = 10;

//请计算
sizeof (str ) = 6 （2分） 
sizeof ( p ) = 4 （2分）
sizeof ( n ) = 4 （2分）

```

```c
void Func ( char str[100])
{
//请计算
sizeof( str ) = 4 （2分）
}

void *p = malloc( 100 );
//请计算
sizeof ( p ) = 4 （2分）

```

>3、在C++程序中调用被 C编译器编译后的函数，为什么要加 extern “C”？ （5分）

答：C++语言支持函数重载，C语言不支持函数重载。函数被C++编译后在库中的名字与C语言的不同。假设某个函数的原型为： void foo(int x, int y);
该函数被C编译器编译后在库中的名字为_foo，而C++编译器则会产生像_foo_int_int之类的名字。
C++提供了C连接交换指定符号extern“C”来解决名字匹配问题。\

4.有关内存的思考题

```c
void GetMemory(char *p)
{
    p = (char *)malloc(100);
}
void Test(void) 
{
    char *str = NULL;
    GetMemory(str); 
    strcpy(str, "hello world");
    printf(str);
}
```

请问运行Test函数会有什么样的结果？

答:程序崩溃。

因为GetMemory并不能传递动态内存，

Test函数中的 str一直都是 NULL。

strcpy(str, "hello world");将使程序崩溃。

```c
char *GetMemory(void)
{ 
    char p[] = "hello world";
    return p;
}
void Test(void)
{
    char *str = NULL;
    str = GetMemory(); 
    printf(str);
}
```

请问运行Test函数会有什么样的结果？
答：可能是乱码。
因为GetMemory返回的是指向“栈内存”的指针，该指针的地址不是 NULL，但其原现的内容已经被清除，新内容不可知。

```C
void GetMemory2(char **p, int num)
{
    *p = (char *)malloc(num);
}
void Test(void)
{
    char *str = NULL;
    GetMemory(&str, 100);
    strcpy(str, "hello"); 
    printf(str); 
}
```

请问运行Test函数会有什么样的结果？
答：
（1）能够输出hello
（2）内存泄漏

```C
void Test(void)
{
    char *str = (char *) malloc(100);
    strcpy(str, “hello”);
    free(str); 
    if(str != NULL)
    {
        strcpy(str, “world”); 
        printf(str);
    }
}
```

请问运行Test函数会有什么样的结果？

答：篡改动态内存区的内容，后果难以预料，非常危险。
因为free(str);之后，str成为野指针，
if(str != NULL)语句不起作用。
