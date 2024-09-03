---
layout: fragment
title: 【深度学习图像处理】使用Kornia来更好的展示transform之后的图片
tags: [DeepLearning, Python]
description: 这个库的输入数据可以直接接受pytorch张量
keywords: Python,DeepLearning
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# Kornia介绍
kornia是一个计算机视觉算法库，数据增强的模块非常好用，可以使用它的数据增强模块完全无缝的嵌入到pytorch中而且不会和torchvision中的transforms模块冲突

以下展示我们的数据增强模块的实例
# 模块实例
```python
import torchvision.io as torchio
import torch
import torch.nn as nn 
import kornia.augmentation as Kg
from loguru import logger
import torch
import os
from augmentation import *
import glob
import torchvision
from torchvision import datasets, transforms
from PIL import Image
from matplotlib import pyplot as plt

class Augmentation(nn.Module):
    def __init__(self,org_size,Aw=1.0, *args, **kwargs) -> None:
        super(Augmentation,self).__init__(*args, **kwargs)
        self.gk = int(org_size * 0.1)
        
        if self.gk % 2 == 0 :
            self.gk += 1
            
        self.aug = nn.Sequential(
            Kg.RandomResizedCrop(size=(org_size, org_size), p=1.0*Aw),
            Kg.RandomHorizontalFlip(p=0.5*Aw),
            Kg.ColorJitter(brightness=0.4, contrast=0.8, saturation=0.8, hue=0.2, p=0.8*Aw),
            Kg.RandomGrayscale(p=0.2*Aw),
            Kg.RandomGaussianBlur((self.gk, self.gk), (0.1, 2.0), p=0.5*Aw),
        )
    def forward(self,x):
        return self.aug(x)
            

input_size = 224

Crop = nn.Sequential(Kg.CenterCrop(input_size))
Norm = nn.Sequential(Kg.Normalize(mean=torch.as_tensor([0.485, 0.456, 0.406]),
                                   std=torch.as_tensor([0.229, 0.224, 0.225]))
                     )             

AugS = Augmentation(input_size,Aw=1.0)
AugT = Augmentation(input_size,Aw=0.2)

normal_transform = transforms.Compose([
    transforms.ToTensor()
])

to_image_transform = transforms.Compose([
    transforms.ToPILImage()
])

```
# 数据读取
我们使用PIL来读取几张图片
如果你有一个根目录，下面有很多图片，我们可以使用secret库来随机抽取几张图来展示不同的transforms效果

```python
img_list = glob.glob('data\\train_transformed\\*.jpg')

import secrets
num = secrets.randbelow(500)
sample_img = Image.open(img_list[num])
sample_img = normal_transform(sample_img) # 将读取的对象转化为pytorch张量

```

# 使用numpy，matplotlib来可视化

```python

T_img = Norm(Crop(AugT(sample_img)))
S_img = Norm(Crop(AugS(sample_img)))
import numpy as np

# 输入之前我们需要将[3,224,224]的张量转化为[224,224,3]这样才会正常显示图片
npT = np.moveaxis(np.squeeze(T_img.detach().numpy()),0,-1)
npS = np.moveaxis(np.squeeze(S_img.detach().numpy()),0,-1)
org_img = np.moveaxis(np.squeeze(sample_img.detach().numpy()),0,-1)

# plt.imshow(npT)
# plt.imshow(npS)
# plt.imshow(org_img)
fig, axs = plt.subplots(1,3,figsize=(20,15))
# 显示每张图片
axs[0].imshow(npT)
axs[0].set_title("Teacher",fontsize=60)
axs[0].axis('off')  # 关闭坐标轴

axs[1].imshow(npS)
axs[1].set_title("Student",fontsize=60)
axs[1].axis('off')  # 关闭坐标轴

axs[2].imshow(org_img)
axs[2].set_title("Origin Image",fontsize=60)
axs[2].axis('off')  # 关闭坐标轴
plt.savefig(f'output{num}.jpg')
# 显示整个图像
plt.show()

```

# 效果展示
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/9fbe026d5cf74285a364285a7151756d.jpeg#pic_center)
		