---
layout: post
title: JavaScript基础数据模型和简单API调用
categories: [JavaScript]
description: 数组，字符串
keywords: JavaScript
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# 目录
1. [JS如何定义变量的](#js如何定义变量的)
   - [老旧的var](#老旧的var)
   - [let定义是推荐使用的](#let定义是推荐使用的)
2. [JS中的数组](#js中的数组)
   - [常用的方法](#常用的方法)
   - [concat方法会返回一个连接后的新数组，不会改变原数组](#concat方法会返回一个连接后的新数组不会改变原数组)
   - [遍历数组的一些方法](#遍历数组的一些方法)
   - [查找元素的一些方法](#查找元素的一些方法)
   - [在数组中查找](#在数组中查找)
   - [对数组每个元素操作，返回一个新数组,map操作](#对数组每个元素操作返回一个新数组map操作)
   - [数组和字符串之间的转化](#数组和字符串之间的转化)
   - [splice方法非常好用，既可以删除元素也可以填充修改](#splice方法非常好用既可以删除元素也可以填充修改)
   - [对比数组是否相等时，请使用（===）而不是（==）](#对比数组是否相等时请使用===而不是==)
   - [Reduce方法（函数操作应用到每个数组的元素上）](#reduce方法函数操作应用到每个数组的元素上)
3. [JS中的字符串](#js中的字符串)
   - [访问](#访问)
   - [查找](#查找)
   - [切片](#切片)
   - [Unicode（charCodeAt()和toString()）](#unicodecharcodeat和tostring)

> 一个寒假确实过的很快，这个寒假除了调包调参突然心血来潮想学一下前端，学习过程比较平滑，我是自己找的技术文档＋写代码实践来学习的，教程视频虽然详细，但是真的一点都看不动。

@[TOC](目录)
# JS如何定义变量的
## 老旧的var

在一些比较古早的教材中，定义js变量通常使用的是var。虽然是大家经常写的做法，但是不推荐这么做因为他又两点坏处
* var 关键字只有**函数作用域**和**全局作用域**。
```javascript
if (true) {
	var flag = False;
}
console.log(flag); // 这个时候显示的是False，意味着这个变量在循环过后依旧存在
```
* 关于var存在变量提升的问题
```javascript
a = 0
var a 
console.log(a) // 0
```
声明的语句可以视为自动提升到文档的顶部

* var关键字可以重新定义不报错

## let定义是推荐使用的。

# JS中的数组
常用的方法有
```javascript
let a = [2, 3, 4 ,5]

let size_a = a.length // 是数组的一个属性

a.unshift(['a','b'])

console.log(`a数组的长度为${size_a}`)
console.log(`a 数组经过 unshift 后变成了a: ${a}`)//a 数组经过 unshift 后变成了a: a,b,2,3,4,5

a.shift()
console.log(`a 数组经过一次shift操作的变化a: ${a}`)//a 数组经过一次shift操作的变化a: 2,3,4,5
// 这会除掉一次性unshift加入头部的元素

a.concat(['concat']) // concat 操作会返回一个新数组所以这里
console.log(`进行concat 操作后的 a: ${a}`) // 进行concat 操作后的 a: 2,3,4,5

let new_a = a.concat(['concat']) 
console.log(`返回的新数组为${new_a}`) // 返回的新数组为2,3,4,5,concat

console.log(`查找元素2 : ${a.indexOf(2)}`) // 查找对应的索引

let b = a.slice(1,3)
console.log(`b 截取的子串为 ${b}`) // [ , ) 方式截取的子串

let c = []

for (let i = 0 ; i < 10 ; i++){
    c[i] = i
}

console.log(`数组c是:${c}`)
console.log(`转置后的数组c是${c.reverse()}`)
console.log(`排序后的转置数组c ${c.reverse().sort()}`)

console.log(`在0-3这个位置删除掉元素然后添加4个字母,删除掉的元素为${c.splice(0,4,'a','b','c',)}`)
//在0-3这个位置删除掉元素然后添加4个字母,删除掉的元素为0,1,2,3
console.log(c) // 这个操作会直接影响数组c，而不是返回新的数组
//[
//  'a', 'b', 'c', 4, 5,
//  6,   7,   8,   9
//]

console.log(`将数组c的所有元素使用一个符号连接 ":" ${c.join(':')}`)
//将数组c的所有元素使用一个符号连接 ":" a:b:c:4:5:6:7:8:9
```
## concat方法会返回一个连接后的新数组，不会改变原数组
## 遍历数组的一些方法：
* arr.forEach(function(value,index,arr){ ...do something})
* for(let i = 0 ; i < arr.lenght; i++){}
* for(let i of arr){}
## 查找元素的一些方法：
* indexOf()
* lastIndexof()
以上两种办法接受，(value,from)从from开始查询，找到了value就返回索引，找不到返回-1
还有另外一种只检查是否涵盖
* includes()
用法一样，返回的真假

## 在数组中查找
```javascript
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

let user = users.find(item => item.id == 1);//返回值
let user1 = user.findIndex(item => item.id ==1);//返回索引
let user2 =  user.findLastIndex();//同上

alert(user.name); // John
```
## 对数组每个元素操作，返回一个新数组,map操作
```javascript
let result = arr.map(function(item, index, array) {
  // 返回新值而不是当前元素
})
```
## 数组和字符串之间的转化
```javascript
let a = [1,1,2,3,2,34,45]
let b = a.join(",") // 1,1,2,3,2,34,45
console.log(b)
let c = b.split(",")
console.log(c) // [ '1',  '1', '2', '3',  '2', '34', '45' ]
  
```
## splice方法非常好用，既可以删除元素也可以填充修改
arr.splice(start[, deleteCount, elem1, ..., elemN])
## 对比数组是否相等时，请使用（===）而不是（==）
因为 == 对比的是两个变量引用的是同一个对象才会相等，他也会存在类型转换
```
0 = []  // true 因为空[]被转化为0
```
## Reduce方法（函数操作应用到每个数组的元素上）
```javascript
let value = arr.reduce(function(accumulator, item, index, array) {
  // accumulator为上一次函数的返回值，initial为初始值
}, [initial]);
```

# JS 中的字符串
JS中创建好的字符串变量是不可以直接通过索引修改的，必须用新的变量存储
## 访问
使用方括号的数字索引来。
## 查找
使用string.indexOf(character)来查找
## 切片
* 使用slice(start,end) # [ )区间
* 使用substring(start,end) # 同上
* 使用substr(start,length) 


## Unicode（charCodeAt()和toString())
所有的字符串都使用 UTF-16 编码
String 的 charCodeAt() 方法返回一个整数，表示给定索引处的 UTF-16 码元，其值介于 0 和 65535 之间。
toString()方法返回一个表示该对象的字符串
```javascript
function Dog(name) {
  this.name = name;
}

const dog1 = new Dog('Gabby');
let dog2 = Dog('agg');//没有使用new创建实例，无意义



Dog.prototype.toString = function dogToString() {
  return `${this.name}`;
};

console.log(dog1.toString());
```


