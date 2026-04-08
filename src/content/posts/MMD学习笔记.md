---
title: MMD学习笔记(blender插件)
published: 2026-03-10
updated: 2026-03-20
description: 学习在blender上的MMD插件
tags: [Blender, MMD]
category: Hobby
author: minai
draft: false
---



# Blender

教程参考B站@miaobox

## 常见问题

- 模型贴图缺失

![image-20260319225430162](MMD学习笔记/image-20260319225430162.png)

解决方案: 选择模型 -> 转换给Blender, 这个操作会清理掉toon贴图:

![image-20260319225608906](MMD学习笔记/image-20260319225608906.png)

然后自行修改剩余有问题的材质: 

![image-20260319225827947](MMD学习笔记/image-20260319225827947.png)

![image-20260319225952948](MMD学习笔记/image-20260319225952948.png)

![image-20260319230211116](MMD学习笔记/image-20260319230211116.png)

## 借助MMDBridge烘焙物理

个人体验来说Blender的mmdtools自带的物理烘焙效果很差，总是穿模，于是听从建议使用MMDBridge来烘焙物理，效果好多了，以下是具体步骤:

步骤图附上:

![](https://cdn.jsdelivr.net/gh/minaiice/minai-image-bed/pictures/20260402211917256.png)

- 导入模型就不说了, 重点是在Blender里面修复骨骼和修复变形, 然后将修复的模型导出
- 重新打开Blender, 导入修复好的模型, 创建预设(渲染, 材质等), 打包成Blender文件后, 另存到工程地址
- 进入MMD, 导入模型和动作, 修改动作防止穿模, 其他同图
- 一定要选择装配变形!!!









