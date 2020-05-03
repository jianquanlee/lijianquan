&emsp;&emsp;最近在学习HLS语言，所以就自己摸索尝试了用HLS实现了图像二值化，把这个内容总结一下，分享出来。
  
&emsp;&emsp;首先打开HLS，然后新建一个Project，之后再在Source栏点击右键，选择New File...，创建名为pixelBinary.cpp和pixelBinary.h这两个文件。

&emsp;&emsp;这个是pixelBinary.cpp文件的内容：
```cpp
#include "pixelBinary.h"
#include <stdio.h>
#include <iostream>

using namespace std;

void hls::pixelBinary(GRAY_IMAGE &src, GRAY_IMAGE &dst)
{
	uchar pixelValue;

	GRAY_PIXEL src_data;
	GRAY_PIXEL dst_data;

	LOOP_ROW:
	for(int idxRow = 0; idxRow < IMG_HEIGHT; idxRow++)
	{
		LOOP_COL:
		for(int idxCol = 0; idxCol < IMG_WIDTH; idxCol++)
		{
			#pragma HLS PIPELINE II=1

			src >> src_data;
			pixelValue = src_data.val[0];
			dst_data.val[0] = pixelValue > 128 ? 255 : 0;
			dst << dst_data;
		}
	}
}

void hlsMain(AXI_STREAM& src_axi, AXI_STREAM& dst_axi)
{
	#pragma HLS INTERFACE axis port=src_axi bundle=INPUT_STREAM
	#pragma HLS INTERFACE axis port=dst_axi bundle=OUTPUT_STREAM

	GRAY_IMAGE img_src;
	GRAY_IMAGE img_dst;

	#pragma HLS dataflow
	hls::AXIvideo2Mat(src_axi,img_src);
	hls::pixelBinary(img_src,img_dst);
	hls::Mat2AXIvideo(img_dst,dst_axi);
}
```
&emsp;&emsp;这个是pixelBinary.h文件的内容：
```cpp
#ifndef _PIXELBINARY_H_
#define _PIXELBINARY_H_

#include "hls_video.h"
#include "hls_math.h"
#include "ap_int.h"
#include "ap_fixed.h"

// maximum image size
#define IMG_WIDTH 184
#define IMG_HEIGHT 273

// I/O Image Settings
#define INPUT_IMAGE "input_image.png"
#define OUTPUT_IMAGE "output_image.png"

// typedef video library core structures
typedef unsigned char uchar;
typedef hls::stream<ap_axiu<8,1,1,1> > AXI_STREAM;
typedef hls::Mat<IMG_HEIGHT, IMG_WIDTH, HLS_8UC1> GRAY_IMAGE;
typedef hls::Scalar<1, uchar> GRAY_PIXEL;

// typedef HLS namespace
namespace hls
{
  void pixelBinary(GRAY_IMAGE &src, GRAY_IMAGE &dst);
}

//top level function for HW synthesis
void hlsMain(AXI_STREAM& src_axi, AXI_STREAM& dst_axi);

#endif
```
&emsp;&emsp;有了这两个文件之后，就可以进行综合了，这里首先要进行顶层函数定义，Project -> Project Settings -> Synthesis -> 在Top Function那里选择hlsMain，点击OK进行确定。然后点击HLS界面的C Synthesis，如下是综合完的结果。
![HLS综合之后的结果](https://img-blog.csdnimg.cn/20200327201007516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmVfbGpx,size_16,color_FFFFFF,t_70)
&emsp;&emsp;从综合结果可知，pixelBinary的Latency和Interval的值是一样的，都是50235，我们的图片尺寸是184*273 = 50232，Latency和Interval的值比图片像素数多3，关于Latency和Interval具体的含义，我还是没有很明白，这两个值到底是怎么来进行计算的，等弄明白了再来分享。

&emsp;&emsp;这是一个综合的结果，也就是把我们的HLS代码综合成了Verilog或者VHDL代码了，你再solution下面的syn文件夹下就可以看到生成的Verilog或者VHDL代码了。

&emsp;&emsp;然后接下去是仿真，在TestBench下新建文件，文件名为testbench.cpp，代码为：

```cpp
#include "iostream"
#include "hls_opencv.h"
#include "pixelBinary.h"

using namespace std;
using namespace cv;

int main()
{
	//获取图像数据
	IplImage* src = cvLoadImage(INPUT_IMAGE,CV_LOAD_IMAGE_GRAYSCALE);
	//获取仿真图片并直接转为灰度图像
	IplImage* dst = cvCreateImage(cvGetSize(src), src->depth, src->nChannels);

	AXI_STREAM src_axi, dst_axi;
	IplImage2AXIvideo(src, src_axi);
	hlsMain(src_axi, dst_axi);
	AXIvideo2IplImage(dst_axi, dst);
	cvSaveImage(OUTPUT_IMAGE,dst);

	//释放内存
	cvReleaseImage(&src);
	cvReleaseImage(&dst);
	return 0;
}
```
&emsp;&emsp;这个仿真代码的意思就是读取图像，然后进行处理，并将处理完的结果输出，为了能够仿真，需要在TestBench下添加图像文件，图像文件名为input_image.png，最后会输出output_image.png这么一个图像。如下分别为输入图像数据和输出图像数据，最终生成的图像数据在 .\solution_pixelBinary\sim\wrapc文件夹下

![输入图像数据](https://img-blog.csdnimg.cn/20200327202837489.png#pic_center)        ![输出图像数据](https://img-blog.csdnimg.cn/2020032720322133.png#pic_center)
&emsp;&emsp;但其实呢，我觉得这样的testbench并不好，看过Example里的TestBench，写法都是Software的结果和Hardware的结果进行对比，如果能对上，就说明写的代码没问题，不过图像处理的话，直接看处理后的图像差不多也行。
