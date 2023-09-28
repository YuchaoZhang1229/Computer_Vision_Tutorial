---
description: ImageNet 2015年冠军 残差模块 何凯明团队 微软亚洲研究院 152层
---

# ResNet

**Paper: Deep Residual Learning for Image Recognition**

**后续变体:** ResNetXt, Inception-ResNet, ResNetXt-Attention, ResNetXt WSL, DenseNet, SENet

## ResNet 创新点

* 残差模块-解决了网络退化现象

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

## ResNet 网络结构



## 残差模块

* F(x) is a residual mapping w.r.t. identity
  * 在 x 的基础上优化
  * 如果恒等模块 x 足够好的话, 残差模块 F(x) 可以为 0
* 为什么恒等映射能解决网络退化问题?
  * 深层梯度回传顺畅
    * 恒等映射这一路的梯度是1, 把底层的信号传到深层, 也可以把深层的信号注入底层, 有效的防止了梯度消失, 没有中间商层层盘剥 (Sigmoid)
  * 类比其他机器学习模型
    * 集成学习 boosting, 每一个弱分类器拟合 "前面的模型与GT之差"
    * 长短时间记忆神经网络 LSTM 的遗忘门
    * ReLU 激活函数
  * 传统线性结构网络难以拟合 "恒等映射"
    * 什么都不做有事很重要
    * skip connection 可以让模型自行选择要不要更新
    * 弥补了高度非线性造成的不可逆的信息损失 (MobileNet V2)
  * ResNet 反向传播传回的梯度相关性好
  * ResNet 相当于几个浅层网络的集成
  * skip connection 可以实现不同分辨率特征的组合 (FPN DenseNet)

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## **拓展资料**

**1st places** in all five main tracks

* ImageNet Classification: "Ultra-deep" **152-layer** nets
* ImageNet Detection: 16% better than 2nd
* ImageNet Localization: 27% better than 2nd
* COCO Detection: 11% better than 2nd
* COCO Segmentation: 12% better than 2nd

Kaiming He

* ResNet
* Faster R-CNN 两个阶段目标检测
* Mask R-CNN 检测 分割 人体关键点检测
* RetinaNet-Focal Loss 密集目标
* Feature Pyramid Network-Fast R-CNN 到 Faster R-CNN的过度模型
* SPPNet

Xiangyu Zhang

* 2016与孙健加入旷视
* Shufflenet v1-v2 轻量级网络 人脸解锁

## 参考资料

* [同济子豪兄](https://www.bilibili.com/video/BV1vb4y1k7BV/?p=4\&spm\_id\_from=pageDriver\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
