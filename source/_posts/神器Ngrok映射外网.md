---
title: Ngrok神器映射外网
date: 2016-09-20 19:59:53
tags: Ngrok
---


<b>ngrok 服务可以分配给你一个域名让你本地的web项目提供给外网访问，特别适合向别人展示你本机的web demo 以及调试一些远程的API (比如微信公众号，企业号的开发) </b>

<img src="https://desk-fd.zol-img.com.cn/t_s1440x900c5/g5/M00/06/0B/ChMkJ1lkKOSIMAebABMY8g_U_F0AAeWIQDF5hoAExkK642.jpg">

<!--more-->
## 具体步骤 ##

 - 下载linux版本的客户端，解压到你喜欢的目录,注意：要给ngrok文件的可执行权限[Ubuntu 64 位下载地址][1]


  [1]: http://pan.baidu.com/s/1jG4fEGu



 - 在命令行下进入到`path/to/linux_amd64/`下
 - 执行 `./ngrok -config=ngrok.cfg -subdomain xxx 8080` //(`xxx` 是你自定义的域名前缀，`8080`是你本机服务器对应的端口，由于我用的是`Apache Tomcat 7`所以端口是`8080`)
 - 如果开启成功 你就可以使用 `xxx.tunnel.qydev.com` 来访问你本机的 `127.0.0.1:80` 的服务啦
 - 如果你自己有顶级域名，想通过自己的域名来访问本机的项目，那么先将自己的顶级域名解析到`123.57.165.240` (域名需要已备案哦),然后执行`./ngrok -config=ngrok.cfg -hostname xxx.xxx.xxx 8080` //(xxx.xxx.xxx是你自定义的顶级域名)
 - 如果开启成功 你就可以使用你的顶级域名来访问你本机的 `127.0.0.1:8080` 的服务啦





<b>参考文献:http://qydev.com/#</b>

                                        Writen on 11.Apri.2016
