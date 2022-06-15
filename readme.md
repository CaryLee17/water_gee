[English](README.md) 中文版
# water_gee v1.0
<img src="" style="width:550px">

`Water_gee`存储库存储了基于Python端实现的深度学习结合GEE实现的水体提取方法。`Water_gee`可实现的功能主要有瓦片影像、水体标签获取、水体标签噪声纠正、模型训练以及GEE水体预测等。`Water_gee`

## 主要解决问题

* **解决大区域自动化水体提取问题**
* **解决深度学习的在线部署问题**

## 主要功能

* **瓦片影像获取**：GEE平台只支持最大512×512像元尺度的瓦片图像下载。通过遍历研究区样带影像，逐影像建立0.135°×0.135°瓦片矢量，对影像数据进行裁切，从而获取小瓦片影像存储在本地。图1展示了部分瓦片影像目录。
<p align="center" style="width:1000px">
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/tile_images.png" style="width:1000px"></br>
<strong>图1.瓦片影像目录</strong>
</p>

* **水体标签噪声纠正**：用上述方法获得遥感影像质量评估波段中的水体信息，并存储在本地。在地物光谱呈正态分布的假设下，构建Pixel-based CNN水体提取模型，实现水体的标签自学习以及噪声纠正。图2、图3分别为水体标签噪声纠正前后的水体标签信息。
<p align="center" style="width:1000px">
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/mask.png" style="width:1000px"></br>
<strong>图2.标签噪声纠正前</strong></br>
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/label.png" style="width:1000px"></br>
<strong>图3.标签噪声纠正后</strong>
</p>

* **模型训练**：构建并初始化Pixel-based CNN模型。利用文件名将之前的到的瓦片影像以及瓦片标签进行对应，裁切中心像元邻域7×7范围内的训练数据并匹配水体标签。为避免水体和非水体训练数据数量相差较大，非水体按一定比例生成，要求水体非水体数据体量相当。图4为Pixel-based CNN模型示意图，其右侧的为具体的模型参数。</br>
<img align="left" src="https://github.com/CaryLee17/water_gee/blob/main/images/CNN.png" style="width:500px">

```
Layer (type)                    Output Shape         Param 
===========================================================
input_1 (InputLayer)            [(None, 7, 7, 8)]    0     
conv2d (Conv2D)                 (None, 5, 5, 16)     1168  
conv2d_1 (Conv2D)               (None, 3, 3, 32)     4640  
tf_op_layer_strided_slice (Tens (None, 3, 3, 8)      0     
concatenate (Concatenate)       (None, 3, 3, 40)     0     
conv2d_2 (Conv2D)               (None, 1, 1, 64)     23104 
tf_op_layer_strided_slice_1 (Te (None, 1, 1, 8)      0     
concatenate_1 (Concatenate)     (None, 1, 1, 72)     0     
conv2d_3 (Conv2D)               (None, 1, 1, 128)    9344  
conv2d_4 (Conv2D)               (None, 1, 1, 2)      258   
===========================================================
Total params: 38,514
```
<p>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp<strong>图4.Pixel-based CNN模型架构示意图</strong>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp<strong>模型参数</strong></p>

* **GEE水体预测**：
<p align="center" style="width:550px">
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/result.png" style="width:550px"></br>
<strong>图4.水体提取结果</strong>
</p>

## 依赖的环境

**与GEE实现数据交互**
```
ee
geemap
```

**影像数据处理**
```
pandas
numpy
keras
osgeo
```

**多线程操作**
```
threading
concurrent
```

**其他**
```
os
time
```

## 使用方法

