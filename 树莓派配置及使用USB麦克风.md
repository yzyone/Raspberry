树莓派配置及使用USB麦克风

---

树莓派控制器可以输出音频，但是不能录制，要录制声音，我们需要使用麦克风，这里给大家介绍一种通用USB麦克风在树莓派上的配置及使用方法

工具/原料

- raspberry pi 控制器
- USB 麦克风
- Putty 软件 + 远程登录到树莓派控制器

方法/步骤

1、连接麦克风和Raspberry Pi控制器，使用命令：

    lsusb 

查看设备是否已经成功连接，如图所示，为已检测到USB麦克风

2、使用命令行测试麦克风

    sudo arecord -D "plughw:1,0" -d 5 temp.wav
    -D 参数：选择设备， 外部设备是 plughw:1,0，内部设备是 plughw:0,0， 树莓派本身并没有录音模块，所以没有内部设备。 
    -d 5参数：录制时间为 5 秒，如果不加这个参数就会一直录音，直到按下 ctrl+c 才会停止
    temp.wav 是生成的文件名字

存储位置就是当前所在目录下

当录制完成后，可以使用 ls 查看当前目录下是否保存了这样一个音频文件

3、播放录制的音频，需要安装 omxplayer

首先获取软件源更新内容

    sudo apt-get update
    
    sudo apt-get upgrade


4、安装 omxplayer 播放器

    sudo apt-get install omxplayer

由于我的控制器中已经安装了 omxplayer 所以系统提示已经有了omxplayer

5、树莓派支持 3.5mm 接口音频输出和 HDMI 音频输出，可以通过 config 界面来进行配置

如下图所示，输入

    sudo raspi-config

6、声音输出模式配置

    （1）选择进入第 7 项 - Advanced Options
    
    （2）选择 Audio，单击回车

7、配置声音输出模式

    0 - 自动选择
    
    1 - 3.5mm 音频接口输出
    
    2 - HDMI 输出，这种方式一般是在显示器有喇叭的情况下，使用显示器自带的喇叭播放音频

选择播放模式后，使用 TAB 键切换到 OK，然后单击回车确认，就完成了音频输出模式配置

8、退出配置界面后，SSH终端会显示模式配置成功

    sudo raspi-config

9、播放录制的音频

    omxplayer -o local temp.wav

音频开始播放，在播放完成后 ，自动退出

END
