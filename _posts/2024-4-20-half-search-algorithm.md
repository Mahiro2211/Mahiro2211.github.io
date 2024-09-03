---
layout: post
title: 二分答案算法和二分搜素算法
categories: [Algorithm]
description: 算法模板，学习记录
keywords: Algorithm
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---


# 二分搜索
```cpp
#include<iostream>

using namespace std;

const int N = 1e-6;

int arr[N];

int findFirst(int arr[], int arrlen, int check)
{
	int left = 0 , right = arrlen - 1;
	while(left <= right)
	{
		int mid = (left + right ) / 2;
		if (arr[mid] >= check)
		{
			right = mid - 1 ; // if bigger just continue
			// 如果已经找到匹配的值了，我们还是要把右指针往左靠，知道找到更小的，此时左指针就是第一次出现的位置，以下同理
		}
		else if (arr[mid] < check)
		{
			left = mid + 1; // if smaller
			// will be first one equal to target
		}
	}
	return left;
}

int findLast(int arr[], int arrlen , int check)
{
	int left = 0 , right = arrlen - 1;
	while(left <= right)
	{
		int mid = (left + right ) / 2;
		if (arr[mid] <= check)
		{
			left = mid + 1; // if smaller
		}
		else if ( arr[mid] > check)
		{
			right = mid - 1 ; // if bigger just continue
			// will be last one equal to target
		}
	}
	return right;
}

int main()
{
	int arrlen , querry;
	cin >> arrlen >> querry;
	
	for ( int i = 0 ; i < arrlen ; i ++ )
	{
		cin >> arr[i];
	}
	
	while(querry--)
	{
		int check;
		cin >> check;
		int first = findFirst(arr,arrlen,check);
		int last = findLast(arr, arrlen,check);
		if (first >= arrlen || arr[first] != check) 
		{
			cout << "-1 -1\n";
			continue;
		}
		cout << first << ' ' << last << '\n';
	}
}
```

# 【二分答案】寻找指定和的整数对
[题目链接](http://acm.ocrosoft.com/problem.php?id=1179)

求解过程 ： 首先对数组从大到小排列 ， 从头到尾处理数组的每一个元素,复杂度是O（n),把求解目标和(target)的过程当成
$arr[a]+arr[b]=target$
也就是
$arr[a]=target-arr[b]$
的过程
我们遍历数组，将每一个元素作为被减的元素arr[b]，然后二分查找arr[a],二分查找的值必定比arr[b]大，所以二分查找总是从索引b+1开始的
```cpp
#include<iostream>
#include<algorithm>

using namespace std;
const int N = 1e5 + 1;

int arr[N];

int findValue(int arr[] , int arrlen , int target,int start)
{
	int left = start , right = arrlen - 1;
	while(left <= right)
	{
		int mid = (left + right) / 2;
		
		if (arr[mid] > target)
		{
			right = mid - 1;
		}else if (arr[mid] < target)
		{
			left = mid + 1;
		}else {
			return mid;
		}
	}
	
	return -1;
}

int main()
{
	// arr[a] + arr[b] = N --> arr[a] = N - arr[b]
	int arrlen , querry ,flag = 0;
	cin >> arrlen >> querry ;
	
	for (int i = 0 ; i < arrlen ; i++)
	{
		cin >> arr[i];
	}
	sort(arr,arr + arrlen);
	while (querry--)
	{
		int target ;
		cin >> target ;
		flag = 0;
		for (int i = 0 ; i < arrlen ; i++)
		{
			int temp = target - arr[i];
			
			int returnValue = findValue(arr, arrlen, temp,i+1);
			
			if (returnValue != -1)
			{
				cout << 1 << '\n';
				flag = 1;
				break;
			}
		}
		if (flag == 0)
		{
			cout << 0 << '\n';
		}
	}
	return 0;
}
```