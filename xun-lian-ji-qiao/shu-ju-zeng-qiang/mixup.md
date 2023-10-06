# Mixup

## Mixup

* 数据增强方法
* 每次取出两张图片, 然后将他们线性组合, 得到新的图片, 以此来作为新的训练样本, 进行网络训练, 如下公式, 其中x代表图像数据, y代表标签, 则得到的新的 xhat, yhat

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

Mixup方法主要增强了训练样本之间的线性表达, 增强了网络的泛化能力, 不过mixup方法需要较长的时间才能收敛得比较好
