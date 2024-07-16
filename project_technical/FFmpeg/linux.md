# 将FFplay 移植到Linux中实现音频播放 交叉编译

## 安装X264

<https://www.videolan.org/developers/x264.html>

```sh
./configure --prefix=$PWD/_install --disable-asm --enable-shared --enable-static --host=arm-openwrt-linux-gnueabi
```

## SDL2-2.0.8

### 下载SDL2源文件

### 编译配置

```sh
./configure --prefix=$PWD/_install --host=arm-openwrt-linux-gnueabi   --enable-shared --enable-video-dummy --enable-video-x11   --disable-video-directfb --disable-arts --disable-esd   --disable-joystick --disable-input-tslib --enable-alsa   --enable-alsa-shared --disable-pulseaudio   --with-alsa-prefix=/home/linux20/ffplay/sound/lib   --with-alsa-inc-prefix=/home/linux20/ffplay/sound/include
```

./configure
--prefix=$PWD/_install 输出路径
--host=arm-openwrt-linux-gnueabi  交叉编译器
 --enable-shared --enable-video-dummy --enable-video-x11   --disable-video-directfb --disable-arts --disable-esd   --disable-joystick --disable-input-tslib --enable-alsa   --enable-alsa-shared --disable-pulseaudio  
 选择alsa启动
  --with-alsa-prefix=/home/linux20/ffplay/sound/lib   --with-alsa-inc-prefix=/home/linux20/ffplay/sound/include
配置alsa的头文件与库文件信息

看到其中要有alsa
![alt text](image-17.png)

```sh
sudo  make -j3
sudo PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/home/linux20/toolchain/bin make install
```

## FFmpeg-4.4.4

### 下载ffmpeg源码

### 配置编译

```sh
sudo PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/home/linux20/toolchain/bin    ./configure --enable-shared --enable-static --cross-prefix=arm-openwrt-linux-gnueabi- --enable-cross-compile --target-os=linux  --arch=arm --prefix=$PWD/_install --enable-shared --enable-static --enable-gpl --enable-nonfree --enable-ffmpeg --enable-ffprobe --enable-ffplay  --enable-alsa  --disable-yasm --disable-vdpau --disable-xvmc --extra-cflags=-I/home/linux20/ffplay/SDL2-2.0.8/_install0/include/SDL2 --extra-ldflags=-L/home/linux20/ffplay/SDL2-2.0.8/_install0/lib  --extra-libs=-lSDL2   --extra-cflags=-I/home/linux20/ffplay/sound/include  --extra-ldflags=-L/home/linux20/ffplay/sound/lib  --extra-libs=-lasound
```

可以通过 `configure -h`查看

sudo PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/home/linux20/toolchain/bin 交叉编译工具地址

./configure --enable-shared --enable-static
--cross-prefix=arm-openwrt-linux-gnueabi-  
交叉编译器前缀
--enable-cross-compile --target-os=linux  --arch=arm
目标机系统 arm结构
--prefix=$PWD/_install
输出文件地址
--enable-shared --enable-static --enable-gpl --enable-nonfree
--enable-ffmpeg --enable-ffprobe --enable-ffplay  
生成工具
--enable-alsa  --disable-yasm --disable-vdpau --disable-xvmc
选择的alsa音频驱动  取消选择vdpau、xvmc（编译时会错误）
 --extra-cflags=-I/home/linux20/ffplay/SDL2-2.0.8/_install0/include/SDL2 --extra-ldflags=-L/home/linux20/ffplay/SDL2-2.0.8/_install0/lib  --extra-libs=-lSDL2   --extra-cflags=-I/home/linux20/ffplay/sound/include  --extra-ldflags=-L/home/linux20/ffplay/sound/lib  --extra-libs=-lasound
-extra-cflags、 --extra-ldflags 是显示指示模块的头文件与库文件的位置。
-extra-libs是指定链接到哪个库文件，如果不指定，应该是使用在配置文件里已经设置了的默认值

* 选择SDL2库和alsa库

### FFplay

在ffbuild目录下的config.mak，则表示可以生成ffplay
`CONFIG_FFPLAY=yes`

### 编译

```sh
sudo PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/home/linux20/toolchain/bin STAGING_DIR=/home/linux20/toolchain/arm-openwrt-linux/bin/  make
```

```sh
sudo PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/home/linux20/toolchain/bin STAGING_DIR=/home/linux20/toolchain/arm-openwrt-linux/bin/  make install
```

## alsa 移植

## 目标机运行

生成库程序，编写测试程序进行测试

```
#include "ffplay.h"
#include "dll.h"
//int play(char fileName[], int setframe) ;
int main(int argc, char* argv[])
{
    int frame=0;
    if (argc < 2)
    {
        printf("Usage: %s <filename>\n", argv[0]);
        return 1;
    }
	//char fileName[]="/usr/bin/APP/test.mp4";
    char* fileName = argv[1];
    printf("play %s\n", fileName);
    int ret = play(fileName, 0);
    printf("play begin ret = %d\n", ret);
	int ret2=isVideoWithAudio(fileName);
	printf("isVideoWithAudio %d \n",ret2);
    while (1)
    {
        // 这里可以添加你的逻辑代码
        frame=GetSharedVariable();
        printf(" now video frame %d \n",frame);
    }

    return 0;
}
```

编译生成可执行文件

在测试运行时，首先出现的的错误是：
`
static int audio_open(void* opaque, int64_t wanted_channel_layout, int wanted_nb_channels, int wanted_sample_rate, struct AudioParams* audio_hw_params)
`

```c
    while (!(audio_dev = SDL_OpenAudioDevice(NULL, 0, &wanted_spec, &spec, SDL_AUDIO_ALLOW_FREQUENCY_CHANGE | SDL_AUDIO_ALLOW_CHANNELS_CHANGE))) {
        av_log(NULL, AV_LOG_WARNING, "SDL_OpenAudio (%d channels, %d Hz): %s\n",
            wanted_spec.channels, wanted_spec.freq, SDL_GetError());
            printf("SDL_OpenAudio (%d channels, %d Hz): %s\n",wanted_spec.channels, wanted_spec.freq, SDL_GetError());
        wanted_spec.channels = next_nb_channels[FFMIN(7, wanted_spec.channels)];
        if (!wanted_spec.channels) {
            wanted_spec.freq = next_sample_rates[next_sample_rate_idx--];
            wanted_spec.channels = wanted_nb_channels;
            if (!wanted_spec.freq) {
                av_log(NULL, AV_LOG_ERROR, "No more combinations to try, audio open failed\n");
                printf("No more combinations to try, audio open failed\n");
                printf("audio_open 1 \n");
                return -1;
            }
        }
        wanted_channel_layout = av_get_default_channel_layout(wanted_spec.channels);
    }

```

在windows中解决的办法是：配置环境变量

同样在Linux中也是配置环境变量:`export SDL_AUDIODRIVER=alsa`

之后又出问题：
在ffplay.c 的main函数中SDL_init报错误：
root@TinaLinux:/usr/bin/APP# SDL_AUDIODRIVER=alsa ./test test.mp4
我是用上面的运行程序。  SDL_GetErrorAudio target 'alsa' not available

使用GPT进行排查：
根据你提供的信息和错误信息SDL_GetErrorAudio target 'alsa' not available，这个错误表明SDL2无法找到或使用指定的ALSA音频驱动。这可能是由于以下原因导致的：

ALSA库路径问题：SDL2在运行时找不到正确的ALSA库。确保ALSA库路径已正确设置。

尝试设置LD_LIBRARY_PATH环境变量：

```sh
export LD_LIBRARY_PATH=/home/linux20/ffplay/sound/lib:$LD_LIBRARY_PATH
SDL_AUDIODRIVER=alsa ./test test.mp4
```

ALSA驱动未正确安装或配置：确保在开发板上ALSA驱动已正确安装并配置。

运行以下命令检查ALSA状态：

```sh
aplay -l
```

确认你能看到类似的输出，并且设备可以被识别。
SDL2编译配置问题：确保在编译SDL2时，ALSA支持已正确启用并检测到。

你可以在configure脚本运行时的输出中查找关于ALSA的配置：

```sh
./configure --prefix=$PWD/_install --host=arm-openwrt-linux-gnueabi \
  --enable-shared --enable-video-dummy --enable-video-x11 \
  --disable-video-directfb --disable-arts --disable-esd \
  --disable-joystick --disable-input-tslib --enable-alsa \
  --enable-alsa-shared --disable-pulseaudio \
  --with-alsa-prefix=/home/linux20/ffplay/sound/lib \
  --with-alsa-inc-prefix=/home/linux20/ffplay/sound/include/
```

缺少必要的运行时库：确认运行时的库文件都在正确的位置，并且可以被访问。

检查你的应用程序的依赖关系：

```sh
ldd ./test
```

ALSA库版本兼容性：确认你使用的ALSA库版本与SDL2兼容。

判断SDL2支持ALSA声音驱动
`strings /usr/lib/libSDL2.so | grep alsa
`

然后根据上面的配置SDL2 重新生成支持alsa的SDL2库
