---
layout: post
title: Python标准库常用数据模型(Set,Deque,Collections)
categories: [Python]
description: Python常用标准库数据模型
keywords: Python
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---
# 目录
1. [集合](#集合)
   - [创建集合](#创建集合)
   - [集合操作 （交 并 补）](#集合操作-交-并-补)
   - [处理集合之间的关系](#处理集合之间的关系)
2. [Collections模块](#collections模块)
   - [counter](#counter)
   - [namedtuple](#namedtuple)
   - [OrderedDict](#ordereddict)
   - [deque](#deque)

你可以将这个目录放在文档的开头，方便读者快速导航到各个部分。

## 集合
### 创建集合
```python
# unordered mutable 
myset = set() # 方式1 创建一个空集合，随后使用add方法添加元素

myset.add(1)
myset.add(2)
myset.add(3)

evens = {2,3,4,5} # 方式2 没有key的字典就是一个集合
```

### 集合操作 （交 并 补)

```python
unity = myset.union(evens) # 并
interset = myset.intersection(evens) # 交
diff = myset.difference(evens) # 不同元素
print('------------------')
print('集合操作')
print(f'unity is {unity}')
print(f'interset is {interset}')
print(f'diffset is {diff}')
print('-------------------')
myset = {i for i in range(5)}
evens = {i for i in range(3,9)}
print(f'myset is {myset} \nevens is {evens}')

diffset = myset.difference(evens) # 返回两个集合中不同的部分
print(f'diffset is {diffset} \n')

symdiffset = myset.symmetric_difference(evens) # 返回两个集合中不共存的元素
print(f'symmetric difference is {symdiffset}') # no same elements
print('------------------------')
```

### 处理集合之间的关系
```python
print('表达关系')
# 复制
setA = {i for i in range(2,8)}
setB = setA
setB.add(9)
print(setB == setA) # setB = setA只是引用

setC = {2,3,4}
# 表达关系
print(setA.isdisjoint(setC)) # if intersection is NULL elements 如果交是空
print(setA.issuperset(setC)) # setA是否是setC的超集，涵盖他的元素
print(setC.issubset(setA)) # setC是否是setA的子集

# imutable set 不可变集合
setD = frozenset([]) # 如果修改会发生KeyError
```
## Collections模块
Collections是python标准库中的一个很常用的模块，内置了双端队列等现成的数据结构
### counter
```python
import collections as co

# counter
listABC = ['a'] * 5 + ['b'] * 3 + ['c'] * 2
string_abc = ''.join(listABC)

counter_abc = co.Counter(string_abc)
print(counter_abc)
print(counter_abc.most_common())
print(counter_abc.most_common(1)) # index
print(list(counter_abc.elements())) # ['a', 'a', 'a', 'a', 'a', 'b', 'b', 'b', 'c', 'c']
```
这是一种会把列表中出现的元素按照出现次数作为value元素本身作为key的字典
```python
# example
import collections as co

lista = ['a'] * 2 + ['b'] * 1 + ['c'] * 3

counter = co.Counter(lista)

print(counter) # Counter({'c': 3, 'a': 2, 'b': 1})
```

### namedtuple
这个类用来构建只有少数属性但是没有方法的对象
```python
# Namedtuple
Card = co.namedtuple('Card','color,value')
Point = co.namedtuple('Point',['x','y'])

poker_1 = Card('heart','A')
pt_1 = Point(1,2)
print('poker : {} \npoint : {}'.format(poker_1,pt_1))
```
### OrderedDict

```python
# OrderedDict 记住添加顺序的字典，3.7版本后没区别
order_dict = co.OrderedDict()
order_dict['b'] = 2
order_dict['c'] = 1
order_dict['d'] = 45
order_dict['a'] = 2
print(order_dict) # OrderedDict([('b', 2), ('c', 1), ('d', 45), ('a', 2)])
```
### deque
```python
# Deque 双端队列
dq = co.deque()

dq.append(1)
dq.appendleft('hello')
# two way to remove the element from deque
# dq.pop()
# dq.popleft()
print(dq) # deque(['hello', 1])
dq.extend(['你好','Keria'])
dq.extendleft(['你好','Zeus']) # 注意顺序 先入在后，是队列的性质
print(dq) # deque(['Zeus', '你好', 'hello', 1, '你好', 'Keria'])
dq.rotate(1)
# rotate之后队列向右平移1位
# deque(['Zeus', '你好', 'hello', 1, '你好', 'Keria'])
# deque(['Keria', 'Zeus', '你好', 'hello', 1, '你好'])
print(dq)
```
