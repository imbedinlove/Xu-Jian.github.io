---
layout: post
title: Ping utility 
tags: Misc
categories: Net
---


## Ping utility 

Windows 默认ping 的数据包 32byte
大小可自定义. 但是最大只能 65500 byt
> 超过这个大小,会处罚一个安全漏洞.使得对方死机. 所以规定了最大值.
配合别的参数  危害依然很大.
比如10+台电脑 同时一直用最大的包 Ping 一个主机. 五分钟左右 对方的网络就会挂.  
    `ping -l 65500 -t 192.168.1.21` 


## PING 

*linux & windows 区别 :  Linux 需要 ⌃+C 手动终止.不然会一直ping。

#### 原理 :

发送 ICMP 数据包到网络主机, 根据返回信息确定目标主机是否可访问
> 但这不是绝对的. 有些服务器或者路由器 为了防止通过ping探测到，
> 通过防火墙设置了禁止ping或者在内核参数中禁止ping

#### 功能 :

- 确定网络状态
- 确定主机状态(开关机)?
- 确定 硬件 软件问题.
- 测试. 评估和管理网络

#### 命令参数 ：

-r 忽略普通的Routing Table，直接将数据包送到远端主机上。通常是查看本机的网络接口是否有问题。
-R 记录路由过程。

-I 网络界面：使用指定的网络界面送出数据包。
-l 前置载入：设置在送出要求信息之前，先行发出的数据包。
-p 范本样式：设置填满数据包的范本样式。



- 命令格式：
		ping [参数] [主机名或IP地址]

- 指定次数  *- c* Count 次数
		ping -c 10 172.19.16.16

- 指定间隔   *- i* interval  间隔
		ping -i 3 172.19.16.16
		// 时间间隔.   默认一秒一个数据包.

- 指定间隔+次数
		ping -c 10 -i 3 172.19.16.16
		// 10次 每次间隔3秒

- Ping 网址 得 IP
		ping www.baidu.com
		// 得到 ip 地址

- 指定数据包大小  *- s*  packetsize 
		ping -s 64 172.19.16.16
		//-s 字节数：指定发送的数据字节数，预设值是56，加上8字节的ICMP头，一共是64ICMP数据字节。
 
- 直接显示结果.   *- p* 
		ping -p -c 10 172.19.16.16
		//最好指定个次数 不然要手动 ⌃+C 才会显示结果.





