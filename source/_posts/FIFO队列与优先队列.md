---
title: FIFO队列与优先队列
tags:
  - 队列
categories:
  - 队列
date: 2016-09-22 21:19:59
---

## 前言
队列是个好东西，可以手写，也可以用C++自带的queue头文件来写。

<!--more-->

## 定义队列
```C++
#include<queue>
```
### FIFO队列

>queue <类型> 变量名

```C++
queue <int> que    //定义一个int类型的名为que的FIFO队列
queue <char> a     //定义一个char类型的名为a的FIFO队列
queue <data> c     //定义一个data（自定义数据类型）类型的名为c的FIFO队列
```

### 优先队列

>priority_queue <类型> 变量名;

```C++
priority_queue <int> heap;      //定义一个int类型的名为heap的优先队列
priority_queue <double> k;      //定义一个double类型的名为k的优先队列
```
这两种定义方式都是大根堆，转为小根堆有两种方法，一是每个数据都乘以-1，二是自定义数据类型：
```C++
struct data{
	int x;
	bool operator < (const data & a)const {
		return a.x<x;
	}
};
priority_queue <data> q;
```

## 队列操作
此处以一个名为q的队列为例：

|操作|用途|
|:---:|:---|
|q.empty()|若队列为空，则返回true，否则返回false|
|q.size()|返回队列中元素的个数|
|q.pop()|删除队首元素，但不返回其值|
|q.front()|返回队首元素的值，但不删除该元素（仅适用于FIFO队列）|
|q.back()|返回队尾元素的值，但不删除该元素（仅适用于FIFO队列）|
|q.top()|返回具有最高优先级的元素的值（默认最大值），但不删除该元素（仅适用于优先队列）|
|q.push()|对queue，在队尾压入一个新元素<br>对于priority_queue，在基于优先级的适当位置插入新元素|

## 简单例题
[洛谷【1090】（合并果子）](http://www.luogu.org/problem/show?pid=1090)
```C++
#include<cstdio>
#include<iostream>
#include<queue>
using namespace std;
int n,x,ans=0,tmp;
priority_queue <int> q;
int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;++i)
	{
		scanf("%d",&x);
		q.push(-x);
	}
	for(int i=1;i<n;++i)
	{
		tmp=q.top();
		ans-=q.top();
		q.pop();
		tmp+=q.top();
		ans-=q.top();
		q.pop();
		q.push(tmp);
	}
	printf("%d",ans);
	return 0;
}
```
