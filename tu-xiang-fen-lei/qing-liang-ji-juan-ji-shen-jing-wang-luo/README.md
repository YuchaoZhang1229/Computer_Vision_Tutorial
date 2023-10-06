# 轻量级卷积神经网络

[Paper：MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications](https://arxiv.org/abs/1704.04861)

## 一、设计目的

为什么需要轻量级网络？

* 人工智能技术落地的关键：移动端、嵌入式端应用
* 现有的高精度模型往往很重 （尺寸大+推理时间长）
* 移动端/嵌入式端设备资源不足 （内存资源少+算力不够）
* 轻量级网络 + 模型压缩 （剪枝、量化、编码）
* 轻量级网络衡量标准：模型尺寸小 （参数量 Params），推理时间快 （浮点计算量 FLOPs）

## 二、设计原理 + Code

#### 设计原理1：常规卷积换成 （逐层卷积 + 逐点卷积）

* 逐层卷积： 对输入特征图的**每一个通道只使用一个卷积核**
* 逐点卷积：常规的 1 × 1 卷积， 目的是**跨通道信息融合**



## 三、整体结构 + Code

## 四、效果分析









参考资料

* [【大话深度学习】轻量级网络--MobileNet V1](https://www.bilibili.com/video/BV1i44y1x7hP/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
