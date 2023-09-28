---
description: ImageNet 2012冠军 Hinton团队 多伦多大学 8层
---

# AlexNet

[Paper: ImageNet Classification with Deep Convolutional Neural Networks](https://papers.nips.cc/paper\_files/paper/2012/hash/c399862d3b9d6b76c8436e924a68c45b-Abstract.html)

## AlexNet 创新点

* 使用深度卷积网络来提取图像的深层特征
* 从识别数字变成识别1000个物体, 在ImageNet超大规模的数据集上进行了训练
* ReLU 激活函数
* 双GPU模型并行
* LRN 局部响应归一化
* 重叠最大池化
* 数据增强 Data Augmentation
* Dropout 正则化

## AlexNet 网络结构

* 输入图像 227 × 227 × 3
* 5 个卷积层 + 3 个全连接层
* **ReLU 激活函数**
  * 增加非线性
  * 解决梯度消失问题, 加快学习的速度
  * 只要它的输入大于0, 他就能回传梯度
  * 比 tanh 激活函数快六倍
* **双GPU的实现**
  * 把模型并行的放在两个GPU上进行训练, 每个GPU各自拥有一半的神经元
  * GPU2- 训练得到的 48 个卷积核提取边缘, 频率, 方向特征
  * GPU1- 训练得到的 48 个卷积核提取颜色特征
* **局部响应归一化** (Local Response Normalization, LRN)
  * 同一位置不需要太多的高激活神经元, 起到了侧向抑制的效果
  * VGG指出LRN层没什么用, 只会徒劳增加计算量
* **重叠池化** (Overlapping Pooling)
  * 认为重叠的池化可以防止过拟合
  * 后续也不用
* 减少过拟合 之 **数据增强**
  * 水平翻转
  * 随机裁剪, 平移变换
  * 颜色, 光照变换
* 减少过拟合 之 **Dropout**
  * 用在全连接层上可以有效防止过拟合
  * 在训练每一步的时候, 把一部分神经元舍去, 让输出为0, 也就是说阻断前向和反向传播, 一般设置 0.5

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## AlexNet训练细节

随机梯度下降 (Stochastic Gradient Descent)

* Batch Size - 128
* 动量 (momentum) - 0.9
* weight decay - 0.0005 → 正则化
* 学习率 - 0.01

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

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
### 搭建AlexNet网络
import torch
import torch.nn as nn
from torchsummary import summary

class AlexNet(nn.Module):
    def __init__(self, num_classes=1000):
        super(AlexNet, self).__init__()
        # 卷积层
        # 第一层 227*227*3 -> 55*55*96 ReLU → LRN → MaxPool
        self.conv1 = nn.Conv2d(in_channels=3, out_channels=96, kernel_size=11, stride=4, padding=0)
        self.relu1 = nn.ReLU()
        self.lrn1 = nn.LocalResponseNorm(size=5, alpha=1e-4, beta=0.75, k=2)
        self.pool1 = nn.MaxPool2d(kernel_size=3, stride=2)
        # 第二层 55*55*96 -> 27*27*256 ReLU → LRN → MaxPool
        self.conv2 = nn.Conv2d(in_channels=96, out_channels=256, kernel_size=5,  padding=2)
        self.relu2 = nn.ReLU()
        self.lrn1 = nn.LocalResponseNorm(size=5, alpha=1e-4, beta=0.75, k=2)
        self.pool2 = nn.MaxPool2d(kernel_size=3, stride=2)
        # 第三层 27*27*256 -> 13*13*384 ReLU
        self.conv3 = nn.Conv2d(in_channels=256, out_channels=384, kernel_size=3, padding=1)
        self.relu3 = nn.ReLU()
        # 第四层 13*13*384 -> 13*13*384 ReLU
        self.conv4 = nn.Conv2d(in_channels=384, out_channels=384, kernel_size=3, padding=1) 
        self.relu4 = nn.ReLU()
        # 第五层 13*13*384 -> 13*13*256  MaxPool
        self.conv5 = nn.Conv2d(in_channels=384, out_channels=256, kernel_size=3, padding=1)
        self.pool5 = nn.MaxPool2d(kernel_size=3, stride=2)
        

        # 全连接层
        self.flatten = nn.Flatten()
        # 第六层 13*13*256 -> 4096 ReLU → Dropout
        self.fc6 = nn.Linear(in_features=9216, out_features=4096)
        self.relu6 = nn.ReLU()
        self.dropout6 = nn.Dropout()
        # 第七层 4096 -> 4096 ReLU → Dropout
        self.fc7 = nn.Linear(in_features=4096, out_features=4096)
        self.relu7 = nn.ReLU()
        self.dropout7 = nn.Dropout()
        # 第八层 4096 -> 1000 Softmax
        self.fc8 = nn.Linear(in_features=4096, out_features=num_classes)
        self.softmax = nn.Softmax(dim=1)
        
    def forward(self, x):
        # 第一层
        x = self.conv1(x)
        x = self.relu1(x)
        x = self.lrn1(x)
        x = self.pool1(x)
        # 第二层
        x = self.conv2(x)
        x = self.relu2(x)
        x = self.lrn1(x)
        x = self.pool2(x)
        # 第三层
        x = self.conv3(x)
        x = self.relu3(x)
        # 第四层
        x = self.conv4(x)
        x = self.relu4(x)
        # 第五层
        x = self.conv5(x)
        x = self.pool5(x)
        # 第六层
        x = self.flatten(x)
        x = self.fc6(x)
        x = self.relu6(x)
        x = self.dropout6(x)
        # 第七层
        x = self.fc7(x)
        x = self.relu7(x)
        x = self.dropout7(x)
        # 第八层
        x = self.fc8(x)
        x = self.softmax(x)
        return x
    
if __name__ == '__main__':
    model = AlexNet()
    summary(model, (3, 227, 227), device='cuda')
```

## 参考资料

* [同济子豪兄](https://www.bilibili.com/video/BV1UQ4y1y7A3/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
* [Hinton个人主页](https://www.cs.toronto.edu/\~hinton/)
* [LRN层与BN层的区别](https://towardsdatascience.com/difference-between-local-response-normalization-and-batch-normalization-272308c034ac)
* [AlexNet的CUDA代码实现](https://code.google.com/archive/p/cuda-convnet/)
