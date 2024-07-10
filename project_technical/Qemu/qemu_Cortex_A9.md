# QEMU 构建流程

## 1. 下载 qemu 软件

```S
https://www.qemu.org/download/#windows
```

ubuntu 中 qemu 为编译好里面 build 的 qemu-system-arm 可以直接使用
或者重新构建参考：<https://wiki.qemu.org/Hosts/Linux>

## 2.Linux 构建

### 源码下载

```S
https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.14.7.tar.xz
```

最新版本 Linux 会无法显示图形界面

### 编译器：sudo apt-get install gcc-arm-linux-gnueabi

### 构建

```S
    make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- vexpress_defconfig

    make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-  menuconfig
        System Type -->[ ] Enable the L2x0 outer cache controller
        #取消该选项，否则QEMU运行不起来
        Kernel Features -->[*] Use the ARM EABI to compile the kernel
        #确保该选项被选择
    make CROSS_COMPILE=arm-linux-gnueabi- ARCH=arm
    make  ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- LOADADDR=0X60003000 uImage

```

## 3.Busybos

### Busybos 源码下载

```S
https://busybox.net/downloads/busybox-1.36.1.tar.bz2
```

### Busybos 构建

```S
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- menuconfig
勾选 Busybox Setting-> Build Options-> [*] Build static binary (no shared libs)
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-  CONFIG_PREFIX=object/rootfs-arm32 install
```

### 构建目录

---

```s
mkdir dev
cd dev
```

```S
sudo mknod -m 666 dev/tty1 c 4 1
sudo mknod -m 666 dev/tty2 c 4 2
sudo mknod -m 666 dev/tty3 c 4 3
sudo mknod -m 666 dev/tty4 c 4 4
sudo mknod -m 666 dev/console  c 5 1
sudo mknod -m 666 dev/null  c 1 3
```

---

mkdir lib
sudo cp -d /usr/arm-linux-gnueabi/lib/_.so_ ./lib

---

配置初始化进程

```s
mkdir -p etc/init.d
touch etc/init.d/rcs
sudo chmod 777 etc/init.d/rcs
vim etc/init.d/rcs
cat etc/init.d/rcs
```

```s
#!/bin/sh
PATH=/bin:/sbin:/usr/bin:/usr/sbin
export LD_LIBRARY_PATH=/lib:/usr/lib
/bin/mount -n -t ramfs ramfs /var
/bin/mount -n -t ramfs ramfs /tmp
/bin/mount -n -t sysfs none /sys
/bin/mount -n -t ramfs none /dev
/bin/mkdir /var/tmp
/bin/mkdir /var/modules
/bin/mkdir /var/run
/bin/mkdir /var/log
/bin/mkdir -p /dev/pts
/bin/mkdir -p /dev/shm
/sbin/mdev -s
/bin/mount -a


#ssh
ifconfig lo 127.0.0.1
ifconfig eth0 192.168.126.123 promisc up
mkdir /var/empty
mkdir /var/empty/sshd
/usr/sbin/sshd &


echo "-----------------------------------------------"
echo "*****welcome to vexpress board by zjx@2024*****"
echo "-----------------------------------------------"

```

---

文件系统
touch etc/fstab
vim etc/fstab

```s
proc    /proc           proc    defaults        0       0
none    /dev/pts        devpts  mode=0622       0       0
mdev    /dev            ramfs   defaults        0       0
sysfs   /sys            sysfs   defaults        0       0
tmpfs   /dev/shm        tmpfs   defaults        0       0
tmpfs   /dev            tmpfs   defaults        0       0
tmpfs   /mnt            tmpfs   defaults        0       0
var     /dev            tmpfs   defaults        0       0
ramfs   /dev            ramfs   defaults        0       0
```

---

初始化脚本构建

touch etc/inittab

```s
::sysinit:/etc/init.d/rcs
::askfirst:-/bin/sh
::restart:/sbin/init
::ctrlaltdel:/sbin/reboot
::shutdown:/bin/umount -a -r
```

---

环境变量
touch etc/profile

```s
#!/bin/sh
USER="root"
LOGNAME=SUSER
export HOSTNAME=vexpress-a9
export HOSTNAME=`cat /etc/sysconfig/HOSTNAME`
export USER=root
export HOME=root
export PS1="[$USER@$HOSTNAME:\w]\#"
PATH=/bin:/sbin:/usr/bin:/usr/sbin
LD_LIBRARY_PATH=/lib:/usr/lib:$LD_LIBRARY_PATH
export PATH LD_LIBRARY_PATH
```

---

主机名文件配置
mkdir etc/sysconfig
vi etc/sysconfig/HOSTNAME

```s
vexpress-a9
```

---

构建其余文件
mkdir mnt proc root sys tmp var

## 根文件系统镜像

sd 卡的方式进行挂载

rootfs-arm32 同级目录下

```t
`dd if=/dev/zero of=rootfs-arm32.ext3 bs=1M count=512`
`mkfs.ext3 rootfs-arm32.ext3`
```

这是再 Ubuntu 中的

```s
sudo mkdir /mnt/rootfs
chmod 777 /mnt/rootfs
```

`sudo mount -t ext3 rootfs-arm32.ext3 /mnt/rootfs -o loop`

`sudo cp -rf rootfs-arm32/* /mnt/rootfs/  把内容复制到卡里`
根文件系统以 SD 的方式进行挂载

## 挂载指令

```t
./qemu/build/qemu-system-arm -M vexpress-a9 -m 512M -kernel ../../linux/arch/arm/boot/zImage -dtb  ../../linux/arch/arm/boot/dts/arm/vexpress-v2p-ca9.dtb -nographic -append "root=/dev/mmcblkoTrw console=ttyAMAO" -sd rootfs-arm32.ext3
```

指令说明
-append 选项的参数，从 sd 卡启动，root 所指定的位置是/dev/mmcblk0 就是 sd 卡设备挂载的目录, rw 表示根文件系统是可以读写的，console 用来设置 linux 终端，表示通过什么设备来和 Linux 交互，设置 console = ttyAMA0，因为 linux 启动以后板卡的串口 1 挂载在 linux 下的设备文件就是/dev/ttyAMA0 -nographic 是无图形界面，取消时，有图形界面的时候，需要更改为 console = tty0

退出 qemu
:Ctrl - A X 按下 Ctrl 键和 A 键, 然后释放这两个键,再按 X 键即可*退出* 注意: 同时按下三个键是无效的

## U-BOOT 方式引导内核和根文件系统

### 创建磁盘

```s
`sudo mkdir  -p /mnt/uboot`
` vi makefs-arm32-emmc.sh`
```

```t
dd if=/dev/zero of=rootfs-arm.ext3 bs=1M count=256
echo "hard disk partition!"
sgdisk -n 0:0:+32M -c 0:uboot rootfs-arm.ext3
sgdisk -n 0:0:0 -c 0:rootfs rootfs-arm.ext3
sgdisk -p rootfs-arm.ext3

echo "mount loop device!"
LOOPDEV=`losetup -f`

echo $LOOPDEV
sudo losetup $LOOPDEV rootfs-arm.ext3
sudo partprobe $LOOPDEV
sudo losetup -l
ls -l /dev/loop\*

echo "format disk to ext3"
echo ${LOOPDEV}P1
echo ${LOOPDEV}p2
sudo mkfs.ext3 ${LOOPDEV}p1
sudo mkfs.ext3 ${LOOPDEV}p2
sudo mount -t ext3 ${LOOPDEV}p1 /mnt/uboot -o loop
sudo mount -t ext3 ${LOOPDEV}p2 /mnt/rootfs -o loop
sudo cp -rf rootfs-arm32/\* /mnt/rootfs/
sudo cp ../linux/arch/arm/boot/dts/arm/vexpress-v2p-ca9.dtb /mnt/uboot/
sudo cp ../linux/arch/arm/boot/uImage /mnt/uboot/
sudo umount /mnt/rootfs/
sudo umount /mnt/uboot/
sudo losetup -d $LOOPDEV

```

运行

```s

sudo chmod -R 755 makefs-arm32-emmc.sh
sudo ./makefs-arm32-emmc.sh

```

### u-boot

#### u-boot 源码下载

`git clone https://source.denx.de/u-boot/u-boot.git --branch v2023.10`

#### u-boot 编译

```t

make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- vexpress_ca9x4_defconfig O=../object/u-boot-arm

make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- menuconfig O=../object/u-boot-arm
environment →mmc device 选中取消 flash
environment offset =0X01fc0000

make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- -j3 O=../object/u-boot-arm

```

之后 object 中会出现新生成的 rootfs-arm.ext3

#### u-boot 指令运行

`sudo ../qemu/build/qemu-system-arm -M vexpress-a9  -m 512M -kernel u-boot-arm/u-boot -nographic -sd rootfs-arm.ext3`

第一次启动会找不到 kernel,依次执行下班操作

```t

setenv bootargs 'root=/dev/mmcblk0p2 rw console=ttyAMA0,38400n8'

setenv bootcmd "load mmc 0:1 0x60003000 uImage;load mmc 0:1 0x65000000 vexpress-v2p-ca9.dtb;bootm 0x60003000 - 0x65000000;"

saveenv

reset

```

## 配置 SSH

参考流程
`https://blog.csdn.net/qq_23685163/article/details/136587203`

生成的 ssh 压缩包已经放到开发板根节点中

```t

tar -vxf usr_zlib.tar.bz2
tar -vxf usr_openssl.tar.bz2
tar -vxf usr.tar.bz2

```

### win 中网卡配置

参考流程,配置网桥 tap0

```t

https://blog.csdn.net/jiangwei0512/article/details/122791901
https://blog.csdn.net/u010834463/article/details/132756450

```

### ssh 配置用户密码

配置 passwd root 使用时
root 密码 root

## 主目录结构 work

```s
├──busybox-1.36.1   ->busybox源码
├──linux-4.14.7   ->linux源码

├──object
│   ├── rootfs-arm32->开发板根文件系统
│   ├── u-boot-arm  -> u-boot 用于uboot启动
│   ├── boot.sh ->启动脚本//图形界面显示
│   ├── rootfs-arm32.ext3  ->根文件系统镜像
│   ├── vexpress-v2p-ca9.dtb ->设备树文件
│   ├── zImage -> 内核镜像

├──objects

├──qemu ->qemu源码 -其中的qemu->build->qemu-system-arm 可以直接使用启动脚本使用

├──ub
│   ├── makefs-arm-32-emmc.sh -> 制作根文件系统镜像
│   ├── rootfs-arm.ext3 ->uboot启动的根文件系统镜像
│   ├── uboot.sh ->启动脚本
│   ├── uImage ->uboot镜像

├──u-boot ->uboot源码

```

## 重新生成磁盘文件

### 非 u-boot 启动时

当 Linux 中需要重新生成挂载文件流程

```s
这是再Ubuntu中的
sudo mkdir /mnt/rootfs
chmod 777 /mnt/rootfs
```

```s
sd卡的方式进行挂载
rootfs-arm32同级目录下
dd if=/dev/zero of=rootfs-arm32.ext3 bs=1M count=512
mkfs.ext3 rootfs-arm32.ext3
sudo mount -t ext3 rootfs-arm32.ext3 /mnt/rootfs -o loop
sudo cp -rf rootfs-arm32/* /mnt/rootfs/  把内容复制到卡里
```

生成的根文件系统

### uboot 启动时

`sudo mkdir  -p /mnt/uboot`

`vi makefs-arm32-emmc.sh`

```t
dd if=/dev/zero of=rootfs-arm.ext3 bs=1M count=256
echo "hard disk partition!"
sgdisk -n 0:0:+32M -c 0:uboot rootfs-arm.ext3
sgdisk -n 0:0:0 -c 0:rootfs rootfs-arm.ext3
sgdisk -p rootfs-arm.ext3

echo "mount loop device!"
LOOPDEV=`losetup -f`

echo $LOOPDEV
sudo losetup $LOOPDEV rootfs-arm.ext3
sudo partprobe $LOOPDEV
sudo losetup -l
ls -l /dev/loop*

echo "format disk to ext3"
echo ${LOOPDEV}P1
echo ${LOOPDEV}p2
sudo mkfs.ext3 ${LOOPDEV}p1
sudo mkfs.ext3 ${LOOPDEV}p2
sudo mount -t ext3 ${LOOPDEV}p1 /mnt/uboot -o loop
sudo mount -t ext3 ${LOOPDEV}p2 /mnt/rootfs -o loop
sudo cp -rf rootfs-arm32/* /mnt/rootfs/
sudo cp ../linux/arch/arm/boot/dts/arm/vexpress-v2p-ca9.dtb /mnt/uboot/
sudo cp ../linux/arch/arm/boot/uImage /mnt/uboot/
sudo umount /mnt/rootfs/
sudo umount /mnt/uboot/
sudo losetup -d $LOOPDEV
```

运行

```t
sudo chmod -R 755 makefs-arm32-emmc.sh
sudo ./makefs-arm32-emmc.sh
```

## framebuffer

里面是测试程序。 line.c 在页面中划线

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/ioctl.h>
#include <fcntl.h>
#include <linux/fb.h>
#include <sys/mman.h>
void draw_point(char *buf ,struct fb_var_screeninfo vinfo,struct fb_fix_screeninfo finfo,int x,int y,int r,int g,int b)
{
	if(x<0||y<0||x>vinfo.xres-1||y>vinfo.yres-1){
		return ;
	}
	int color=r<<8|g<<4|b;

	int location =(x+vinfo.xoffset)*(vinfo.bits_per_pixel/8) +(y+vinfo.yoffset)*finfo.line_length;
	*((int*)(buf+location))=color;

}

void draw_line(char *buf ,struct fb_var_screeninfo vinfo,struct fb_fix_screeninfo finfo,
int start_x,int start_y,
int end_x,int end_y,
int r,int g,int b)
{
	if(start_x==end_x)
	{
		int x=start_x;
		int y=0;
		if(start_y<end_y)
		{
			for(y=start_y;y<end_y;y++)
			{
				draw_point(buf,vinfo,finfo,x,y,r,g,b);
			}
		}
		else
		{
			for(y=end_y;y<start_y;y++)
			{
				draw_point(buf,vinfo,finfo,x,y,r,g,b);
			}
		}

	}

}
int main(void)
{
	int fbfb=-1;

	fbfb=open("/dev/fb0",O_RDWR);

	if(fbfb<0)
	{
		printf ("open /dev/fb0 fails \n");
		return -1;
	}

	int res=-1;
	struct fb_var_screeninfo vinfo;
	res=ioctl(fbfb,FBIOGET_VSCREENINFO,&vinfo);
	if(res <0)
	{
		printf("iovtl get var info fails \n");
		return -1;
	}
	printf("xoffset %d . yoffset %d \n",vinfo.xoffset,vinfo.yoffset);

	struct fb_fix_screeninfo finfo;
	res=ioctl(fbfb,FBIOGET_FSCREENINFO,&finfo);
	if(res<0)
	{
		printf("iovtl get fix info fails \n");
		return -1;
	}
	int length=finfo.line_length;
	printf(" finfo.length %d \n",length);

	int vfbsize =vinfo.xres_virtual *vinfo.yres_virtual*vinfo.bits_per_pixel/8;

	int screensize =vfbsize;

	char *buf =(char *)mmap(NULL,screensize,PROT_READ|PROT_WRITE,MAP_SHARED,fbfb,0);



	draw_line(buf,vinfo,finfo,100,100,100,300,255,0,0);
	if(buf!=NULL && buf!=MAP_FAILED)
	{
		munmap(buf,screensize);
	}
	printf("real fenbiablv ,X: %d , Y: %d  BBP:%d  \n",vinfo.xres,vinfo.yres,vinfo.bits_per_pixel);
	printf("vir fenbianlv ,X:%d , y: %d \n",vinfo.xres_virtual,vinfo.yres_virtual);
	int fbsize =vinfo.xres * vinfo.yres * vinfo.bits_per_pixel/8;
	printf("real byte %d\n",fbsize);
	printf("vir byte %d\n",vfbsize);
	close(fbfb);
	return 0;

}

```

arm-linux-gnueabihf和arm-linux-gnueabi是交叉编译工具链的名称，用于在主机系统上编译针对ARM架构的目标代码。

区别如下：

ABI（Application Binary Interface）：区别之一是它们使用的ABI。arm-linux-gnueabihf使用硬浮点ABI（Hard Float ABI），而arm-linux-gnueabi使用软浮点ABI（Soft Float ABI）。

硬浮点ABI（gnueabihf）：使用硬件浮点寄存器（FPU）来进行浮点计算，提供更高的性能。
软浮点ABI（gnueabi）：将浮点计算委托给软件库来处理，适用于没有硬件浮点支持的处理器。
编译器选项：arm-linux-gnueabihf和arm-linux-gnueabi工具链使用不同的编译器选项和默认设置，以适应相应的ABI。

一般来说，如果您的目标设备支持硬件浮点运算（具有FPU），则建议使用arm-linux-gnueabihf工具链。这样可以利用硬件浮点支持，获得更好的性能。如果您的目标设备不支持硬件浮点运算，或者您希望与使用软浮点ABI的其他组件兼容，那么可以使用arm-linux-gnueabi工具链。

vexpressA9是一款基于ARM Cortex-A9处理器的开发板。它由ARM公司提供，用于开发和评估ARM Cortex-A9处理器及其相关技术。

ARM Cortex-A9是一款高性能的32位多核处理器设计，属于ARM的Cortex-A系列处理器之一。它采用了ARM架构的ARMv7指令集，并具有高度集成的特性，适用于广泛的应用领域，包括移动设备、嵌入式系统、网络设备和消费类电子产品等。

区别：

目标用途：vexpressA9是一款开发板，旨在为开发人员提供一个用于软件开发、系统调试和性能评估的平台。而ARM Cortex-A9是一款处理器设计，用于嵌入式系统和移动设备等领域的产品中。

硬件实现：vexpressA9是一个具体的硬件开发板，包含了ARM Cortex-A9处理器和其他外围设备，如内存、接口等。ARM Cortex-A9则是一个处理器核心的设计，可以被实现在不同的硬件平台上。

可定制性：vexpressA9开发板的设计通常提供了丰富的可定制性，包括额外的接口、扩展插槽和其他功能，以适应不同的开发需求。而ARM Cortex-A9的设计本身提供了一组基本的处理器功能，但具体的外围设备和接口可以根据具体系统的需求进行定制。

总之，vexpressA9是一款基于ARM Cortex-A9处理器的开发板，用于软件开发和系统调试；而ARM Cortex-A9是一款处理器核心的设计，可实现在各种嵌入式和移动设备中。
