---
layout: post
title: 把OpenCV Mat当一个普通数组来用
date: 2019-10-04
Author: Jianquan Li
tags: [OpenCV, Mat]
comments: false
---

&emsp;&emsp;最近在移植MATLAB图像处理算法，要将这个算法移植到OpenCV当中去，因为MATLAB对于数组的操作非常简单，而C++里面的数组就没那么好用，于是就想到了要用OpenCV的Mat。

&emsp;&emsp;在网上，关于OpenCV，关于Mat的文章，代码都非常多，但是都是相对简单的应用，当做一个图像数据来用，用到的格式也基本都是CV_8U和CV_8UC3类型。

&emsp;&emsp;`CV_8UC`就是创建单通道的图像，在读取图像的时候就用`image.at<uchar>(i,j)`来进行像素值读取。`CV_8UC3`就是创建三通道图像，读取图像的时候就用`image.at<Vec3b>(i,j)[k]`来进行读取。除了这两种形式，基本上就有了。但确实，Mat类型还有很多类型和应用。

&emsp;&emsp;首先来讲一下如何创建一个三维的矩阵，如果此刻我要创建一个6×6×6的矩阵，那就应该`Mat MatArray(6,6,CV_8UC(6)`，稍微解释一下这个类型，8是8 bits的意思，就是char型，然后这里的U是unsigned的意思，看这个类型的样子跟CV_8UC3很像，那为什么这里要把6用括号括起来呢。我们转到CV_8UC3的定义去看一下。
```C++
#define CV_8UC1 CV_MAKETYPE(CV_8U,1)
#define CV_8UC2 CV_MAKETYPE(CV_8U,2)
#define CV_8UC3 CV_MAKETYPE(CV_8U,3)
#define CV_8UC4 CV_MAKETYPE(CV_8U,4)
#define CV_8UC(n) CV_MAKETYPE(CV_8U,(n))
```
&emsp;&emsp;我们在types_c.h头文件中看到了这个定义，从这个定义里面我们可以看到OpenCV将常用的一些类型进行了定义，其余的进行另一类定义。我们读取CV_8UC3的时候是`image.at<Vec3b>(i,j)[k]`这样来进行读取的，那`CV_8UC(6)`这个是怎么读取的呢，那我们转到`<Vec3b>`的定义看一下。
```C++
typedef Vec<uchar, 2> Vec2b;
typedef Vec<uchar, 3> Vec3b;
typedef Vec<uchar, 4> Vec4b;
```
&emsp;&emsp;在core.hpp头文件中，有以上的定义，同样，我们平常用的`<Vec3b>`是`Vec<uchar,3>`的一个缩写，那我们要访问`CV_8UC(6)`的话，就直接用`Vec<uchar,6>`这样的形式去访问就行了，也就是`image.at<Vec<uchar,6>>(i,j)[k]`。

&emsp;&emsp;上面讲到的都是uchar类型的，只能是8 bits的无符号整形，那我要用int型，float型或者有符号型的呢，那这里就需要用到`CV_32FC`,`CV_64SC`之类的类型。相关的定义在OpenCV的types_c.h和core.hpp中都有。
```C++
Mat MatArray30x30; 
MatArray30x30.create(30,30,CV_32SC(81));
```
&emsp;&emsp;上面是创建一个30×30的81维向量。
