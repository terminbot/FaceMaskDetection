# FaceMaskDetection
## [updates]
### 人脸口罩检测，现开源所有主流框架模型和推理代码，支持的框架如下：

 - [x] PyTorch
 
**检测人脸并判断是否佩戴了口罩， 并开源近8000张人脸口罩标注数据**

Detect faces and determine whether they are  wearing mask.

* 开源了标注的7971张人脸标注图片，数据集来自于
* [WIDER Face](http://shuoyang1213.me/WIDERFACE/)
* [MAFA](http://www.escience.cn/people/geshiming/mafa.html)
* 我们重新修改了标注并进行了校验(主要是MAFA和WIDER Face的人脸位置定义不一样，所以我们进行了修改标注)并将其开源出来。

## 模型结构
我们在本项目中使用了SSD类型的架构，为了让模型可以实时的跑在浏览器以及终端设备上，**我们将模型设计的非常小，只有101.5万个参数**。模型结构在本文附录部分。

本模型输入大小为260x260，主干网络只有8个卷积层，加上定位和分类层，一共只有24层（每层的通道数目基本都是32\64\128），所以模型特别小，只有101.5万参数。模型对于普通人脸基本都能检测出来，但是对于小人脸，检测效果肯定不如大模型。
[跑在您浏览器的口罩检测模型](https://demo.aizoo.com/face-mask-detection.html)

网页使用了Tensorflow.js库，所以模型是完全运行在您浏览器里面的。运行速度的快慢，取决于您电脑配置的高低。

模型在五个卷积层上接出来了定位分类层，其大小和anchor设置信息如下表.


| 卷积层 | 特征图大小 | anchor大小 | anchor宽高比（aspect ratio）|
| ---- | ---- | ---- | ---- |
|第一层|33x33|0.04,0.056|1,0.62,0.42|
|第二层|17x17|0.08,0.11|1,0.62,0.42|
|第三层|9x9|0.16,0.22|1,0.62,0.42|
|第四层|5x5|0.32,0.45|1,0.62,0.42|
|第五层|3x3|0.64,0.72|1,0.62,0.42|


### pytorch
如果您要运行图片：
```
python pytorch_infer.py  --img-path /path/to/your/img
```
如果您要在视频上跑，只需要：
```
python pytorch_infer.py --img-mode 0 --video-path /path/to/video  
# 如果要打开本地摄像头, video_path填写0就可以了，如下
python pytorch_infer.py --img-mode 0 --video-path 0
```

### 模型结构图
为了可视化方便，我们省略了BN层，如果您要查看完整模型，可以查看`img`文件夹的`face_mask_detection.hdf5.png`图片

![](img/face_mask_detection.caffemodel.png)

### 测试集PR曲线

因为WIDER face是一个任务比较复杂的数据集，我们的模型又设计的非常小，所以对于人脸的PR曲线并不是那么性感。这点可以通过设计大模型来提升对于小人脸的检测效果。
![](img/pr_curve.png)









