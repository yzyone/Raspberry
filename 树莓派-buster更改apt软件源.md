raspbian-buster更改apt软件源

```
//更改apt源
$ sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
$ sudo echo "" > /etc/apt/sources.list
$ sudo nano /etc/apt/sources.list	//编辑sources.list添加如下内容

deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
//或修改为阿里云源 
deb https://mirrors.aliyun.com/raspbian/raspbian/ buster main contrib non-free rpi
deb-src https://mirrors.aliyun.com/raspbian/raspbian/ buster main contrib non-free rpi

$ sudo apt-get update		//更新软件包索引
$ sudo apt-get upgrade		//更新软件包

```