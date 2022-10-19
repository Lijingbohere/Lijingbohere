# Yolov5核心基础知识



**（1）输入端：**Mosaic数据增强、自适应锚框计算、自适应图片缩放
**（2）Backbone：**Focus结构，CSP结构
**（3）Neck：**FPN+PAN结构
**（4）Prediction：**GIOU_Loss

## 输入端

**（1）Mosaic数据增强**

**Mosaic数据增强**则采用了4张图片，**随机缩放、随机裁剪、随机排布**的方式进行拼接。



![img](https://pic4.zhimg.com/80/v2-dddc368bc1c8ec6239d152c609774673_720w.jpg)

主要有几个优点：

1. **丰富数据集：**随机使用**4张图片**，随机缩放，再随机分布进行拼接，大大丰富了检测数据集，特别是随机缩放增加了很多小目标，让网络的鲁棒性更好。
2. **减少GPU：**Mosaic增强训练时，可以直接计算4张图片的数据，使得Mini-batch大小并不需要很大，一个GPU就可以达到比较好的效果

**（2） 自适应锚框计算**

在Yolo算法中，针对不同的数据集，都会有**初始设定长宽的锚框**。

在网络训练中，网络在初始锚框的基础上输出预测框，进而和**真实框groundtruth**进行比对，计算两者差距，再反向更新，**迭代网络参数**。

**（3）自适应图片缩放**

在常用的目标检测算法中，不同的图片长宽都不相同，因此常用的方式是将原始图片统一缩放到一个标准尺寸，再送入检测网络中。

比如Yolo算法中常用**416\*416，608\*608**等尺寸，比如对下面**800\*600**的图像进行缩放。

![img](https://pic1.zhimg.com/80/v2-7cfa86448e0a543d613f2f8e64c63ce4_720w.jpg)

**第二步：计算缩放后的尺寸**

![img](https://pic3.zhimg.com/80/v2-2c9a77f1b484d49ce3beba9c70a2effe_720w.jpg)

原始图片的长宽都乘以最小的缩放系数0.52，宽变成了416，而高变成了312。

**第三步：计算黑边填充数值**

![img](https://pic3.zhimg.com/80/v2-799a7cb6bb7e5994472f0162abc4cf02_720w.jpg)

将416-312=104，得到原本需要填充的高度。再采用numpy中np.mod取余数的方式，得到8个像素，再除以2，即得到图片高度两端需要填充的数值。

**a.训练时没有采用缩减黑边的方式**，还是采用传统填充的方式，即缩放到416*416大小。**只是在测试，使用模型推理时，才采用缩减黑边的方式，提高目标检测，推理的速度。**

**b.为什么np.mod函数的后面用**32**？**因为Yolov5的网络经过5次下采样，而2的5次方，等于**32**。所以至少要去掉32的倍数，再进行取余。

## Neck

采用FPN+PAN的结构

![img](https://pic4.zhimg.com/80/v2-f903f571b62f07ab7c30d72d54e5e0c3_720w.jpg)

FPN层自顶向下传达**强语义特征**，而特征金字塔则自底向上传达**强定位特征**，两两联手，从不同的主干层对不同的检测层进行参数聚合

Yolov4的Neck结构中，采用的都是普通的卷积操作。而Yolov5的Neck结构中，采用借鉴CSPnet设计的CSP2结构，加强网络特征融合的能力。

![preview](https://pic4.zhimg.com/v2-d8d0ff4768a92c9a10adbe08241c0507_r.jpg)

## Prediction

目标检测任务的损失函数一般由**Classificition Loss（分类损失函数）**和**Bounding Box Regeression Loss（回归损失函数）**两部分构成。

Bounding Box Regeression的Loss近些年的发展过程是：**Smooth L1 Loss-> IoU Loss（2016）-> GIoU Loss（2019）-> DIoU Loss（2020）->CIoU Loss（2020）**

**IoU(Intersection over Union)**

**GIoU(generalized IoU)**

**DIoU(Distance-IoU)**

**CIoU( Complete-IoU**)

**a.IOU_Loss**

![img](https://pic3.zhimg.com/80/v2-c812620791de642ccb7edcde9e1bd742_720w.jpg)

可以看到IOU的loss其实很简单，主要是**交集/并集**，但其实也存在两个问题。

![img](https://pic4.zhimg.com/80/v2-e3d9a882dec6bb5847be80899bb98ea3_720w.jpg)

**问题1：**即状态1的情况，当预测框和目标框不相交时，IOU=0，无法反应两个框距离的远近，此时损失函数不可导，**IOU_Loss无法优化两个框不相交的情况。**

**问题2：**即状态2和状态3的情况，当两个预测框大小相同，两个IOU也相同，**IOU_Loss无法区分两者相交情况的不同。**

**b.GIOU_Loss**

![img](https://pic4.zhimg.com/80/v2-443123f1aa540f7dfdc84b233edcdc67_720w.jpg)

存在一种**不足**：

![img](https://pic3.zhimg.com/80/v2-49024c2ded9faafe7639c5207e575ed6_720w.jpg)

**问题**：状态1、2、3都是预测框在目标框内部且预测框大小一致的情况，这时预测框和目标框的差集都是相同的，因此这三种状态的**GIOU值**也都是相同的，这时GIOU退化成了IOU，无法区分相对位置关系。



好的目标框回归函数应该考虑三个重要几何因素：**重叠面积、中心点距离，长宽比。**

**c.DIOU_Loss**

![img](https://pic1.zhimg.com/80/v2-029f094658e87f441bf30c80cb8d07d0_720w.jpg)

DIOU_Loss考虑了**重叠面积**和**中心点距离**，当目标框包裹预测框的时候，直接度量2个框的距离，因此DIOU_Loss收敛的更快。

但就像前面好的目标框回归函数所说的，没有考虑到长宽比。

![img](https://pic4.zhimg.com/80/v2-22bf2e9c8a2fbbbb877e0f1ede69009f_720w.jpg)

比如上面三种情况，目标框包裹预测框，本来DIOU_Loss可以起作用。

但预测框的中心点的位置都是一样的，因此按照DIOU_Loss的计算公式，三者的值都是相同的。

**d.CIOU_Loss**

CIOU_Loss和DIOU_Loss前面的公式是一样的，不过在此基础上**增加了一个影响因子，将预测框和目标框的长宽比都考虑了进去。**

![img](https://pic2.zhimg.com/80/v2-a24dd2e0d0acef20f6ead6a13b5c33d1_720w.jpg)

其中v是衡量长宽比一致性的参数，我们也可以定义为：

![img](https://pic2.zhimg.com/80/v2-5abd8f82d7e30bdf21d2fd5851cb53a1_720w.jpg)

这样CIOU_Loss就将目标框回归函数应该考虑三个重要几何因素：重叠面积、中心点距离，长宽比

各个Loss函数的不同点：

**IOU_Loss：**主要考虑检测框和目标框重叠面积。

**GIOU_Loss：**在IOU的基础上，解决边界框不重合时的问题。

**DIOU_Loss：**在IOU和GIOU的基础上，考虑边界框中心点距离的信息。

**CIOU_Loss：**在DIOU的基础上，考虑边界框宽高比的尺度信息。