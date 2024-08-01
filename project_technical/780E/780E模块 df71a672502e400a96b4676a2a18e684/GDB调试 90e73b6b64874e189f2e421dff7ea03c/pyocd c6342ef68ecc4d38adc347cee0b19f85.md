# pyocd

[玩转 pyocd_Thunder - 格物博客-PC万里](https://gewu.pcwanli.com/front/article/14737.html)

一般可以烧录三种格式的文件：hex bin elf .其中bin和elf不带地址信息

libusb

libusb是一个跨平台的用户空间USB库，它允许您访问USB设备。它提供了一种简单的方法来编写用户空间驱动程序，以便与USB设备通信。

libusb和jlink

libusb和J-Link的官方驱动有很大的区别。libusb是一个跨平台的用户空间USB库，它允许您访问USB设备。而J-Link是一种常用的调试器，它支持多种CPU和开发环境。J-Link驱动程序是专门为J-Link调试器设计的，而libusb驱动程序是通用的USB驱动程序。因此，如果您使用J-Link进行调试，需要安装J-Link驱动程序，而不是libusb驱动程序。

udev 规则

在Linux上，从用户空间访问USB设备的权限必须通过udev规则显式授予。

[udev规则以及编写 - 大海中的一粒沙 - 博客园](https://www.cnblogs.com/fah936861121/p/6496608.html)

udev是Linux（linux2.6内核之后）默认的设备管理工具。udev 以守护进程的形式运行，通过侦听内核发出来的 uevent 来管理 /dev目录下的设备文件。