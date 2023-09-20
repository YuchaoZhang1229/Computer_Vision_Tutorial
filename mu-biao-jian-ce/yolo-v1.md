# YOLO-V1

## 参考博客

* [https://cuijiahua.com/blog/2021/05/dl-basics-5.html](https://cuijiahua.com/blog/2021/05/dl-basics-5.html)

## 核心思想

采用利用整张图作为网络的输入，直接在输出层回归 bounding box 的位置和 bounding box 所属的类别

## 检测过程

第一步-Resize image 448×448

将不同尺寸的图片适配到统一的网络结构中，需要 resize 到相同的尺寸

第二步-Run convolutional network



* Non-max suppression-非极大值抑制

##



## 预测阶段后处理

[https://github.com/watersink/yolov1\_tutorial](https://github.com/watersink/yolov1\_tutorial)

NMS算法
