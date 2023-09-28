---
description: 损失函数为交叉熵时, 会使用标签平滑
---

# Label Smoothing

## **基本介绍**

**标签平滑**是一种**正则化技术**, 在训练过程中为目标标签添加少量噪声。其目的是防止模型对自己的预测**过于自信**，减少过拟合

* 在 Inception-v2中提出
* 可以解决过度拟合 overfitting (early stopping, dropout, weight regularization)
* 也可以解决过度置信 overconfidence

比如说我们可以将损失的目标值从 1 稍微降到 0.9, 或者从 0 稍微升到 0.1

## 参考资料

* [What is Label Smoothing?](https://towardsdatascience.com/what-is-label-smoothing-108debd7ef06)
* [Label Smoothing in PyTorch](https://saturncloud.io/blog/label-smoothing-in-pytorch-a-complete-guide-for-data-scientists/)

