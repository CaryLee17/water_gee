English [中文版](README_zh.md) 
# Water_gee v1.0
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/figure.png" style="width:1000px">

`Water_gee` repository stores a water extraction methods based on deep learning methods and `GEE` with Python language. The functions that can be achieved by `Water_gee` are mainly tile images acquisition, water body label acquisition, water body label noise correction, model training, and water body prediction using `GEE`. `Water_gee`

## Main Purpose

* **Solving the problem of automated water extraction in large areas**
* **Solving deep learning deployment problems on GEE platforms**

## Main Functions

* **Tile image acquisition**: The `GEE` platform only supports the download of tile images at a maximum of 512 × 512 pixel scale. By traversing the sample strip images in the study area, 0.135°×0.135° tile vectors are created image by image, and the image data is cropped to obtain small tile images for local storage. Figure 1 shows a partial tile image catalog.
<p align="center">
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/tile_images.png" style="width:100%"></br>
<strong>Figure 1. Tile image catalog</strong>
</p>

* **Water label noise correction**：The information of water bodies in the quality assessment bands of remote sensing images is obtained by the above method and stored locally. Under the assumption that the feature spectra are normally distributed, a `Pixel-based CNN` water body extraction model is constructed to realize the label self-learning as well as noise correction of water bodies. Figure 2 and Figure 3 show the water body label information before and after the water body label noise correction, respectively.
<p align="center">
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/mask.png" style="width:100%"></br>
<strong>Figure 2. Before label noise correction</strong></br>
<img src="https://github.com/CaryLee17/water_gee/blob/main/images/label.png" style="width:100%"></br>
<strong>Figure 3. After label noise correction</strong>
</p>

* **Model Training**：Construct and initialize the `Pixel-based CNN` model. The previously obtained tile images as well as the tile labels are corresponded using the file names, cropping the training data in the 7×7 range of the central image element neighborhood and matching the water body labels. In order to avoid the large difference between the number of water body and non-water body training data, the non-water body is generated in a certain proportion, and the volume of water body non-water body data is required to be comparable. Figure 4 shows the schematic diagram of the `Pixel-based CNN` model, and the right-hand side of it shows the specific model parameters.</br>
<img align="left" src="https://github.com/CaryLee17/water_gee/blob/main/images/CNN.png" style="width:400px">

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
<p><strong>Figure 4. Schematic diagram of Pixel-based CNN model </strong>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp<strong>Model Parameters</strong></p>



* **GEE water body Projection**：The `input`, `conv2d`, `concatenate`, and `slice` in the structure of the `Pixel-based CNN` model correspond to the `ee.Image`, `ee.Kernel.convolve`, `ee.Image.cat`, and `ee.Image. select` modules in GEE. The implementation of the model conversion function in Python enables the conversion of models with CNN-based deep learning architectures to `GEE` format. Using this conversion function, the acquisition of weight information and the cloud deployment are achieved.

<img align='right' src="https://github.com/CaryLee17/water_gee/blob/main/images/result.png" style="width:400px">
  
```
Layer (type)                    Output shape             Connected to               GEE module
=====================================================================================================
input_1 (InputLayer)          [(None, 7, 7, 8)]  ee.Image
conv2d (Conv2D)                (None, 5, 5, 16)  input_1[0][0]                     ee.Kernel.convolve
conv2d_1 (Conv2D)              (None, 3, 3, 32)  conv2d[0][0]                      ee.Kernel.convolve
tf_op_layer_strided_slice      (None, 3, 3, 7)   input_1[0][0]                     ee.Image.select
concatenate (Concatenate)      (None, 3, 3, 39)  conv2d_1[0][0]                    ee.Image.cat
                                                 tf_op_layer_strided_slice[0][0]	
conv2d_2 (Conv2D)              (None, 1, 1, 64)  concatenate[0][0]                 ee.Kernel.convolve
tf_op_layer_strided_slice_1    (None, 1, 1, 7)   input_1[0][0]                     ee.Image.select
concatenate_1 (Concatenate)    (None, 1, 1, 71)  conv2d_2[0][0]                    ee.Image.cat
                                                 tf_op_layer_strided_slice_1[0][0]	
conv2d_3 (Conv2D)              (None, 1, 1, 128) concatenate_1[0][0]               ee.Kernel.convolve
conv2d_4 (Conv2D)              (None, 1, 1, 2)   conv2d_3[0][0]                    ee.Kernel.convolve
=======================================================================================================
```
</br></br>
<p>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp<strong>Conversion parameters</strong>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp<strong>Figure 5. Water body extraction results</strong></p>


## Dependent Environment

**Data interaction with GEE**
```
ee
geemap
```

**Image Data Processing**
```
pandas
numpy
keras
osgeo
```

**Multi-threaded operation**
```
threading
concurrent
```

**Others**
```
os
time
```

## Usage
The code of the `ipynb` version mainly consists of `get_images.ipynb`, `train_cnn.ipynb` and `gee_predict.ipynb` three files. The composition structure of each file is shown below. Necessary comments are made in the file for the places that need to be modified, and other parts can also be customized by users.
* **`get_images.ipynb`**: for acquiring tile images, QA band generated water body labels, and noise corrected water body labels.
```
|---get_images.ipynb
    |---summer_img
    |---generate_grid
    |---project
        |---predict
        |---get_array_from_image
        |---write_tiff
        |---CNN
        |---write_grid_image
        |---cloud_free        
```
* **`train_cnn.ipynb`**: Initialize and train the `Pixel-based CNN` water body extraction model using the acquired sample strip images and labeled dataset.
```
|---train_cnn.ipynb
    |---read_tiff
    |---features
    |---get_input
    |---CNN
    |---generate_inputs        
```
* **`gee_predict.ipynb`**：It is used for model weight acquisition and cloud weight deployment, and the high computing power of `GEE` is used to realize the deep learning model computing in the cloud. The water body extraction results are stored in `Assets` of `Google Earth Engine`.
```
|---gee_predict.ipynb
    |---cloud_free
    |---summer_img_bounds
    |---features
    |---pixel_based_CNN
    |---relu       
```
***
# Water_gee v1.0 is the initial ipynb version, there may be omissions, later will continue to improve (encapsulated into a Python package).

