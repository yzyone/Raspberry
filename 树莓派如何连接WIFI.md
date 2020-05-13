树莓派如何连接WIFI

---

上一期呢，给大家介绍了如何给树莓派安装系统，有细心的朋友就会发现上一期安装系统的文章漏了一点点知识，不知道机智的你是否有发现呢~~（尴尬😅）下次一定注意。。😂

在第五步 使用putty 连接树莓派的时候，是使用的ssh 连接的，所以呢树莓派一定要开启ssh，树莓派开启ssh也很简单，就是写完系统以后，直接在根目录新建一个文件，改名ssh 然后删掉后缀名 就OK啦，记住一定要删除.txt 后缀！然后就可以用putty连接了。是不是很简单，你学会了吗~




下面介绍树莓派设置如何连接Wifi

在电脑上插入之前写好系统的TF卡，打开TF卡，在根目录（boot）下新建文本文件，改名wpa_supplicant.conf，然后按照下面的参考格式填入内容并保存

    country=CN
    
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    
    update_config=1
    
    network={
    
        ssid="WiFi-A"
    
          psk="12345678"
    
          key_mgmt=WPA-PSK
    
          priority=1
    
    }

说明以及不同安全性的 WiFi 配置示例：

#ssid:网络的ssid

#psk:密码

#priority:连接优先级，数字越大优先级越高（不可以是负数）

#scan_ssid:连接隐藏WiFi时需要指定该值为1

如果你的 WiFi 没有密码

    network={
    
          ssid="你的无线网络名称（ssid）"
    
            key_mgmt=NONE
    
    }

如果你的 WiFi 使用WEP加密

    network={
    
            ssid="你的无线网络名称（ssid）"
    
            key_mgmt=NONE
    
            wep_key0="你的wifi密码"
    
    }

如果你的 WiFi 使用WPA/WPA2加密

    network={
    
            ssid="你的无线网络名称（ssid）"
    
            key_mgmt=WPA-PSK
    
            psk="你的wifi密码"
    
    }

设置完成以后保存文件，然后把TF卡插上树莓派，上电，等一两分钟，用之前介绍的Advanced_IP_Scanner 工具去扫描树莓派的ip。到此已经完成了，你学会了吗( ﹁ ﹁ ) ~→ 

---

作者：WireHome

链接：https://www.jianshu.com/p/74b8cc666054

来源：简书

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。