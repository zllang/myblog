---
layout: post
title: 升级ubuntu14.04
---


这篇日志来自于ubuntu14.04 lts

#### 前言
12.04也用了这么久了，到了下一个长支持版（LTS）了,虽然还没到最终版，但等不及了，上一个12.04装的是32位，这次果断换作64位的了。因为试用docker的时候竟然说docker现阶段只64位。一个说法是64位平台上可以支持32位服务，但32位平台就不能支持64位了，这说法在理。

也谈不上升级，想当年7.0的时候升级失败，可能留下了阴影，不希望使用升级功能。所以这次只把home目录拷了出来，重新安装，完成后再把home目录中的东西拷过来。

####准备
首先就是下载iso镜像了，我是从[cdimage.ubuntu.com](cdimage.ubuntu.com)上的daily目录下载最新的版本。然后网上查了下grub2启动iso镜像的命令，因为现在光盘神马的有点不划算。查到命令如下

    loopback loop (hd0,msdos6)/trusty-desktop-amd64.iso
    linux (loop)/casper/vmlinuz.efi  boot=casper iso-scan/filename=/trusty-desktop-amd64.iso
    initrd  (loop)/casper/initrd.lz
    boot

第一行使用loopback命令把iso挂载到loop上，注意(hd0,msdos6)替换成自己实际的硬盘目录。挂载后就可以访问iso里的内容了，挂载后可以使用`set root=(loop)`命令把它设成root，以后的命令就可以省略掉(loop)了

第二三行使用linux命令和initrd挂载内核和做一些初始化。

第四行boot 引导



###安装
把要用到的命令记下然后重启进入grub2操作系统选择界面，选择系统那里按c进行命令行模式。依次输入上面的命令，就可以启动到livecd下了。**提示下在grub2的命令行下可以使用TAB键自动补全的，包括文件名路径，如果补全有问题，那多半就是你输错了。**

boot命令后就会很快进入livecd界面，直接双击桌面的快捷图标安装，可能会提示需要卸载sda设备什么的，是因为iso可能存放在和你正要安装的硬盘上，在控制台下输入`mount`命令可以看到有个/isodevice设备被挂载上的，直接umount 可能卸载不掉，加上-l参数就可以了

`umount -l  /isodevice`

其他就是下一步下一步傻瓜操作了。

![](../../../images/new_system.png)




