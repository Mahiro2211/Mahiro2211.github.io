---
layout: post
title: 从0使用python实现线性回归
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
1. [线性回归的从零开始实现](#线性回归的从零开始实现)
2. [生成数据集](#生成数据集)
3. [绘制散点图](#绘制散点图)
4. [读取数据集](#读取数据集)
5. [模型初始化](#模型初始化)
6. [定义模型](#定义模型)
7. [定义损失函数（均方误差）](#定义损失函数均方误差)
8. [定义优化算法](#定义优化算法)
9. [训练](#训练)

# 线性回归的从零开始实现
# linear regression from scratch


```python
import random
import numpy
import torch
import numpy as np
import torch.nn as nn
from d2l import torch as d2l
```

# 生成数据集


```python
def synthetic_data(w , b , num_examples) : #
    """生成y = X * w + b + noise"""
    X = torch.normal(mean=0 , std=1 , size=(num_examples , len(w)) )#生成正态分布
    y = torch.matmul(X , w) + b # matmul矩阵乘法 mv只支持2维张量和一维张量
    y += torch.normal(0 , 0.01 , y.shape) #误差
    return X , y.reshape((-1,1))
```


```python
true_w = torch.tensor([2 , -3.4])
true_b = 4.2
feature , labels = synthetic_data(true_w , true_b , 1000)
```


```python
print('feartures:' , feature[0] , '\n normal:' , labels[0])
```

    feartures: tensor([0.2132, 0.3922]) 
     normal: tensor([3.2994])
    

# 绘制散点图


```python
d2l.set_figsize()
d2l.plt.scatter(feature[: , 1].detach().numpy(), labels.detach().numpy() , 1)
#计算图中分离张量，并返回一个新的张量，这个新的张量和原来的张量的数值相同，但是不再与计算图相关联，也就是说，这个新的张量不会影响原来张量的梯度计算。
```


```python
d2l.set_figsize()
d2l.plt.scatter(feature[: , 0].detach().numpy(), labels.detach().numpy() , 1)
```

# 读取数据集


```python
def data_iter(batch_size , features , labels) :
    num_examples = len(features)
    indice = list(range(num_examples))
    random.shuffle(indice)
    for i in range(0 , num_examples , batch_size) :
        batch_indice = torch.tensor(indice[i:min(i + batch_size , num_examples)])
        yield features[batch_indice] , labels[batch_indice]
```


```python
features , labelss = synthetic_data(true_w  ,true_b , 1000)
for x , y in data_iter(batch_size , features , labelss) :
    print(x , '\n' , y)
    break

```

# 模型初始化


```python
w = torch.normal(mean=0 , std=0.01 , size=(2,1) , requires_grad=True) #是一个列向量
b = torch.zeros(1 , requires_grad = True)
```

# 定义模型


```python
def linreg(X , w , b):# 线性变换，等价于nn.linear(in_dim , out_dim)
    return torch.matmul(X , w) + b
```

# 定义损失函数（均方误差）


```python
def squared_loss(y_hat , y):
    return (y_hat - y.reshape(y_hat.shape)) ** 2 / 2
```

# 定义优化算法


```python
def sgd(params , lr , batch_size):
    with torch.no_grad():
        for param in params :
            param -= lr * param.grad/batch_size
            param.grad.zero_()
```

# 训练


```python
# 示例模型
class LinearRegressionModel(nn.Module) :
    def __init__(self , in_dim , out_dim) :
        super(LinearRegressionModel , self).__init__() # 继承
        self.linear = nn.Linear(in_dim , out_dim)# 输入维度和输出维度 输入进(batch_size , in_dim )输出(batch_size ,out_dim)
    def forward(self , x) :
        out = self.linear(x)
        return out
    def forward(self , x) :
        out = self.linear(x)
        return out
```


```python
#从0开始
# 配置参数
lr = 0.03 # 超参数——学习率
num_epochs = 3 # 3轮次
net = linreg
loss = squared_loss
```


```python
for epoch in range(num_epochs) :
    for X , y in data_iter(batch_size , features , labelss) :
        l = loss(net(X , w , b) , y) # 计算损失函数 最后l是一个(batch_size , 1)的二维张量， 
        l.sum().backward()#这里的sum其实就是变相的降维
        sgd([w,b] , lr , batch_size) #梯度下降法更新梯度，使得损失函数最小
    with torch.no_grad():
        train_l = loss(net(features , w , b) , labelss)
        print(f'epoch {epoch + 1} , loss {float(train_l.mean()): f}')
```

