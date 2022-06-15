[English](README.md) 中文版
# water_gee v1.0
<img src="" style="width:550px">

`Water_gee`存储库存储了基于Python端实现的深度学习结合GEE实现的水体提取方法。`Water_gee`可实现的功能主要有瓦片影像、水体标签获取、水体标签噪声纠正、模型训练以及GEE水体预测等。`Water_gee`

## 主要解决问题

* **解决大区域自动化水体提取问题**
* **解决深度学习的在线部署问题**

## 主要功能

* **瓦片影像获取**：
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/tile_images.png" style="width:550px">

* **水体标签噪声纠正**：
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/mask.png" style="width:550px">
<center>图1.标签噪声纠正前</center>
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/label.png" style="width:550px">
图2.标签噪声纠正后

* **模型训练**：
* **GEE水体预测**：
<img src="" style="width:550px">

## 依赖的环境

**与GEE实现数据交互**
* ee
* geemap

**影像数据处理**
* pandas
* numpy
* keras
* osgeo

**多线程操作**
* threading
* concurrent

**其他**
* os
* time
