---
layout: wiki
title: numpy.apply_along_axis用法
cate1:
cate2:
description: 避免使用for循环操作高维数组
keywords: Python, Numpy
type:
link:
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# 场景
设想我有一列高维向量，读取之后的数据都是字符串变量，我需要把这些字符串数据转换为复数之后求绝对值
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/58e119aad2c6471c8b833cf0d6a0c41e.png)
## 实际操作
在使用pd.read_csv()读取数据之后，将这一列数据转换为numpy数组
```python
data = pd.read_csv(path,header=3).to_numpy() # header表示读取数据的时候要跳过前多少行，我这里需要调过录制的备注信息
print(data.shape) # [12800,1]
```
## 编写相关函数
```python
convert_s_c = lambda x : np.abs(np.complex128(x[0].replace('i','j')))
```
针对每个元素，取到第一个元素就是对应的字符串，替换成j后才能正常的从str对象转换成np.complex复数对象，然后才能使用绝对值进行操作
## np.apply_along_axis
```python
ccc = np.apply_along_axis(convert_s_c,-1,ccc)
```
参数从左到右依次是，需要应用到对应元素的函数，操作的维度，被操作的numpy数组对象
打印形状之后发现正常工作，比写for循环更加可读
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/ce4ce7e2e31a43f295b1d2c841817c24.png)
