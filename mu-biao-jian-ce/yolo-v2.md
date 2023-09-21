# YOLO-V2

## 参考博客

* [YOLO9000: Better, Faster, Stronger](https://arxiv.org/abs/1612.08242)
* [https://zhuanlan.zhihu.com/p/35325884](https://zhuanlan.zhihu.com/p/35325884)

## YOLOv2的训练

<figure><img src="../.gitbook/assets/v2-a2167f0ab8d7b65a849ce4f38e53e6b5_720w.webp" alt=""><figcaption><p>YOLOv2训练的三个阶段</p></figcaption></figure>

<figure><img src="../.gitbook/assets/v2-b23fdd08f65266f7af640c1d3d00c05f_720w.png" alt=""><figcaption><p>YOLOv2结构示意图</p></figcaption></figure>

YOLO-V2最后特征图的分辨率是 13×13

13 × 13 × 125 = 13 × 13 × (5+20) × 5

## YOLO-V2的改进策略

<figure><img src="../.gitbook/assets/v2-8f48ba80f887fd5b26bd4211cf740c39_720w.webp" alt=""><figcaption><p>YOLOv2相比YOLOv1的改进策略</p></figcaption></figure>

## Batch Normalization

**Batch Normalization**可以提升模型**收敛速度**，而且可以起到一定**正则化**效果，降低模型的过拟合。在YOLOv2中，每个卷积层后面都添加了Batch Normalization层，并且不再使用droput。使用Batch Normalization后，YOLOv2的mAP提升了2.4%。

## Convolutional With Anchor Boxes

* Dimension Clusters 在边界框进行聚类, 以获得先验框
* Direct location prediction 预测边界框中心点相对于对应cell左上角位置的相对偏移值

在YOLOv1中，输入图片最终被划分为 7×7 网格，每个单元格预测2个边界框。YOLOv1最后采用的是全连接层**直接**对边界框进行预测，其中边界框的宽与高是相对整张图片大小的，而由于各个图片中存在**不同尺度**和**长宽比**（scales and ratios）的物体，YOLOv1在训练过程中学习适应不同物体的形状是比较困难的，这也导致YOLOv1在精确定位方面表现较差。

YOLOv2借鉴了Faster R-CNN中RPN网络的**先验框**（anchor boxes，prior boxes，SSD也采用了先验框）策略。

* YOLOv2移除了YOLOv1中的全连接层而采用了卷积和anchor boxes来预测边界框
* 思想来源于 Faster R-CNN中的 Region Proposal Network (RPN), 它通过预定义一些具有不同形状和尺寸的边界框, 然后让模型预测这些边界框的偏移量, 而不是直接预测边界框的坐标. 这种方式可以帮助模型更好地处理不同形状和尺寸的对象.
  * **数据准备:** 归一化处理宽度和高度, 以消除图片大小影响, 大量 w, h (即每个边界框)可以表示成二维坐标上的点
  * **聚类分析:** 对上述得到的点进行聚类, k表示想要得到先验框的数量. 聚类分析时选用box与聚类中心box之间的IOU值作为距离指标
  * **先验框确定:** 最后, 聚类的结果就是预定义的先验框.&#x20;
  * 五个先验框, 训练时, 对于每一个真实的边界框, 我们会选择和它有最高IoU的那个先验框进行匹配, 然后这个先验框就被用来预测真实边界框

<figure><img src="../.gitbook/assets/v2-0c6e54419d7fb2ffada1aa7ac9540c7e_720w.png" alt=""><figcaption><p>数据集VOC和COCO上的边界框聚类分析结果</p></figcaption></figure>

预测边界框中心点相对于对应cell左上角位置的相对偏移值

* 对于中心点 (x, y) 的预测, 我们首先对**网络的输出**应用**sigmoid函数**, 将其映射到 0 到 1 之间, 然后再加上**对应的单元格的左上角坐标**, 这样就得到了相对于整个图像的坐标
* 对于宽高 (w, h) 的预测, 我们会将**网络的输出**直接应用**指数函数**, 然后再乘以**对应的先验框的宽高**, 这样就得到了相对于整个图像的宽高
* 通过采用指数相乘的方式, 可以更好地处理不同大小的物体.&#x20;
  * 对于小物体, 模型可以通过预测较小的值来表示较小的改变
  * 对于大物体, 即使预测值相同, 经过指数函数和先验框大小的相乘后, 也可以表示较大的改变

<figure><img src="../.gitbook/assets/v2-7fee941c2e347efc2a3b19702a4acd8e_720w.png" alt=""><figcaption><p>边界框位置与大小的计算示例图</p></figcaption></figure>

##





