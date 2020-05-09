树莓派使用ffmpeg硬编码将音视频推流到直播平台

---

1、安装ffmpeg

    sudo apt-get install ffmpeg

2、启动推流（包含声音录制）

    ffmpeg -f alsa -ar 16000 -ac 1 -i hw:1 -f video4linux2 -s 640x480 -r 25 -i /dev/video0 -c:v h264_omx -f flv "rtmp地址"

如果没有麦克风，可以用以下命令：

    ffmpeg -f video4linux2 -s 640x480 -r 25 -i /dev/video0 -c:v h264_omx -f flv "rtmp地址"

参数说明：

    ffmpeg -f alsa -ar 16000 -ac 1 -i hw:1 -f video4linux2 -s 640x480 -r 25 -i /dev/video0 -c:v h264_omx -f flv "rtmp地址"

上述ffmpeg命令有三个部分组成，标注橙色部分为音频输入，红色为视频输入，绿色为串流输出

-f  指定输入或输出文件格式

alsa 高级Linux声音架构的简称

video4linux2 为linux中视频设备的内核驱动

-ar 16000 指定音频采样率为16kHz

-ac 1 指定音频声数

-i hw:1 指定输入来源

-s 640x480 指定帧大小（不指定的话就为原画尺寸）

-r 25 指定帧率fps

-c:v h264_omx 指定用h264_omx解码（树莓派的硬件解码，速度快得多）

常见告警：

Thread message queue blocking; consider raising the thread_queue_size option (current value: 8)

官方解释：此选项设置从文件或设备读取时排队数据包的最大数目。在低延迟/高速率的实时流中，如果没有及时读取数据包，则可能会丢弃它们；提高此值可以避免此情况。

因此，可以适当提高thread_queue_size的值，直到不再出现此告警。

    -thread_queue_size 1024

最终，可以将推流命令这样写：

    ffmpeg -f alsa -thread_queue_size 1024 -ar 16000 -ac 1 -i hw:1 -f video4linux2 -thread_queue_size 1024 -s 640x480 -r 25 -i /dev/video0 -c:v h264_omx -f flv "rtmp地址"

更多详细说明可参考ffmpeg官方文档：http://ffmpeg.org/ffmpeg.html

3、后台执行

（1）安装screen

    sudo apt-get install screen

（2）新建终端

    screen -S live

（3）在新的终端执行命令后

    Ctrl+A+D

（4）重新进入终端

    screen -r live

（5）删除某一个终端

    screen -S 进程号 -X quit
