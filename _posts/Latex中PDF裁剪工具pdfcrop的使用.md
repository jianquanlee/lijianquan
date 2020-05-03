&emsp;&emsp;很多时候，我们在WORD或者是PPT里画完图之后，需要转换为矢量图，转换为矢量图有很多种方式，有一种方式就是转换为PDF文件，转换为PDF有一个问题在于，打印出的文件大小是A4纸大小，边上的空白处仍然留存在PDF文件内，如果在LaTex中直接导入未进行裁边的PDF，图片就变成了外圈包着空白的图片了。所以这个时候我们就需要将PDF文件中图片边上的白色裁剪掉。

&emsp;&emsp;裁边的工具有很多，不过既然我们在用LaTex，就直接用它自带的工具吧，pdfcrop，挺好用的，这个工具在LaTex的安装目录下，搜索一下就可以。

&emsp;&emsp;我们将pdfcrop.exe复制出来，将我们要裁剪的pdf文件与pdfcrop.exe放在同一个文件夹下，然后打开cmd命令行，通过cd进入所在的文件夹。有一种简单的方式可以直接在当前的文件夹路径打开cmd命令行。在当前文件夹空白处，按下Shift键的同时右击鼠标。点击在此处打开命令窗口就可以。
![](https://img-blog.csdnimg.cn/20200503150309770.png#pic_center)
&emsp;&emsp;pdfcrop的语法很简单：pdfcrop input.pdf output.pdf。

&emsp;&emsp;input.pdf和output.pdf是输入和输出的pdf文件名，改为自己的文件名即可。点击回车之后，output.pdf就会出现在当前文件夹下。

&emsp;&emsp;以下是测试输入与输出结果：
![测试输入](https://img-blog.csdnimg.cn/20200503150906282.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200503151521200.png#pic_center)
