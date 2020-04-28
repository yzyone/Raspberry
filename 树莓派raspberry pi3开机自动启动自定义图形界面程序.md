此种方法是在树莓派正常启动并进入图形界面后打开自定义的图形界面程序【亦应适用于非图形界面程序】，具体如下：

1.进入/home/pi/.config文件夹:

    $cd /home/pi/.config

2.在.config文件夹中创建autostart文件夹:

    $mkdir autostart

3.在autostart文件夹中创建my.desktop文件:

文件内容如下：
    
    #file start
    
    [Desktop Entry]
    
    Type=Application
    
    Exec=#你要启动的图形界面程序的绝对路径#
    
    #file end

4.reboot看下有没有效果吧。


ps;在pi上试了多种开机启动的方法，效果都没达到预期，最终试了这种方法，感谢下面参考连接的作者:)


参考：

http://blog.sina.com.cn/s/blog_6b2252130102wjx6.html

————————————————

版权声明：本文为CSDN博主「vaylb」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/u014642880/article/details/54341869