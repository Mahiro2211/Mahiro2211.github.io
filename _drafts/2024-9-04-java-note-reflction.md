---
layout: post
title: 反射 泛型编程 Java编程入门
categories: [Java]
description: 整合之前零零散散学到的东西
keywords: Java 
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# 什么是反射
反射是Java提供的一套工具集，通过反射我们可以对类进行操作，得到类的各种信息

## Class 类
Class对象是Java用来描述一个类的各种属性的类<br>
通过传入指定变量的getClass()方法就可以得到变量对应的类的Class对象<br>

### 通过getClass()方法得到Class对象
```java
/*
在Person.java中
public class Person {
    public String name;
    public Person(String name)
    {
        this.name = name;
    } 
}
*/

Person demo = new Person("Jax");
Class demo_cls = demo.getClass();
System.out.println(demo_cls);
/* 得到的输出为
class min.Person
其中min是这个类所属的package
*/
```
通过对一个实例化类的变量使用getClass()方法可以得到Class对象<br>


### 调用Class对象的内置方法得到原始类的属性
```java
public class reflection {
   public static void main(String[] args) {
        // Class cls = String.class;

        // String s = "ASDKJD";
        // Class cls2 = s.getClass();
        
        // System.out.println(cls == cls2);
        printClassInfo("".getClass()); // 
        // printClassInfo(Runnable.class);
        // printClassInfo(java.time.Month.class);
        // printClassInfo(String[].class);
        // printClassInfo(int.class);
   } 
   static void printClassInfo(Class cls) {
        System.out.println("Class name: " + cls.getName());
        System.out.println("Simple name: " + cls.getSimpleName());
        if (cls.getPackage() != null) {
            System.out.println("Package name: " + cls.getPackage().getName());
        }
        System.out.println("is interface: " + cls.isInterface());
        System.out.println("is enum: " + cls.isEnum());
        System.out.println("is array: " + cls.isArray());
        System.out.println("is primitive: " + cls.isPrimitive());
    }
}
```
传入printClassInfo方法的一个Class对象会有以下的的输出（以String为例）<br>
```
Class name: java.lang.String
Simple name: String
Package name: java.lang
is interface: false
is enum: false
is array: false
is primitive: false
```
针对反射的详细方法记录会另写一篇新的博客
传送门

<hr>

# 什么是泛型编程
Java和C++一样都是强类型语言，定义变量需要声明类型，没有泛型编程的时候采取强制类型转换会影响程序的可读性和安全性<br>
泛型编程允许一个类可以接受任意一个类型，比如自己写的容器可以容纳String，Integer，还有自己编写的自定义的类，通常在一种类型的容器里交互是没有问题的，但是思考如果有以下场景，有一个Manager类和一个Employee类，Manager类可以把想添加的元素加入的Employee类，但是反过来则是非法的。**如何允许前一个调用而不允许后一种
调用呢**，Java有一种**通配符类型(wildcard type)**来解决这个问题。
<br>

## 定义一个简单的泛型类

```java
package min;

public class G_demo<T> {
    private T first;
    private T second;

    public G_demo() {first = null; second = null;}
    public G_demo(T first, T second) { this.first = first; this.second = second; }

    public T getFirst() { return this.first; }
    public T getSecond() { return this.second; }
    
    public void setFrist(T newValue) { first = newValue; }
    public void setSecond(T newValue) { second = newValue; }
}
```
T 这个关键字就是代表着泛型，它可以接受各种各样的类型，泛型类就像是一个加工普通类的工厂

## 实例化G_demo类

```java
var var1 = new G_demo<String>();
G_demo<Integer> var2 = new G_demo<>();
```
以上两种方式作用都是一样的

## 定义一个泛型方法