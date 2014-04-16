---
layout: post
title: Ubuntu下搭建appinventor离线开发环境
---

###前言   Appinventor是什么？
Appinventor早前是google实验室的一个项目，旨在实现让没有丰富开发经验的人也可以对android进行编程开发，它抛弃了传统的手写代码方式，使用一种基于浏览器的Blockly像堆积木一样的完成程序，代码编写界面、界面设计、和打包所有的操作都可以通过浏览器实现。详细的可以去官网看下[http://appinventor.mit.edu/explore/](http://appinventor.mit.edu/explore/)项目本来是google的后面google放弃了这个项目，被 MIT接下来继续搞的，所以官网是mit.edu。另这里是[appinventor中文论坛](http://bbs.appinventor.com.cn)，上面也有一些示例代码。
![overview.png](../../../images/overview.png)

前面提到，所有的操作都通过浏览访问[这里](http://appinventor.mit.edu/explore/),就可以完成，但因为f**k的GFW,很多人连接不上，或者非常慢，就有了这篇在本地搭建开发环境 。本文主要的环境是UbuntuLinux环境，Windows环境的可以到[Appinventor中文论坛]()上去下载。

###环境准备
因为appinventor有很多实现是Java，所以 需要先安装java编译环境，可以去java官网上安装，我使用的是直接用apt-get安装的。
> sudo apt-get install openjdk-7-jdk ant

这条命令安装了jdk和ant，ant是类似make的方便的编译工具。

因为appinventor的在线环境是通过google appengine 运行的，所以 我们需要下载appengine的java-sdk。这里是[下载链接](https://commondatastorage.googleapis.com/appengine-sdks/featured/appengine-java-sdk-1.9.3.zip)，这是写这篇博客时最新的版本，如果链接 失效请到这个[google](https://developers.google.com/appengine/downloads)官网上找**Google App Engine SDK for Java**的下载。 

![download_appengine.png](../../../images/download_appengine.png)

然后就是下载代码，appinventor是开放源码，源码托管在[github.com](https://github.com/mit-cml/appinventor-sources)上,,点击链接 进去后，在右下角有Download ZIP,就是下载源代码了。

如果网速太慢，有个办法是把链接 发到baidu去盘或者 360网盘上离线下载，很快就下好了，再从网盘下载就会快很多了。

###编译源码
**CTRL+ALT+T**打开Ubuntu的终端，执行以下命令：

创建一个dev目录。
>mkdir ~/dev  

切换到刚才下载的两个文件所在的目录 ,如果你安装环境为英文 也可能是**~/Download**
>cd ~/下载  

解压appinvengor源码到我们新建的dev目录。如果你的下载包和我的名字不同，请自行补上
>`unzip   appinventor-sources-master.zip -d ~/dev/  `

解压 appengine-sdk
>`unzip   appengine-java-sdk-1.9.3.zip  -d ~/dev/   `

这里转到dev目录，并列出目录内容，输出会包含我们解压的两个目录。
>`cd  ~/dev   && ls   `

转到appinventor目录并输出文件列表 ，输出里可以看到大概十个左右文件夹，还包含一个build.xml文件。
>`cd appinventor-sources-master/appinventor && ls`

**在正式开始编译之前，还需要改两处**

执行以下命令使用gedit打开这个文件，
>`gedit common/build/src/com/google/appinventor/common/version/GitBuildId.java`

在大概17行左右找到如下代码。

```
  // The following values are set during the ant build.
  public static final String GIT_BUILD_VERSION = "fatal: Not a git repository (or any parent up to mount point /home)
Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYSTEM not set).";
  public static final String GIT_BUILD_FINGERPRINT = "fatal: Not a git repository (or any parent up to mount point /home)
Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYSTEM not set).";
```
我们需要将两个=号后面引号中的字符全都删除，或者将引号中第一行最后的换行符删除。这是代码中故意留下的与BUG检测相关的设置，我们这可以不关注。
可以直接为如下

```
  // The following values are set during the ant build.
  public static final String GIT_BUILD_VERSION = "";
  public static final String GIT_BUILD_FINGERPRINT = "";
```

按ctrl+s 或点击菜单保存，然后关闭gedit，返回我们的终端界面 。

开始编译
>`ant  `

应该就可以看到字符流了。然后就是等待了。
如果最后没有报错，恭喜编译成功了。下一步就是运行了。

###运行服务器
因为离线环境包括编写程序时的服务器，和编译打包用的服务，所以需要使用**CTRL+ALT+T**新开一个终端。

输入命令
>`~/dev/appengine-java-sdk-1.9.3/bin/dev_appserver.sh   -p    8888    -a   0.0.0.0    ~/dev/appinventor-sources-master/appinventor/appengine/build/war/`

注意这是一条长命令，命令中含有空格，这条命令执行时会有很多输出，如果命令出错，先确认一下前面部分的路径和后面部分路径是不是存在。

如果最后输出如下信息
>>Server default is running at http://localhost:8888/

>>....

>>Dev App Server is now running


就表示服务正常启动了。可以使用浏览器在地址栏里输入http://localhost:8888/查看结果。出现登陆界面就OK啦。注意使用过程中这个黑屏终端要一直打开不能关哟。
![login.png](../../../images/login.png)

还有最后一步，启动打包构建服务器。切换到我们最开始打开的那个终端，运行命令
>ant RunLocalBuildServer

稍微等一下，有输出
> [java] 信息: Running at: http://127.0.1.1:9990/buildserver

>...

>[java] 信息: Server running
这样就代表构建服务器也正常运行了。

现在可以自己编写一个helloworl小程序，并试着打包安装到手机上了。

Have fun

***因为本博暂时还没前使用评论系统，如果您发现本文的错误，请给我邮件不甚感激。***











