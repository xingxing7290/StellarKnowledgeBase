# FFmpeg

项目简介：
基于ffmpeg开发dll文件，用于与VB6中的窗口应用程序使用
原有应用只有显示视频帧功能，没有播放声音。使用ffmpeg开发源码开发，基于音频与视频的同步机制，以音频播放为主，时刻输出音频播放时所对应的视频帧。然后在原有的程序中，根据输出帧画面去匹配视频帧
同时配备功能：播放 暂停 停止，指定视频帧播放，跳转到指定视频帧

注意，其中跳转到指定视频帧，实现精确seek.视频中会有关键帧的影响因素，影响到视频帧跳转不准，本项目实现基于音频精确seek,从而跳转到指定视频帧

项目文件参见：
<https://github.com/xingxing7290/ffplay_dll>

- [ffplay移植linux](/project_technical/FFmpeg/linux.md)
- [ffplay](/project_technical/FFmpeg/FFplay.md)
- [参考网址](/project_technical/FFmpeg/background.md)
