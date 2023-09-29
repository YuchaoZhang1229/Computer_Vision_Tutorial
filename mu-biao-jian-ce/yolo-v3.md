# YOLO-V3

## YOLOV3 整体结构

* **DarkNet53 (backbone):** 对输入进来的图片进行特征提取, 获得三个有效特征层 (主干特征提取网络)
* **FPN:** 对获取到的三个有效特征层, 上采样后进行特征融合 (加强特征融合网络)
* **YOLO Head:** 对获得到的三个加强过的特征层, 预测三个先验框和物体的种类
  * (52, 52, 255) - 大物体
  * (26, 26, 255) - 中物体
  * (13, 13, 255)  - 小物体

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## YOLOV3 网路结构解析

### 1. 主干网络 DarkNet53

* **使用了残差网络 Residual**
  * 对 input 做 1 × 1 的卷积和 3 × 3 的卷积后, 再加上 input (残差边不做任何处理)
  * 好处: 1. 容易优化, 2. 增加深度能提高准确率 3. 残差边是恒等映射, 梯度为1, 有效缓解了梯度消失问题&#x20;
* 每一个DarknetConv2D后面都紧跟了**BatchNormalization**标准化与**LeakyReLU**部分

<figure><img src="https://production-media.paperswithcode.com/methods/Screen_Shot_2020-06-24_at_12.53.56_PM_QQoF5AO.png" alt=""><figcaption></figcaption></figure>

### 2. FPN 特征金字塔



## YOLO-V3主要改进

<mark style="color:red;">一般使用 yolov3-spp-ultralytics</mark>

* **多尺度预测:** 与前两个版本相比, YOLO-V3在3个不同的尺度上进行预测, 这使得模型在检测大小不同的对象时效果更好. 它通过应用 1×1 滤波器并下采样特征图来获得不同的尺度
* **使用了3中不同尺度的先验框:** 对于每个尺度, YOLO-V3使用3个先验框, 这个改变使得模型在检测各种形状的对象时能够做的更好
* **类别无关的对象性检测:** YOLO-V3引入了一个类别无关的对象性检测机制, 这使得它可以更好地处理覆盖, 遮挡或难以识别的对象 (二值交叉熵损失代替平方差损失)
* **使用 Darknet-53 网络结构:** YOLO-V3使用 DarkNet-53 作为其基础架构, 这是一个深度网络, 由卷积层, 池化层和连接层组成. 这使得模型在保持高速度的同时也具有很高的准确性
* **使用逻辑回归进行类别预测:** 不同于前两个版本使用 softmax, YOLO-V3采用了多标签分类的策略, 使用逻辑回归进行类别预测, 这使得模型在处理多类别标签时更为灵活

## YOLO-V3模型

* 训练的是COCO数据集, 80个类别
* 网络输出3个不同大小的 feature map, 这三个的感受野不同, 作者用他们负责识别不同大小的目标
* 作者在聚类挑选**先验框**的时候, 分成了9个cluster
  * 特征图 13×13: (116×90), (156×198), (373×326)
  * 特征图 26×26: (30×61), (62×45), (59×119)
  * 特征图 52×52: (10×13), (16×30), (33×23)
* N × N × \[3 \* (4 + 1 + 80)]
  * 4 bounding box offsets
  * 1 objectness prediction
  * 80 class predictions

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/YOLOV3.png" alt=""><figcaption></figcaption></figure>

### YOLO-V3训练过程

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption><p>训练过程</p></figcaption></figure>

### YOLO-V3测试过程

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption><p>测试过程</p></figcaption></figure>

## 损失函数

**置信度损失**和**类别损失**都采用**二值交叉熵损失**, 作者认为同一目标可以同时归为多类, 比如猫可归为猫类以及动物类, 这样能够应对更加复杂的场景. YOLO-V1-2中采用平方差损失

**位置损失**仍然使用**平方差损失**

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

IoU vs L2损失

优点:

* 能够更好地反应重合程度
* 具有尺度不变性

缺点:

* 当不想交时, loss为0, 无法进行反向传播

### 回归定位损失

* IoU
* GIoU
* DIoU
* CIoU
* Cross Entropy Loss
  * 交叉熵损失
* Focal Loss-用在 yolov3-spp-u 置信度 分类损失
  * 用来解决正负样本不平衡的问题
  * α 关注的是正负样本的权重
  * 容易受到噪音的影响

## YOLO-V3的进一步改进

### YOLO-V3-SPP

* SPP-空间金字塔池化 (Spatial Pyramid Pooling)
* 主要改进是在网络结构中添加了一个SPP模块
* 此外还有更好地训练策略, 更多的数据增强方法, 自动模型剪枝

## 参考资料

* [YOLOv3: An Incremental Improvement](https://arxiv.org/abs/1804.02767)
* [B站 Bubbliiiing](https://www.bilibili.com/video/BV1XJ411D7wF?p=3\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
* [CSDN Bubbliiiing](https://blog.csdn.net/weixin\_44791964/article/details/103276106)
* [YOLO-V3 精度论文 + 代码复现](https://www.bilibili.com/video/BV1Vg411V7bJ/?spm\_id\_from=333.337.search-card.all.click)

