---
title: Qt5.12.0+OpenCV4.0配置
date: 2019-04-03 11:34:52
tags:
	- Qt
	- OpenCV
categories: notes
icon: /images/notes.svg
abstract:  Qt5.12.0+OpenCV4.0配置，改使用MSVC编译器，操作过程简单。
---

前段时间，我弄我的毕设，需要把Qt配置上OpenCV库实现打开摄像头、录制视频的功能。然而以前完全没学过，在网上搜索资料都是要求把OpenCV用MinGW重新编译一下才能使用。看得我一脸懵逼，编译了一晚上也没能使用。但现在我已经解决了这个问题。

首先，我们之所以要将OpenCV用MinGW重新编译的原因是，我们写的Qt是用的MinGW编译器编译的，但OpenCV并不是这个编译器编译的，因此我们需要把它的二进制文件用MinGW编译过后才能调用。

但编译过程很复杂，报错都不知道该如何改。因此我决定把将Qt换个编译器，我换成了MSVC2017编译器。

先介绍一下我的配置：

- **操作系统：Windows10 64bit**
- **编译器：MSVC2017**
- **Qt版本：Qt5.12.0**
- **QtCreator版本：Qt Creator4.8.0**
- **OpenCV版本：OpenCV4.0**

---

# 安装OpenCV

- 去官网下载[OpenCV](http://opencv.org/downloads.html)，并解压到一个目录中，例如，我解压到了“C:\opencv”中。
- 配置环境变量，在**我的电脑——单击右键——属性——高级系统设置——环境变量——系统变量**中，找到Path，并添加"C:\opencv\build\x64\vc15\bin"。请注意上面路径中的x64\vc15是需要根据你系统的版本自行替换的。

# 下载MSVC2017编译器

## if(你还没安装Qt)

- 你在安装的时候，在选择需要下载的组件里把MSVC2017勾上，如图，其他组件按需勾选。

  ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic1.png?raw=true)

## else if(你已经安装了Qt)

- 找到你Qt安装的位置，例如我的，我的Qt就安装在“C:\Qt\Qt5.12.0”，列表里面有一个“**MaintenanceTool.exe**”，用这个就可以装你之前没有安装的组件。其中因为官网的下载速度过慢，我们需要添加国内的镜像来下载组件。如图：

    ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic2.png?raw=true)

- 详细过程：**打开——设置——储存库——添加——输入存储库——测试——测试成功——退出设置——下一步——跳过用户名填写——添加组件——勾选MSVC2017或MSVC2015**。如图：

    ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic3.png?raw=true)

    ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic4.png?raw=true)

    ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic5.png?raw=true)

    ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic6.png?raw=true)

- 存储库可以从这里选择[http://download.qt.io/static/mirrorlist/](http://download.qt.io/static/mirrorlist/),找“China”的镜像，点击其中一个镜像的HTTP，以此进入如下路径：/online/qtsdkrepository/windows_x86/root/qt/ 。再把该链接，复制粘贴到添加的静态库即可。我写的是这样的：http://mirrors.ustc.edu.cn/qtproject/online/qtsdkrepository/windows_x86/root/qt/。如图：

  ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic7.png?raw=true)

- **正常来说这个方法是OK的，因为我同学这样做完全可以成功，但我却出现了如下问题**：

    ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic8.png?raw=true)

    - **解决方法**：
      - **下载Updates.xml**， 在中国任意的一个仓库里，例如：http://mirrors.ustc.edu.cn/qtproject/online/qtsdkrepository/windows_x86/root/qt/，下载[Updates.xml](http://mirror.bit.edu.cn/qtproject/online/qtsdkrepository/windows_x86/root/qt/Updates.xml)，如图：
        ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic9.png?raw=true)

      - **下载Meta文件**，例如：http://mirror.bit.edu.cn/qtproject/online/qtsdkrepository/windows_x86/root/qt/qt/，下载最新的Meta文件，如下图的[1.0.7meta.7z](http://mirror.bit.edu.cn/qtproject/online/qtsdkrepository/windows_x86/root/qt/qt/1.0.7meta.7z)，如图：

        ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic10.png?raw=true)

      - 把他俩都放在**一个文件夹**里，例如：

        ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic11.png?raw=true)

      - 然后回到刚刚**设置**那里，**添加路径**，如图：

        ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic12.png?raw=true)

      - **测试成功即可正常添加组件**，如图：

        ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic13.png?raw=true)

# 配置
- **用Qt Creator，打开你的程序，在你的程序名上单击右键选择“添加库”——打开对话框选“外部库”——下一步——先选好"包含路径"，就是你OpenCV所解压路径的include，例如我的是“C:\opencv\opencv\build\include”——然后“库文件”，我的路径是“C:\opencv\opencv\build\x64\vc15\lib\opencv_world400.lib”——下一步——完成**，如图：

  ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic14.png?raw=true)

  ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic15.png?raw=true)

  ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic16.png?raw=true)

  ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic17.png?raw=true)

- 然后**设置编译器**，如图：

  ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic19.png?raw=true)

- 最后“**在你的程序名上单击右键——执行qmake**”，如图。一般正常基本就成功了。如果你想快速验证一下自己有没有成功配置，你可以试试下面实例代码。

  ![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/qt_opencv_study_pic18.png?raw=true)

# 测试

```c++
#include <iostream>
#include <opencv2/opencv.hpp>
 
using namespace std;
using namespace cv;
 
int main(int argc, char *argv[])
{
     Mat image = imread("D:\\test.jpg");
     cvNamedWindow("window", CV_WINDOW_NORMAL);
     imshow("window",image);
     waitKey(0);
 }
```

点击运行之后，如果没有任何错误，可以显示图片，那么环境安装成功。

# 最后

上述操作，都为亲身经历，如果不可用，可以在评论区评论，我们一起交流解决~

---

如需转载，请说明出处，谢谢！