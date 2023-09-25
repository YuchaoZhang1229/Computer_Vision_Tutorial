---
description: ImageNet 2014亚军 VGG16和VGG19 牛津大学
---

# VGGNet

[Paper: Very Deep Convolutional Networks for Large-Scale Image Recognition](https://www.robots.ox.ac.uk/\~vgg/publications/2015/Simonyan15/)

## VGGNet 创新点

* 3 × 3 卷积

## VGGNet 网络结构

* 卷积层+最大池化（2 + 2 + 3 + 3 + 3 ）+ 全连接层（3）
* Total memory： 96 MB/image
* Total params：138M parameters
* 全部使用 3 × 3 卷积
  * 最小的能包含左右上下和中心点的卷积核
  * stride 1 - 没有信息丢失
  * 用两层 3 × 3 卷积代替原来的一层 5 × 5 卷积
  * 模型变深， 非线性能力变强， 参数数量更少

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* Other details:
  * ReLU - non-linearity
  * 5 max-pool layers
  * no normalisation
  * 3 full-connected layers

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

## VGGNet 训练细节





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
