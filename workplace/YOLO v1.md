# YOLO v1

## 什么是IoU(Intersection over Union)
​      IoU是一种测量在特定数据集中检测相应物体准确度的一个标准。IoU是一个简单的测量标准，只要是在输出中得出一个预测范围(bounding boxex)的任务都可以用IoU来进行测量。为了可以使IoU用于测量任意大小形状的物体检测，我们需要：

ground-truth bounding boxes（人为在训练集图像中标出要检测物体的大概范围）
我们的算法得出的结果范围。
       也就是说，这个标准用于测量真实和预测之间的相关度，相关度越高，该值越高。如下图所示。绿色标线是人为标记的正确结果（ground-truth），红色标线是算法预测的结果（predicted）。

![img](https://img-blog.csdnimg.cn/20190114221032593.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dhb3l1MTI1MzQwMTU2Mw==,size_16,color_FFFFFF,t_70)

### 1、IoU的计算 
IoU是两个区域重叠的部分除以两个区域的集合部分得出的结果，通过设定的阈值，与这个IoU计算结果比较。

![img](https://img-blog.csdnimg.cn/20190114221649458.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dhb3l1MTI1MzQwMTU2Mw==,size_16,color_FFFFFF,t_70)

举例如下：绿色框是准确值，红色框是预测值。


![img](https://img-blog.csdnimg.cn/20190114221957853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dhb3l1MTI1MzQwMTU2Mw==,size_16,color_FFFFFF,t_70)

![image-20220912190031876](YOLO v1.assets/image-20220912190031876.png)

![image-20220912190225216](YOLO v1.assets/image-20220912190225216.png)

## 预测阶段

![image-20220912191451165](YOLO v1.assets/image-20220912191451165.png)

![image-20220912190356599](YOLO v1.assets/image-20220912190356599.png)

**线的粗细代表置信分数**

![image-20220912190704931](YOLO v1.assets/image-20220912190704931.png)

**类别框，保存了置信分数最高的框的类**

## 训练阶段

![image-20220912191940814](YOLO v1.assets/image-20220912191940814.png)

![image-20220912191915065](YOLO v1.assets/image-20220912191915065.png)

![image-20220912192013692](YOLO v1.assets/image-20220912192013692.png)

![image-20220912192046089](YOLO v1.assets/image-20220912192046089.png)

![image-20220912192223140](YOLO v1.assets/image-20220912192223140.png)

**计算bound box框出的置信分数**

![image-20220912192440990](YOLO v1.assets/image-20220912192440990.png)

## 目标检测处理

![image-20220912201122365](YOLO v1.assets/image-20220912201122365.png)

![image-20220912201411591](YOLO v1.assets/image-20220912201411591.png)

### 非最大值抑制NMS(Non-Maximum Suppression)

![image-20220912201448359](YOLO v1.assets/image-20220912201448359.png)

![image-20220912201616355](YOLO v1.assets/image-20220912201616355.png)

![image-20220912201627456](YOLO v1.assets/image-20220912201627456.png)

![image-20220912201844293](YOLO v1.assets/image-20220912201844293.png)

![image-20220912201935248](YOLO v1.assets/image-20220912201935248.png)

![image-20220912202141559](YOLO v1.assets/image-20220912202141559.png)

![image-20220912204709020](YOLO v1.assets/image-20220912204709020.png)

![image-20220912204659202](YOLO v1.assets/image-20220912204659202.png)

  ## 训练阶段（反向传播）

一个grid cell 只能预测出一个物体，49个grid cell 只能预测出49个物体

![image-20220912210048124](YOLO v1.assets/image-20220912210048124.png)

### 损失函数

![image-20220912211747330](YOLO v1.assets/image-20220912211747330.png)