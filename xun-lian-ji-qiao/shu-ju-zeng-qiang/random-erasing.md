# Random Erasing

## Random Erasing

* 与 cutout 非常类似, 也是一种模拟物体遮挡情况的数据增强方法
* 区别在于, cutout是把图片中抽取的矩形区域的像素值设置为0, 相当于裁剪掉
* 而 random erasing 使用随机数或者数据集中的像素平均值替换原来的像素值
* 并且 cutout 每次裁剪掉的区域大小是固定的, random erasing 替换掉的区域大小是随机的
