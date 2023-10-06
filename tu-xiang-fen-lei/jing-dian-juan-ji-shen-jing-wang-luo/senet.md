---
description: ImageNet 2017年冠军
---

# SENet

原理源流[Paper：Squeeze-and-Excitation Networks](https://arxiv.org/abs/1709.01507)

## 一、为什么需要 Attention 机制

**提出背景：**常规卷积核利用感受野在**空间维度** （H×W）和**通道维度** （channel）对信息进行相乘求和的计算； 现有的网络很多主要是在空间维度方面来进行特征的融合 （Inception的多尺度）； 模仿人类视觉机制， 只关注部分区域， 忽略无关区域；在 NLP 领域 Attention 取得了巨大成功， CV 领域开始借鉴 **（空间存储的是特征表示，通道存储的是不同的特征）**

**通道维度的注意力机制：**在常规卷积中， 输入信息的每个通道进行计算后的结果会进行求和输出，这时每个通道的重要程度是相同的**（直接求和，每个通道的权重为1）**。而通道维度的注意力机制，则通过学习的方式来自动获取每个特征通道的重要程度，以增强有用的通道特征，抑制不重要的通道特征 **（加权求和，自动学习）**

## 二、SE结构

* **Squeeze：**X 经过一系列传统卷积得到 U，对 U （H×W×C）先做一个 Global Average Pooling，输出的 1×1×C数据，这个特征向量一定程度上可以代表之前的输入信息
* **Excitation：**再经过两个全连接层来学习通道间的重要性， 用 sigmiod 限制到 \[0，1] 的范围，这时得到的输出可以看作每个通道重要程度的权重
* **Reweight：**将 Excitation 输出的权重看作每个通道的重要性，然后通过乘法加权到之前的特征上， 完成对U的channels进行了重要程度的重新分配

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption><p>原理图</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption><p>计算图</p></figcaption></figure>

## 三、设计原理

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

## 四、代码实现

```python
class SELayer(nn.Module):
    def __init__(self, channel, reduction=16):
        super(SELayer, self).__init__()
        self.avg_pool = nn.AdaptiveAvgPool2d(1)
        self.fc = nn.Sequential(
            nn.Linear(channel, channel//reduction, bias=False)
            nn.ReLU(inplace=True)
            nn.Linear(channel//reduction, channel, bias=False)
            nn.Sigmoid()
        )
    
    def forward(self, x):
        b, c, _, _ = x.size()
        y = self.avg_pool(x).view(b,c)
        y = self.fc(y).view(b, c, 1, 1)
        return x * y.expand_as(x)
```

## 五、效果分析

<figure><img src="../../.gitbook/assets/8f06d9030a1330818d43a4723e81b00.png" alt=""><figcaption></figcaption></figure>

## 六、总结

SE block 可以理解为 channel 维度上的注意力机制 （及重新分配通道上的 feature map 对后续计算的权重）（后续  SKNet， MobileNet都会有 SE block 的身影）

## 参考资料：

* [【CV中的注意力机制】SENet](https://www.bilibili.com/video/BV1QA411F7rR/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
* [ImageNet 冠军模型 SE-Net 详解（作者报告，搬运油管，中文）](https://www.bilibili.com/video/BV1Up4y187qb/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
