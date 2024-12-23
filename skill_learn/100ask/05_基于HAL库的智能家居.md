# 基于HAL的智能家居项目

## 学习顺序

## 2. 学习顺序

* 环境搭建及C语言基础知识
* 项目必备的HAL库基础
  * LED和按键
  * I2C协议和OLED操作
  * 串口操作
* AT指令(基于ESP8266)
  * 用于联网
* 项目1_基于HAL库的智能家居
  * 综合起来实现项目
  
## 系统框架

![alt text](image.png)

## 输入子系统

按键输入
网络输入
标准输入scanf

```c++
#define TIME_T int;
#define INPUT_BUF_LEN 1024  
typedef enum
{
  INPUT_EVENT_TYPE_KEY,//按键，触摸屏。网络。标准输入
  INPUT_EVENT_TYPE_TOUCH,
  INPUT_EVENT_TYPE_NET,
  INPUT_EVENT_TYPE_STDIO
}INPUT_EVENT_TYPE;

typedef struct InputEvnet
{
  TIME_T time;//实践
  int itype;
  int ikey;
  int iX;
  int iY;
  int ipressure;//1 按下 0 松开
  char str[INPUT_BUF_LEN];
}InputEvent,*PInputEvent;

typedef struct InPutDevice
{
  char *name;//key、net、 stdio
  int (*GetInputEvent)(PInPutEvent pEvent);
  int (*DeviceInit)(void);
  int (*DeviceExit)(void);
  struct InputDevice *pNext;//链表结构体
}InputDevice,*PInputDevice;

```
