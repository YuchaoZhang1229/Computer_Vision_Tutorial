---
description: ImageNet 2012冠军 Hinton团队 多伦多大学
---

# AlexNet

[Paper: ImageNet Classification with Deep Convolutional Neural Networks](https://papers.nips.cc/paper\_files/paper/2012/hash/c399862d3b9d6b76c8436e924a68c45b-Abstract.html)

## AlexNet 创新点

* 使用深度卷积网络来提取图像的深层特征
* 从识别数字变成识别1000个物体, 在ImageNet超大规模的数据集上进行了训练
* 使用了 ReLU 激活函数, 解决梯度消失问题 (比 tanh 激活函数快六倍)
* 模型的基本结构和双GPU的实现

## AlexNet 网络结构

* 输入图像 227 × 227 × 3
* 使用 ReLU 激活函数
  * 解决梯度消失问题, 加快学习的速度
  * 只要它的输入大于0, 他就能回传梯度
* 双GPU的实现
  * 把模型并行的放在两个GPU上进行训练, 每个GPU各自拥有一半的神经元
  * GPU2- 训练得到的 48 个卷积核提取边缘, 频率, 方向特征
  * GPU1- 训练得到的 48 个卷积核提取颜色特征
* 局部响应归一化 (Local Response Normalization, LRN)
  * 同一位置不需要太多的高激活神经元, 起到了侧向抑制的效果
  * VGG指出LRN层没什么用, 只会徒劳增加计算量
* 重叠池化 (Overlapping Pooling)
  * 认为重叠的池化可以防止过拟合
  * 后续也不用
* 减少过拟合 之 数据增强
  * 水平翻转
  * 随机裁剪, 平移变换
  * 颜色, 光照变换
* 减少过拟合 之 Dropout
  * 用在全连接层上可以有效防止过拟合
  * 在训练每一步的时候, 把一部分神经元舍去, 让输出为0, 也就是说阻断前向和反向传播, 一般设置 0.5

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

## 卷积神经网络

**卷积层:**&#x20;

* 一个卷积核 (kernel or filter) 在图片上进行滑动, 每次滑动都有和卷积核一样大的一个小窗口在图片上盖着, 这个小窗口称之为感受野 (receptive field, 浅蓝色区域)
* 浅蓝色区域与卷积核上的权重相对应位置相乘然后求和, 就生成了一个 feature map
* 卷积核的值不变, 局部连接, 全局共享

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

**池化层:**

* 又称为下采样, 目的是把大的 feature map 变成一个小 feature map
* 平均池化, 最大值池化
* 作用: 一方面减少 feature map 的尺寸, 减少计算量, 另一面它还可以防止过拟合

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

**激活函数:**

* 激活函数必须是非线性的, 这能为神经网络模型引入非线性的特征
* 传统激活函数有: Sigmoid, tanh
  * 饱和激活函数
  * 当输入的过小或者过大的时候
  * 就会被局限在一个很小的区域内
  * 会造成梯度消失问题, 会影响学习的效率

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>





## 搭建 AlexNet 模型

```python
1
```









## 参考资料

* [https://www.bilibili.com/video/BV1UQ4y1y7A3/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d](https://www.bilibili.com/video/BV1UQ4y1y7A3/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
