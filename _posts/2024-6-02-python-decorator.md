---
layout: post
title: Python装饰器和闭包入门
categories: [Python]
description: 闭包和装饰器的入门
keywords: Python
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# 目录
1. [Python函数式编程进阶：函数装饰器和闭包介绍](#python函数式编程进阶函数装饰器和闭包介绍)
   - [一个简单的装饰器实现和行为表现](#一个简单的装饰器实现和行为表现)
   - [装饰器通常会把函数替换成另一个函数](#装饰器通常会把函数替换成另一个函数)
   - [Python导入模块时首先就会运行装饰器](#python导入模块时首先就会运行装饰器)
   - [闭包](#闭包)
   - [__closure__属性可以查看闭包的自由变量](#__closure__属性可以查看闭包的自由变量)
   - [总结](#总结)
   - [nonlocal声明](#nonlocal声明)



# Python函数式编程进阶：函数装饰器和闭包介绍

## 一个简单的装饰器实现和行为表现

函数装饰器用于在源代码中“标记”一个函数，增强它的行为。理解他的前提是理解闭包

先来看一个装饰器的例子

```python
def decorator(func):
    print('Running -->{}'.format(func.__name__))
    return func

@decorator
def add(a,b):
    return a + b

print(add(1,2))
# 输出
# Running -->add
# 3
```

对于一个函数target，和其装饰器decorator，在运行target函数时本质上就是运行target = decorator(target)
我们去掉这个装饰符号后运行一下代码

```python
def decorator(func):
    print('Running -->{}'.format(func.__name__))
    return func

# @decorator
def add(a,b):
    return a + b

print(decorator(add))
# 输出
# Running -->add
# <function add at 0x00000217C90D7280>
```

这下我们就发现了使用装饰符的好处了，我们可以增强这个函数的行为，只需要和之前一样直接传参数给这个就可以了

以上只是一个简单的例子，是一个很平凡的装饰器。接下来我们观察一个稍微复杂一点的例子

## 装饰器通常会把函数替换成另一个函数

```python
def deco(func): 
    def inner():
        print('Running --> inner()')
    return inner
@deco
def target():
    print('Running --> target()')
  
print(target) # <function deco.<locals>.inner at 0x00000132A1427310>
target() # Running --> inner()
```

此时target函数的行为被替换成了装饰器中定义的函数了,而且我们查看target函数的时候发现其已经变成了装饰器中定义的函数inner的引用

## Python导入模块时首先就会运行装饰器

装饰器在被装饰函数定义之后立刻运行

```python
registry = []

def register(func):
    registry.append(func)
    print('Running register(%s)' % func)
    return func

@register
def func1():
    print('Running func1()')

@register
def func2():
    print('Running func2()')
  
@register
def func3():
    print('Running func3()')

print(registry)
func1()
func2()
func3()
# Running register(<function func1 at 0x0000021E0A9B7280>)
# Running register(<function func2 at 0x0000021E0A9B7310>)
# Running register(<function func3 at 0x0000021E0A9B73A0>)
# [<function func1 at 0x0000021E0A9B7280>, <function func2 at 0x0000021E0A9B7310>, <function func3 at 0x0000021E0A9B73A0>]  
# Running func1()
# Running func2()
# Running func3()
```

## 闭包

如果有提前了解过全局变量和局部变量，那么就可以知道闭包是什么意思了，它其实指的是**延伸了作用域的函数**
在函数内部又定义了一个新的函数

```python
def make_average():
    series = []
    def averager(new_value):
        series.append(new_value)
        total = sum(series) / len(series)
        return total
    return averager

avg = make_average()

print(avg(10)) # 10
print(avg(20)) # 15 
print(avg(30)) # 20
```

观察这个函数，我们发现，除了函数有的输入输出特性之外，这个函数还可以保存历史的值
series是make_average()的局部变量，在averager函数的作用域之外
在定义avg变量时，函数内部已经对series这个变量进行了初始化
在averager这个函数中 series 这个变量叫做**自由变量**，特指没有在averager这个函数作用域中绑定的函数
可以通过查看__code__(表示编译后的函数定义体)属性来检查函数有哪些局部变量和自由变量

```python
print(avg.__code__.co_varnames) # 局部变量
print(avg.__code__.co_freevars) # 自由变量 
# 输出
# ('new_value', 'total')
# ('series',)
```

### __closure__属性可以查看闭包的自由变量

```python
print(avg.__closure__)  # 自由变量组成的元组
print(avg.__closure__[0]) 
print(avg.__closure__[0].cell_contents) # 查看cell_contents属性
# 输出 
# (<cell at 0x0000016AA4E70B50: list object at 0x0000016AA4E17940>,)
# <cell at 0x0000016AA4E70B50: list object at 0x0000016AA4E17940>
# [10, 20, 30]
```

### 总结

闭包是一种函数，它会保存在函数定义时存在的自由变量，这样调用函数时虽然定义作用域不可用了，但仍能使用绑定的变量

## nonlocal声明

python中列表是一种可变类型，而数字，字符串时不可变类型

```python
def make_average():
    count = 0
    total = 0
    def averager(new_value):
        count += 1
        total += new_value
        return total / count
    return averager
avg = make_average()
print(avg(1))
```

以上代码中，count += 1 其实就是 count = count + 1
也就是说我们在对这个变量进行赋值，意味着它会变成这个函数里头的一个局部变量（函数会隐式的创建count变量），所以在这个函数的作用域中，没有定义count变量，会报错
列表是可变的对象，因为我们没有给它重新赋值而是调用了append之类的方法添加元素
像数字字符串元组这种的都是不可变类型，只能读取不能更新
python3中引入的nonlocal声明就是把变量标记为自由变量

```python
def make_average():
    count = 0
    total = 0
    def averager(new_value):
        nonlocal count , total
        count += 1
        total += new_value
        return total / count
    return averager
avg = make_average()
print(avg(1)) # 1
print(avg(3)) # 2 
print(avg(5)) # 3
```
