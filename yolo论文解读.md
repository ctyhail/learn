# yolov1

## 摘要

yolo的意思是：you only look once。它是一个`one stage`且`端到端(end to end)`的模型，一次向前推断就可以得到`bouding box`定位和分类结果。

## 大概过程描述

### 训练阶段

模型会先把图片划分为`n*n`个`grid cell`，图片的`gruond truth`框的中心点会落在一个`grid cell`里，就有该`grid cell`来预测那个物体，为一个`grid cell`有B个`bounding box`，其中`bounding box`（以下简称bbox）与`ground truth`框的`IOU`(交并比)最大的bbox就负责预测该物体，该`grid cell`的其他bbox会被放弃，每个`gird cell`只能预测一个物体。

训练的损失函数为：

![image-20240327171757000](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240327171757000.png)

模型的网络结构为：

![image-20240323155610633](C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240323155610633.png)

网络中的激活函数为Leaky ReLU。如果要检测C个物体，经过网络最后得到一个n\*n*(B\*2)\*C形状的tensor，如上图所示7x7是`grid cell`的数量，在30中有20为预测物体数，一个bbox有5个参数，有4个是位置参数，还有一个是置信度。

### 预测阶段

网络得出的tensor中剩下的20就是物体分类的条件概率，有20个物体就有20个概率。条件概率与bbox的置信度相乘得到真正的概率，故而在这种条件下，会得到一个2\*20\*7\*7的关于概率的tensor。如图，第一排为dog的概率

<img src="C:/Users/20247/AppData/Roaming/Typora/typora-user-images/image-20240327165723623.png" alt="image-20240327165723623"  />



#### 后处理

设置一个阈值，小于这个阈值的概率会被直接抹零。

#### 非极大值抑制(Non-Maximum Suppression   NMS)

先把概率最高的bbox拿出来，与第二高的作比较，如果这两的bbox的`IOU`大于某个阈值，就认为这两个bbox重复识别一个物体，就把低置信度低概率的bbox抹零（如果想加强NMS，就可以把阈值设置低）。然后第三高的也会按上述步骤与第一高比较，直到最小的与最高的比较完，然后第二高的会与第三高的作比较，以此推类。每一个类别都会这样处理。

经过NMS后将结果可视化在图片上就是目标检测结果。

NMS值存在于预测阶段，没有出现在训练阶段。

## 

