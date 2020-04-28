树莓派删除Python2.7 默认Python3.7

---

树莓派自带python2和3版本，要想使用3的话，还得特地敲python3、pip3等等一系列的指令

但是python2我们基本上都已经不学了

所以删除python2.7，输入：

    sudo apt-get autoremove python2.7

卸载完后，我们发现想用python3的时候，还得敲python3

想敲python直接出来python3的话，那么先把python链接删掉：

    sudo rm /usr/bin/python

载新建一个链接：

    sudo ln -s /usr/bin/python3.7 /usr/bin/python

查看版本：

    python

显示如下：

    Python 3.7.3 (default, Apr 3 2019, 05:39:12) 
    [GCC 8.2.0] on linux
    Type "help", "copyright", "credits" or "license" for more information.
 

————————————————

版权声明：本文为CSDN博主「草千里」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/liqiancao/article/details/94712863