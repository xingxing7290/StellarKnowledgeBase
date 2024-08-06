# 串口DMA

### dma直接内存访问

780E内有内核cortex_m3 同时还应该有DMA控制器（不知道有几个）同时DMA控制器应该有7个通道

DMA控制器是独立的部分，独立的外设

dma控制器驱动头文件

luatos-soc-2022\PLAT\driver\chip\ec618\ap\inc\dma.h

i2s 集成电路内置音频总线。是数字音频设备之间的音频数据传输而制定的一种总线标准。