---
title: stm32标准库项目构建
published: 2025-11-02
description: 解决stm32标准库配置问题
tags: [stm32]
category: Technology
author: minai
draft: false
---

# 新建一个项目

## keil5内配置步骤

### 基本配置

![image-20251102200405656](stm32标准库项目构建/image-20251102200405656.png)

然后在工程文件里面分别新建`Start`, `Library`和`User`三个文件夹(名称不做要求), 复制固件库里面的文件到工程文件夹:

![image-20251102195857838](stm32标准库项目构建/image-20251102195857838.png)

### 在`Start`文件夹添加工程文件

首先复制在`...\STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x\startup\arm`内的`startup`文件, 具体复制哪一个根据你的芯片型号选择, 我这里选择后缀为`_md`的文件: 

![image-20251102200937015](stm32标准库项目构建/image-20251102200937015.png)

接下来复制在`...\STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x`内的三个文件, 分别对应着一个外设寄存器描述文件, 两个时钟配置文件: 

![image-20251102201140082](stm32标准库项目构建/image-20251102201140082.png)

最后复制在`...\STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\CoreSupport`下的文件, 也就是内核寄存器描述文件: 

![image-20251102201610944](stm32标准库项目构建/image-20251102201610944.png)

把上述文件粘贴至刚刚创建的`Start`文件夹中: 

![image-20251102201828845](stm32标准库项目构建/image-20251102201828845.png)

### 在`Library`文件夹添加工程文件

复制`...\STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\STM32F10x_StdPeriph_Driver\inc`下的所有文件: 

![image-20251102202256225](stm32标准库项目构建/image-20251102202256225.png)

复制`...\STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\STM32F10x_StdPeriph_Driver\src`下的所有文件: 

![image-20251102202324419](stm32标准库项目构建/image-20251102202324419.png)

把上述文件粘贴至刚刚创建的`Library`文件夹中.

### 在`User`文件夹添加工程文件

复制`...\STM32F10x_StdPeriph_Lib_V3.6.0\Project\STM32F10x_StdPeriph_Template`下的这三个文件, 分别用来配置库函数头文件的包含关系和中断处理函数:

![image-20251102202541838](stm32标准库项目构建/image-20251102202541838.png)

把上述文件粘贴至刚刚创建的`User`文件夹中.

### keil5中引入文件

回到keil5, 创建三个Group, 名字分别对应: 

![image-20251102203108146](stm32标准库项目构建/image-20251102203108146.png)

双击每一个文件夹, 将刚刚复制的文件全部添加进去: 

![image-20251102203345789](stm32标准库项目构建/image-20251102203345789.png)

![image-20251102203409106](stm32标准库项目构建/image-20251102203409106.png)

![image-20251102203434748](stm32标准库项目构建/image-20251102203434748.png)

然后点击`魔术棒->C/C++->Include Paths`, 把刚刚三个文件夹全部include一下: 

![image-20251102203629137](stm32标准库项目构建/image-20251102203629137.png)

如果需要在keil5里面编译和调试, 首先在魔术棒选项把编译器改成AC5, 在`Debug`选项选择你的烧录器, 我这里是`ST-Link`, 在`Settings->Flash Download`里勾选Reset and Run, 在`Pack`里取消勾选Enable即可.

在`C/C++`选项添加宏定义`USE_STDPERIPH_DRIVER`: 

![image-20251102204951905](stm32标准库项目构建/image-20251102204951905.png)

我们这里右键User文件夹, 添加新的.c文件作为main文件, 并且include一下`stm32f10x.h`文件, 注意最后一行留空, 不然会警告: 

![image-20251102204700740](stm32标准库项目构建/image-20251102204700740.png)

![image-20251102205216584](stm32标准库项目构建/image-20251102205216584.png)

我们最后编译并烧录, 没有问题: 

![image-20251102205310635](stm32标准库项目构建/image-20251102205310635.png)

## VSCode配置步骤

具体要下载的插件和前期配置参考这个博客[Vscode开发STM32标准库-CSDN博客](https://blog.csdn.net/qq_44764442/article/details/147399888)

打开VSCode, 选择左侧的EDIE图标, 导入刚刚的项目, 依次选择`MDK->ARM`:

![image-20251102210214047](stm32标准库项目构建/image-20251102210214047.png)

在`芯片支持包->From Disk`添加所需的支持包: 

![image-20251102210519831](stm32标准库项目构建/image-20251102210519831.png)

修改构建器选项, `构建配置->构建器选项->全局选项`, 勾选Use MicroLIB, 在`链接器`取消勾选不生成Hex/Bin/S19等二进制文件: 

![image-20251102211156498](stm32标准库项目构建/image-20251102211156498.png)



![image-20251102211200000](stm32标准库项目构建/image-20251102211200000.png)

`烧录配置`我这里使用ST-Link, 所以选择OpenOCD: 

![image-20251102211312502](stm32标准库项目构建/image-20251102211312502.png)

最后在`C/C++属性->预处理器定义`里添加一个宏定义`STM32F10X_MD`: 

![image-20251102211509290](stm32标准库项目构建/image-20251102211509290.png)

最后, 编译, 烧录, 大功告成: 

![image-20251102211639772](stm32标准库项目构建/image-20251102211639772.png)







