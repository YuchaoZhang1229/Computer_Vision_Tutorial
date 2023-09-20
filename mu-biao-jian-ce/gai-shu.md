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

![](../.gitbook/assets/Milestones.png)



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

