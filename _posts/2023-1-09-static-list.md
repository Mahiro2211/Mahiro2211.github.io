---
layout: post
title: 静态链表
categories: [DataStructrue]
description: 静态链表
keywords: 链表, 基础数据结构
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

* 今天复盘一下以前学过的基础数据结构
@[TOC](目录)
# 链表（单向链表与双向链表）
在算法竞赛种很少使用动态链表，因为写起来很繁琐，大都是用数组模拟链表显得更加方便
## 使用结构体实现静态链表（以约瑟夫环为例洛谷P1996）
---


* $n$ 个人围成一圈，从第一个人开始报数,数到 $m$ 的人出列，再由下一个人重新从 $1$ 开始报数，数到 $m$ 的人再出圈，依次类推，直到所有的人都出圈，请输出依次出圈人的编号。

---

### 结构体数组实现单向静态链表
* 个人觉得结构体数组的优势就是可以储存不止一个数据类型

```cpp
#include<cstdio>
using namespace std;
const int N = 1e5+10;
struct node{
	int id , nextid;
}nodes[N];
int main(){
	int n , m ;
	scanf("%d%d",&n,&m);
	nodes[0].nextid = 1;//初始化
	for(int i = 1 ; i <= n ; i++){
		nodes[i].id = i;
		nodes[i].nextid = i + 1;**//初始化链表，next指针指向下一个要去的节点**
	}
	nodes[n].nextid = 1;//尾指针再指向头指针，实现闭环
	int pre = 1 , now = 1;//用两个指针记录遍历时的节点
	while(n-- > 1){//直到所有人都出列
		for(int i = 1 ; i < m ; i++){
			//开始数数，数到m，去掉这个节点
			pre = now;//存储以前的值
			now = nodes[now].nextid;//这个也代表着当前数到的人
		}
		printf("%d ",nodes[now].id);
		//让第m出列我们直接越过now，直接让pre指向now下一个,也就是删除掉now节点
		nodes[pre].nextid = nodes[now].nextid;
		now = nodes[pre].nextid;//新建now节点被pre指向,下一次从这里开始数
	}
	printf("%d",nodes[now].nextid);
	return 0 ;
	
}
```
---
### 结构体数组实现双向静态链表

*	双向链表只不过就是有两个next指针一个指向前面的元素，一个指向后面的元素
*	![双向链表示意图](https://i-blog.csdnimg.cn/blog_migrate/dfc870add655bd936db99fe9c7b15447.png#pic_center)
```cpp
#include<cstdio>
using namespace std;
const int N = 1e5+10;
struct node{
	int id , preid , nextid;
}nodes[N];
int main(){
	int n , m ;
	scanf("%d%d",&n,&m);
	nodes[0].nextid = 1;
	for(int i = 1 ; i <= n ; i++){
		nodes[i].id = i;
		nodes[i].nextid = i + 1;
		nodes[i].preid = i - 1;
	}
	nodes[n].nextid = 1;
	nodes[1].preid = n;
	int pre = 1 , now = 1 , next = 1;
	while(n -- > 1){
		for(int i = 1 ; i < m ; i++){
			now = nodes[now].nextid;
		}
		printf("%d ",nodes[now].id);
		//此时的now就是要被删除掉的节点
		pre = nodes[now].preid;
		next = nodes[now].nextid;
		nodes[pre].nextid = nodes[now].nextid;
		nodes[next].preid = nodes[now].preid;
		now = next;
	}
	printf("%d",nodes[now].nextid);
	return 0;
}
```

### 数组模拟实现单向链表
* 需要开两个数组一个用来保存值，一个next数组用来指向每个元素下一个指向的位置
* 设置一个$next[i]$指向i的下一个元素位置，i本身就是元素值
```cpp
#include<cstdio>
using namespace std;
const int N = 1e5+10;
int a[N] , next[N];
int main(){
	int n , m ;
	next[0] = 1;
	scanf("%d%d",&n,&m);
	for(int i = 1 ; i <= n ; i++){
		next[i] = i + 1;
	}
	next[n] = 1;
	int now = 1 , pre = 1;
	while(n-- > 1){
		for(int i = 1 ; i < m ; i++){
			pre = now;
			now = next[now];
		}
		printf("%d ",now);
		next[pre] = next[now];
		now = next[now];
	}
	printf("%d",now);
	return 0 ;
}
```
#### 单向链表的删除，添加

```cpp
int head ;//头指针
int idx ; //表示现在用到哪个点
int e[N],nexte[N];//节点i的值何next指针指向的下一个节点
//在头节点添加一个元素
void init(){
	head = - 1;
	idx = 0;//头指针是空指针，谁也不指
}
void add_to_head(int x){
	e[idx] = x;//储存值
	nexte[idx] = head;//让第一个元素先指向头指针
	head = idx ++;//更新表头，头指针加一
	//刚加进去的元素既是头元素也是尾元素
}
void add(int k ,int x){//在第k个插入的数后面加入x
	e[idx] = x;
	nexte[idx] = nexte[k];
	next[k] = idx++;
}
void remove(int k){
	nexte[k] = nexte[nexte[k]];//直接跳过
```
### 数组实现双向链表
```cpp
int e[N] , l[N] , r[N] , idx;
void init(){
    //默认头指针指向0，尾指针指向1
    idx = 2;
    r[0] = 1 , l[1] = 0;
}
void r_add(int k ,int x ){
    e[idx] = x;
    r[idx] = r[k];
    l[idx] = k;
    l[r[k]] = idx;
    r[k] = idx;
    idx++;
}

void remove(int k){
    r[l[k]] = r[k];
    l[r[k]] = l[k];
}
```