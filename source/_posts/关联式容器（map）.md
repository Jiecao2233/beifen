---
title: 关联式容器（map）
tags:
  - STL
categories:
  - STL
date: 2016-10-03 15:10:46
---

## 前言
关联式容器就像一个下标是任何类型的数组，比如，一个int类型的数组，下标用string类型的，$int\_{string}$
官方解释是一个有序的映射表。。。

<!--more-->

## 1.定义

头文件`#include<map>`

>map<类型1，类型2>变量名;

```C++
map<string,int>ma;            //定义ma为一个从string到int的一个映射
ma["abc"]=2;                  //将字符串“abc”映射到整数“2”上
cout<<ma["abc"]<<endl;        //输出“abc”的映射“2”
```

## 2.操作

|操作|用途|
|:---:|:---|
|operator[]|访问map中的元素，若该元素不存在，将创建一个新元素映射到类型2的初始值上|
|ma.begin()|返回map中第一个元素的指针|
|ma.end()|返回map中最后一个元素的后一个元素的指针|
|ma.size()|返回map中元素的个数|
|ma.count(element)|若元素element存在于map中返回1，否则返回0|
|ma.clear()|初始化map|

示例：
```C++
#include<iostream>
#include<cstdio>
#include<map>
#include<string>

using namespace std;

map<string,int>ma;         //定义一个下标为int类型的string类型的“数组” 

int main()
{
	ma["apple"]=1;             //“apple”的下标为1 
	ma["banana"]=2;               //“banana”的下标为2 
	ma["lemon"]=3;               //“lemon”的下标为3 
	cout<<ma.size()<<endl;          //输出ma的元素个数 
	cout<<ma["apple"]<<endl;        //输出“apple”的下标 
	if(ma.count("pear")) cout<<"pear"<<endl;      //如果ma里有“pear”，则输出“pear” ，否则输出“no pear” 
	else cout<<"no pear"<<endl;
	return 0;
}
```

运行结果：
![image](/images/1475480337463.png)