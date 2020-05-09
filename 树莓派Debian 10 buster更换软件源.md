树莓派Debian 10 buster更换软件源

---

树莓派软件源服务器在国外更新太慢，要更新速度快一点就需要更换国内源！加速更新速度！网上的教程很多不是基于Debian 10 buster，所以重新记录一下！

编辑 /etc/apt/sources.list 文件，

    sudo nano /etc/apt/sources.list

删除原文件所有内容，用以下内容取代：

    deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib
    deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib

编辑 /etc/apt/sources.list.d/raspi.list 文件；

    sudo nano /etc/apt/sources.list.d/raspi.list

删除原文件所有内容，用以下内容取代：

    deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui

更换源后，需要更新本地软件索引：

    sudo apt-get update

---

作者：Chinghom

链接：https://www.jianshu.com/p/93ef6bace0a4

来源：简书

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。