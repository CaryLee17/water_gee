[English](README.md) 中文版
# water_gee v1.0
<img src="" style="width:550px">

`Water_gee`存储库存储了基于Python端实现的深度学习结合GEE实现的水体提取方法。`Water_gee`可实现的功能主要有瓦片影像、水体标签获取、水体标签噪声纠正、模型训练以及GEE水体预测等。`Water_gee`

## 主要解决问题

* **解决大区域自动化水体提取问题**
* **解决深度学习的在线部署问题**

## 主要功能

* **瓦片影像获取**：GEE平台只支持最大512×512像元尺度的瓦片图像下载。通过遍历研究区样带影像，逐影像建立0.135°×0.135°瓦片矢量，对影像数据进行裁切，从而获取小瓦片影像存储在本地。
<p align="center" style="width:1000px">
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/tile_images.png" style="width:1000px"></br>
图1.瓦片影像目录
</p>

* **水体标签噪声纠正**：用上述方法获得遥感影像质量评估波段中的水体信息，并存储在本地。在地物光谱呈正态分布的假设下，构建Pixel-based CNN水体提取模型，实现水体的标签自学习以及噪声纠正。
<p align="center" style="width:1000px">
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/mask.png" alt style="width:1000px"></br>
图2.标签噪声纠正前</br>
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/label.png" style="width:1000px"></br>
图3.标签噪声纠正后
</p>

* **模型训练**：
* **GEE水体预测**：
<p align="center" style="width:550px">
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/result.png" style="width:550px"></br>
图4.水体提取结果
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

