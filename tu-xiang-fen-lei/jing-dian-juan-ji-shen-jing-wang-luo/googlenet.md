---
description: ImageNet 2014年冠军 Inception模块 谷歌
---

# GoogLeNet

Paper: Going deeper with convolutions

## Inception 技术演进

V1 (GoogleNet) → BN-Inception → V2 → V3 → V4 → Inception-ResNet → XInception

## GoogLeNet 创新点

* Inception 模块



## GoogLeNet 网络结构

* 增加深度 (层数) 和宽度 (卷积核个数) 的同时减少计算量, 比 2012 年的 AlexNet 参数少 12 倍
* 用稀疏连接取代密集连接 (Arora et al. \[2], Hebbian pricipal)
* 重要启发源文献1-Network in Network&#x20;
  * 1 × 1 卷积 + ReLU
    * 降维
    * 减少参数运算律
    * 增加模型深度, 提高非线性表达能力
  * Global Average Pooling 取代全连接层
* 重要启发源文献1-Provable Bounds for Learning Some Deep representions
  * 理论研究
  * 用稀疏, 分散的网络取代以前庞大密集臃肿的网络
* 目标检测阶段应用 R-CNN 类似的方法
  * muti-box prediction

### Inception 模块

* 利用多尺度的分支进处理, 构建了一个模块, 然后逐渐堆叠这个模块; 并且随着模型的加深, 3×3 和 5×5 卷积核比例逐渐提高
* 版本 (a) 存在的问题是作业本堆叠越来越厚 (通道数越来越多) 导致计算量爆炸

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

