# 深度学习基础

https://paddlepedia.readthedocs.io/en/latest/tutorials/CNN/Pooling.html

## 卷积模型

### Convolution-卷积层

典型卷积神经网络结构:

* 多层卷积和池化层组合 + 全连接层
* ReLu 激活函数一般加在卷积或者全连接层的输出上, 网络中通常还会加入 Dropout 来防止过拟合

**卷积层:** 卷积层用于对输入的图像进行特征提取

**池化层:** 池化层通过对卷积层输出的特征图进行约减，实现了下采样。同时对感受域内的特征进行筛选，提取区域内最具代表性的特征，保留特征图中最主要的信息。

**激活函数:** 激活函数给神经元引入了非线性因素，对输入信息进行非线性变换，从而使得神经网络可以任意逼近任何非线性函数，然后将变换后的输出信息作为输入信息传给下一层神经元。

**全连接层:** 全连接层用于对卷积神经网络提取到的特征进行汇总，将多维的特征映射为二维的输出。其中，高维代表样本批次大小，低维代表分类或回归结果。

**池化的几种常见方法包括**: 平均池化、最大池化、K-max池化

* Average Pooling: 计算区域子块所包含所有像素点的均值，将均值作为平均池化结果
* Max Pooling: 从输入特征图的某个区域子块中选择值最大的像素点作为最大池化结果
* K-max Pooling: 对输入特征图的区域子块中像素点取前K个最大值，常用于自然语言处理中的文本特征提取

**归一化层:** Batch Norma

**线性层:** Linear

**卷积算子:**

* 标准卷积
* 1\*1卷积
* 3D卷积
* 转置卷积
* 空洞卷积
* 分组卷积
* 可分离卷积
* 可变型卷积