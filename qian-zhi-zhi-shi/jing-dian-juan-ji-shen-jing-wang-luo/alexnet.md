---
description: ImageNet 2012冠军 Hinton团队 多伦多大学
---

# AlexNet

[Paper: ImageNet Classification with Deep Convolutional Neural Networks](https://papers.nips.cc/paper\_files/paper/2012/hash/c399862d3b9d6b76c8436e924a68c45b-Abstract.html)

## AlexNet 网络结构

* 使用深度卷积网络来提取图像的深层特征





## 卷积神经网络

**卷积层:**&#x20;

* 一个卷积核 (kernel or filter) 在图片上进行滑动, 每次滑动都有和卷积核一样大的一个小窗口在图片上盖着, 这个小窗口称之为感受野 (receptive field, 浅蓝色区域)
* 浅蓝色区域与卷积核上的权重相对应位置相乘然后求和, 就生成了一个 feature map
* 卷积核的值不变, 局部连接, 全局共享

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

**池化层:**

aaa



## 搭建 AlexNet 模型

```python
1
```









## 参考资料

* [https://www.bilibili.com/video/BV1UQ4y1y7A3/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d](https://www.bilibili.com/video/BV1UQ4y1y7A3/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
