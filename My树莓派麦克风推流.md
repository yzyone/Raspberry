使用命令行测试麦克风

    sudo arecord -D "plughw:1,0" -d 5 temp.wav

推送麦克风到RTSP服务器

    ffmpeg -f alsa -i plughw:1,0 -codec:a aac -ac 1 -ar 16k -rtsp_transport tcp -f rtsp rtsp://192.168.0.217:554/123456.sdp

参考笔记本推送麦克风

    ffmpeg -f dshow -i audio="麦克风 (Realtek High Definition Audio)" -codec:a mp3 -ac 1 -ar 16k -rtsp_transport tcp -allowed_media_types audio -f rtsp rtsp://192.168.0.217/123456.sdp

推流实时监听

    ffplay -fflags nobuffer rtsp://192.168.0.217:554/123456.sdp