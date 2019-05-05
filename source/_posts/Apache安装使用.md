---
title: Apache服务安装使用
date: 2019-05-05 10:20:08
tags:
categories:
- 技术
- 服务器
---

1.Apache的win版本[下载地址](https://www.apachehaus.com/cgi-bin/download.plx#up)。
2.选择Apache 2.4.39 x64，可根据自己的需求下载对应版本。
3.解压。解压出来的文件夹叫apache24。
4.打开apache24/conf/httpd.conf ，修改Define SRVROOT  后面的路径为解压出来的aphace24文件夹的路径。找到Listene 80，若你的80端口被占用（可在cmd下用命令netstat -ano查看），则将80端口改为别的值，然后保存httpd.conf文件。
5.用 Windows管理员身份打开cmd，切换到aphace\Apache24\bin目录下，执行httpd -k install。
5.安装好了打开apache24/bin/ 双击ApacheMonitor，电脑右下角会出现图标。点击进去点击start!会变成绿色。如果没有，就解决提示的相应问题。
6.去浏览器，输入localhost有页面就是成功了！出现的是aphace默认的页面！
7.把自己写的html文件放进去能访问，默认的是在apache24/htdocs文件夹下面！ 
8.局域网里面的人能访问这个文件，把localhost改成ip就好。