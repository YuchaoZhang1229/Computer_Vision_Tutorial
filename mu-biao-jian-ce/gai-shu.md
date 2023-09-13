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

先从图像中提取若干候选框, 在逐一地对这些候选框甄别, 以及调整他们的坐标 (准确但耗时)

* RCNN
* Fast-RCNN
* Faster-RCNN



Faster-RCNN&#x20;

* 有预选框 RPN
* 速度通常较慢 (5FPS), 但是效果通常还是不错
* 了解Mask-RCNN

<figure><img src="../.gitbook/assets/image-20230911160747327.png" alt=""><figcaption></figcaption></figure>

### One Stage

直接把图形喂到模型中, 直接输出结果 (快但不准确)

* YOLO
* SSD
* Retina-Net



YOLO -one stage

* 不用预选框
* 速度非常快, 适合做实时检测任务
* 效果不太好

YOLO-V5 速度和准确性都不错
