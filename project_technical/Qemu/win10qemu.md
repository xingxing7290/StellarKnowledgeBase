# windows10中配置使用QEMU

## 下载qemu

<https://www.qemu.org/download/>

## 配置网桥

参考文章
<https://blog.csdn.net/jiangwei0512/article/details/122791901>

## 直接使用

qemu3 中使用是非uboot 的启动方式

```T
qemu-system-arm.exe -M vexpress-a9 -m 512M -kernel qemu3/zImage -dtb qemu3/vexpress-v2p-ca9.dtb -append "root=/dev/mmcblk0  rw console=tty0" -sd .\qemu3\rootfs-arm32.ext3 -net nic -net tap,ifname=tap0,script=no 
```

uboot中的是uboot 的启动方式

```T
qemu-system-arm.exe -M vexpress-a9 -m 512M -kernel .\uboot\u-boot -sd .\uboot\rootfs-arm.ext3 -net nic -net tap,ifname=tap0,script=no 
```

使用ssh 连接时，ssh root@192.168.126.123  密码:root
根目录下line是frame buffer的测试程序 ，画一根红色的线
