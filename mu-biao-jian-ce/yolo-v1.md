# YOLO-V1

## 参考博客

* [https://cuijiahua.com/blog/2021/05/dl-basics-5.html](https://cuijiahua.com/blog/2021/05/dl-basics-5.html)

## 核心思想

采用利用整张图作为网络的输入，直接在输出层回归 bounding box 的位置和 bounding box 所属的类别

## 检测过程

**第一步-Resize image**&#x20;

**输入图片大小为 448×448**

将不同尺寸的图片适配到统一的网络结构中，需要 resize 到相同的尺寸

**第二步-Run convolutional network**

<figure><img src="../.gitbook/assets/dl-basics-5-8.png" alt=""><figcaption></figcaption></figure>

最终输出是 7×7×30

* 7×7很好理解, 图像分为 7x7 个区域进行预测
* 前五个数值, 分别是 bbox 的 x,y, w, h, c (两组)
  * x, y 指的是 bbox 的中心坐标
  * w, h 指的是 bbox 的宽高
  * c 指的是 bbox的置信度
* 后面的20个指的是类别概率 (VOC数据集)
* tensor - 7×7×(2×5+20) - S×S×(B×5+C)

**第三部-Non-max suppression-非极大值抑制**

##



## 预测阶段后处理

[https://github.com/watersink/yolov1\_tutorial](https://github.com/watersink/yolov1\_tutorial)

NMS算法
