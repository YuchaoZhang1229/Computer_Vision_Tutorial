---
description: 学习率 针对较大的 batch size
---

# Linear Scaling Learning Rate

基本介绍

* 在ResNet中提出, 针对比较大的batch size
* 问题: 使用相同的epoch时, 大batch size训练的模型与小batch size训练的模型相比, 验证准确率会减小. 大batch size训练的模型可以增大学习率来加快收敛
