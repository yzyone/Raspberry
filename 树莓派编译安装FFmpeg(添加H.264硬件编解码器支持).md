树莓派编译安装FFmpeg(添加H.264硬件编解码器支持)

---

# 说明 #

FFmpeg是一套开源的音视频编解码库，有非常强大的功能，包括视频采集功能、视频格式转换等。众所周知视频编解码是一个非常消耗系统资源的过程，而树莓派自带了H.264的硬件编解码器，因此本文将详解在树莓派配置FFmpeg使其支持硬件编解码器并编译安装的过程。

# 准备工作 #

- 树莓派一个（1至3代都可以）
- 已连接到网络（github无障碍）

# 步骤 #

更新源并安装git

    sudo apt-get update
    sudo apt-get install git

x264配置脚本config_x264_rpi.sh，放进x264目录

    #!/bin/sh
    ./configure \
    --disable-shared --enable-static \
    --enable-strip \
    --disable-cli

下载x264源码并编译安装

    git clone git://git.videolan.org/x264.git
    cd x264
    mv ../config_x264_rpi.sh ./
    chmod +x config_x264_rpi.sh
    ./config_x264_rpi.sh
    make -j4
    sudo make install

ffmpeg配置脚本config_ffmpeg_rpi.sh，放进ffmpeg目录

    #!/bin/sh
    PREFIX=/usr/local
    ./configure \
    --enable-gpl--enable-version3 --enable-nonfree \
    --enable-static --disable-shared \
    \
    --prefix=$PREFIX \
    \
    --disable-opencl \
    --disable-thumb \
    --disable-pic \
    --disable-stripping \
    \
    --enable-small \
    \
    --enable-ffmpeg \
    --enable-ffplay \
    --enable-ffserver \
    --enable-ffprobe \
    \
    --disable-doc \
    --disable-htmlpages \
    --disable-podpages \
    --disable-txtpages \
    --disable-manpages \
    \
    --disable-everything \
    \
    --enable-libx264 \
    --enable-encoder=libx264 \
    --enable-decoder=h264 \
    --enable-encoder=aac \
    --enable-decoder=aac \
    --enable-encoder=pcm_s16le \
    --enable-decoder=pcm_s16le \
    --enable-encoder=ac3 \
    --enable-decoder=ac3 \
    --enable-encoder=rawvideo \
    --enable-decoder=rawvideo \
    --enable-encoder=mjpeg \
    --enable-decoder=mjpeg \
    \
    --enable-demuxer=concat \
    --enable-muxer=flv \
    --enable-demuxer=flv \
    --enable-demuxer=live_flv \
    --enable-muxer=hls \
    --enable-muxer=segment \
    --enable-muxer=stream_segment \
    --enable-muxer=mov \
    --enable-demuxer=mov \
    --enable-muxer=mp4 \
    --enable-muxer=mpegts \
    --enable-demuxer=mpegts \
    --enable-demuxer=mpegvideo \
    --enable-muxer=matroska \
    --enable-demuxer=matroska \
    --enable-muxer=wav \
    --enable-demuxer=wav \
    --enable-muxer=pcm* \
    --enable-demuxer=pcm* \
    --enable-muxer=rawvideo \
    --enable-demuxer=rawvideo \
    --enable-muxer=rtsp \
    --enable-demuxer=rtsp \
    --enable-muxer=rtsp \
    --enable-demuxer=sdp \
    --enable-muxer=fifo \
    --enable-muxer=tee \
    \
    --enable-parser=h264 \
    --enable-parser=aac \
    \
    --enable-protocol=file \
    --enable-protocol=tcp \
    --enable-protocol=rtmp \
    --enable-protocol=cache \
    --enable-protocol=pipe \
    \
    --enable-filter=aresample \
    --enable-filter=allyuv \
    --enable-filter=scale \
    --enable-libfreetype \
    \
    --enable-indev=v4l2 \
    --enable-indev=alsa \
    \
    --enable-omx \
    --enable-omx-rpi \
    --enable-encoder=h264_omx \
    \
    --enable-mmal \
    --enable-hwaccel=h264_mmal \
    --enable-decoder=h264_mmal \
    \

在FFmpeg官网获取源码 http://ffmpeg.org/download.html ，当前版本为 ffmpeg-3.3.2.tar.bz2 ，配置完成后编译并安装

wget http://ffmpeg.org/releases/ffmpeg-3.3.2.tar.bz2
tar jxvf ffmpeg-3.3.2.tar.bz2
cd ffmpeg-3.3.2
mv ../config_ffmpeg_rpi.sh ./
chmod +x config_ffmpeg_rpi.sh
./config_ffmpeg_rpi.sh
make -j4
sudo make install

输入ffmpeg并回车，可以看到以下内容，其中有h264_omx和h264_mmal字样，说明ffmpeg已支持树莓派的H.264硬件编解码器。


安装ffmpeg成功截图


最后

下一篇文章将介绍硬件H.264硬件编解码器的应用。

---

作者：HintLee

链接：https://www.jianshu.com/p/dec9bf9cffc9

来源：简书

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。