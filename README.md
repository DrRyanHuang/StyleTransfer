中文|[English](./README_EN.md)

# Style Transfer



本项目 Fork 自：https://github.com/dxyang/StyleTransfer

原项目使用 `Python2.7`和 `PyTorch 0.2`，本项目使用 `Python3.6+`和   `PyTorch 1.6+`进行改写



------------



本项目是论文 [Perceptual Losses for Real-Time Style Transfer and Super-Resolution](https://arxiv.org/abs/1603.08155) 的`PyTorch`实现. 该文章训练了一种 “图像转换网络” 来执行样式转换，而不是像最初由[Gatys et al.](https://arxiv.org/abs/1508.06576)提出的那样沿着图像的流形进行优化.

图像转换网络(**image transformation network**) 结构如下图所示：

<img src="figure/model.png" height="300"/>

对于给定的样式图像，使用[MS-COCO数据集](http://cocodataset.org/#download)训练网络，最大程度地减少感知损失，同时通过总方差对其进行归一化(此处需要深入代码). 

感知损失是由 **特征重建损失** 以及来自`VGG16`的预训练层的**样式重建损失**的组合定义的。

特征重建损失是特征之间的均方误差，而样式重建损失是特征图的Gram矩阵之差的Frobenius范数平方。





## Prerequisites
- Python 2.7
- [PyTorch 0.2.0](http://pytorch.org/)
- [NumPy](http://www.numpy.org/)
- [PIL](http://pillow.readthedocs.io/en/3.1.x/installation.html)



## Usage
### 训练

You can train a model for a given style image with the following command:

```bash
$ python style.py train --style-image "path_to_style_image" --dataset "path_to_coco"
```

Here are some options that you can use:
* `--gpu`: 你想使用的GPU id (不指定则将在CPU上训练)
* `--visualize`: visualize the style transfer of a predefined image every 1000 iterations during the training process in a folder called "visualize"

因此，要使用`mosaic.jpg`作为我的样式图像在GPU上进行训练，同时，MS-COCO已下载到名为coco的文件夹中，若要在整个训练过程中可视化样本图像，我可以使用以下命令：

```bash
$ python style.py train --style-image style_imgs/mosaic.jpg --dataset coco --gpu 1 --visualize 1
```

### 评估

You can stylize an image with a pretraind model with the following command. Pretrained models for mosaic.jpg and udine.jpg are provided.

```bash
$ python style.py transfer --model-path "path_to_pretrained_model_image" --source "path_to_source_image" --target "name_of_target_image"
```

You can also specify if you would like to run on a GPU:
* `--gpu`: id of the GPU you want to use (if not specified, will train on CPU)

For example, to transfer the style of mosaic.jpg onto maine.jpg on a GPU, I would use:

```bash
$ python style.py transfer --model-path model/mosaic.model --source content_imgs/maine.jpg --target maine_mosaic.jpg --gpu 1
```



## Results

### Mosaic
Model trained on mosaic.jpg applied to a few images: 
<p align="center">
    <img src="style_imgs/mosaic.jpg" height="200px">
</p>

<p align="center">
    <img src="content_imgs/amber.jpg" height="200px">
    <img src="content_imgs/dan.jpg" height="200px">
    <img src="content_imgs/maine.jpg" height="200px">
</p>

<p align="center">
    <img src="figure/mosaic_amber.jpg" height="200px">
    <img src="figure/mosaic_dan.jpg" height="200px">
    <img src="figure/mosaic_maine.jpg" height="200px">
</p>
这里有一个gif文件，展示了在训练过程中output的变化. 

<p align="center">
    <img src="figure/mosaic_amber.gif" height="200px">
    <img src="figure/mosaic_dan.gif" height="200px">
    <img src="figure/mosaic_maine.gif" height="200px">
</p>


### Udine
Model trained on udine.jpg applied to a few images: 
<p align="center">
    <img src="style_imgs/udnie.jpg" height="200px">
</p>

<p align="center">
    <img src="content_imgs/amber.jpg" height="200px">
    <img src="content_imgs/dan.jpg" height="200px">
    <img src="content_imgs/maine.jpg" height="200px">
</p>

<p align="center">
    <img src="figure/udnie_amber.jpg" height="200px">
    <img src="figure/udnie_dan.jpg" height="200px">
    <img src="figure/udnie_maine.jpg" height="200px">
</p>

这里有一个gif文件，展示了在训练过程的1000次迭代中output的变化. 

<p align="center">
    <img src="figure/udnie_amber.gif" height="200px">
    <img src="figure/udnie_dan.gif" height="200px">
    <img src="figure/udnie_maine.gif" height="200px">
</p>


## Acknowledgements

* This repo is based on code found in [this PyTorch example repo](https://github.com/pytorch/examples/tree/master/fast_neural_style)
