# C语言基础

## 基础知识

1. 数据类型：char,int,long,signed,unsigned,float,double,sizeof()
2. 运算与控制
3. 数据存储: static,const,const,volatile
4. 结构:struct.union,enum,typedef
5. 位操作与逻辑运算:<< ,>>,&,|,~,^
6. 预处理

位、字节、字关系
位：
字节
字word:计算机进行数据处理和运算的单位，32位中4字节=1字，64位中，8字节=1字

## 进制转换

二进制 0b
八进制 oct
十进制 dec
十六进制 hex

C语言中，变量只能由数字、字母、下划线构成，且必须由字母开头。

## 位运算

"<<":左移：高位丢弃，低位补零，每次左移相当于乘以2

">>"右移：低位丢弃，高位补零。每次右移相当于除以2

~ 取反：0变1，1变0

& 与运算：双方都为1，才为1否则为0

| 或运算：双方只有一个1，结果就为1

让GPIO_7和GPIO_9输出高电平
volatitlr unsigned int *pGpiodObr =；
*pGpiodbr |= ((1<<7)|(1<<9));
让GPIO_7和GPIO_9输出低电平
*pGpiodbr &=~((1<<7)|(1<<9));
获取GPIO_7当前状态
if(*pGpiodvr & (1<<7))
{
    a=1;
}else
    a=0;

## 封装寄存器

volatile unsigned int *pGpiobOdr =(volatile unsigned int *)(0x40010c00+0xc0);

![alt text](img/image0.png)
STM32
存储器与IO 外设采用统一编址
空间分为8快，每块512M ，总计8G
卡中Block2用于片上外设0x4000 0000 ~0x5fff ffff
GPIOB挂在APB2下，0x40001 0c00~0x4001 0fff

volatile unsigned int *pGpiobOdr =(volatile unsigned int *)(0x40010c00+0xc0);

volatile:声明从原始地址取值，告诉编译器不要应为优化而忽略这个指令，必须每次读取数据都从地址上直接读取值，确保每次读取值都读取到最新的值
unsigned int 声明数据类型为无符号整型
*pGpiobOdr：声明指针变量
(volatile unsigned int *)强制转化数据类型
(0x40010c00+0xc0)存储的地址，寄存器地址

![alt text](image.png)

## 函数指针

![alt text](image-1.png)

![alt text](image-2.png)

"""
//加法函数
int add(int a,int b )
{
    return a+b;
}
int sub(int a ,int b)
{
    return a-b;
}
#if 1
//定义函数指针别名
typedef int(*pfun)(int,int);

//计算函数
int calc(pfun fp)(int,int)
{
    return fp(a,b);
}
#else
    //不定义函数指针别名
    //typedef int(*pfun)(int,int);

    //计算函数
    //int calc(pfun fp)(int,int)
    //{
    //return fp(a,b);
    //}
#endif

int main(void)
{
    int c;
    c=calc(add,5,3);
    c=calc(sub,5,3);
    return 0;
}

"""

## 链表

![链表与数组](image-3.png)
![链表](image-4.png)
![插入元素](image-5.png)
![定义节点与链表](image-6.png)
![初始化节点与链表](image-7.png)
![插入节点](image-8.png)
![删除节点](image-9.png)

## 扩展_指针与变量
