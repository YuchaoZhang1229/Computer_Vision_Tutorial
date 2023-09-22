# YOLO-V5

## YOLOV5的改进

AutoAnchor (For training custom data), 训练自己数据集可以根据自己数据集里的目标进行重新聚类生成Anchors模板

Warmup and Cosine LR scheduler, 训练前先进行 Warmup 热身, 然后采用 Cosine 学习率下降策略

EMA (Exponential Moving Average), 可以理解为给训练的参数增加了一个动量, 让更新过程更加平滑

Mixed precision, 混合精度训练, 能够减少显存的占用并且加快训练速度, 前提是GPU硬件支持

Eliminate grid sensitivities

![](../.gitbook/assets/image.png)

## YOLOV5的网络结构

* Neck部分: 将SPP换成了SPPF. 两者的计算结果是一样的, 但SPPF比SPP计算速度快了不止两倍
  * 并行方式变为串行方式
  * 全部用 5×5的 kernel

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

## YOLOv5的损失

* Classes loss, 分类损失, 采用的是BCE loss, 注意只计算正样本的分类损失
* Objectness loss, obj损失, 采用的依然是BCE loss, 注意这里的obj指的是网络预测的目标边界框与GT Box的CIoU. 这里计算的是所有样本的obj损失
* Location loss, 定位损失, 采用的是CIoU loss, 注意计算正样本的定位损失

## 数据增强方法

* Mosaic + Random Affine&#x20;
* Copy Paste
* MixUp
* Albumentations

其他方法

* Blur 模糊
* VerticalFlip 水平翻转
* HorizontalFlip 垂直翻转
* Flip 翻转
* Normalize 归一化
* Transpose 转置
* RandomCrop 随机裁剪
* RandomGamma 随机Gamma
* RandomRotate90 随机旋转90度
* Rotate 旋转
* ShiftScaleRotate 平移缩放旋转
* CenterCrop 中心裁剪
* OpticalDistortion 光学畸变
* GridDistortion 网格失真
* Elastic Transform 弹性变换
* RandomGridShffle 随机网格洗牌
* HueSaturation Value 色调饱和度值
* PadIfNeeded 填充
* RandomBrightness随机亮度
* RandomContrast 随机对比度
* MotionBlur 运动模糊
* MedianBlur 中心模糊
* GasussianBlur 高斯模糊
* GaussNoise 高斯噪声
* CLAHE 对比度受限自适应直方图均衡
* InvertImg 反转图像
* ChannelShuffle 通道洗牌

## 多尺度训练 (Muti-scale training)

* 0.5\~1.5×4
* 假设设置输入图片的大小为 640 × 640
* 640 训练时采用尺寸是在 0.5 × 640 - 1.5 × 640 之间随机取值
* 取值时取得都是32的整数倍











































