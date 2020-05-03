&emsp;&emsp;最近需要将几张图片合成视频，找了一些软件没发现特别好用的，于是就想着自己用代码写呗，查了一下MATLAB和OpenCV，试了一下，发现MATLAB代码挺好写的，于是就写出来了以下代码。
```Matlab
clc; clear all;
writerObj = VideoWriter('peaks.avi');
open(writerObj);
for x = 1:1024
    imageFileName = sprintf('%04d.tif',x);
    image = uint8(imread(imageFileName)./256);
    writeVideo(writerObj,image);
end
close(writerObj);
```
&emsp;&emsp;这是最简单的一段图片合成视频的代码。
