---
layout: post
title: 自动化使用GradCAM处理图片(用于ViT和Swintransformer)附Github源码
categories: [Python, DeepLearning]
description: 在实验室期间处理实验结果数据编写的自动脚本
keywords: Python, DeepLearning
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
topmost: true
---
# 目录
1. [GradCAM_On_ViT](#gradcam_on_vit)
2. [如何在 GradCam 中调整 XXXFormer](#如何在-gradcam-中调整-xxxformer)
   - [请确保您的模型格式正确。](#请确保您的模型格式正确)
   - [获取你的目标层](#获取你的目标层)
   - [为什么我们选择LayerNorm作为目标层？](#为什么我们选择layernorm作为目标层)
3. [使用EigenCam示例](#使用eigencam示例)
4. [需要注意的参数](#需要注意的参数)
5. [链接](#链接)
6. [Method](#method)


# GradCAM_On_ViT
用于可视化模型结果的 GradCAM 自动脚本
# 如何在 GradCam 中调整 XXXFormer
## 请确保您的模型格式正确。
<p>如果您应用的ViT是类似 swin（无ClassToken）或类似 ViT （有ClassToken）
  </p>
  <p>张量的形状可能看起来像<em>[Batch,49,768]</em>，那么你应该按照以下步骤处理你的模型，以避免一些可怕的<strong>运行时错误</strong>
  </p>
 
```python

Class XXXFormer(nn.Moudle):
    def __init(self,...):
        super().__init__()
        .....
        self.avgpool = nn.AdaptiveAvgPool1d(1) #this is essential
    def forward(self,x):
        x = self.forward_feartrue(x) # Supose that the out put is [Batch,49,768]
        x = self.avgpool(x.transpose(1,2)) # [Batch,49,768] --> [Batch,768,49] --> [Batch,768,1]
        x = torch.flatten(x,1) # [Batch,768]
```

## 获取你的目标层
<p>找到最后一个transformer block并选择 LayerNorm() 属性作为目标层，如果您有多个 LayerNorm() 属性，您可以将它们全部放在列表中或仅选择其中一个</p>
<p>您的目标图层可能如下所示</p>
 
 ```python
# choose one LayerNorm() attribute for your target layer
target_Layer1 = [vit.block[-1].norm1]
target_Layer2 = [vit.block[-1].norm2]
# or stack up them all
target_Layer3 = [vit.block[-1].norm1,vit.block.norm2]
 ```
### 为什么我们选择LayerNorm作为目标层？ 
### Reference: On the Expressivity Role of LayerNorm in Transformer's Attention (ACL 2023).
<p>The reason may be like this as shown in the picture</p>

![解释](assets\images\cam_post\cam2.png)





* Automatic_Swim_variant_CAM.py
* Automatic_ViT_variant_CAM.py
 
上面显示的两个 .py 文件是您需要运行的主要 Python 脚本
只需设置图像文件并运行这两个脚本即可！

### 使用EigenCam示例

![示例](assets\images\cam_post\cam_1.png)




### 需要注意的参数

```python
parser.add_argument('--path', default='./image', help='the path of image')
parser.add_argument('--method', default='all', help='the method of GradCam can be specific ,default all')
parser.add_argument('--aug_smooth', default=True, choices=[True, False],
                    help='Apply test time augmentation to smooth the CAM')
parser.add_argument('--use_cuda', default=True, choices=[True, False],
                    help='if use GPU to compute')
parser.add_argument(
    '--eigen_smooth',
    default=False, choices=[True, False],
    help='Reduce noise by taking the first principle componenet'
         'of cam_weights*activations')
parser.add_argument('--modelname', default="ViT-B-16", help='Any name you want')
```

链接:https://github.com/Mahiro2211/GradCAM_Automation

|Method|
|-----|
| CrossFormer (ICLR 2022) |
| Vision Transformer (ICLR 2021) |


