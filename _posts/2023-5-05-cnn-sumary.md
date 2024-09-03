---
layout: post
title: 学习卷积神经网络记录
categories: [Python, DeepLearning]
description: 相关知识集合
keywords: Python, DeepLearning
mermaid: false  
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
topmost: false
---


# 目录
1. [帮助收敛和泛化的一些做法](#帮助收敛和泛化的一些做法)
   - [数据归一化](#数据归一化)
   - [范数惩罚 ----> 权重衰减](#范数惩罚----权重衰减)
   - [丢弃法（dropout）随机的使神经元失活](#丢弃法dropout随机的使神经元失活)
2. [更加复杂的结构：深度](#更加复杂的结构深度)
   - [跳跃连接](#跳跃连接)
   - [参数初始化](#参数初始化)
   - [共享参数_创建一个shared层](#共享参数_创建一个shared层)
   - [为什么1*1的卷积层比全连接层更有效率](#为什么11的卷积层比全连接层更有效率)
   - [如何构建层和块](#如何构建层和块)
   - [参数访问](#参数访问)
   - [内置初始化](#内置初始化)
3. [读写文件](#读写文件)
4. [获取模型的子模块](#获取模型的子模块)
5. [图像检索方法](#图像检索方法)
6. [图像增广](#图像增广)




# 帮助收敛和泛化的一些做法

## 数据归一化

* 保持激活检查 ： 批量归一化 （正则化）
我们要的数据是输出在底部，底层的训练比较慢，而且底层的变化带动着所有层的变化，最后层的数据往往要学习多次，这样也导致他们的收敛会变慢

在求解梯度的时候，输入层的梯度是最大的

我们会尝试在学习底部层的的时候避免变化顶部层，因为顶层开始收敛的比较快，训练持续的时候，顶部变化，底部变化，顶部收敛快，底部收敛慢。

想法 ： 1 ， 固定小批量里面的均质和方差 ， 然后在调整学习参数，也就是超参数

$$\hat{z_i}=\frac{z_i-\mu}{\sqrt{\sigma^2+\epsilon}}$$   （归一化）
其中，$\gamma$是缩放参数，$\beta$是平移参数。
$$y_i=\gamma\frac{z_i-\mu}{\sqrt{\sigma^2+\epsilon}}+\beta$$
（批量归一化）

* 卷积层或者全连接层，输出之后直接贴了一个批量归一化，也就是在激活函数的前面 ， 之后就可以作为输入了， 把均质方差拉的比较好，不会变化太剧烈
* 全连接层，特征维度上的
>对于一个二维输入，每个行都是一个样本，每一列都是特征 ， 对每个特征进行运算一个均质和方差，他也会自己学习伽马和贝塔继续做校任
卷积输入（batch_size , hight , weight , channels) <-tensorflow


>对于1 * 1卷积的特殊情况，其实就是全连接层 ， 批量大小*高*宽其实就是每一个像素都是一个样本 ， 作用在通道层（每个像素都有通道）


* 本质上有点像在通道内部模糊化，或者声音里头掺噪音 （mu -> 随机偏移） （sigma -> 随机缩放）

* 所以没必要跟dropout使用 （可以调大学习率，收敛速度会比变快，但是不会改变模型精度）

## 范数惩罚 ----> 权重衰减

范数 ———— 表示一个向量有多大
L1 范数 ———— 所有权重的绝对值的和 —————— 通过小因子进行缩放

L2 范数 ———— 所有权重的平方和


```python
# L1 范数的实现 
def l1_Norm(model):
    output = model(img) ; l1_lamda = 1e-3 ; l1_norm = sum(p.abs().sum() for p in model.parameters())
# L2 范数只需要把abs() 换成 pow(2.0)
```

## 丢弃法（dropout）随机的使神经元失活

nn.DropoutXd(num)

# 更加复杂的结构：深度

### 跳跃连接

增加深度的同时也代表着训练更加收敛 ， 损失函数对参数的求导 ， 可能会因为很长的求导链式法则产生很多其他的数字 ， 这些数字可能会很小，也可能会很大

这样不断的求下去，可能会导致某些参数对梯度的贡献减小，导致某一些层的训练，最后是没有效果的

所以跳跃连接就是我们直接把输入，添加到层快的一个输出中 ， 比如说直接把第一个激活函数的输出，作为第3层的输入，这样我们就缓解了梯度消失的问题

我们也可以理解为创建了一个从深层参数（要跳跃的）到损失的直接路径 ， 使得他们对梯度的贡献更加直接了，

# 参数初始化

写一个封装函数
```python
def init_(m):
    if type(m)==nn.Linear :
        nn.init.constant_(m.weight ,mean , std)
        nn.init.zeros_()
model.apply()
```

共享参数_创建一个shared层

shared = nn.Linear()

是因为他的梯度叠加导致他对损失的贡献一直很明显，所以可以更好的拟合 ————跳跃连接

输入特征图的大小为H×W，卷积核的大小为K×K，则输出特征图的大小为 (H-K+1)×(W-K+1)。这种情况下，卷积层只进行特征提取，不改变特征图的深度，因为卷积核的深度与输入特征图的深度相同



如果想要保持输入特征图和输出特征图尺寸相同可以采用padding

### 为什么1*1的卷积层比全连接层更有效率

* 输入大小为N * H * C 的1*1卷积层，输出大小为C * K个 ， K 为输出通道数
* 输入大小为N * H * C 的全连接层 ，输出大小为 H * C * K ， K 为需要学习的偏执层（每个全连接层中的隐藏层都对应着一个偏执层
* 卷积计算具有权重共享，虽然是1*1叫卷积，但是依旧可以在通道维度上进行权重共享 ， 针对的是每个张量再输入通道上的权重
* 全连接层只在水平方向上具有连接性 ， 卷积层在水平和垂直 都具有连接性 ， 所以可以更好的进行拟合和泛化

#### 如何构建层和块
```python
class MLP(nn.Module): # 层
    def __init__(self):
        super().__init__()
        self.hidden = nn.Linear(20,256)
        self.out = nn.Linear(256,10)
    def forward(self , X):
        return self.out(F.relu(self.hidden(X))                      
```

```python
class MySequential(nn.Module): # 顺序块
    def __init__(self , *args):
        super().__init__()
        for block in args :
            self._modules[block] = block
    def forward(self , X):
        for block in self._modules.values():
            X = block(X)
        return X
```

#### 参数访问

state_dict() 查看weight和bias

bias引用 和 bias.data引用 bias.grad引用

通过name_parameters() 返回一个(name,param.shape)字典

net.add_module()可以添加一个字符串对网络的说明


#### 内置初始化

```python
def init_normal(m):
    if type(m) == nn.Linear:
        nn.init.normal_(m.weight , mean = 0 , std = 1e-2)
        nn.init.zeros_(m.bias) 
net.apply(init_normal) # 遍历整个神经网络

def init_constant(m):
    if type(m)==nn.Linear :
        nn.init.constant_(m.weight ,mean , std)
        nn.init.zeros_()

# 自定义线性层
class MyLinear(nn.Module):
    def __init__(self , in_units , units):
        super().__init__()
        self.weight = nn.Parameter(torch.randn(in_units , units))
        self.bias = nn.Parameter(torch.randn(units,))
    def forward(self , X):
        linear = torch.matuml(X , self.weight.data) + self.bias.data
        return F.relu(linear)
```

### 读写文件

```python
torch.save(x , 'x-file')
torch.load('filename')
torch.save(net.state_dict(),'filename')
```

# 获取模型的子模块
```python
model = models.alexnet(pretrained=True)
mode.featrues.children()
```


# 图像检索方法


```python
import random
import torch
```


```python
inputs = torch.randn(1 , 2 , 3 , 4)
```


```python
inputs
```




    tensor([[[[-0.5529, -0.0458, -1.3319, -0.1879],
              [-1.7151,  0.2929, -0.4752,  1.4707],
              [-2.7129,  0.8992,  0.9589, -1.2511]],
    
             [[-0.5911,  0.7812, -0.4372,  0.1149],
              [-0.6848, -1.0907,  0.2420, -1.4308],
              [ 1.7769, -0.3700, -2.9622, -0.3533]]]])




```python
# global pooling
m = torch.nn.AdaptiveAvgPool2d((6 , 6))
```


```python
output = m(inputs)
```


```python
output.size() ,  output
```




    (torch.Size([1, 2, 6, 6]),
     tensor([[[[-0.5529, -0.2993, -0.0458, -1.3319, -0.7599, -0.1879],
               [-0.5529, -0.2993, -0.0458, -1.3319, -0.7599, -0.1879],
               [-1.7151, -0.7111,  0.2929, -0.4752,  0.4977,  1.4707],
               [-1.7151, -0.7111,  0.2929, -0.4752,  0.4977,  1.4707],
               [-2.7129, -0.9069,  0.8992,  0.9589, -0.1461, -1.2511],
               [-2.7129, -0.9069,  0.8992,  0.9589, -0.1461, -1.2511]],
     
              [[-0.5911,  0.0950,  0.7812, -0.4372, -0.1612,  0.1149],
               [-0.5911,  0.0950,  0.7812, -0.4372, -0.1612,  0.1149],
               [-0.6848, -0.8878, -1.0907,  0.2420, -0.5944, -1.4308],
               [-0.6848, -0.8878, -1.0907,  0.2420, -0.5944, -1.4308],
               [ 1.7769,  0.7035, -0.3700, -2.9622, -1.6578, -0.3533],
               [ 1.7769,  0.7035, -0.3700, -2.9622, -1.6578, -0.3533]]]]))




```python
max_m = torch.nn.AdaptiveMaxPool2d((6 , 6))
```


```python
max_output = max_m(inputs)
```


```python
max_output.size() , max_output
```




    (torch.Size([1, 2, 6, 6]),
     tensor([[[[-0.5529, -0.0458, -0.0458, -1.3319, -0.1879, -0.1879],
               [-0.5529, -0.0458, -0.0458, -1.3319, -0.1879, -0.1879],
               [-1.7151,  0.2929,  0.2929, -0.4752,  1.4707,  1.4707],
               [-1.7151,  0.2929,  0.2929, -0.4752,  1.4707,  1.4707],
               [-2.7129,  0.8992,  0.8992,  0.9589,  0.9589, -1.2511],
               [-2.7129,  0.8992,  0.8992,  0.9589,  0.9589, -1.2511]],
     
              [[-0.5911,  0.7812,  0.7812, -0.4372,  0.1149,  0.1149],
               [-0.5911,  0.7812,  0.7812, -0.4372,  0.1149,  0.1149],
               [-0.6848, -0.6848, -1.0907,  0.2420,  0.2420, -1.4308],
               [-0.6848, -0.6848, -1.0907,  0.2420,  0.2420, -1.4308],
               [ 1.7769,  1.7769, -0.3700, -2.9622, -0.3533, -0.3533],
               [ 1.7769,  1.7769, -0.3700, -2.9622, -0.3533, -0.3533]]]]))



Global Average Pooling 更适合于场景中目标的位置变化不大的情况，而 Global Max Pooling 更适合于场景中目标的位置变化较大的情况。

### TripletLoss


```python
# 在pytorch中的对应API为 
loss_fn = torch.nn.TripletMarginLoss() # ·使用L1 距离 或者 L2距离
loss_fn_flexible = torch.nn.TripletMarginWithDistanceLoss() # 参数 ， 以及用于计算距离的函数
```

他的作用是最大话锚点样本和负样本的距离 ， 最小化锚点与正样本的距离

loss = max(margin + distance(anchor, negative) - distance(anchor, positive), 0)

### 在高维空间里头采用欧几里德距离的坏处
* 维度的增加导致距离膨胀 ， 数据的稀疏性会导致距离失效（空间分布是很散的，导致大家距离都很大，甚至比不出什么差异）

#### 补充
```python
torch.norm(tensor , p = 1 or 2) # p是制定l1还是l2
# 添加范数惩罚
for parameter in model.parameters():
    loss += lamda_  * torch.norm(...)
```
想控制输出范围在-1到1之间可以考虑使用tanh激活函数

#### 离散化
其中，等宽离散化是将连续的取值区间均等地划分成 k 个区间，每个区间的取值范围相等，数据会按照取值所在区间进行离散化；等频离散化是将数据等分为 k 个区间，每个区间内包含相同数量的样本，数据会按照取值所在区间进行离散化；聚类离散化则是将数据进行聚类分析，并将样本划分到不同的类别中，每个类别就是一个离散值。

## 图像增广


```python
path = '../../data-unversioned/p1ch7'
```


```python
import torch
import torchvision
import torch.nn as nn
from d2l import torch as d2l
all_images = torchvision.datasets.CIFAR10(train=True , root=path , download=False)
train_augs = torchvision.transforms.Compose([
    torchvision.transforms.RandomHorizontalFlip(),
    torchvision.transforms.ToTensor()
])
test_augs = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor()
])
```


```python
d2l.show_images([all_images[i][0] for i in range(32)] , 4 , 8 , scale=0.8)
```




    array([<AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>,
           <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>,
           <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>,
           <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>,
           <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>,
           <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>,
           <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>,
           <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>, <AxesSubplot:>],
          dtype=object)




    
[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-fvWXYrwP-1683293263326)(output_19_1.png)]
    



```python
def load_cifar(is_train , augs , batch_size):
    datasets = torchvision.datasets.CIFAR10(root=path , train=is_train , transform=augs , download=False)
    dataloader = torch.utils.data.DataLoader(datasets , batch_size=batch_size , shuffle=is_train 
                                             , num_workers=d2l.get_dataloader_workers)
    return dataloader
```


```python

```