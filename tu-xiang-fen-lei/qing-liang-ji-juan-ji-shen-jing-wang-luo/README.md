---
description: '2017'
---

# MobileNet V1

[Paper：MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications](https://arxiv.org/abs/1704.04861)

## 一、设计目的

为什么需要轻量级网络？

* 人工智能技术落地的关键：移动端、嵌入式端应用
* 现有的高精度模型往往很重 （尺寸大+推理时间长）
* 移动端/嵌入式端设备资源不足 （内存资源少+算力不够）
* 轻量级网络 + 模型压缩 （剪枝、量化、编码）
* 轻量级网络衡量标准：模型尺寸小 （参数量 Params），推理时间快 （浮点计算量 FLOPs）

## 二、设计原理 + Code

#### 设计原理1：常规卷积换成深度可分离卷积 Depthwise Separable Convolution （逐层卷积 + 逐点卷积）

* **逐层卷积 (Depthwise Convolutions)：** 对输入特征图的**每一个通道只使用一个卷积核**
* **逐点卷积 (Pointwise Convolutions)：**常规的 1 × 1 卷积， 目的是**跨通道信息融合**

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

```python
def con_dw(inp, oup, stride):
    dw=nn.Sequential(
        # 逐层卷积 （不改变输出通道数）
        nn.Conv2d(inp, inp, 3, stride, 1, groups=inp, bias=False)
        nn.BatchNorm2d(inp)
        nn.ReLU(inplace=True)
        
        # 逐点卷积
        nn.Conv2d(inp, oup,1 ,1, 0, bias=False),
        nn.BatchNorm2d(oup),
        nn.ReLU(inplace=True)
    return dw
```

#### 设计原理2：用ReLU6替换激活函数ReLU

目的：为了在**移动端设备float16/int8**的低精度的时候也能有很好的数值分辨率。如果对ReLU的激活范围不加限制，输出**范围为0到正无穷**，如果激活值非常大，分布在一个很大的范围内，则**低精度的float16/int8无法很好地精确描述如此大范围的数值**，带来精度损失

曲线：

<figure><img src="../../.gitbook/assets/image (45).png" alt="" width="292"><figcaption></figcaption></figure>

公式： $$RELU6 = min(6,max(0,x))$$

代码：fun = nn.ReLU6()



## 三、整体结构 + Code

<figure><img src="../../.gitbook/assets/image (46).png" alt="" width="375"><figcaption><p>MobileNet V1整体结构</p></figcaption></figure>

```python
class Net(nn.Module):
    def __init__(self):
        supr(Net,self).__init__()
        def conv_bn(inp, oup,stride):
            return nn.Sequential(
                nn.Corv2d(inp, oup,3,stride,1,bias=False),
                nn.BatchNorm2d(oup),
                nn.ReLu(inplace=True)
            )
        def conv_dw(inp, oup,stride):
            return nn.Sequential(
                nn.Conv2d(inp，inp，3, stride，1,groups-inp，bias=False),
                nn.BatchNorm2d(inp),
                nn.ReLU(inplace=True),

                nn.Conv2d(inp,oup，1,1,0,bias-False),
                nn.BatchNorm2d(oup),
                nn. ReLU(inplace=True),
            )
        
        self.model = nn.Sequential(
            conv_bn( 3，32，2),
            conv_dw(32,64，1),
            conv_dw( 64,128，2),
            conv_dw(128，128,1),
            corv_dw(128，256，2)，
            conv_dw(256，256，1),
            conv_dw(256，512，2),
            conv_dw(512,512，1),
            conv_dw(512，512，1),
            conv_dw(512，512，1)，
            conv_dw(512,512，1),
            conv_dw(512，512，1)，
            conu_dw(512,1024，2)，
            conv_dw(1024，1024，1),
            nn.AvgPool2d(7),
            )
        
        self.fc = nn.Linear(1024,1000)
        
        def forward(self，x):
            x = self.model(x)
            x = x.view(-1,1024)
            x = self.fc(x)
            return x
```

## 四、效果分析

<figure><img src="../../.gitbook/assets/image (47).png" alt="" width="563"><figcaption></figcaption></figure>

## 参考资料

* [【大话深度学习】轻量级网络--MobileNet V1](https://www.bilibili.com/video/BV1i44y1x7hP/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
* [六分钟精通 MobileNet（思想，公式，代码）--计算机视觉系列 06【推荐】](https://www.bilibili.com/video/BV1mu411i7rg/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
