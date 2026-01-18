---
title: OpenCV手册
published: 2026-01-18
tags: [Python, openCV]
category: Technology
author: minai
draft: false
---

# OpenCV基础

## 基本读写

|          函数          |             描述             |
| :--------------------: | :--------------------------: |
|      cv.imread()       |        从文件读取图像        |
|     cv.imdecode()      |     从内存缓冲区读取图像     |
|      cv.imshow()       |     在openCV窗口显示图像     |
|      cv.imwrite()      |        将图像写入文件        |
|      cv.waitKey()      | 等待按键输入, 参数为等待时间 |
| cv.destroyAllwindows() |         释放所有窗口         |

函数`imread()`用于读取图像文件, 其用法为: `cv.imread(filename[, flags])` 其中`flags`为读取模式, 取值如下:

- `cv.IMREAD_GRAYSCALE`: 将图像作为灰度图读取
- `cv.IMREAD_COLOR`: 默认值, 将图像作为BGR三通道彩色图像读取
- `cv.IMREAD_ANYDEPTH`: 当输入图像具有相应深度时返回 16 位/32 位图像，否则将其转换为 8 位图像
- `cv.IMREAD_UNCHANGED`: 载入原图(包括alpha通道)

更多参数可以查阅OpenCV官方文档

使用示例:

```py
import cv2 as cv
import sys
img = cv.imread("cirno.png", cv.IMREAD_COLOR) #注意:imread不支持中文路径
cv.imshow("hello opencv", img)
if img is None:
    sys.exit("无法读取图像")
cv.waitKey(0)
cv.destroyAllWindows()
```

使用`imdecode()`函数可以读取内存缓冲区中的图像, 配合`fromfile()`函数可完成中文路径的图像读取, 其用法为: `cv.imdecode(buf, flags)` 其中`buf`是存放图像数据的内存缓存, 通常用数组或字节向量表示. 如果内存缓冲区太短或者包含无效数据, 则返回空矩阵图像

使用示例:

```py
import cv2 as cv
import sys
import numpy as np
img = cv.imdecode(np.fromfile("琪露诺.png", dtype=np.uint8), cv.IMREAD_GRAYSCALE)
cv.imshow("hello opencv", img)
if img is None:
    sys.exit("无法读取图像")
cv.waitKey(0)
cv.destroyAllWindows()
```

 由于读取到的图像都是用矩阵表示的, 因此可以获取图像的长宽和通道数:

```py
img = cv.imread("cirno.png", cv.IMREAD_UNCHANGED)
height, width, channels = np.shape(img)[:3]
print("height=", height, "width=", width, "channels=", channels)
```

```
height= 640 width= 640 channels= 4
```



