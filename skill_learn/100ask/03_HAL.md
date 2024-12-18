# 使用CubeMX创建F103工程模板

课程流程
![alt text](image.png)

- 项目流程
![alt text](image-1.png)

## LED和按键原理概述

![alt text](image-2.png)

![alt text](image-3.png)

- LED:

![alt text](image-4.png)
![alt text](image-5.png)
![alt text](image-6.png)

- Key:

![alt text](image-7.png)
![alt text](image-8.png)
![alt text](image-9.png)
![alt text](image-10.png)

## OLED

- 结构
![alt text](image-11.png)

- I2C协议

硬件连接
![alt text](image-12.png)

I2C传输数据格式
一主多从

 写操作
    ![alt text](image-13.png)
 读操作
    ![alt text](image-14.png)
 I2C信号
    ![alt text](image-15.png)
    ![alt text](image-16.png)
    避免主从两端同时控制避免烧机
    ![alt text](image-17.png)