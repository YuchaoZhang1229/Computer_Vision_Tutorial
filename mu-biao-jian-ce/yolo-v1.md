# YOLO-V1

## 参考博客

* [https://cuijiahua.com/blog/2021/05/dl-basics-5.html](https://cuijiahua.com/blog/2021/05/dl-basics-5.html)
* [https://github.com/watersink/yolov1\_tutorial](https://github.com/watersink/yolov1\_tutorial)

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

**第三步-Non-max suppression-非极大值抑制**

[https://github.com/watersink/yolov1\_tutorial](https://github.com/watersink/yolov1\_tutorial)

非极大值抑制顾名思义就是抑制不是极大值的元素, 搜索局部的极大值

## ![](../.gitbook/assets/dl-basics-5-15.png)

就像上面的图片一样，定位一个车辆，最后算法就找出了一堆的方框，我们需要判别哪些矩形框是没用的。非极大值抑制的方法是：先假设有6个矩形框，根据分类器的类别分类概率做排序，假设从小到大属于车辆的概率 分别为A、B、C、D、E、F

1. 从最大概率矩形框F开始，分别判断A\~E与F的重叠度IOU是否大于某个设定的阈值;
2. 假设B、D与F的重叠度超过阈值，那么就扔掉B、D；并标记第一个矩形框F，是我们保留下来的
3. 从剩下的矩形框A、C、E中，选择概率最大的E，然后判断E与A、C的重叠度，重叠度大于一定的阈值，那么就扔掉；并标记E是我们保留下来的第二个矩形框

## 损失函数

<figure><img src="../.gitbook/assets/dl-basics-5-17.png" alt=""><figcaption></figcaption></figure>
