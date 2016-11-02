---
title: 树形DP
tags:
  - 动态规划
  - DFS
categories:
  - 动态规划
  - DFS
date: 2016-10-26 20:26:25
---

## 前言
在树上搞DP？好高大上的DP。。。

<!--more-->

## 1.思想
外瑞库，康明松。

## 2.树形DP例题之选课
【题目链接】
[洛谷【2014】](https://www.luogu.org/problem/show?pid=2014)
【题目描述】
在大学里每个学生，为了达到一定的学分，必须从很多课程里选择一些课程来学习，在课程里有些课程必须在某些课程之前学习，如高等数学总是在其它课程之前学习。现在有N门功课，每门课有个学分，每门课有一门或没有直接先修课（若课程a是课程b的先修课即只有学完了课程a，才能学习课程b）。一个学生要从这些课程里选择M门课程学习，问他能获得的最大学分是多少？
【输入格式】
第一行有两个整数N,M用空格隔开。(1<=N<=300,1<=M<=150)
接下来的N行,第I+1行包含两个整数ki和si, ki表示第I门课的直接先修课，si表示第I门课的学分。若ki=0表示没有直接先修课（1<=ki<=N, 1<=si<=20）。
【输出格式】
只有一行，选M门课程的最大得分。
【输入样例】

>7  4
2  2
0  1
0  4
2  1
7  1
7  6
2  2

【输出格式】

>13

【程序】
```C++
#include<cstdio>
#include<iostream>
#define M 500500

using namespace std;

int n,m;
int head[M],next[M],w[M];
int f[M/100][M/100];

int dfs(int x)
{
	if(head[x]==0) 
		return 0;
	int ans=0;
	for(int i=head[x];i;i=next[i])
	{
		int t=dfs(i);
        ans+=t+1;
        for (int j=ans;j>=0;j--)
		{
            for (int k=0;k<=t;k++)
            {
            	if (j-k-1>=0) 
					f[x][j]=max(f[x][j],f[x][j-k-1]+f[i][k]);
            }
        }
	}
	return ans;
}

int main()
{
	scanf("%d%d",&n,&m);
	int ta,tb;
	for(int i=1;i<=n;++i)
	{
		scanf("%d%d",&ta,&tb);
		next[i]=head[ta];
		head[ta]=i;
		w[i]=tb;
	}
	for(int i=0;i<=n;++i) f[i][0]=w[i];
	dfs(0);
	printf("%d",f[0][m]);
	return 0;
}
```