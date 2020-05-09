树莓派下paudio安装与声音监控运用

---

在树莓派平台上使用pyaudio实现usb麦克风的录音功能，进而可以实现人机交互，实现语音识别和语音合成。

参考pyaudio官方文档，链接地址如下：

> http://people.csail.mit.edu/hubert/pyaudio/docs/

pyaudio是python的模块，在树莓派下安装pyaudio 首先需安装portaudio.dev
安装步骤如下：

1、安装portaudio.dev : 

    sudo apt-get install portaudio.dev

2、安装python-pyaudio: 

    sudo apt-get install python-pyaudio

3、安装sox快速检测麦克风配置是否正确：

    sudo apt-get install sox

4、测试麦克风配置是否正确，树莓派终端输入以下命令：

    rec temp.wav

5、测试pyaudio 代码如下：录音40s并保存为audio.wav播放。

```
#_*_ coding:UTF-8 _*_
# @author: zdl 
# 测试pyaudio 使用pyaudio录音，录音完毕播放录音内容
# 需要安装pyaudio 安装过程在教程中讲解
# pyaudio API函数库参考: http://people.csail.mit.edu/hubert/pyaudio/docs/#pyaudio.Stream.write

import wave
from pyaudio import PyAudio,paInt16

# 设置采样参数
NUM_SAMPLES = 2000
TIME = 2
chunk = 1024

# read wav file from filename 
def read_wave_file(filename):
	
    fp = wave.open(filename,'rb')
    nf = fp.getnframes()     #获取采样点数量
    print('sampwidth:',fp.getsampwidth())  
    print('framerate:',fp.getframerate())
    print('channels:',fp.getnchannels())
    f_len = nf*2
    audio_data = fp.readframes(nf)

# save wav file to filename 
def save_wave_file(filename,data):  

    wf = wave.open(filename,'wb')
    wf.setnchannels(1)      # set channels  1 or 2
    wf.setsampwidth(2)      # set sampwidth 1 or 2
    wf.setframerate(16000)  # set framerate 8K or 16K
    wf.writeframes(b"".join(data))  # write data
    wf.close()

#recode audio to audio.wav
def record():
    pa = PyAudio()     # 实例化 pyaudio
	
    # 打开输入流并设置音频采样参数 1 channel 16K framerate 
    stream = pa.open(format = paInt16,
                     channels=1,
                     rate=16000,
                     input=True,
                     frames_per_buffer=NUM_SAMPLES)
	
    audioBuffer = []   # 录音缓存数组
    count = 0
	
    # 录制40s语音
    while count<TIME*20:
        string_audio_data = stream.read(NUM_SAMPLES) #一次性录音采样字节的大小
        audioBuffer.append(string_audio_data)
        count +=1
        print('.'),  #加逗号不换行输出
	
    # 保存录制的语音文件到audio.wav中并关闭流
    save_wave_file('audio.wav',audioBuffer)
    stream.close()

# 播放后缀为wav的音频文件
def play():

    wf = wave.open(r"audio.wav",'rb') # 打开audio.wav
    p = PyAudio()                     # 实例化pyaudio
	
    # 打开流
    stream = p.open( format=p.get_format_from_width(wf.getsampwidth()),
                     channels=wf.getnchannels(),
                     rate=wf.getframerate(),
                     output=True)
    # 播放音频
    while True:
        data = wf.readframes(chunk)
        if data == "":break
        stream.write(data)
	
    # 释放IO
    stream.stop_stream()
    stream.close()
    p.terminate()

# main函数 录制40s音频并播放
if __name__ == '__main__':

    print('record ready...')
    record()
    print('record over!') 
    play()
```

程序演示结果：


使用不同的usb摄像头会出现采样率报错的问题，解决方案参考以下博客：



> https://blog.csdn.net/u013860985/article/details/79326379

> https://blog.csdn.net/u013372900/article/details/80296125


添加一个新的~/.asoundrc 到pi目录下：

    sudo nano ~/.asoundrc

输入以下内容：

```
pcm.!default {
    type hw
    card 1
}
ctl.!default {
    type hw
    card 1
}
```


或者：

```
pcm.!default {
        type asym
            playback.pcm {
                type plug
                slave.pcm "hw:0,0"
            }
            capture.pcm {
                type plug
                slave.pcm "hw:1,0"
            }        
}
ctl.!default {
        type hw
        card 1
}
```


树莓派音频输出设置：

1、选择树莓派 audio output 为AUTO或者3.5mm sudo raspi-config 设置

2、如何调整输出音量，终端输入 alsamixer 命令然后上下键调整

3、语音合成的音频为MP3文件，pyaudio只能播放wav文件，所以需要安装MP3播放插件 mplayer。

安装 sudo apt-get install mplayer

测试 mplayer xx.mp3 （需先进入xx.mp3的文件夹下）

Python 程序播放 MP3: os.system('mplayer %s' % 'xx.mp3')

思考：如何采用pyaudio制作声音的监控装置呢？监控噪声、人或者物体运动然后提示报警？

```
#coding:utf-8
# @author: zdl 

#需要安装pyaudio
import wave
import numpy as np
from pyaudio import PyAudio,paInt16
import time
NUM_SAMPLES = 2000
global t

chunk = 1024
def play(filename):
    wf = wave.open(filename,'rb')
    p = PyAudio()
    stream = p.open( format=p.get_format_from_width(wf.getsampwidth()),
                     channels=wf.getnchannels(),
                     rate=wf.getframerate(),
                     output=True)
    while True:
        data = wf.readframes(chunk)
        if data == "":break
        stream.write(data)
    stream.stop_stream()
    stream.close()
    p.terminate()
    
def save_wave_file(filename,data):   #save data to filename

    wf = wave.open(filename,'wb')
    wf.setnchannels(1)   #set channel
    wf.setsampwidth(2)  #采样字节  1 or 2
    wf.setframerate(16000)  #采样频率  8K or 16K
    wf.writeframes(b"".join(data))
    wf.close()

# 声音监视函数
def Monitor():
    pa = PyAudio()      # 调用句柄
    stream = pa.open(format = paInt16,
                     channels=1,
                     rate=16000,
                     input=True,
                     frames_per_buffer=NUM_SAMPLES)
    print('开始缓存录音')
    audioBuffer = []
    rec = []
    audioFlag = False
    t = False
    while True:
        data = stream.read(NUM_SAMPLES,exception_on_overflow = False) #add exception para
        #if audioFlag == True:
           # rec.append(data)        #剪切的语音文件
        audioBuffer.append(data)     #录音源文件
        audioData = np.fromstring(data,dtype=np.short) #字符串创建矩阵
        largeSampleCount = np.sum(audioData > 2000)
        temp = np.max(audioData)

        print temp
        if temp > 8000 and t == False:  # 声音阈值，结合实验确定，实现声音监控
            t = 1 #开始录音
            print "检测到语音信号，开始录音"
            begin = time.time()
            print temp
        if t:
            end = time.time()
            if end-begin > 5:
                timeFlag = 1 #5s录音结束
            if largeSampleCount > 20:
                saveCount = 4
            else:
                saveCount -=1
            if saveCount <0:
                saveCount =0
            if saveCount >0:
                rec.append(data)
            else:
                if len(rec) >0 or timeFlag:
                    save_wave_file('01.wav',rec)
                    rec = []
                    t = 0
                    timeFlag = 0
    stream.stop_stream()
    stream.close()
    p.terminate()

if __name__ == '__main__':

    Monitor()
```


可以查看我的github获取更多信息：

> https://github.com/dalinzhangzdl/AI_Car_Raspberry-pi

---

版权声明：本文为CSDN博主「da木木」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/sinat_35162460/article/details/86541702