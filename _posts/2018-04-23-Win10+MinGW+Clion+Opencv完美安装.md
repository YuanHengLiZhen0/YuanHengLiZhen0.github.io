
---
layout:     post
title:      2018-04-23-Win10+MinGW+Clion+Opencv完美安装
subtitle:   
date:       2018-04-23
author:     zwht
header-img: img/post-bg-BJJ.jpg
catalog: true
tags:
    - OpenCv
    - java
---



# Win10+MinGW+Clion+Opencv完美安装

@(OpenCv)[Clion|编译器|配置]



  -  最近由于图像识别项目需要装OpenCV，平时都是用java开发。习惯了www.jetbrains.com的intellij idea。因此这次也选择该公司的Clion。毕竟学生党来说VS还是太巨量，不太方便。
 


-------------------

 一,*[Clion 安装，配置](#Clion)
 二,*[MinGW](#MinGW)
 三,*[OpenCv](#OpenCv)
 四,*[CMake](#CMake)
 五i,*[测试](#测试)

### Clion 

> 参考  https://blog.csdn.net/qq_38013968/article/details/70660349。注册并激活成功    

### MinGW
下载地址如下:(安装完成将bin目录放进环境变量)
>http://kent.dl.sourceforge.net/project/mingw-w64/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/6.3.0/threads-posix/seh/x86_64-6.3.0-release-win32-seh-rt_v5-rev1.7z

![Alt text](/img/1524538333158.png)







###  OpenCv
> https://www.opencv.org/releases.html

###  CMake 
> https://cmake.org (下载地址)
>  打开相应目录  C:\Program Files\CMake\bin中 cmake-gui.exe

![Alt text](/img/1524538516841.png)










    文件夹分别选择opencv目录下的sources和build目录 ，然后configure  。完成之后再generate;
    完成之后打开CMD窗口 定位到 opencv/build/目录下  执行命令  mingw32-make;


![Alt text](/img/1524538574217.png)











如果报以下错误：'thread' is not a member of 'std

![Alt text](/img/1524538673863.png)









       If you are compiling this on windows, you will need Mingw-Builds v4.8.1 with posix-threads: sourceforge.net/projects/mingwbuilds/files/host-windows/… You can choose between sjlj and seh. Seh is only x64 and sjlj is both x32 and x64.


就是说mingw版本不对，我在这里纠结了半天，终于找到一个老外的回答。赶紧回头重新下载了一个 。一次通过 。

![Alt text](/img/1524538751092.png)










### 测试 
好了 建个项目试试，打开Clion  File-settings  配置MinGW。

新建项目，再cmakelists.txt 添加如下代码：

代码块
``` c++
#include<opencv2/opencv.hpp>  
using namespace std;  
using namespace cv;  
  
  
  
int main() {  
    Mat a=imread("../res/board.jpg",CV_8UC4);  
    namedWindow("test",WINDOW_AUTOSIZE);  
    imshow("test",a);  
    waitKey(0);  
    return 0;  
  
  
}  
```

效果图如下:
![Alt text](/img/1524538919028.png)




祝大家都能一次成功
