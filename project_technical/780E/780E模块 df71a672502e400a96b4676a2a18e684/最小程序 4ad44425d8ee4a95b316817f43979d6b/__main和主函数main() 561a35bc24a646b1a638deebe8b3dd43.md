# __main和主函数main()

### 一、__main和main()之间的关系

startup__ec618.s启动文件里面的Reset_Handler：

![Untitled](__main%E5%92%8C%E4%B8%BB%E5%87%BD%E6%95%B0main()%20561a35bc24a646b1a638deebe8b3dd43/Untitled.png)

调用过程：

![Untitled](__main%E5%92%8C%E4%B8%BB%E5%87%BD%E6%95%B0main()%20561a35bc24a646b1a638deebe8b3dd43/Untitled%201.png)

1. stm32在启动后先进入重启中断函数Reset_Handler，其中会先后调用SystemInit和__main函数，
2. __main函数属于c库函数，其内部依次进行三步工作，即先初始化rw段，然后初始化zi段，最后调用另一个c库函数__rt_entry()，
3. __rt_entry()该函数先初始化堆栈和库函数，然后即调用主函数main()，从而进入用户程序。可以看出主函数main()若退出，则在__rt_entry()最后会再调用exit()函数进行退出操作。
4. __main和__rt_entry这俩函数实际上我都没能力进的去，我也是找网上大神逆向分析出来的“借鉴”学习一下
    
    ![https://oss-emcsprod-public.modb.pro/image/auto/modb_20221108_4bf8024e-5f4f-11ed-bca3-fa163eb4f6be.png](https://oss-emcsprod-public.modb.pro/image/auto/modb_20221108_4bf8024e-5f4f-11ed-bca3-fa163eb4f6be.png)
    

**总结：stm32启动文件里面Reset_Handler最后调用了__main，而在__main里面最后调用了__rt_entry()，然后__rt_entry()在做完堆栈和库函数初始化工作之后才调用main()。**

### 二、修改主函数名称的方法

![Untitled](__main%E5%92%8C%E4%B8%BB%E5%87%BD%E6%95%B0main()%20561a35bc24a646b1a638deebe8b3dd43/Untitled%202.png)

Reset_Handler中导入和执行的**__main函数换成自己在c文件里随便定义的函数**即可，比如上图的testmain，我现在就是把在c文件里面定义的testmain函数作为主函数来用的。

注意这里有个误区，有的人可能会将__main换成比如__testmain，然后实际自己定义的是testmain，这样编译肯定通不过，然后就说什么stm32的主函数名改不了。因为__testmain没有定义啊，这个和__main不一样，__main是c的库函数，标准库自己包含的，**它是在内部调用的main()，而不是编译过程给它去掉了两个下划线__**

但由于**__main函数除了调用main()以外在前后还有初始化堆栈和库函数、调用exit()的操作**，而我这里直接把__main函数替换成自己想要运行的函数则不包含那些操作，**换句话说启动文件前面设置的堆和栈大小都白设置了，库里面有的值如果有设置的也白设置了，现在都成了默认的值，还有exit()里面有啥特别的用处也不了解，等于把一个本来该有但未知用途的模块删了**，这肯定是不行的。

### _main函数代码

startup函数通过ec_main函数进入，然后会将利用ld文件中的那写 LOAD$$RW__$$BASE （源地址，加载域）、Image$$RE_$$Base（目标地址，执行域） Image$$RW_$$Length （数据长度）

用这些将数据从加载域拷贝到执行域。 

entry point 程序的入口地址

中断向量表的入口地址就是程序的首地址

程序的首地址不一定是程序的入口地址

但程序的首地址是数据类型，而放的并不是代码。这个数据就是中断向量表里面的数据

cpu上电复位→0x08000000首地址是程序的数据段，→然后跳转到中断服务函数的程序入口处

中断向量表里面1，堆栈指针；2，复位中断函数的入口地址

```bash
weak 弱函数，如果在别的地方声明了一个函数名相同的函数，则编译时就会优先编译自己声明的。
而不会编译weak声明的函数
```

_main函数是keil里面生成的，如果说是不在keil的环境里，还想要编译运行在单片机中，就需要自己去写_main函数，从而去实现代码动加载域到执行域的拷贝。

[STM32启动详细流程之__mai.pdf](__main%E5%92%8C%E4%B8%BB%E5%87%BD%E6%95%B0main()%20561a35bc24a646b1a638deebe8b3dd43/STM32%25E5%2590%25AF%25E5%258A%25A8%25E8%25AF%25A6%25E7%25BB%2586%25E6%25B5%2581%25E7%25A8%258B%25E4%25B9%258B__mai.pdf)

中断向量表的首地址就是程序的入口地址

[STM32 | hex文件、bin文件、axf文件的区别？_stm32烧录axf程序_嵌入式大杂烩的博客-CSDN博客](https://blog.csdn.net/zhengnianli/article/details/103967617)

### 最小启动配置

注意：设置好SP（栈顶指针），就可以运行用户程序

1. 编写中断向量表
2. 编写复位中断函数
    
    2.1 设置堆栈指针
    
    2.2 跳转到main函数
    

![Untitled](__main%E5%92%8C%E4%B8%BB%E5%87%BD%E6%95%B0main()%20561a35bc24a646b1a638deebe8b3dd43/Untitled%203.png)