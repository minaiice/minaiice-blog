---
title: OpenCV手册
published: 2026-01-18
updated: 2026-04-08 23:45:00
description: OpenCV基础
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

函数`imread()`用于读取图像文件, 其用法为: `cv.imread(filename[, flags])`

其中`flags`为读取模式, 取值如下:

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

使用`imdecode()`函数可以读取内存缓冲区中的图像, 配合`fromfile()`函数可完成中文路径的图像读取, 其用法为: `cv.imdecode(buf, flags)`

其中`buf`是存放图像数据的内存缓存, 通常用数组或字节向量表示. 如果内存缓冲区太短或者包含无效数据, 则返回空矩阵图像

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

使用`imwrite()`可以输出图像到文件, 用法为: `cv.imwrite(filename, img[, params])`

其中`params`表示特定格式保存的参数, 例如:

- `cv.IMWRITE_PNG_COMPRESSION`: 对于PNG格式保存, 压缩级别可在 0 到 9 之间. 值越高, 文件大小越小, 压缩时间越长, 默认值为1(最佳速度设置)
- `cv.IMWRITE_JPEG_QUALITY`: 对应JPEG格式保存, 质量范围在0~100, 默认值为95

更多参数可以查阅OpenCV官方文档

使用示例:

```py
#把png图片右上角改为半透明, 并输出
import cv2 as cv

img = cv.imread("cirno.png", cv.IMREAD_UNCHANGED)
b, g, r, a = cv.split(img)
print(a.shape)
a[:int(a.shape[0]/2), int(a.shape[1]/2):] = 127
img = cv.merge((b, g, r, a))
cv.imshow("img", img)
cv.waitKey(0)
cv.destroyAllWindows()
cv.imwrite("cirno1.png", img, [cv.IMWRITE_PNG_COMPRESSION, 9]) #opencv的查看窗口无法看到透明效果, 必须保存后查看
```



## 基础图像处理

### 颜色空间转换

通过`cvtColor()`函数进行颜色空间的转换, 可以将图片转换成灰度图, 二值图, HSV以及HSI等不同颜色空间, 其用法为: `cv.cvtColor(src, code[, dst[, dstCn]])` 

其中`code`表示颜色空间转换参数, `dst`表示输出与`src`相同大小和深度的图像, `dstCn`表示目标图像通道数, 其默认值为0, 即根据源图像和目标图像自动确定

```py
#将图像分别转换为灰度图和HSV图像
src_img = cv.imread("cirno.png", cv.IMREAD_UNCHANGED)
gray_img = cv.cvtColor(src_img,cv.COLOR_BGR2GRAY)
hsv_img = cv.cvtColor(src_img,cv.COLOR_BGR2HSV)
h, s, v = cv.split(hsv_img)
cv.imshow("gray_img", gray_img)
cv.imshow("h", h) #色调
cv.imshow("s", s) #饱和度
cv.imshow("v", v) #灰度
#由于opencv内部窗口只能显示BGR格式的图像, 所以不能正常显示HSV图像
cv.waitKey(0)
cv.destroyAllWindows()
```



### 基本图形绘制

|      函数      |      描述      |
| :------------: | :------------: |
| cv.rectangle() |    绘制矩形    |
|  cv.circle()   |    绘制圆形    |
|  cv.ellipse()  |    绘制椭圆    |
|   cv.line()    |    绘制线段    |
| cv.polylines() |   绘制多边形   |
| cv.fillPoly()  | 绘制填充多边形 |

更多绘制函数可以查看OpenCV官方文档

- 使用`rectangle()`可以绘制矩形, 其用法为

`cv.rectangle(img, pt1, pt2, color, thickness=1, lineType=LINE_8, shift=0)`

或者

`cv.rectangle(img, rec, color, thickness=1, lineType=LINE_8, shift=0)` 

其中`pt1`和`pt2`表示对角线的两个点坐标元组, `rec`表示左上角坐标和宽高组成的元组, `color`表示矩形的颜色或亮度(灰度图像), `thickness`表示线条粗细, 取负值时(如`cv.FILLED`)表示绘制填充矩形, `lineType`表示线条类型, `shift`表示点坐标的移位位数

- 使用`circle()`可以绘制圆形, 其用法为

`cv.circle(img, center, radius, color, thickness=1, lineType=LINE_8, shift=0)`

其中`center`表示圆心坐标, `radius`表示半径

- 使用`ellipse()`绘制椭圆, 其用法为

`cv.ellipse(img, center, axes, angle, start_angle, end_angle, color, thickness=1, lineType=LINE_8, shift=0)`

其中`axes`表示轴长, `angle`表示偏转角, `start_angle`和`end_angle`分别表示圆弧起始位置角度和终止位置角度

使用示例:

```py
#在图像左上角分别绘制一个矩形, 圆形, 椭圆
src_img = cv.imread("cirno.png", cv.IMREAD_UNCHANGED)
rect_img = src_img.copy()
cv.rectangle(rect_img, (30, 40), (130, 180), (255, 204, 102, 255), 2)
cir_img = src_img.copy()
cv.circle(cir_img, (150, 150), 80, (255, 204, 102, 255), 2)
ellipse_img = src_img.copy()
cv.ellipse(ellipse_img, (150, 150), (80, 40), -60, 0, 360, (255, 204, 102, 255), 2) #-60表示逆时针旋转椭圆
```

输出图像:

<div style="display:flex;"> <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/20260121212801224.png" width="32%"/> <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/20260121215049571.jpg" width="32%"/><img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/20260121220810666.jpg" width="32%"/> </div> 

- 使用polylines()可以绘制多边形, 其用法为

`cv.polylines(img, pts, isClosed, color, thickness=1, lineType=LINE_8, shift=0)`

其中`pts`表示包含多边形顶点的数组, `isClosed`表示绘制的多边形线段是否闭合

- 使用`fillPoly()`可以绘制填充多边形, 其用法为

`cv.fillPoly(img, pts, color, thickness=1, lineType=LINE_8, shift=0[, offset])`

其中`offset`表示轮廓上所有点的偏移量, 方便绘制多个位置不同的相同多边形

使用示例:

```py
src_img = cv.imread("cirno.png", cv.IMREAD_UNCHANGED)
poly_img = src_img.copy()
pts = np.array([(50, 40), (150, 60), (60, 180)], np.int32) #多边形点集, 数据类型一定要是int32
cv.polylines(poly_img, [pts], True, (255, 204, 102, 255), 2)
fillpoly_img = src_img.copy()
cv.fillPoly(fillpoly_img, [pts], (255, 204, 102), offset=(80, 80)) #绘制填充多边形, 并且向右下方位移(80, 80)
```

输出图像:

<div style="display:flex;"> <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/20260121231103185.jpg" width="48%"/> <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/20260121231103186.jpg" width="48%"/> </div> 



### 打印文字

使用`putText()`函数可以在图片上打印文字(注意, 该函数无法识别中文, 因此不能打印中文) 其用法为

`cv.putText(img, text, org, fontFace, fontScale, color, thickness=1, lineType=LINE_8, bottomLeftOrigin=false)`

其中`text`表示要绘制的文本字符串, `org`表示文本字符串的锚点坐标, `fontFace`字体类型，参见 [HersheyFonts](https://docs.opencv.ac.cn/4.12.0/d6/d6e/group__imgproc__draw.html#ga0f9314ea6e35f99bb23f29567fc16e11)。`fontScale`字体缩放因子, 负数时会使文本镜像或者反转, `bottomLeftOrigin`默认为`false`, 即以左上角为锚点, 反之在左下角(但是我自己用的时候实际效果是字体颠倒了)

可以通过函数`getTextSize()`来确定文本框大小和基线偏移量(即文本框与基线的垂直距离), 方便绘制, 用法为

`cv.getTextSize(text, fontFace, fontScale, thickness)`

使用示例:

```py
src_img = cv.imread("cirno.png", cv.IMREAD_UNCHANGED)
text_img = src_img.copy()
cv.putText(text_img, "Minaiice", (80, 80), cv.FONT_HERSHEY_COMPLEX, 2.0, (255, 204, 102, 255), 2)
cv.imshow("text", text_img)
```

输出图像:

![text](OpenCV手册/text.jpg)

### 添加边框

使用`copyMakeBorder()`可以为图像设置边界, 即在图像边沿进行额外填充, 用法如下

`cv.copyMakeBorder(src, top, bottom, left, right, borderType[, dst[, value]])`

其中`top`, `bottom`, `left`, `right`表示在原图像四个方向填充的像素量; `borderType`表示边界类型, 取值如下:

- `BORDER_REPLICATE`: 复制法, 复制最边缘像素, 填充扩充的边界, 中值滤波就采取这种方法
- `BORDER_REFLECT_101`: 对称法, 以最边缘像素为轴, 对称填充, 这是高斯滤波边界处理的默认方法
- `BORDER_CONSTANT`: 常量法, 以一个常量像素值(`value`)填充扩充的边界, 在仿射变换和透视变换中常见
- `BORDER_REFLECT`: 与对称法原理一致, 不过最边缘像素也要对称过去
- `BORDER_WRAP`: 用另一侧元素来填充这一侧的扩充边界

图片示例:

<div style="display:flex; gap: 10px;">
  <div style="text-align:center; width:46%;">
    <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/replicate.jpg" style="width:100%;">
    <div>replicate</div>
  </div>

  <div style="text-align:center; width:46%;">
    <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/reflect.jpg" style="width:100%;">
    <div>reflect</div>
  </div>
</div>

<div style="display:flex; gap: 10px;">
  <div style="text-align:center; width:46%;">
    <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/reflect101.jpg" style="width:100%;">
    <div>reflect101</div>
  </div>

  <div style="text-align:center; width:46%;">
    <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/wrap.jpg" style="width:100%;">
    <div>wrap</div>
  </div>
  <div style="text-align:center; width:46%;">
    <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/constant.jpg" style="width:100%;">
    <div>constant</div>
  </div>




### 查找轮廓

使用`findContours()`可以检测物体轮廓, 用法如下

`cv.findContours(image, mode, method[, contours[, hierarchy[, offset]]])`

其中`image`表示原始图像, 必须是8位单通道二值图像, 参数`mode`决定轮廓提取方式, 具体有如下四种:

- `PETR_EXTERNAL`: 只检测外轮廓
- `PETR_LIST`: 对检测到的轮廓不建立等级关系
- `PETR_CCOMP`: 检索所有轮廓并将它们组织成两级层级结构. 上面一层为外边界, 下面一层为内孔边界. 如果内孔内还有一个连通物体, 那么这个物体的边界仍然位于顶层
- `PETR_TREE`: 建立一个等级树结构的轮廓

参数method决定表达轮廓的方式, 可选值如下：

- `CHAIN_APPROX_NONE`: 存储所有轮廓点, 相邻两个点的像素位置差不超过1
- `CHAIN_APPROX_SIMPLE`: 压缩水平方向, 垂直方向, 对角线方向的元素, 只保留该方向的终点坐标. 例如一个矩形只需要四个点保存轮廓信息

参数`contours`是一个向量, 并且是一个双重向量, 向量内每个元素保存了一组由连续的`Point`构成的点的集合向量, 每一组`Point` 点集就是一个轮廓, 有多少轮廓, 向量`contours`就有多少元素

参数`hierarchy`也是一个向量, 向量内每个元素保存了一个包含4个int型的数组. `hierarchy`向量和轮廓向量`contours`内元素一一对应, 容量相同. `hierarchy`向量每一个元素的4个int型变量(`hierarchy[i][0] - hierarchy[i][3]`)分别对应后一个轮廓, 前一个轮廓, 父轮廓和子轮廓的索引编号. (若没有对应轮廓编号为`-1`)

参数`offset`是每个轮廓点移动的可选偏移量

使用`drawCountours()`可以绘制轮廓, 用法如下

`cv.drawCountours(image, contours, contourIdx, color[, thickness[, lineType[, hierarchy[, maxLevel[, offset]]]]])`

参数`contours`表示输入的轮廓组, 每一组轮廓由点`vector`构成; `contourIdx`指明画第几个轮廓, 若为负值则画出所有轮廓; `thickness`为线宽, 若为负值或`CV_FILLED`则表示填充内部; `lineType`为线型; `hierarchy`为轮廓结构信息; `maxLevel`为绘制轮廓的最高级别(只在`hierarchy`有效时生效, 值为`0`时绘制与输入轮廓同等级的所有轮廓`即相邻轮廓`), 值为`1`时绘制同一等级轮廓和其子节点, 值为`2`时绘制与输入轮廓所有同级轮廓, 及其子节点, 和子节点的子节点; `offset`表示可选的轮廓偏移参数

使用示例:

```python
scale_percent = 30

src_img = cv.imread("columbina_2.jpg", cv.IMREAD_UNCHANGED)
width = int(src_img.shape[1] * scale_percent / 100)
height = int(src_img.shape[0] * scale_percent / 100)
dim = (width, height)
resized = cv.resize(src_img, dim, interpolation=cv.INTER_AREA)
gray = cv.cvtColor(resized, cv.COLOR_BGR2GRAY)
cv.imshow("gray", gray), cv.waitKey(0), cv.destroyAllWindows()
_, binary = cv.threshold(gray, 0, 255, cv.THRESH_BINARY_INV + cv.THRESH_OTSU) #反转黑白图, 因为轮廓检测白色物体
cv.imshow("binary", binary), cv.waitKey(0), cv.destroyAllWindows()
contours, hierarchy = cv.findContours(binary, cv.RETR_EXTERNAL, cv.CHAIN_APPROX_NONE)
cv.imshow("binary2", binary), cv.waitKey(0), cv.destroyAllWindows() #查找轮廓会修改原图像
cv.drawContours(resized, contours, -1, (200, 155, 255), 3)
cv.imshow("contours", resized), cv.waitKey(0), cv.destroyAllWindows()
```

输出图像:
<div style="display:flex; gap: 10px;">
  <div style="text-align:center; width:46%;">
    <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/gray.jpg" style="width:100%;">
    <div>gray</div>
  </div>

  <div style="text-align:center; width:46%;">
    <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/binary.jpg" style="width:100%;">
    <div>binary</div>
  </div>
</div>

<div style="display:flex; gap: 10px;">
  <div style="text-align:center; width:46%;">
    <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/binary2.jpg" style="width:100%;">
    <div>binary2</div>
  </div>

  <div style="text-align:center; width:46%;">
    <img src="https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/contours.jpg" style="width:100%;">
    <div>countours</div>
  </div>
  </div>













