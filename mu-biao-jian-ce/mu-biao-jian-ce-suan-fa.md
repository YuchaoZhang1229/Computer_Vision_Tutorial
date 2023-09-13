# 目标检测算法

* Faster-RCNN
  * 有预选框 RPN
  * 速度通常较慢 (5FPS), 但是效果通常还是不错
  * 了解Mask-RCNN![image-20230911160747327](C:%5CUsers%5Cuser%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230911160747327.png)
* YOLO
  * 不用预选框 RPN
  * 速度非常快, 适合做实时检测任务
  * 效果不太好
*
*
*
* FPS, mAP值, IoU
  * 精度-检测的框与实际符不符合, 召回率-有没有框没检测到
  * $$
    IoU = \frac{Area of Overlap}{Area of Union}
    $$
