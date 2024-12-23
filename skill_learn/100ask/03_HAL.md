# 使用CubeMX创建F103工程模板

课程流程
![alt text](img/HAL/image.png)

- 项目流程
![alt text](img/HAL/image-1.png)

## LED和按键原理概述

![alt text](img/HAL/image-2.png)

![alt text](img/HAL/image-3.png)

- LED:

![alt text](img/HAL/image-4.png)
![alt text](img/HAL/image-5.png)
![alt text](img/HAL/image-6.png)

- Key:

![alt text](img/HAL/image-7.png)
![alt text](img/HAL/image-8.png)
![alt text](img/HAL/image-9.png)
![alt text](img/HAL/image-10.png)

## OLED

- 结构
![alt text](img/HAL/image-11.png)

- I2C协议

硬件连接
![alt text](img/HAL/image-12.png)

I2C传输数据格式
一主多从

 写操作
    ![alt text](img/HAL/image-13.png)
 读操作
    ![alt text](img/HAL/image-14.png)
 I2C信号
    ![alt text](img/HAL/image-15.png)
    ![alt text](img/HAL/image-16.png)
    避免主从两端同时控制避免烧机
    ![alt text](img/HAL/image-17.png)
I2C底层驱动
    ![alt text](img/HAL/image-18.png)
    ![alt text](img/HAL/image-19.png)
    ![alt text](img/HAL/image-20.png)
SSD1306的I2C数据格式和显存访问
   ![alt text](img/HAL/image-21.png)
   ![alt text](img/HAL/image-22.png)
   ![alt text](img/HAL/image-23.png)
   ![alt text](img/HAL/image-24.png)
   ![alt text](img/HAL/image-25.png)
显示器驱动开发与显示应用

## 串口通信

   ![alt text](img/HAL/image-26.png)
   基本概念
   ![alt text](img/HAL/image-27.png)
   ![alt text](img/HAL/image-28.png)
   ![alt text](img/HAL/image-29.png)
   ![alt text](img/HAL/image-30.png)
   ![alt text](img/HAL/image-31.png)
   ![alt text](img/HAL/image-32.png)
   ![alt text](img/HAL/image-33.png)
   ![alt text](img/HAL/image-34.png)
   ![alt text](img/HAL/image-35.png)

![alt text](img/HAL/image-36.png)
![alt text](img/HAL/image-37.png)
![alt text](img/HAL/image-38.png)
![alt text](img/HAL/image-39.png)
![alt text](img/HAL/image-40.png)
![alt text](img/HAL/image-41.png)

![alt text](img/HAL/image-42.png)
![alt text](img/HAL/image-43.png)
![alt text](img/HAL/image-44.png)