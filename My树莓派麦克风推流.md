使用命令行测试麦克风

sudo arecord -D "plughw:1,0" -d 5 temp.wav

推送麦克风到RTSP服务器

ffmpeg -f alsa -i plughw:1,0 -codec:a aac -ac 1 -ar 16k -rtsp_transport tcp -f rtsp rtsp://192.168.0.217:554/123456.sdp
