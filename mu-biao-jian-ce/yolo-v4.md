# YOLO-V4

## YOLO-V4主要改进

* **精度:** YOLOv4在目标检测领域的基准测试中, 表现出了出色的性能和准确性
* **速度:** 尽管YOLOv4更加准确, 但并没有牺牲速度. YOLOv4仍然可以实时进行目标检测, 这在实时系统中非常重要
* **改进的特性:** YOLOv4引入了多种新特性来提高性能, 包括**CSPDarknet53**的骨干网络, **PANet**和**SAM**块的路径聚合网络, 以及**Mish**激活函数. 这些都是为了提高网络的准确性和速度
* **更少的计算资源:** 尽管YOLOv4更加强大, 但其设计考虑了实用性, 所以在计算资源有限的设备上, 仍可以运行

## 改进了网络结构

* **Backbone:** 在Darknet53中引入了CSP
* **Neck:** 采用了SPP模块以及PAN模块
* **Head:** 没有变化

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

CSPDarknet53就是将CSP结构融入了Darknet53中. CSP结构是在CSPNet (Cross Stage Partial Network)论文中提出的

* 思想来源shuffleNetv2 旷世科技孙健
* ![](<../.gitbook/assets/image (1) (1).png>)

PAN (Path Aggregation Network) 结构其实就是在FPN (从顶到底信息融合) 的基础上加上了从底到顶的信息融合

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## YOLOv4

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

## CIoU (定位损失)

在YOLOv3中定位损失采用的是MSE损失, 但在YOLOv4中作者采用的是CIoU损失



## Mosaic Data Augmentation (数据增强)

在数据预处理时将四张图片拼接成一张图片, 增加学习样本的多样性



其他改进

* eliminate grid sensitivities
* IoU Threshold (正负样本匹配)
