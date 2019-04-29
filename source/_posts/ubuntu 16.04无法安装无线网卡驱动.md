---
title: ubuntu 16.04无法安装无线网卡驱动 
date: 2019-03-26 15:49:04
categories:
- linux
---

昨天把笔记本格盘装了Ubuntu16.04同样是WiFi打不开（点击开启却又自动关闭）（蓝牙也是同样打不开），和题主一样的情况，在附加驱动里面是没有无线网卡驱动。 
查了一下网卡是 Intel® Dual Band Wireless-AC 3165 ，查了不少CSDN博客，3165 的网卡驱动在 Kernel 4.1+ 才被添加，但我 Kernel 都 4.13 了喂（摔）。

``` bash
uname -a
$ Linux Twilight 4.13.0-32-generic #35~16.04.1-Ubuntu SMP Thu Jan 25 10:13:43 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux      
```
 
lib/firmware/    下也有相应的固件。（但就是打不开我也很绝望啊）         可以根据自己的网卡来这儿找固件（如果没有的话）：https://link.zhihu.com/?target=https%3A//wireless.wiki.kernel.org/en/users/Drivers/iwlwifi
然后顺手 rfkill 看了下：
```bash
rfkill list all
$ 0: ideapad_wlan: Wireless LAN
          Soft blocked: no
          Hard blocked: yes
  1: ideapad_bluetooth: Bluetooth
          Soft blocked: yes
          Hard blocked: yes
  2: hci0: Bluetooth
	Soft blocked: yes
	Hard blocked: no
  3: phy0: Wireless LAN
	Soft blocked: no
	Hard blocked: no 
```
绝了，这硬件给锁了。  那就直接把这个模块移除了。

```bash
sudo modprode -r < WIRELESS DRIVER >
# 答主的是直接这样了：
# sudo modprobe -r ideapad_laptop        
```

Rua！然后就可以打开无线网络了(蓝牙也可以使用了)。         看了下好像有两个网卡（？）还有个大螃蟹的。
```bash
lspci
$ 03:00.0 Network controller: Intel Corporation Intel Dual Band Wireless-AC 3165 Plus Bluetooth (rev 99)
  04:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 10)   
```
  没有找到官方相关Linux下的驱动         emmm 找到了一个开源驱动：[rtlwifi_new](https://link.zhihu.com/?target=https%3A//github.com/lwfinger/rtlwifi_new)

```bash
git clone https://github.com/lwfinger/rtlwifi_new.git
cd rtlwifi_new
sudo make && make install       
```

重启就好了。但是每次重启都要重新移除模块比较麻烦，把下面的添加到 /etc/rc.local
```bash
echo 'password' | sudo sudo modprode -r < WIRELESS DRIVER >         
```
以上

作者：匿名用户
链接：https://www.zhihu.com/question/264008505/answer/314220196
来源：知乎
著作权归作者所有.