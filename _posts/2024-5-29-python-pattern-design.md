---
layout: post
title: Python使用函数实现简单的设计模式
categories: [Python]
description: 函数式编程的实践
keywords: Python
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Content here


# 目录
1. [函数式编程进阶：用函数实现设计模式](#函数式编程进阶用函数实现设计模式)
   - [案例实现：构建“策略”模式](#案例实现构建策略模式)
   - [使用函数实现”策略“模式](#使用函数实现策略模式)
   - [享元](#享元)
   - [选择最佳策略：简单的方式](#选择最佳策略简单的方式)
   - [globals关键字](#globals关键字)


# 函数式编程进阶：用函数实现设计模式

## 案例实现：构建“策略”模式

策略模式：我们把一系列算法封装起来，使得他们可以相互替换，本模式可以独立于他们的客户而变化

```python
from abc import ABC, abstractmethod
from collections import namedtuple

Customer = namedtuple("Customer",'name fidelity')

class LineItem:
    def __init__(self,product,quantity,price) -> None:
        self.product = product
        self.quantity = quantity
        self.price = price
  
    def total(self):
        return self.quantity * self.price

class Order: # 上下文
    def __init__(self, customer, cart, promotion=None) -> None:
        self.customer = customer
        self.cart = list(cart)
        self.promotion = promotion

    def total(self):
        if not hasattr(self,'__total'):
            self.__total = sum(item.total() for item in self.cart)
        return self.__total
  
    def due(self):
        if self.promotion is None:
            discount = 0
        else:
            discount = self.promotion.discount(self)
        return self.total() - discount

    def __repr__(self) -> str:
        fmt = '<Order total: {:.2f} due: {:.2f}'
        return fmt.format(self.total,self.due)

class Promotion(ABC): #抽象基类
    @abstractmethod
    def discount(self, order):
        """返回折扣金额"""

class FidelityPromo(Promotion):
    def discount(self, order):
        """积分为1000以上的顾客提供5%的折扣"""
        return order.total() * .05 if order.customer.fidelity >= 1000 else 0

class BulkItemPromo(Promotion):
    def discount(self,order):
        """单个商品为20个或以上时提供10%折扣"""
        discount = 0 
        for item in order.cart:
            if item.quantity >= 20:
                discount += item.total() * .10
        return discount

class LargeOrderPromo(Promotion):
    """订单中的不同商品达到10个以上时提供7%折扣"""
    def discount(self,order):
        discount = 0
        for item in order.cart:
            if item.quantity >= 10:
                discount += item.total() * .07
        return discount
      
      
```

这个实例中我们实例化了所有的策略，还有客户订单，使用抽象基类和抽象方法装饰符来明确所用的方式。

测试以上用例

```python
joe = Customer('John Doe', 0)
ann = Customer('Ann Smith',1000)
cart = [LineItem('banana',4,.5),
        LineItem('apple',10,1.5),
        LineItem('watermelon',5,5.0)]

order_joe = Order(joe,cart,FidelityPromo())
order_ann = Order(ann,cart,FidelityPromo())
print(repr(order_ann))
print(repr(order_joe))
# 输出
# <Order total: 42.00 due: 39.90>
# <Order total: 42.00 due: 42.00>
```

### 使用函数实现”策略“模式

以上实例都是基于类实现的，而且每个类都只定义了一个方法，而且每个实例都没有自己的状态，看起来和普通的函数没有区别

我们可以把具体策略换成函数实现

```python
from abc import ABC, abstractmethod
from collections import namedtuple

Customer = namedtuple("Customer",'name fidelity')

class LineItem:
    def __init__(self,product,quantity,price) -> None:
        self.product = product
        self.quantity = quantity
        self.price = price
  
    def total(self):
        return self.quantity * self.price

class Order: # 上下文
    def __init__(self, customer, cart, promotion=None) -> None:
        self.customer = customer
        self.cart = list(cart)
        self.promotion = promotion

    def total(self):
        if not hasattr(self,'__total'):
            self.__total = sum(item.total() for item in self.cart)
        return self.__total
  
    def due(self):
        if self.promotion is None:
            discount = 0
        else:
            discount = self.promotion(self)
        return self.total() - discount

    def __repr__(self) -> str:
        fmt = '<Order total: {:.2f} due: {:.2f}>'
        return fmt.format(self.total(),self.due())
  
def fidelity_promo(order):
    """积分大于1000给予5%的折扣"""
    return order.total() * .05 if order.customer.fidelity >= 1000 else 0

def bulk_item_promo(order):
    """单个商品20个以上提供10%的折扣"""
    discount = 0 
    for item in order.cart:
        if item.quantity >= 20:
            discount += item.total() * .1
    return discount

def large_order_promo(order):
    """订单中不同商品的个数达到10个以上时提供7%的折扣"""
    distinct_item = {item.product for item in order.cart}
    if len(distinct_item >= 10):
        return order.total() * .07
    return 0

joe = Customer('John Doe', 0)
ann = Customer('Ann Smith',1000)
cart = [LineItem('banana',4,.5),
        LineItem('apple',10,1.5),
        LineItem('watermelon',5,5.0)]

order_joe = Order(joe,cart,fidelity_promo)
order_ann = Order(ann,cart,fidelity_promo)
print(repr(order_ann))
print(repr(order_joe))
  
```

把折扣策略通过函数实现可以减少一部分的代码量，但是以上两种办法，都没有办法实现最佳调用方法，它们缺少内部状态

这些具体策略都没有内部状态，只是单纯的对上下文进行处理

这个时候需要引入享元的做法

#### 享元

享元是可以共享的对象，同时可以在多个上下文中使用，这样不必再新的上下文中根据不同策略不断创建新的实例对象

### 选择最佳策略：简单的方式

```python
promos = [fidelity_promo,bulk_item_promo,large_order_promo]
def best_promo(order):
    return max(promo(order) for promo in promos)
```

以上代码可用但是如果添加新的方法就需要把他加到promos列表中否则best_promo函数不会考虑新的策略，要如何保证新加的策略立刻就能应用到bestpromo函数呢

## globals关键字

globals()是python的一个内置方法，表示当前的全局符号表.

比如当我在程序运行最后打印这个关键字，其返回值是一个字典

```python
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x00000234CBDD6CD0>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, '__file__': 'c:\\Users\\Administrator\\GithubRepo\\study_recording\\fluent_python\\ch06_07\\functional_pattern_design.py', '__cached__': None, 'ABC': <class 'abc.ABC'>, 'abstractmethod': <function abstractmethod at 0x00000234CC2780D0>, 'namedtuple': <function namedtuple at 0x00000234CC466550>, 'Customer': <class '__main__.Customer'>, 'LineItem': <class '__main__.LineItem'>, 'Order': <class '__main__.Order'>, 'fidelity_promo': 
<function fidelity_promo at 0x00000234CC486DC0>, 'bulk_item_promo': <function bulk_item_promo at 0x00000234CC4881F0>, 'large_order_promo': <function large_order_promo at 0x00000234CC488280>, 'promos': [<function fidelity_promo at 0x00000234CC486DC0>, <function bulk_item_promo at 0x00000234CC4881F0>, <function large_order_promo at 0x00000234CC488280>], 'best_promo': <function best_promo at 0x00000234CC488310>, 'joe': Customer(name='John Doe', fidelity=0), 'ann': Customer(name='Ann Smith', fidelity=1000), 'cart': [<__main__.LineItem object at 0x00000234CC281430>, <__main__.LineItem object at 0x00000234CC2D66A0>, <__main__.LineItem object at 0x00000234CC2D68E0>], 'banana_cart': [<__main__.LineItem object at 0x00000234CC2D62E0>, <__main__.LineItem object at 0x00000234CC2D63A0>], 'order_joe': <Order total: 42.00 due: 42.00>, 'order_ann': <Order total: 42.00 due: 39.90>, 'banana_order_joe': <Order total: 30.00 due: 28.50>, 'banana_order_ann': <Order total: 30.00 due: 28.50>}
```

可以利用这个全局符号表来帮我们找到一些刚创建的策略

```python
promos = [globals()[name] for name in globals()
          if name.endswith('_promo')
          and name != 'best_promo'] # 防止递归
def best_promo(order):
    return max(promo(order) for promo in promos)
```
