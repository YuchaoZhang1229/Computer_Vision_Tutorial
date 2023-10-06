# YOLO-V4

## 一、YOLO-V4主要改进

1. 主干特征提取网络的改进，**DarkNet53 → CSPDarknet53**,&#x20;
2. 加强特征提取网络的改进，使用了**SPP**和**PANet**结构, 以及
3. 使用了 **CIOU** 作为回归 Loss
4. 使用了**Mish**激活函数.&#x20;
5. 训练用到的小技巧： **Mosaic 数据增强， Label Smoothing平滑， 学习率余弦退火衰减**

## 二、YOLO V3 的整体结构

* **Backbone:**  Darknet53 → CSPDarknet53&#x20;
* **Neck （FPN）:** 采用了SPP模块以及PAN模块
* **Head:** 没有变化

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

CSPDarknet53就是将CSP结构融入了Darknet53中. CSP结构是在CSPNet (Cross Stage Partial Network)论文中提出的

* 思想来源shuffleNetv2 旷世科技孙健
* ![](<../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)

PAN (Path Aggregation Network) 结构其实就是在FPN (从顶到底信息融合) 的基础上加上了从底到顶的信息融合

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## YOLOv4

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

## CIoU (定位损失)

在YOLOv3中定位损失采用的是MSE损失, 但在YOLOv4中作者采用的是CIoU损失



## Mosaic Data Augmentation (数据增强)

在数据预处理时将四张图片拼接成一张图片, 增加学习样本的多样性



其他改进

* eliminate grid sensitivities
* IoU Threshold (正负样本匹配)

## 参考资料

* [Pytorch 搭建自己的YoloV4目标检测平台（Bubbliiiing 深度学习 教程）](https://www.bilibili.com/video/BV1Q54y1D7vj/?spm\_id\_from=333.999.0.0\&vd\_source=4afb0374462e2a6a5fe3309f3b19500d)
* [睿智的目标检测30——Pytorch搭建YoloV4目标检测平台](https://blog.csdn.net/weixin\_44791964/article/details/106214657)
