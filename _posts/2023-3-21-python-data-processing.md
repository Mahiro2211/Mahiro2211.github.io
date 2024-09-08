---
layout: post
title: Pyhton进行数据分析前三章总结
categories: [Python, Numpy]
description: 包含了一些常用的python处理数据的技巧
keywords: Python, Numpy
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---
# 目录
1. [numpy 中的 eye函数](#numpy-中的-eye函数)
2. [numpy 中创建对角矩阵](#numpy-中创建对角矩阵)
3. [Ipython快捷键](#ipython快捷键)
4. [魔术命令](#魔术命令)
5. [pyth on中的赋值号](#python中的赋值号)
6. [is 和 == 的区别](#is-和--的区别)
7. [python 中的字符串](#python-中的字符串)
8. [python 中的类型转换](#python-中的类型转换)
9. [python 中的二分查找(bisect库)](#python-中的二分查找bisect库)
10. [python快速获取逆序列表](#python快速获取逆序列表)
11. [python 枚举 enumerate(x , num)](#python-枚举-enumeratex--num)
12. [怎么不显示？——python中的zip函数](#怎么不显示——python中的zip函数)
13. [将行的列表转换为列的列表](#将行的列表转换为列的列表)
14. [python 中的字典（哈希表）](#python-中的字典哈希表)
15. [python 中的集合set 和数学上的概念一致](#python-中的集合set-和数学上的概念一致)
16. [python 中的map函数](#python-中的map函数)
17. [python 中的迭代器与生成器](#python-中的迭代器与生成器)
18. [python 中的 rstrip()](#python-中的-rstrip)


# numpy 中的 eye函数
```python
import numpy as np
I = np.eye(N, M=None, k=0, dtype=float)
#其中，N是矩阵的行数，M是矩阵的列数。如果M未指定，则默认为N。k是对角线的偏移量，如果k=0，则表示主对角线，k>0表示主对角线上方的对角线，k<0表示主对角线下方的对角线。dtype是矩阵的数据类型，默认为float。
```

# numpy 中创建对角矩阵
```python
arr = np.diag(list_name , k = 0)
#k为对角线偏移量
```

# Ipython快捷键
* ctrl + k 删除光标之前的
* CTRL + u 删除本行之前的
* ctrl + s 历史
* ctrl + a 开头 + e jiewei + b（back） + f（forward）


# 魔术命令
* %run xxx.py 在ipython中运行
* %run -i xxx.py 在ipython中加载模块
* %pwd 工作路径
* %who 列表 %whos 分类 %who_ls
* %xdel variable 删除这个变量
* %debug 调试
>>> n：执行下一行代码。
>>> s：进入当前行的函数。
>>> c：继续执行代码直到下一个断点或程序结束。
>>> p 变量名：打印变量的值。
>>> q：退出调试模式。

# python中的赋值号
* 赋值号不是真正的给值，**而是对一块相同内存的引用**
* 如果想要两块内容一样但是内存地址不同的变量，要用copy的
## is 和 == 的区别
    is是用来判断两个对象是否指向同一个地址 ， 即他们是否属于对象
    ==只是判断两个值是否相等
# python 中的字符串
    是不可变的的，不能修改
    可以通过str()实现其他变量转化为字符串变量
# python 中的类型转换
    可以直接用**类型(variable)**来实现
    连接两个列表也可以用extend()
    sort和sorted的区别也就是是不是改变原先的变量
# python 中的二分查找(bisect库)
    ```python
    import bisect
    c = [1,2,2,3,3,3,4]
    # bisect.bisect(a , x , lo = 0 , hi = len(a))
    bisect.bisect(c , 2) # 输出3 , 保持表示插入2后仍然保持有序的位置，一般来说就是后一位
    # bisect.bisect_left(a , x , lo = 0 , hi = len(a)) 
    # 已排序的序列中插入x后仍然保持有序的位置，如果存在了就返回第一个的位置
    # 同理bisect_right
    # insort_right方法，在已排序序列a中插入x 保持序列有序，跟bisect一样

    bisect.bisect(c , 5)
    ```
# python快速获取逆序列表
```python
a = a[::-1]
```
# python 枚举 enumerate(x , num)
# 怎么不显示？——python中的zip函数
```python
In [5]: a = [ 'foo' , 'bar' , 'vz']

In [6]: b = [ 'one' , 'two' , 'three']

In [7]: c = zip(a , b)

In [8]: c
Out[8]: <zip at 0x2e570536140>

In [9]: print(c)
<zip object at 0x000002E570536140>

In [10]: print(list(c))
[('foo', 'one'), ('bar', 'two'), ('vz', 'three')]

In [11]: print(dict(c))
{}

In [12]: print(tuple(c))
()
#为什么之后不显示了呢？ 这是因为转换为列表之后，c对象的迭代已经完成
```
* zip(a , b)会生成一个迭代器 ， 每次迭代返回一个元组
```python
In [24]: a = ['foo', 'bar', 'vz']^M
    ...: b = ['one', 'two', 'three']^M
    ...: for i, (a_val, b_val) in enumerate(zip(a, b)):^M
    ...:     print('{0}: {1}, {2}'.format(i, a_val, b_val))^M
    ...:
0: foo, one
1: bar, two
2: vz, three
```
# 将行的列表转换为列的列表
```python
In [25]: a = [ 11 , 22 , 33]

In [26]: b = [ 44 , 55 , 66]

In [27]: pp = [(11,22) , (33,44) , (55,66)]

In [28]: new_a , new_b = zip(*pp)
# 这里要用到拆包符号* ， 这样我们传递进去的参数就可以被正确的拆解成3个元组
# 如果这里不加拆包符号的话 ， 我们传递进去的就是三个元素 最后的结果会变成
# new_a = ([11, 22], [33, 44], [55, 66])
# new_b = ()

In [29]: new_a
Out[29]: (11, 33, 55)

In [30]: new_b
Out[30]: (22, 44, 66)
```

# python 中的字典（哈希表）
    可以使用in来询问是否存在键
    属性 keys()  values()
    方法 add() , insert() , pop() 
## 使用hash()函数检测是否可以哈希化 -- 是否可以用来当字典的键
        字典的键必须是不可变的对象

# python 中的集合set 和数学上的概念一致
方法 ： a.union(b) 并集 a | b   a.update(b) a|=b
        a.intersection(b) 交集   a.intersection_update(b) a &= b
        a.issubset(b) a.issuperset(b)
        a.isdisjoint(b)
# python 中的map函数
```python
map(function , iterable)
```

# python 中的迭代器与生成器
```python
In [14]: for i , v in some_dict.items() :
    ...:     print(i , v)
    ...:
a 1
b 2
c 3
In [15]: dict_iterator = iter(some_dict)
In [16]: dict_iterator
Out[16]: <dict_keyiterator at 0x20eb48c3540>
In [17]: c = list(dict_iterator)
In [18]: c
Out[18]: ['a', 'b', 'c']
# 迭代器

# 生成器构造新的可遍历对象 ， 惰性的返回一个多结果序列 ， 在每个元素产生后暂停
# 只需将函数返回值从return 改变为 yield

# itertools 模块中的 groupby()
# 将迭代器中的对象按照规则进行分组
# 以下为使用默认规则排序
import itertools
a = [1,2,2,2,3,3,3,4,4,4,2,2,2,2]
for key, values in itertools.groupby(a):
    print(key, list(values))
#以下为使用自定义规则分类
import itertools
# 自定义key函数，根据奇偶性进行分类
def custom_key_func(x):
    if x % 2 == 0:
        return "even"
    else:
        return "odd"
a = [1, 2, 3, 4, 5, 6, 7, 8, 9]
for key, group in itertools.groupby(a, key=custom_key_func):
    print(key, list(group))
# itertools 中的 permutation(iterator , k = None) 
# 根据列表的每一个元素提供给与其他元素形成的k元组
# 样例 ： 输入n 输出n的全排列个数
import itertools
n = int(input())
seq = list(range(1, n+1))
perms = sorted(itertools.permutations(seq))
for perm in perms:
    print(''.join(str(num) for num in perm))
```
# python 中的 rstrip()
```python
import string
a = 'wdjoiwdjoiadaaaaa'
In [62]: a = a.rstrip('a')
In [63]: a
Out[63]: 'wdjoiwdjoiad'
```