# 概述

## 目标检测综述论文:&#x20;

Object Detection in 20 Years: A Survey

## 著名的数据集

* VOC-2007
* VOC-2012
* ILSVRC-2014
* ILSVRC-2017
* MS-COCO-2015
* MS-COCO-2018
* OID-2018

## 目标检测里程碑

<figure><img src="../.gitbook/assets/Milestones.png" alt=""><figcaption></figcaption></figure>



### Two Stage

先进行区域生成，该区域称之为 region proposal（简称RP，一个有可能包含待检物体的预选框），再通过卷积神经网络进行样本分类

任务流程：特征提取 --> 生成RP --> 分类/定位回归。

* RCNN
* SPP-Net
* Fast R-CNN
* Faster  R-CNN
* R-FCN



Faster-RCNN&#x20;

<figure><img src="../.gitbook/assets/image-20230911160747327.png" alt=""><figcaption></figcaption></figure>

### One Stage

直接把图形喂到模型中, 直接输出结果 (快但不准确)

* YOLO
* SSD
* Retina-Net



YOLO系列

* YOLO-V1 CVPR 2016
* YOLO-V2 YOLO 9000 CVPR 2017
* YOLO-V3 CVPR 2018
* YOLO-V4 CVPR 2020
* YOLO-V5 CVPR 2021  YOLO-X 2021



传统的目标检测流水线

1. 候选区生成-通过滑动窗口选择感兴趣区域ROI; 使用多尺寸的输入图像和多尺度的滑动窗口识别多尺度和不同比例的目标
2. 特征提取-SIFT, Harr, HOG, SURF
3. 区域分类-常用支持向量机, 结合集成, 串联学习, 梯度提升的方法提高准确率

卷积神经网络应用于目标检测

1.  两阶段-Two Stage

    先生成候选区域, 在对区域做预测. 精度高, 速度慢

    如: R-CNN, SPP-net, fast R-CNN, faster R-CNN, R-FCN, FPN, Mask R-CNN
2.  单阶段-One Stage

    把图像的每个可能区域看做候选区域. 精度低, 速度快

    如: OverFeat, YOLO, SSD, RetinaNet, YOLOv2, YOLOv3, CornerNet, YOLOv4

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

* Backbone: 卷积网络, 将输入图像转换成特征映射
* Neck:&#x20;
* Dense Prediction
* Sparse Prediction











