# 评价指标

## Intersection over Union (IoU)

两个box区域的交集比上并集

* 首先计算两个box左上角点坐标的最大值和右下角坐标的最小值
* 然后计算交集面积
* 最后把交集面积除以对应的并集面积

<figure><img src="../.gitbook/assets/dl-basics-5-14.png" alt=""><figcaption></figcaption></figure>

## mAP
