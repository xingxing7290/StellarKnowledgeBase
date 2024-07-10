# qemu+cortex_m4+freertos+gdbs

F407_Freertos文件中为移植freertos系统的keil文件。
依靠freertos中STM32F407的demo配置的文件

其中qemu 支持的cortex M4的开发板选择为mps2-an386，
芯片为STM32F405 内核为CortexM4

直接运行freertos中的程序，使用指令
qemu-system-arm.exe -M mps2-an386  -cpu cortex-m4  -kernel ./freertos/RTOSDemo.out   -net nic -net tap,ifname=tap0,script=no  -s -S
其中最后的-s -S 为配置debug模式。
以图形化显示时，设置显示端口时Serial0才会显示出来
