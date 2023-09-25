---
description: ImageNet 2013年冠军 纽约大学
---

# ZFNet

Paper: Visualizing and Understanding Convolutional Network

## ZFNet 创新点

* 对AlexNet的小修小改
* 反卷积可视化的可解释性分析-尤其是医疗领域, 自动驾驶
* 平移, 缩放, 旋转敏感性分析
* 进行了局部遮挡敏感性分析和相关性分析

## ZFNet 网络结构

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

## 反卷积可视化的可解释性分析

* DeConvNet: 反池化, 反激活, 转置卷积
* 浅层关注的是长宽方向上的special信息, 而深层关注的是语义信息
* 彩色图是从原始数据找出. 并能够使得某个卷积核激活最大的9张图片
* 灰色图是将彩色图喂到网络里找出 feature map, 然后用反卷积的技巧重构回原始像素空间

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

## 参考资料

* [同济子豪兄](https://www.bilibili.com/video/BV17b4y1m7x8?p=2\&spm\_id\_from=pageDriver\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
