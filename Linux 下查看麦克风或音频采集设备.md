Linux 下查看麦克风或音频采集设备

---

前言

最近需要在树莓派上做音频采集和音频处理，所以第一步得在树莓派系统下查看到当前的音频输入和音频输出设备。树莓派安装了raspberry系统，raspberry系统隶属于debian系统。

一、如何查看音频设备

如果你的系统有 /proc/asound/cards 路径,说明 ALSA 驱动已经使用上,可查看 sound devices。
执行以下命令可看到当前的音频设备。

    cat /proc/asound/cards

执行结果：

```
pi@raspberrypi:~$ cat /proc/asound/cards
 0 [ALSA           ]: bcm2835_alsa - bcm2835 ALSA
                      bcm2835 ALSA
 1 [Device         ]: USB-Audio - USB PnP Sound Device
                      C-Media Electronics Inc. USB PnP Sound Device at usb-0000:01:00.0-1.4, full spe
```

可以看到当前有两个音频设备：板子自带的bcm 2835 ALSA声卡和usb音频采集卡。

也可执行以下命令：

    cat /proc/asound/devices 

执行结果：

```
  0: [ 0]   : control
 16: [ 0- 0]: digital audio playback
 17: [ 0- 1]: digital audio playback
 32: [ 1]   : control
 33:        : timer
 56: [ 1- 0]: digital audio capture
```

二、查看音频输入设备（microphone 、麦克风、音频采集卡等）

执行以下命令可看到当前的音频输入设备。

    arecord -l

执行结果：

```
**** List of CAPTURE Hardware Devices ****
card 1: Device [USB PnP Sound Device], device 0: USB Audio [USB Audio]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

可以看到当前只有一个音频输入设备，即插上的usb音频采集卡，同时也说明树莓派自带的音频口只能音频输出，不带有麦克风功能。

三、查看音频输出设备（扬声器，speaker）

    aplay -l

执行结果：

```
**** List of PLAYBACK Hardware Devices ****
card 0: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
  Subdevices: 7/7
  Subdevice #0: subdevice #0
  Subdevice #1: subdevice #1
  Subdevice #2: subdevice #2
  Subdevice #3: subdevice #3
  Subdevice #4: subdevice #4
  Subdevice #5: subdevice #5
  Subdevice #6: subdevice #6
card 0: ALSA [bcm2835 ALSA], device 1: bcm2835 IEC958/HDMI [bcm2835 IEC958/HDMI]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

可以看到有两个音频输出设备，一个是耳机口，一个是hdmi口。

参考资料

1、Linux 下测试 microphone (麦克风) - OOCLAB 开发者社区 https://plus.ooclab.com/note/article/149

---

版权声明：本文为CSDN博主「唐传林」的原创文章，遵循CC 4.0 BY-SA版权协议，转载

请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/tang_chuanlin/article/details/86081102