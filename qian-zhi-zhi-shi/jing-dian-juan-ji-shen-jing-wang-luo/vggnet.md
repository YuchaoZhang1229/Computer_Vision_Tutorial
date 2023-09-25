---
description: ImageNet 2014亚军 VGG16和VGG19 牛津大学
---

# VGGNet

[Paper: Very Deep Convolutional Networks for Large-Scale Image Recognition](https://www.robots.ox.ac.uk/\~vgg/publications/2015/Simonyan15/)

## VGGNet 创新点

* 多层 3 × 3 小卷积核取代大卷积核
* VGG-16， VGG-19 迁移学习的基模型
* 模型简洁， 常规经典CNN结构的极致但参数量较大，集中在 FC1 略显臃肿

## VGGNet 网络结构

* 输入 224 × 224 RGB 图像
* 卷积层+最大池化（2 + 2 + 3 + 3 + 3 ）+ 全连接层（3）
* Total memory： 96 MB/image
* Total params：138M parameters
* 全部使用 3 × 3 卷积
  * 最小的能包含左右上下和中心点的卷积核
  * stride 1 - 没有信息丢失
  * padding 1&#x20;
  * 用两层 3 × 3 卷积代替原来的一层 5 × 5 卷积
  * 以此类推 用三层 3 × 3 卷积代替原来的一层 7 × 7 卷积
  * 模型变深， 非线性能力变强， 参数数量更少
* 5个最大池化层
  * kernel\_size-2
  * stride-2
* 3个全连接层
* 4096 → 4096 → 1000 (softmax)
* 全部使用 ReLU 激活函数
* 1 × 1 卷积的理解
  * 增加非线性的一种方法
  * 不会影响 feature map 的尺寸的
  * 本身是线性变换, 加激活函数可以增加非线性
  * 论文 Network in Network (2014) 重要
    * 1 × 1 卷积
    * 全局平均池化

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* Other details:
  * ReLU - non-linearity
  * 5 max-pool layers
  * no normalisation
  * 3 full-connected layers

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

## VGGNet 训练和评估细节

### 训练 Training

优化多分类交叉熵损失函数使用 mini-batch gradient descend

* Batch Size - 256
* 动量 (momentum) - 0.9
* weight decay - 5 × 10^(-4) → 正则化
* 学习率 - 0.01
  * 当精读停止提升的时候, 将学习率除以10
* 74个eopchs就收敛了

网络权重的初始化 (坏的初始化会让模型停止学习由于梯度的不稳定)

* 随机初始化: 在均值为0和方差为0.01的正态分布中随机取值, 偏置项设为0
* 使用随机初始化先训练浅模型 (11层)
* 训练深模型的时候使用浅模型前四个卷积层和三个全连接层的权重作为初始化, 其他层使用随机初始化

数据增强

* 水平翻转
* 随机裁剪, 平移变换
* 颜色, 光照变换

Training image size

* 方案一: 固定S
* 方案二: 多尺度S

### 测试细节 Testing



## VGGNet 应用

* 迁移学习
  * 以VGG作为迁移学习的基模型
  * 仅需把最后一层全连接层换成自己类别个数
* 可解释性分析
  * 方法一
    * 用VGG作为特征提取器
    * 把每张图片编码成4096维向量
    * 查看这些向量之间是否存在一些关系
    * 将其降维到二维 （TSNE， PCA）
  * 方法二-各层输出结果可视化
    * 可视化分析， 高亮处理，

## 论文精读总结

* ConvNets 能成功源于: 大规模公开数据集 ImageNet, 高性能的算力 GPU, 大规模分布式集群&#x20;
* 获得更好的 accuracy 的两个分支
  * ZFNet 和 GoogleNet 用更小的感受野 (receptive window size) 和更小的步长 (stride)
  * 训练和测试的时候 (densely over the whole image) 和 (over multiple scale )

## 搭建VGG16网络

```python
import torch.nn as nn
from torchsummary import summary

class VGGNet(nn.Module):
    def __init__(self, num_classes=1000):
        super(VGGNet, self).__init__()
        self.conv1 = nn.Sequential(
            nn.Conv2d(3, 64, kernel_size=3, padding=1),  # 224
            nn.ReLU(inplace=True),
            nn.Conv2d(64, 64, kernel_size=3, padding=1),  # 224
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2)  # 112
        )
        self.conv2 = nn.Sequential(
            nn.Conv2d(64, 128, kernel_size=3, padding=1),  # 112
            nn.ReLU(inplace=True),
            nn.Conv2d(128, 128, kernel_size=3, padding=1),  # 112
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2)  # 56
        )
        self.conv3 = nn.Sequential(
            nn.Conv2d(128, 256, kernel_size=3, padding=1),  # 56
            nn.ReLU(inplace=True),
            nn.Conv2d(256, 256, kernel_size=3, padding=1),  # 56
            nn.ReLU(inplace=True),
            nn.Conv2d(256, 256, kernel_size=3, padding=1),  # 56
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2)  # 28
        )
        self.conv4 = nn.Sequential(
            nn.Conv2d(256, 512, kernel_size=3, padding=1),  # 28
            nn.ReLU(inplace=True),
            nn.Conv2d(512, 512, kernel_size=3, padding=1),  # 28
            nn.ReLU(inplace=True),
            nn.Conv2d(512, 512, kernel_size=3, padding=1),  # 28
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2)  # 14
        )
        self.conv5 = nn.Sequential(
            nn.Conv2d(512, 512, kernel_size=3, padding=1),  # 14
            nn.ReLU(inplace=True),
            nn.Conv2d(512, 512, kernel_size=3, padding=1),  # 14
            nn.ReLU(inplace=True),
            nn.Conv2d(512, 512, kernel_size=3, padding=1),  # 14
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=2, stride=2)  # 7

        )

        self.classifier = nn.Sequential(
            nn.Linear(512 * 7 * 7, 4096),
            nn.ReLU(inplace=True),
            nn.Dropout(),
            nn.Linear(4096, 4096),
            nn.ReLU(inplace=True),
            nn.Dropout(),
            nn.Linear(4096, num_classes),
        )

    def forward(self, x):
        x = self.conv1(x)
        x = self.conv2(x)
        x = self.conv3(x)
        x = self.conv4(x)
        x = self.conv5(x)
        x = x.view(x.size(0), -1)
        x = self.classifier(x)
        return x    
    
model = VGGNet()
VGG16 = summary(model, (3, 224, 224))
```

## 参考资料

* [同济子豪兄 VGGNet](https://www.bilibili.com/video/BV1fU4y1E7bY/?spm\_id\_from=autoNext\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
* [VGGNet 详解](https://blog.csdn.net/zzq060143/article/details/99442334)
* [VGGNet 可视化](https://dgschwend.github.io/netscope/#/preset/vgg-16)
* [牛津大学视觉组 （VGG）官方网站](https://www.robots.ox.ac.uk/\~vgg/)
* [VGG 原版 Slides](http://www.robots.ox.ac.uk/\~karen/pdf/ILSVRC\_2014.pdf)
