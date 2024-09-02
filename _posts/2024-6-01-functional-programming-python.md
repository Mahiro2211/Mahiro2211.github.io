---
layout: post
title: Python函数式编程窥探(Fluent Python)
categories: []
description: 阅读Fluent Python所写的笔记
keywords: Python
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

## 目录

- [函数式编程](#函数式编程)
- [把函数当作对象](#把函数当作对象)
- [高阶函数](#高阶函数)
    - [map 的替代品](#map-的替代品)
    - [reduce 的替代品](#reduce-的替代品)
    - [filter 的替代品](#filter-的替代品)
- [匿名函数](#匿名函数)
- [可以向函数一样可调用的对象](#可以向函数一样可调用的对象)
- [自定义的调用类型](#自定义的调用类型)
- [函数内省](#函数内省)
- [传递给函数的参数：从定位参数到仅限关键字参数](#传递给函数的参数从定位参数到仅限关键字参数)
- [获取关于函数参数的信息--inspect 模块](#获取关于函数参数的信息--inspect-模块)
    - [获取函数签名的 signature 方法](#获取函数签名的-signature-方法)
    - [inspect.Signature 对象的 bind 方法](#inspectsignature-对象的-bind-方法)
- [Python3 的一个特性——函数注解](#python3-的一个特性函数注解)
- [支持函数式编程的包 (operator，functools)](#支持函数式编程的包-operatorfunctools)
    - [另一种冻结参数的方法 functools.partial](#另一种冻结参数的方法-functoolspartial)
- [小结](#小结)



# 函数式编程

## 把函数当作对象

函数式编程是把函数作为一等公民，把一些算数运算符当作函数使用,python不是一门纯粹的函数式编程语言，但是在一些库的加持下（operator,functools）使得他的函数式编程功能同样强大

在python中我们会把函数当作对象使用

```python
def foo(x):
    """
    x * x * x
    """
    return x * x * x

print(foo(3))
print(foo.__doc__) # python 中的一种特殊方法用于查看函数的注解
```

## 高阶函数

在其他语言的函数式编程中经常使用map，reduce在python中也可以使用，不过python对着两种方法都有更便捷的实现方式

### map的替代品

map方法可以应用python的列表表达式得到更简便更可读的实现

```python
# map 和 列表表达式
def square(x):
    return x * x
square_one_to_ten = map(square,range(1,11))

print(list(square_one_to_ten))

square_one_to_ten = [square(i) for i in range(1,11)]
print(square_one_to_ten)
# 输出
c:/Users/Administrator/GithubRepo/study_recording/fluent_python/ch05/test_case.py
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

### reduce的替代品

reduce方法在python3之后就被移除了内置方法，我们可以在functools中找到这个函数，我们可以使用python中的sum()方法作为替代品，而且sum方法的效率更加高效

```python
# reduce 和 sum
# 计算累计求和
from functools import reduce
from operator import  add
acumulation = reduce(add , range(0,100))# 0 - 99 sum
new_trick = sum(range(0,100))
print(acumulation == new_trick) # True
```

sum 和 reduce 的特点是应用某一种操作到指定序列上，累计之前的结果，把一个系列值归约成一个值
除此之外 python 中的all()和any()方法也是一种归约函数，前者是只要序列中都不为0返回True后者是只要有真值就返回True

### filter的替代品

过滤一些序列中的元素我们同样可以用列表表达式来实现

```python
# filter and list generator
format1 = filter(lambda x : x % 2 == 0,range(11))
format2 = [i for i in range(11) if i % 2 == 0]

print(f'user filter : {list(format1)}\nuse list generator : {format2}')
# 输出
# user filter : [0, 2, 4, 6, 8, 10]
# use list generator : [0, 2, 4, 6, 8, 10]
```

同样的结果我们可以使用列表表达式来减少lambda函数的使用

## 匿名函数

python中的匿名函数无法对传入的变量进行赋值，它只能是纯表达式的形式
除了作为参数传递给一些高阶函数，平凡的lambda函数容易写出，难的lambda表达式就难以阅读

## 可以向函数一样可调用的对象

python中判断一个对象是否可以被调用，可以调用其内置的callable()方法

```python
# callable
from operator import add
print([callable(i) for i in [add, str , 1]]) # [True, True, False]
```

## 自定义的调用类型

在python中一切皆为对象，不仅函数可以表现得像对象，甚至对象也可以表现得像函数，我们只需要去实现python对象中的__call__这个实例方法

```python
# callable object
import random

class RandomNumberSelector():
    def __init__(self) -> None:
        self._item = list(range(100))
        random.shuffle(self._item)
    def __call__(self):
        return self._item[random.randint(0,100)]

obj = RandomNumberSelector()
print(obj())
print(obj())
```

## 函数内省

除了__call__和__doc__函数还有其他很多的属性，使用dir方法可以了解一个函数还有那些属性

```python
print([i for i in dir(square)])
['__annotations__', '__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__get__', '__getattribute__', '__globals__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__kwdefaults__', '__le__', '__lt__', '__module__', '__name__', 
'__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
```

我们也可以了解对象有哪些属性

```python
print([i for i in dir(RandomNumberSelector)])
['__call__', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
```

同时我们也可以查看二者不共存的属性

```python
print(sorted(set(dir(square)) - set(dir(RandomNumberSelector))))
['__annotations__', '__closure__', '__code__', '__defaults__', '__get__', '__globals__', '__kwdefaults__', '__name__', '__qualname__']
```

以上是类的属性中没有而函数属性有的属性

我们主要介绍与把函数视为对象的一些方法比如__dict__属性，是一种为函数提供注解的属性，Django框架中对函数赋予了这种注解的属性

## 传递给函数的参数：从定位参数到仅限关键字参数

python提供了及其灵活的参数处理方式,python3又提供了一种**仅限关键字参数**，同时密切相关的有*和**符号，可以展开可迭代对象这个过程就是拆包的过程
*符号，会把没有明确指定名称关键字的参数捕获作为列表传入，**符号会把没有明确指定名称的关键字参数捕获作为字典输入函数

```python
# Example
def tag(name,*content,cls=None,**attrs):
    """生成一个或者多个HTML标签"""
    if cls is not None:
        attrs['class'] = cls
    if attrs :
        attrs_str = ' '.join('%s="%s"' % (attr,value)
                        for attr , value
                        in sorted(attrs.items()))
    else :
        attrs_str = ''
    if content:
        return '\n'.join('<%s %s>%s</%s>' % 
                         (name , attrs_str , c ,name) for c in content)
    else :
        return '<%s%s />' % (name , attrs_str)
  
print(tag("br"))
print(tag("p","hello","world!"))
print(tag("h1","hello world!"))
print(tag('div',"FOo",cls="frame",id="dow")) # cls只能作为关键字参数传入
print(tag('div',"FOo",id="dow"))
```

输出 ：

```
<br />
<p >hello</p>
<p >world!</p>
<h1 >hello world!</h1>
<div class="frame"id="dow">FOo</div>
<div id="dow">FOo</div>
```

## 获取关于函数参数的信息--inspect模块

函数对象具有__default__属性，其值是一个元组，保存着**定位参数和关键字参数**的默认值，仅限**关键字参数**的默认值存放在__kwdefaults__属性中,参数的名称存放在__code__属性中，其是一个code对象的引用，本身也有很多属性

```python
# Example
def clip(text,max_len=80):
    """在max_len前面或者后面的第一个空格处截断文本"""
    end = None
    if len(text) > max_len:
        space_before = text.rfind(' ',0,max_len) # rfind函数的第二个参数beg规定从哪里开始搜索，如果不设置则默认从尾部开始搜索
        if space_before >= 0:
            end = space_before
        else :
            space_after = text.rfind(' ',max_len)
            if space_after >= 0 :
                end = space_after
        if end is None:
            end = len(text)
        return text[:end].rstrip() # 去掉右边的空格部分

```

### 获取函数签名的signature方法

```python
from inspect import signature
sig = signature(clip)

print(sig) # 输出 ： (text, max_len=80)

for name, param in sig.parameters.items():
    print(f'name:{name} ,param:{param}')
    # name:text ,param:text
    # name:max_len ,param:max_len=80
```

signature方法返回了一个inspect.Signature对象，它有一个paramerters属性，对应了一个有序映射，是字典。把参数的名字和inspect.Parameter对象对应起来,每个Parameter都有自己的属性

### inspect.Signature对象的bind方法

```python
sig = signature(tag)
my_tag = {"name":'p',"content":["Hello","World!"],
          "cls":'news',"attrs":{"id":1}}
bound_args = sig.bind(**my_tag)
print(bound_args)
# <BoundArguments (name='p', cls='news', attrs={'content': ['Hello', 'World!'], 'attrs': {'id': 1}})>
for name,value in bound_args.arguments.items():
    print(name , value)
# name p
# cls news
# attrs {'content': ['Hello', 'World!'], 'attrs': {'id': 1}}
```

Signature对象有一个bind方法可以把任意个参数绑定到签名中的形参上，这里通过输入可以发现，当输入的形参含有序列信息的时候，这个方法会把序列信息给**attrs参数捕获，存入一个字典

## Python3 的一个特性——函数注解

python3可以在函数声明和返回值附加元数据

```python
def func(foo:str) -> int:
	return int(foo)
```

这种注解不会对函数做任何处理，只会存储在__annotations__属性中（一个字典）

```python
for k , v in func.__annotations__.items():
    print(f'{k} = {v}')
# foo = <class 'str'>
# return = <class 'int'>
```


## 支持函数式编程的包(operator，functools)

函数式编程中经常需要把算术运算符当作函数使用，operator包提供了完整的算术运算符的函数

使用operator中的mul,add，配合reduce可以实现累乘或者累加

例如使用mul函数来实现阶乘

```python
from operator import mul
from functools import reduce
def fact(n):
	return reduce(mul,range(1,n+1))
```

operator库中除了有算数运算符还有一些能从序列中读取对象属性的方法，分别是itemgetter和attrgetter顾名思义分别是读取对象索引和读取对象参数，本质上是一些简单lambda表达式的更有可读性的实现

operator中余下的模块中还有一个methodcaller方法，适用于对一个对象使用指定参数的方法

```python
s = 'upper these characters'
from operator import methodcaller
uppercase = methodcaller('upper')
s = uppercase(s)
print(s) # UPPER THESE CHARACTERS

replace_backspace = methodcaller('replace',' ','___')
s = replace_backspace(s)
print(s) # UPPER___THESE___CHARACTERS
```

最后一个print可以发现methodcaller还有冻结部分参数的功能

### 另一种冻结参数的方法functools.partial

冻结参数的本质其实就是将一个函数的部分参数应用于一个对象

```python
from functools import partial
from operator import mul

multiply_3 = partial(mul,3)
print(multiply_3(21)) # 63
```

partial的第一个参数是要可执行的一个方法，后面紧跟的是关键字参数或者不定参数

# 小结

Python本身不是一门函数式编程语言，但是它参考了一些函数式编程语言很好的地方，除了可以写出更可读的代码外。还能用它来实现一些特定功能，本身也提供了强大的注解系统和函数和对象之间的灵活调用
