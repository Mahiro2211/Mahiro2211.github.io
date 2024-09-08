---
layout: post
title: C++11特性总结
categories: [C++]
description: 收集了C++11所有特性
keywords: C++
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

1. [目录]
    -[列表初始化](#列表初始化)

# 列表初始化
```cpp
#include <iostream>
#include <cstdio>
#include <vector>
using namespace std;

int main()
{
    int var_A = 0;
    int var_B{0};
    int var_C(0);
   
    cout << var_A << ' ' << var_B << ' ' << var_C << '\n';
    vector<int> vec_A(10); // vec_A[0....9]=0
    vector<int> vec_B{10}; // vec_B[0]=10
    vector<int> vec_C{10,1}; // vec_C[0]=10 vec_C[1]=1
    vector<int> vec_D(10,1); // vec_D[0...9]=10

    return 0;
}
```
