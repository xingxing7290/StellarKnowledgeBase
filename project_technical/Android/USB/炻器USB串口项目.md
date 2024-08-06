# 炻器USB串口项目

项目负责人：周金鑫 

项目要求：

1、在安卓端实现TCP中继服务，建立USB与炻器物联APP之间的通讯，并模拟为原中继服务器功

2、将附件的中继服务器的代码删除UI、身份认证、http部分的功能 ，移植为安卓的so库。

3、在Linux设备端建立一个TCP转USB的服务，只做转发功能。

4、将设备端的连接中继的IP改为 127.0.0.1 。 

完成时间: 10月中旬前完成。

 关于项目奖金： 没有制定项目奖金的项目应在项目完成后，根据工作情况和参与人员等申请项目奖金。

TCP端口号 16001

`tar -zxvf boost_1_66_0.tar.gz`

Android使用的USB模块：

一种常用的库是 Android 提供的 android.hardware.usb 包，它包含了用于与 USB 设备进行通信的类和接口。通过使用这个包，你可以在你的应用程序中实现 USB 连接、数据传输和设备控制等功能。

除了 Android 提供的原生库外，还有一些第三方库可用于简化 USB 操作的开发。例如，usb-serial-for-android 是一个常用的库，用于在 Android 设备上与串行 USB 设备进行通信。其他类似的库还包括 usb4java、libusb 等。

# 交叉编译工具链

arm-openwrt-linux-gnueabi-gcc

- 交叉编译工具链的命名规范是：

**arch [-vendor] [-os] [-(gnu)eabi]**

**arch : 架构的意思，如ARM ，MIPS**

**vendor： 工具链的提供厂商**

**os： 支持的操作系统**

**eabi：嵌入式应用二进制接口（Embedded Application Binary Interface）**

- arm gcc还分为 是否支持操作系统

**支持操作系统：** arm-none-linux-eabi-gcc

**不支持操作系统：**arm-none-eabi-gcc

`./configure CROSS_COMPILE=arm-linux CC=arm-linux-gcc --host=arm-linux --prefix=/home/linux20/Desktop/arm/libxml2 --with-python=/home/linux20/Desktop/libxml2-2.9.4/python --with-zlib=/home/linux20/Desktop/wcf/arm/curl/output		//配置`

`export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/home/linux20/toolchain/bin`

```c
export STAGING_DIR=/home/linux20/toolchain/arm-openwrt-linux/bin/

export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/home/linux20/toolchain/bin

arm-openwrt-linux-gnueabi-gcc -o usbandBT usbandBT.c -I/home/linux20/bluez/bluez-5.66/lib \
-L/home/linux20/bluez/bluez-5.66/lib/.libs  -lbluetooth -Wl,-rpath=/home/linux20/bluez/readline-6.3/build/lib/ \
-Wl,-rpath=/home/linux20/bluez/ncurses-5.8/lib/ \
-Wl,-rpath=/home/linux20/bluez/libffi-3.3/build/lib/ \
-Wl,-rpath=/home/linux20/bluez/eudev-3.2.9/build/lib/ \
-Wl,-rpath=/home/linux20/bluez/libical-2.0.0/lib/ -lpthread  

```

linux20@ubuntu:~/bluez/Relay$ arm-openwrt-linux-gnueabi-gcc -o usbandBT usbandBT.c -I/home/linux20/bluez/bluez-5.66/lib \

> -L/home/linux20/bluez/bluez-5.66/lib/.libs  -lbluetooth -Wl,-rpath=/home/linux20/bluez/readline-6.3/build/lib/ \
-Wl,-rpath=/home/linux20/bluez/ncurses-5.8/lib/ \
-Wl,-rpath=/home/linux20/bluez/libffi-3.3/build/lib/ \
-Wl,-rpath=/home/linux20/bluez/eudev-3.2.9/build/lib/ \
-Wl,-rpath=/home/linux20/bluez/libical-2.0.0/lib/ -lpthread
> 

# Linux编译设置环境变量

export STAGING_DIR=/home/linux20/toolchain/arm-openwrt-linux/bin/

export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/home/linux20/toolchain/bin

位置： 

linux20@ubuntu:~/toolchain/bin$ arm-openwrt-linux-gnueabi-gcc bing.c -o bing

```jsx
arm-openwrt-linux-gnueabi-gcc bing.c -o bing -lpthread
```

# SFTP

传输文件

使用软件：**MobaXterm**

[MobaXterm](https://mobaxterm.mobatek.net/download-home-edition.html)

连接SFTP

连接ip地址：192.168.10.230  ifconfig查看

![Untitled](炻器USB串口项目 /Untitled.png)

在将server复制到linux开发板后

先给程序权限，之后再去运行

chmod 777 server 

./server

手动切换USB0 为host 模式：
cat /sys/devices/platform/soc@3000000/soc@3000000:usbc0@0/usb_host

# 设置 从机模式

 

vi /lib/functions/set/set.sh

set.sh中修改：
insmod /run/usb_f_acm.ko
insmod /run/g_serial.ko
cat /sys/devices/platform/soc@3000000/soc@3000000:usbc0@0/usb_device

[Untitled](炻器USB串口项目 /Untitled.txt)

[Untitled](炻器USB串口项目 /Untitled%201.txt)

两文件让run下面

```jsx
cat /sys/devices/platform/soc@3000000/soc@3000000:usbc0@0/usb_device
```

/lib/functions/set/set.sh

set.sh中修改：
insmod /run/usb_f_acm.ko
insmod /run/g_serial.ko
cat /sys/devices/platform/soc@3000000/soc@3000000:usbc0@0/usb_device //问题是第一次的时候

# 中继程序流程

1. 初始化usb 和server
2. while里面循环
    
    usb :收 read  发 write
    
    server : 收 recv  发 send 
    

循环流程

当以USB 收，当收到时

![Untitled](炻器USB串口项目 /Untitled%201.png)

查看进程

ps 

# 将附件的中继服务器的代码删除UI、身份认证、http部分的功能 ，移植为安卓的so库。

skSvrMain.c

/*使用说明
1. 主服务器监听端口 16000~16999
2. 连接服务器后:
a. 控制端和设备端登录(svr_login, params:{type:[0|1], id:, pass:}
b. 启动连接到中继(svr_relconnect, params: devid:)
如果设备在线:
*服务器查询数据库, 选择在线的中继服务器, 生成MD5的key值
*服务器向设备发送: rmc_connect, params: ip, port, cid, key(did+cid+pass)
*服务器向控制端返回 result: ip, port, key:cid+did+pass
失败返回:
超过服务期, 没有权限, SK_RET_NO_REC, 对端没有在线, SK_RET_NO_RES
c. 控制端和设备端登录到中继服务器, rel_login, params: myid, toid, key
失败返回: SK_RET_NO_PASS, SK_RET_NO_RES
d. 控制端向设备发送: rmc_start, params: lcdshow, mode
*/

SVR_PORT:主服务端口

REL_PORT：中继服务端口

skSvrRelay.c中继服务器

/* 使用说明
1. 中继服务器监听端口 17000~17999,  密码每24小时重置一次(随机生成)
2. 定时(10秒)向业务(主)服务器更新状态, 掉线时立即通知.
3. 控制端和设备端登录中继服务器, rel_login, params: myid, toid, key
*/

# rc文件

rc 是**resource**的简称，对应着VS中的资源视图文件。rc 文件是资源文件，包括比如对话框、菜单、图标、字符串、**版本**等资源信息。使用.rc 资源文件的目的是为了对程序中用到的大量的资源进行统一的管理。在win32编程中，为方便项目中的资源统一管理，提供了一个格式统一的资源文件，对各种资源进行管理。**它的扩展名是.rc，在程序编译时它会被资源编译器编译生成一个.res的二进制文件**

# C语言标准 std

[在C语言中，**`-std`**选项用于指定要使用的C语言标准。以下是一些有效的值1](https://study.com/academy/lesson/validating-input-data-in-c-programming.html)：

- **`std=c90`** 或 **`std=iso9899:1990`**：支持ISO C90标准（也称为ANSI C）。
- **`std=c99`** 或 **`std=iso9899:1999`**：支持ISO C99标准。
- **`std=c11`** 或 **`std=iso9899:2011`**：支持ISO C11标准。

lib文件是静态链接库,*dll文件是动态链接库。*

# 引用库文件问题

在AS中引用.a静态库文件

1. **`libskos.a`**库文件并不是为**`aarch64linux`**架构编译的。

您可以在Android的NDK中使用一个Linux v7的.a库文件，但需要确保以下几点：

ABI匹配：您提到您的NDK设置包括'armeabi-v7a' ABI，这是对应于32位ARM架构的ABI。如果您的.a库是为32位ARM架构编译的（Linux v7通常是32位），那么可以在'armeabi-v7a' ABI上使用它。

C/C++: ld: error: relocation R_AARCH64_ADR_PREL_PG_HI21 cannot be used against symbol _cfg; recompile with -fPIC

这个错误是因为链接的.a文件在编译时没有加上-fPIC参数1。你可以尝试在编译时加上-fPIC参数，这样在链接时就不会有问题1。如果你正在尝试交叉编译一个带有预编译的.a或.so.x文件的库，gcc可能会尝试使用库的静态版本，因为它可以找到.so文件2。你可以尝试使用ln -s libX.so.1.2.3 libX.so来解决这个问题2

[C++编译问题，解决arm下链接静态库，引起的relocation R_AARCH64_ADR_PREL_PG_HI21 against symbol错误 - umichan - 博客园](https://www.cnblogs.com/umichan0621/p/17037032.html)

# UI界面

![Untitled](炻器USB串口项目 /Untitled%202.png)

为处理bool值问题

注销了<stdbool.h>中的内容

在C++中使用C语言的函数

 在.c中定义int  void()

在.h文件中声明

```jsx
#ifdef __cplusplus
extern "C" {
#endif

int fun();

#ifdef __cplusplus
}
#endif
```

然后就在cpp中，#include ”main.h“ 然后直接使用fun函数

# 验证部分

在登录验证过程，因为中继服务器中的设备端（SD72）的和控制端(Android)都直接链接的Android上的。所以在这个过程中，sd72登陆时的验证（svr_login）返回的都是ture 就能免去验证了

、、//接收到了socket字符串，保存在里item->jstr中，然后使用USB发送；

# USB→Socket

## 具体流程

中继服务器接收

svr_main”()

→ svr_login →_jr_recv_json()→_jr_recv_pack()

```jsx
bool _jr_recv_json(int fd, char *jstr, int jsize)
{
    sk_head pack;
	char *buff;
    buff = malloc(SK_PACK_JS_MAX);
    if(_jr_recv_pack(fd, &pack, buff) == RECV_RET_OK) {
		if(pack.flag != SK_PACK_FLAG)
			return false;
        if(pack.format == SK_FORMAT_JSON) {
			buff[pack.size] = 0;
			if(jsize > pack.size)
				memcpy(jstr, buff, pack.size + 1);
			free(buff);
			return true;
        }else
			return false;
    }
	free(buff);
	return false;
}

em_recv_ret _jr_recv_pack(int fd, sk_head *pack, void *data)
{
    int ret;
    ret = sockapi->recv(fd, (void*)pack, SK_PACK_HEAD);
    if(ret <= 0) {
		perror("jr_recv_pack:");
        return RECV_RET_CLOSE;
	}
    if(pack->flag != SK_PACK_FLAG)
        return RECV_RET_CLOSE;
	else if(pack->format == 1 && pack->size > SK_PACK_BIN_MAX)
		return RECV_RET_CLOSE;
    else if(pack->format == 2 && pack->size > SK_PACK_JS_MAX)
        return RECV_RET_CLOSE;
    else if(ret && pack->size > 0) {
        ret = sockapi->recv(fd, data, pack->size);
        if(ret < 0) {
			perror("jr_recv_pack:");
            return ret;
		}
        else
            return RECV_RET_OK;
    }
	return ret;
}

```

想到了一个问题：

中继服务器：使用SD72时使用的USB 去链接的 USB接收就是Socket 接收

但同时链接Android 炻器物联时，使用的就时scoket 接收

在接收的过程中，要单独写一个线程，一直去while循环

然后一直去监听USB 发送过来的数据串。

当收之后，直接就按照process 的过程去处理这个串

但不用了login的登录过程

# addr2line 地址定位

[Android NDK开发崩溃 signal 11 (SIGSEGV), code 1 (SEGV_MAPERR),解决方法 - 普通R - 博客园](https://www.cnblogs.com/rocket-ban/p/14717425.html)

指令

```jsx
addr2line -i -C -f -e librelayserver.so 0xab64
```

6c2f0

![Untitled](炻器USB串口项目 /Untitled%203.png)

![Untitled](炻器USB串口项目 /Untitled%204.png)

# 程序占用问题

ib\arm64-v8a: 另一个程序正在使用此文件，进程无法访问。

这是命令提示符正在打开进行地址分析，所以在占用这个文件，正在使用

# 更换SD72中的APP

位置在 usr/bin/APP

更换后reboot

然后给APP权限：chmod usr/bin/APP/APP

然后重启一下就行了

```
//加载库
static {
    System.loadLibrary("relayserver");
}
public native String processString(String input);
public native String SockToUsbString();
public native void UsbToSockString(String str);
```

eg[{"method":"svr_login","params":{"type":1,"id":"","pass":"1936C918C19507C35ED0B4ACEF4B2A90"},"id":"1"}]

I/native-activity: _jr_recv_pack:��e
I/native-activity: _jr_recv_pack2 :[{"method":"svr_relconnect","params":{"devid":"SSG-SD628-00050"},"id":1}]678"},"id":1}]

![Untitled](炻器USB串口项目 /Untitled%205.png)

`data=UsbToSockStr+2;`

I/native-activity: _jr_recv_pack2 :ams[{"method":"svr_login","params":{"type":1,"id":"","pass":"1936C918C19507C35ED0B4ACEF4B2A90"},"id":"1"}]

eg

egams

炻器物联连接过程

![Untitled](炻器USB串口项目 /Untitled%206.png)

![Untitled](炻器USB串口项目 /Untitled%207.png)

灵感

别的东西都不变，然后连接了就新建一个item ，假设socket==1;

再发送的时候都去判断一下，如果socket=1的话就是USB 发送，其他的话就是正常的TCPsend

但是可能会断开等影响。

..\..\ssgosLib\bin\${configName}

# USB write

READ/WRITE的传输长度是指逻辑块的数量，而**一个逻辑块是512个字节**；CBW的传输数据指的就是字节数了。

也就是说名，socket 接收的时候大小是接收缓存的大小。2048

但是usb write发送的字节大小确实512 一个块

这就造成了接收的数据被划分开了  

也就是每接收一个socket就会分为四个块通过usb发送出去。

## 第一方面：ttyGS0的传输速率

usb虚拟成串口的这是

[测试USB虚拟串口(CDC)的极限速度-物联沃-IOTWORD物联网](https://www.iotword.com/8137.html)

![Untitled](炻器USB串口项目 /Untitled%208.png)

![Untitled](炻器USB串口项目 /Untitled%209.png)

## 第二方面：wirte写的块大小最大为512

当发送rmcf_downstart指令时，是否可以做一个判断

当是脱机模式时，就让他能够去后台服务器去找app.ski

在Linux系统中，你可以使用`stty`命令来查看或设置串口的波特率¹²³⁵。以下是一些示例：

查看串口参数：

```bash
stty -F /dev/ttySn -a  # ttySn为要查看的串口
```

设置串口参数：

```bash
stty -F /dev/ttyS0 ispeed 115200 ospeed 115200 cs8  # 将串口1（/dev/ttyS0）设置成115200波特率，8位数据模式。
```

这些命令将显示或设置指定串口的波特率和其他参数。请注意，你需要根据你的系统和设备来替换`/dev/ttySn`¹²³⁵。希望这个答案对你有所帮助！

源: 与必应的对话， 2023/10/19
(1) 【精选】linux下查看与设置串口（或串口终端）信息和属性_cat命令查看linux串口接收数据_艾特号的博客-CSDN博客. [https://blog.csdn.net/lpwsw/article/details/120153643](https://blog.csdn.net/lpwsw/article/details/120153643).
(2) linux查看串口波特率_StarLish0715的博客-CSDN博客. [https://blog.csdn.net/baidu_37503452/article/details/120264269](https://blog.csdn.net/baidu_37503452/article/details/120264269).
(3) 查看串口波特率等信息_ubuntu查看串口波特率_杭城江城子的博客-CSDN博客. [https://blog.csdn.net/caoshunxin01/article/details/121282142](https://blog.csdn.net/caoshunxin01/article/details/121282142).
(4) Linux开发板下检查及配置串口（波特率/数据发送接收等）的操作_linux查看串口波特率-CSDN博客. [https://blog.csdn.net/heda3/article/details/132574911](https://blog.csdn.net/heda3/article/details/132574911).
(5) 串口如何确定波特率-百度经验. [https://jingyan.baidu.com/article/4f34706e8ae829a287b56d9b.html](https://jingyan.baidu.com/article/4f34706e8ae829a287b56d9b.html).

或者设置串口参数1：

stty -F /dev/ttyS0 ispeed 115200 ospeed 115200 cs8 -parenb -cstopb -echo

这个命令将串口1（/dev/ttyS0）输入输出都设置成115200波特率，8位数据模式，无奇偶校验位-parenb，一个停止位-cstopb, 同时禁止终端回显-echo1。

使用的usb连接，但是实际内部使进行的cdc协议：

# 发送大文件解决办法

在Linux中转程序端：

要判断这个包是不是完整的包 （根据结构体）如果使完整的包，就在最后加标志位，

如果不是完整的包，就需要将包放入到一个缓冲区中，等到使完整包了 就去加标志位

Android中继服务器端：

创建一个缓冲区，如果是同样去末尾不是***zjx的话，就说明不是一个完整包；放到缓冲区中去；

但是在这里Java中没办法根据大小去判断；

# 628发送包的格式

分块大小32*1024=32768 的数据；

实际发送的大小为32816 

其中差额为48 其中结构体sk_head 占16个字节， _fhead占16个字节；还有16个字节的位置

文件APP.ski大小为18917KB

down block 590!
send socket 32816
down block 591!
send socket 5832

实际发送了591个包

18917/32=591.15625=592包

在中转程序中，socket收到的都是32816 字节  与发送值相同，

但是，write 写给usb的值为什么都是17920 

测试将一个完整数据分段发送，但还是发送17500之后的下一个包就发送失败了

就算能够完整的进行操作，的但还是无法完整的发送一包数据；

# 解决了上面的问题

发送数据包大小为16*1024 

socket发送的时间间隔为usleep(30 000)//30毫秒

这样就能避免write  17590字节的缺点

但是同事问题产生，Android接收到了数据，但是并没有抓发出去。

# 每一个功能操作都是创建一个17001的连接

发送时16432

其中peck→size =16416

---

两者相差32 但_fhead的结构体大小也为16啊应该

---

_fhead→size =16384

文件块的大小为16384；

推测

是不是在发送文件的时候，还要发送其他信息，为什么就超时中断了呢

# 操作重连

在进行操作时：每进行一个操作，炻器物联就会让628去重新连接17001的中继服务器

这个应该如何解决；

在原系统的中继服务器中

通过16001 发送rmc_connect 去连接17001,

17001 接到了 然后去连接；

验证一下：炻器物联连接中继服务器17001 是一个操作连接一次嘛？

验证结果是：是的，每次连接都是去重新连接一次17001

 

还有一个超时问题

就是：完成rmc_connect的连接问日

待解决问题

明天更改云上服务器

直接更改 不重新下载SKI 进行调试

然后云上重新下载SKI，看显示界面不？然后再去手机控制-》》》》重新下载了页面

判断是不是传输丢数据，导致的界面黑屏

然后触屏问题的关键是;

发送指令的时候。标志位别分割问题

然后就是发送的指令都不是发送指令大小问题

# 升级包

名为:APP_DIRECT_UP

需要更换其中的APP中的APP和APP.ski

其中APP位于项目文件的SKc中的bin 的编译的编译版本的APP

[然后就是SKG中的output中的app.ski](http://然后就是SKG中的output中的app.ski) SD50X.ski改名为APP.ski

### 最后错误：

目录层级不对，是将APP和iot封装到APP_DIRECT_UP压缩包中，并不是：APP_DIRECT_UP.zip→APP_DIRECT_UP→APP/iot

# 程序始终去连接cdc设备，且断开自动重新连接

但是

问题是，要断开之后，全部的连接必须全部失去；

也就是，断开之后，要结束中继服务器的线程；

密码zxcvbnm123

![Untitled](炻器USB串口项目 /Untitled%2010.png)

关于每次插上usb 16001

考虑使用连接进入之后首先给发送一个指令，让断开，然后再去连接这个16001

但是，可能出现的情况是。再已经连接的时候可能出现很快的一份断开和连接

```jsx
I/native-activity: send socket to  17001
I/BlockMonitor: dispatchingThrewException In MainThread
D/AndroidRuntime: Shutting down VM
I/QarthLog: [PatchStore] createDisableExceptionQarthFile
I/QarthLog: [PatchStore] create disable file for com.hoho.android.usbserial.examples uid is 10282
    
    --------- beginning of crash
E/AndroidRuntime: FATAL EXCEPTION: main
    Process: com.hoho.android.usbserial.examples, PID: 12961
    java.lang.IndexOutOfBoundsException: offset(52) should be less than line limit(0)
        at android.text.TextLine.measure(TextLine.java:356)
        at android.text.Layout.getHorizontal(Layout.java:1213)
        at android.text.Layout.getHorizontal(Layout.java:1190)
        at android.text.Layout.getPrimaryHorizontal(Layout.java:1160)
        at android.widget.TextView.bringPointIntoView(TextView.java:10084)
        at android.widget.TextView.onPreDraw(TextView.java:7703)
        at android.view.ViewTreeObserver.dispatchOnPreDraw(ViewTreeObserver.java:1098)
        at android.view.ViewRootImpl.performTraversals(ViewRootImpl.java:3444)
        at android.view.ViewRootImpl.doTraversal(ViewRootImpl.java:2235)
        at android.view.ViewRootImpl$TraversalRunnable.run(ViewRootImpl.java:9043)
        at android.view.Choreographer$CallbackRecord.run(Choreographer.java:1280)
        at android.view.Choreographer.doCallbacks(Choreographer.java:1019)
        at android.view.Choreographer.doFrame(Choreographer.java:911)
        at android.view.Choreographer$FrameDisplayEventReceiver.run(Choreographer.java:1248)
        at android.os.Handler.handleCallback(Handler.java:900)
        at android.os.Handler.dispatchMessage(Handler.java:103)
        at android.os.Looper.loop(Looper.java:219)
        at android.app.ActivityThread.main(ActivityThread.java:8673)
        at java.lang.reflect.Method.invoke(Native Method)
        at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:513)
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1109)
I/Process: Sending signal. PID: 12961 SIG: 9
```

![Untitled](炻器USB串口项目 /Untitled%2011.png)

# rmc out max limit 10

超过最大限制 10个

![Untitled](炻器USB串口项目 /Untitled%2012.png)

# 设置中转程序自启

vi /lib/functions/set/set.sh

其中添加

/test/file &

末尾带一个& 表示的是后台运行

# 测试;连接的时候

心跳包改为3秒

测试，在下载文件的时候，回和心跳包冲突吗？

```jsx
private void receive(byte[] data) {
        Log.i(TAG, "receicereceive: " + data.length);
        int logo = 0;
        byte[] newData;
        count++;

        if (fileflag == 0) {//如果不是文件传输
//只有在不是文件的传输的时候才判断
            ByteBuffer buffer = ByteBuffer.wrap(data);
            buffer.order(ByteOrder.LITTLE_ENDIAN);//小端序
            SKHead head = new SKHead(buffer);
            int size = head.getSize(); //根据街头提判断出size的大小；
            Log.i(TAG, "receiveskheadsize:  " + size);
            allsize= size+16+4;//加标志位

            if(allsize== data.length) {//是一条指令
                if (data[data.length - 1] == '6' && data[data.length - 2] == 'g' && data[data.length - 3] == 's' && data[data.length - 4] == 's') {
                    logo = 16001;
                    newData = new byte[data.length - 4];
                    System.arraycopy(data, 0, newData, 0, data.length - 4);
                    Log.i(TAG, "logreceive6666: " + HexDump.dumpHexString(newData));
                    SpannableStringBuilder spn = new SpannableStringBuilder();
                    receiveText.append(spn);
                    UsbToSockString(newData, HexDump.dumpHexString(newData), logo);
                    return;
                } else if (data[data.length - 1] == '7' && data[data.length - 2] == 'g' && data[data.length - 3] == 's' && data[data.length - 4] == 's') {
                    logo = 17001;
                    newData = new byte[data.length - 4];
                    System.arraycopy(data, 0, newData, 0, data.length - 4);
                    Log.i(TAG, "logreceive777: " + HexDump.dumpHexString(newData));
                    SpannableStringBuilder spn = new SpannableStringBuilder();
                    UsbToSockString(newData, HexDump.dumpHexString(newData), logo);
                    Arrays.fill(filespace, (byte) 0);//清空数据内容
                    return;
                }
            }
            if(allsize > data.length)//大于就说明是一条数据；
            {
                fileflag = 1;//是文件传输
                countfile++;
            }

            
        }

        
        if (fileflag == 1) {
            try {
                //lock.lock();
                if (ret+ data.length> filespace.length) {
                
                }else 
                    System.arraycopy(data, 0, filespace, ret, data.length);
                ret += data.length;
                if (filespace[ret - 1] == '7' && filespace[ret - 2] == 'g' && filespace[ret - 3] == 's' && filespace[ret - 4] == 's') { //最后一段
                    byte[] filespace2 = new byte[ret - 4];
                    System.arraycopy(filespace, 0, filespace2, 0, ret - 4);
                    logo = 17001;
                    UsbToSockString(filespace2, HexDump.dumpHexString(filespace2), logo);
                    Log.i(TAG, "file" + countfile + " receive" + ret + " bytes\n");
                    fileflag = 0;
                    ret = 0;
                   // lock.unlock();
                    return;
                }
                //lock.unlock();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }这段代码我想更改一下实现的效果是。获取到size是有效数据的大小，
allsize是整条指令的大包括16位字节和4位标志位，我想是实现的效果是：
接收数据，如果数据是一条数据，就直接判断标志位使用。如果allsize的大小大于data.len，
说明这条数据很长，需要循环接收多个回合，直到完整的接收这个指令，
也就是接收数据总量allsize的大小，然后再去使用。但同时会出现这种情况，
就是上一条的结尾，和第二条的开始粘练在了一起，而且这也是一个大包
，也需要接收好几个回合才能接收完整。请帮我更改一下这个程序，来满足我的要求
```

中转程序中，出现segmentation fault 

```jsx
cat /proc/sys/fs/pipe-max-size
```

![Untitled](炻器USB串口项目 /Untitled%2013.png)

**`pipe-max-size`** 参数用于设置管道的最大容量，即管道允许的数据传输大小。通常情况下，它默认为一个比较大的值，允许大量数据在管道中传输，但具体取决于系统配置。你可以查看它的值以了解当前系统的设置

![Untitled](炻器USB串口项目 /Untitled%2014.png)

![Untitled](炻器USB串口项目 /Untitled%2015.png)

![Untitled](炻器USB串口项目 /Untitled%2016.png)

![Untitled](炻器USB串口项目 /Untitled%2017.png)

![Untitled](炻器USB串口项目 /Untitled%2018.png)

![Untitled](炻器USB串口项目 /Untitled%2019.png)

# 开机没有ttyGS0

我将Linux设置为CDC从机模式，开机时，并不会显示ttyGs0,但是再连接手机之后，就会显示出ttyGS0，这是为什么?

[在Linux系统中，`/dev/ttyGS0`是USB Gadget串口设备的设备节点1](https://blog.csdn.net/weixin_42450002/article/details/116949076)[2](https://blog.csdn.net/qqliyunpeng/article/details/105564974)。当你的Linux设备作为USB CDC（Communication Device Class）从机启动时，如果没有USB主机（如手机）连接，那么`/dev/ttyGS0`可能不会立即出现。这是因为在USB CDC模式下，设备节点的创建通常是在USB主机连接并枚举设备时发生的。

[当你连接手机后，手机作为USB主机，会开始枚举已连接的USB设备。在这个过程中，Linux设备会被识别为CDC设备，然后`/dev/ttyGS0`设备节点就会被创建出来1](https://blog.csdn.net/weixin_42450002/article/details/116949076)[2](https://blog.csdn.net/qqliyunpeng/article/details/105564974)。

[这就是为什么在连接手机后，你会看到`/dev/ttyGS0`设备节点。如果你希望在开机时就看到`/dev/ttyGS0`，你可能需要在系统启动时就有一个USB主机连接到你的Linux设备上。1](https://blog.csdn.net/weixin_42450002/article/details/116949076)[2](https://blog.csdn.net/qqliyunpeng/article/details/105564974)。

开机第一次插入时：手机：no driver

linux内并没有dev/ttyGS0

![Untitled](炻器USB串口项目 /Untitled%2020.png)

![Untitled](炻器USB串口项目 /Untitled%2021.png)

第二次插入

手机显示了CDC ACM

![Untitled](炻器USB串口项目 /Untitled%2022.png)

![Untitled](炻器USB串口项目 /Untitled%2023.png)

用Android手机通过USB连接linux usb cdc从机时，为什么第一次连接是显示no driver 但是进行一次插拔之后，就能显示CDCACM并进行连接，这是因为什么

[当你的Android手机首次连接到Linux设备时，可能会显示"no driver"，这是因为系统可能还没有加载适当的USB驱动来识别你的设备1。当你插拔一次后，系统可能已经正确地加载了CDC ACM（Communication Device Class Abstract Control Model）驱动，这个驱动是用于USB通信设备的，所以你的设备就可以被识别并建立连接了1](https://blog.csdn.net/huangbiao86/article/details/7563260)。

[此外，有些Android设备在连接到计算机时会默认为"只充电"模式，这种模式下设备不会与计算机进行数据通信。你需要在设备的USB选项中选择"MTP（媒体设备）"或"PTP（相机）"模式，或者开启"USB调试"选项，这样设备才能与计算机进行数据通信2](https://blog.csdn.net/zhandoushi1982/article/details/7563702)。

[如果你的设备在连接后仍然无法被识别，你可能需要检查你的USB线是否支持数据传输，或者尝试在不同的USB端口上连接你的设备2](https://blog.csdn.net/zhandoushi1982/article/details/7563702)。希望这个信息能帮到你！

吧发送时间间隔调大

加了锁盒判断循环

首先十分感谢你的回答。

同时我还有一个问题需要请教一下：我在Linux设备上设置

11月2日   17:30 ~19:30

40 000 3ci

# Android插上USB发送失败重连问题

sd628设备上中转程序打开失败

打开中转程序失败， 连接不上；

可能是因为上次中转程序直接退出，没有关闭16001 17001 的socket端口，所以就会造成这一直发心跳包，但是没有连接上sd628的问题

# 多指令调试

问题是：

在创立新的连接时，要先退出上一个线程，

首先，建立一个新的线程，然后，设置一个变量，这个变量就是在上个线程中去循环判断的变量，上一个线程判断这个变量，就结束了

先断开在去连接新的：断开那边如何去判断，首次新建的时候

先连接新的在去断开：可能和导致新的和旧的一起断开

会导致的情况：久 的还没结束，新的建立连接的连接语句就来了，直接转发出去了

在中继服务器这边，加了延时，先设置重连，让那边先断开，然后再发送过去

收到旧线程断开信号了之后，再去请求发送

出现了这么一种状况

上一个线程结束了：但是ing没有立刻创建下一个线程，

创建时间晚了所以就导致：已经收回的两条消息并没有获得即使的处理

一条是登录rel_login。另一条是socket转发过去 的

![Untitled](炻器USB串口项目 /Untitled%2024.png)

放到这里

会出现的问题是：创建新线程的进入的时候太晚了。所以会导致接会j接收登录指令太晚

![Untitled](炻器USB串口项目 /Untitled%2025.png)

183

141

118

169

出现状况，就是 同一条登录消息

在处理完灯登录之后，紧接着就有转发出去了

这种问题应该是之前的线程没有结束，所以可能会导致有几个后台线程。

183

`*连续来了两条指令，那么也就只会读取后面的那条指令，前面的指令会被覆盖掉。*`

![Untitled](炻器USB串口项目 /Untitled%2026.png)

200+

![Untitled](炻器USB串口项目 /Untitled%2027.png)

256次

![Untitled](炻器USB串口项目 /Untitled%2028.png)

![Untitled](炻器USB串口项目 /Untitled%2029.png)

![Untitled](炻器USB串口项目 /Untitled%2030.png)

炻器物联的socket 没有转发过来登录信息 rmc_login

# 出现问题

若在传输中途断开了连接：但是在SD72端仍会继续传输

传输失败：原因是有两个线程在传输这个文件

为什么会出现多个线程的同时存在的 问题

# errno 24

![Untitled](炻器USB串口项目 /Untitled%2031.png)

[在C语言中，**`errno`**是一个全局变量，用于存储系统错误号。当**`printf("loginToSvr Error: %d \n", errno);`**打印出的**`errno`**为24时，表示的错误是"Too many open files"（打开的文件过多）1](https://stackoverflow.com/questions/18280612/ioerror-errno-24-too-many-open-files)[2](https://stackoverflow.com/questions/24862733/difference-between-linux-errno-23-and-linux-errno-24)[3](https://community.progress.com/s/article/18180)[4](https://www.ibm.com/docs/en/zos/2.3.0?topic=codes-sockets-return-errnos)。

[这个错误通常发生在你的程序试图打开的文件数量超过了系统允许的最大文件描述符数量。在Linux系统中，你可以使用**`ulimit -n`**命令来查看或修改这个限制1](https://stackoverflow.com/questions/18280612/ioerror-errno-24-too-many-open-files)。如果你的程序需要打开大量的文件，你可能需要增加这个限制。

![Untitled](炻器USB串口项目 /Untitled%2032.png)

# linux 上设置端口

我在一个C语言程序中打开了socket 16001 的server端口并iqe可以正常使用，程序运行是，我使用ctrl+c终止了程序，但之后在运行这个程序的时候，就没办法打开了16001 的端口，请问有什么方法能够解救这个问题

这个问题可能是由于你的程序在被终止时没有正确地关闭套接字导致的。当你使用`Ctrl+C`终止程序时，操作系统会向你的程序发送一个`SIGINT`信号。如果你的程序没有处理这个信号，它就会立即退出，而不会关闭它打开的套接字。

当一个套接字被关闭时，操作系统会保留它的端口一段时间，以确保所有的数据都被正确地发送。这个状态被称为`TIME_WAIT`。在这个状态下，你不能再次使用这个端口。这就是为什么你在重新运行你的程序时不能打开16001端口的原因。

要解决这个问题，你可以在你的程序中添加一个信号处理函数，用来在程序被终止时关闭套接字。以下是一个示例：

```c
#include <signal.h>#include <unistd.h>int sockfd;  // 套接字描述符

void sigint_handler(int sig) {
    close(sockfd);  // 关闭套接字
    exit(0);  // 退出程序
}

int main() {
    signal(SIGINT, sigint_handler);  // 设置SIGINT信号的处理函数

    // 创建套接字
    sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd == -1) {
        perror("socket");
        return 1;
    }

    // 绑定套接字到端口
    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(16001);
    addr.sin_addr.s_addr = INADDR_ANY;
    if (bind(sockfd, (struct sockaddr*)&addr, sizeof(addr)) == -1) {
        perror("bind");
        return 1;
    }

    // 其他代码...

    return 0;
}

```

这段代码会在程序被`Ctrl+C`终止时关闭套接字，这样你就可以在程序重新运行时再次使用16001端口了。

另外，你也可以在创建套接字后立即设置它的`SO_REUSEADDR`选项，这样即使套接字处于`TIME_WAIT`状态，你也可以再次使用这个端口。以下是一个示例：

```c
int sockfd = socket(AF_INET, SOCK_STREAM, 0);
if (sockfd == -1) {
    perror("socket");
    return 1;
}

int optval = 1;
if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &optval, sizeof(optval)) == -1) {
    perror("setsockopt");
    return 1;
}

```

这段代码会设置套接字的`SO_REUSEADDR`选项，这样你就可以在套接字处于`TIME_WAIT`状态时再次使用这个端口。

![Untitled](炻器USB串口项目 /Untitled%2033.png)

出现情况，无法创建连接

前面有个一直在登录状态，所以导致后面的无法登录

出现原因，

![Untitled](炻器USB串口项目 /Untitled%2034.png)

解决创建多个连接 16001

![Untitled](炻器USB串口项目 /Untitled%2035.png)

如果是正在发送文件的时候，中继服务器意外关闭，那么可能出现的状况就是：

关闭之后，立即打开中继服务器（3秒之内，没有断开）就会接着传输数据

但是断开之后，间隔三秒打开服务器，就可以正常使用（sd628没有收到心跳包就断开了）

出现这么一个问题；

假如收到一包长包之后，第一个就是长包开始，size=16432

那么接下来的消息都将是填充到这个16432里面去？

length is 0 or exceed MAX: 1024

代码运行报错：E/HiView.HiEvent: length is 0 or exceed MAX: 1024

解决方法：这个问题是华鸿蒙系统的问题，只要把真机更换为非鸿蒙的安卓平台即可(像红米这种~)

E win=Window{535e85d u0 Splash Screen com.hoho.android.usbserial.examples EXITING} destroySurfaces: appStopped=false win.mWindowRemovalAllowed=true win.mRemoveOnExit=true

这个日志信息表示，你的应用 **`com.hoho.android.usbserial.examples`** 的启动画面（Splash Screen）窗口正在退出，但应用并未停止。这可能是因为你的应用在后台运行时，窗口的生命周期并未完全结束。

这个问题可能是由于Android的窗口管理机制导致的。当你的应用进入后台时，如果窗口（特别是启动画面）没有被正确地销毁，那么当应用重新进入前台时，这个窗口可能会再次被调用，从而导致一些不可预期的行为。

解决这个问题的一个方法是确保在应用进入后台时正确地销毁所有的窗口。你可以在你的应用的主活动（MainActivity）中重写**`onPause()`**或**`onStop()`**方法，然后在这些方法中销毁所有的窗口。例如

在Android开发过程中，在APP程序退出时，activity中如何才能在退出界面时，结束所有进行和UI等信息

在Android开发中，如果你想在退出应用时结束所有的活动和UI更新，你可以在你的主Activity中重写**`onDestroy()`**方法，并在其中结束你的所有操作。例如：

**Java**AI 生成的代码。仔细查看和使用。 [有关常见问题解答的详细信息](https://www.bing.com/new#faq).

```java
@Override
protected void onDestroy() {
    super.onDestroy();
    // 在这里结束你的所有操作
}

```

此外，你还可以调用**`System.exit(0)`**来结束你的应用的进程。这将会结束你的应用的所有活动和服务。但是请注意，这是一种比较极端的方法，通常不推荐使用，除非你有特别的需求。

**Java**AI 生成的代码。仔细查看和使用。 [有关常见问题解答的详细信息](https://www.bing.com/new#faq).

```java
@Override
public void onDestroy() {
    super.onDestroy();
    System.exit(0);
}

```

如果你的应用包含多个Activity，你可能需要在每个Activity中都添加类似的代码，以确保在退出应用时能够正确地结束所有的活动。

希望这个答案对你有所帮助！如果你还有其他问题，欢迎随时向我提

为什么说他是一种极端的方法？

调用**`System.exit(0)`**被认为是一种极端的方法，因为它会立即停止你的应用的所有进程，包括所有的活动（Activity）、服务（Service）和广播接收器（BroadcastReceiver）。这意味着你的应用的所有状态都会立即丢失，包括所有未保存的用户数据和应用状态。

此外，**`System.exit(0)`**也会终止你的应用的所有后台操作，包括那些可能在后台运行的服务。这可能会导致你的应用的用户体验下降，因为用户可能会注意到你的应用突然停止运行。

最后，**`System.exit(0)`**也会使得Android系统无法正常管理你的应用的生命周期。在Android中，系统会自动管理应用的生命周期，包括在内存不足时结束应用的进程，以及在应用再次被用户打开时重新启动应用的进程。但是如果你的应用调用了**`System.exit(0)`**，那么系统就无法进行这些操作，可能会导致一些不可预期的行为。

因此，虽然**`System.exit(0)`**可以确保你的应用完全退出，但是由于上述的原因，它通常不被推荐使用。在大多数情况下，你应该让Android系统来管理你的应用的生命周期，而不是自己强制结束应用的进程

开启用用的时候 ，清除清空一下usb的缓冲区

设置开机的心跳包

如果是 初始心跳包，中转程序也要跟着进行初始化

```
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);
// 获取USB管理器
UsbManager usbManager = (UsbManager) getSystemService(Context.USB_SERVICE);

// 获取USB设备列表
List<UsbSerialDriver> availableDrivers = UsbSerialProber.getDefaultProber().findAllDrivers(usbManager);
if (availableDrivers.isEmpty()) {
    return;
}

// 打开第一个可用的驱动连接
UsbSerialDriver driver = availableDrivers.get(0);
UsbDeviceConnection connection = usbManager.openDevice(driver.getDevice());
if (connection == null) {
    // 在这里处理没有权限的情况
    return;
}

UsbSerialPort port = driver.getPorts().get(0); // 大多数设备只有一个端口（端口0）
port.open(connection);
port.setParameters(115200, 8, UsbSerialPort.STOPBITS_1, UsbSerialPort.PARITY_NONE);

// 现在你可以使用read()和write()方法来读取和写入数据
}
```

传输过程中拔插一次USB 情况时没问题的

250次失败三次连接超时

![Untitled](炻器USB串口项目 /Untitled%2036.png)

# 测试问题

## 1.实际锁屏，中间服务器掉线了

## 2.重启设备也会导致连不上中继服务器 重启个两三次就不在线了

出现的问题：连接设备。但是中转程序没有启动，进行指令的时候就会出现错误

打开连接

但是这种问题出现出现的错误仅在测试，因为使用时重装程序会一直在运行

# USB 插拔：

客户端 ，只接收心跳包。判断插拔从心跳包来判断

write :Broken pipe 

发送失败，或者是收不到心跳包就直接断开了

但是再连接的时候，就会出现错误 

收到了心跳包，但是没有没有

不过与串口唯一的不同在于，如果通讯正在进行中拔掉USB线，那么再次连接USB线后需要重新open一次ttyGS0设备文件才可以重新发送，否则write不能向串口一样正常工作而返回出错

# 设置设备号

root@TinaLinux:/# vim /usr/bin/i
id                iozone            iptables          iptables-save
iot-mon/          iperf             iptables-restore
root@TinaLinux:/# vim /usr/bin/io
iot-mon/  iozone
root@TinaLinux:/# vim /usr/bin/iot-mon/config.ini
root@TinaLinux:/# vim /usr/bin/iot-mon/config.ini
root@TinaLinux:/#

![Untitled](炻器USB串口项目 /Untitled%2037.png)

# 编译微软雅黑问题解决

1. 打开注册表编辑器。按下 Win + R 键，键入 "regedit"，然后按 Enter。
2. 导航到以下注册表项： HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows
3. 在右侧窗格中，找到名为 "GDIProcessHandleQuota" 的 DWORD 值。如果没有找到该值，请右键单击空白处，选择 "新建" -> "DWORD (32 位) 值"，然后将其命名为 "GDIProcessHandleQuota"。
4. 输入GDI 对象限制值（可以为 0x00007530(30,000)）。
5. 重新启动计算机，使更改生效。

原始值

![Untitled](炻器USB串口项目 /Untitled%2038.png)