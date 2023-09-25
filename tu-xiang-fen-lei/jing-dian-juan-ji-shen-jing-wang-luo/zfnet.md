---
description: ImageNet 2013年冠军 纽约大学
---

# ZFNet

Paper: Visualizing and Understanding Convolutional Network

## ZFNet 创新点

* 对AlexNet的小修小改
* 反卷积可视化的可解释性分析
* 进行了遮挡测试

## ZFNet 网络结构

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

## 反卷积可视化的可解释性分析

* 浅层关注的是长宽方向上的special信息, 而深层关注的是语义信息
* 彩色图是从原始数据找出. 并能够使得某个卷积核激活最大的9张图片
* 灰色图是将彩色图喂到网络里找出 feature map, 然后用反卷积的技巧重构回原始像素空间

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>
