# 无main函数打包静态库/.s→.o

### 静态库的制作

编译时候链接静态库

创建静态库用ar指令，他将多个,o转换为.a

ar rcs libmyhello.a hello.o

libmyhello.a 是库文件名

其中的myhello是库名

gcc -c hello 生成.o文件

ar rcs libmyfun.a fun.o hello.o 

使用测试函数test.c

gcc test.c -L. -lmyfun 连接编译

-L：指定库的路径

. :当前目录下

-l：指定库名

### 动态库

运行阶段加载动态库

创建动态库

gcc -c -fPIC hello.c

-fPIC 生成与地址无关的代码。

使用-shared 

gcc -shared -o libmyhello.so hello.o

使用测试

gcc teat.c -L. -lmyhello 

在使用时，因为是一边运行一边加载，所以要把动态库放到能找到的地方

1.应将动态库拷贝到 /usr/lib或/lib下 

cp libmyhello.so /usr/lib/

2.修改环境变量，只对当前终端有效

export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:路径 (.：当前路径)

3.修改配置文件 vi /etc/ld.so.conf.d/my.conf

将自己的库路径加进去 ： /root/IO/io/lib/so

（pwd： 查看当前路径）

接着使用命令 ldconfig 刷新刚才的配置文件

### .s文件到.o文件

arm-linux-gnueabihf-gcc -c asm-led.s -o asm-led.o