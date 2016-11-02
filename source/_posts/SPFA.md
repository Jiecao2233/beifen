---
title: SPFA
tags:
  - 贪心
  - 最短路
  - SPFA
categories:
  - SPFA
  - 贪心
  - 最短路
date: 2016-10-16 16:00:01
---

## 前言
恕我直言，在负边权面前，Dijkstra就是ZZ...

<!--more-->

## 1.算法思想
用一个dis[]数组储存出发点到该点的最短距离，用一个[FIFO队列](/2016/09/22/FIFO队列与优先队列/)来记录待优化的结点，每次将队首结点取出，并用该结点的dis来更新与其直接连通的结点的dis值，如果一个点的dis值被修改了而且该点不在队列中，则将其放在队尾；不断重复该过程直到队列为空。此时，dis[]数组里储存的便是每个点到出发点的最短距离。

## 2.SPFA模板题
【题目链接】
[洛谷【3371】](https://www.luogu.org/problem/show?pid=3371)
【题目描述】
如题，给出一个有向图，请输出从某一点出发到所有点的最短路径长度。
【输入格式】
第一行包含三个整数N、M、S，分别表示点的个数、有向边的个数、出发点的编号。
接下来M行每行包含三个整数Fi、Gi、Wi，分别表示第i条有向边的出发点、目标点和长度。
【输出格式】
一行，包含N个用空格分隔的整数，其中第i个整数表示从点S出发到点i的最短路径长度（若S=i则最短路径长度为0，若从点S无法到达点i，则最短路径长度为2147483647）
【输入样例】

>4 6 1
1 2 2
2 3 2
2 4 1
1 3 5
3 4 3
1 4 4

【输出样例】

>0 2 4 3

【程序】
```C++
#include<cstdio>
#include<iostream>
#include<queue>
#define MAX 2147483647
#define MIN 0
#define M 500500

using namespace std;

int n,m,s;
struct E{
	int to,next,w;
}e[M];
int tope,head[M];
inline void ae(int x,int y,int z)
{
	tope++;e[tope].to=y;e[tope].next=head[x];head[x]=tope;e[tope].w=z;
}
queue<int>q;
int dis[M];
bool in[M];

inline void spfa(int x)
{
	int topp;
	for(int i=1;i<=n;++i)
	{
		dis[i]=MAX;
	}
	q.push(x);dis[x]=0;in[x]=1;
	do
	{
		topp=q.front();
		q.pop();
		in[topp]=0;
		for(int i=head[topp],p=e[i].to;i;i=e[i].next,p=e[i].to)
		{
			if(dis[topp]+e[i].w<dis[p])
			{
				dis[p]=dis[topp]+e[i].w;
				if(!in[p])
				{
					q.push(p);
					in[p]=1;
				}
			}
		}
	}while(!q.empty());
}

int main()
{
	scanf("%d%d%d",&n,&m,&s);
	int ta,tb,tc;
	for(int i=1;i<=m;++i)
	{
		scanf("%d%d%d",&ta,&tb,&tc);
		ae(ta,tb,tc);
	}
	spfa(s);
	for(int i=1;i<=n;++i)
	{
		printf("%d ",dis[i]);
	}
	return 0;
}
```

## 3.最短路计数
【题目链接】
[洛谷【1144】](https://www.luogu.org/problem/show?pid=1144)
【题目描述】
给出一个N个顶点M条边的无向无权图，顶点编号为1～N。问从顶点1开始，到其他每个点的最短路有几条。
【输入格式】
输入第一行包含2个正整数N，M，为图的顶点数与边数。
接下来M行，每行两个正整数x, y，表示有一条顶点x连向顶点y的边，请注意可能有自环与重边。
【输出格式】
输出包括N行，每行一个非负整数，第i行输出从顶点1到顶点i有多少条不同的最短路，由于答案有可能会很大，你只需要输出mod 100003后的结果即可。如果无法到达顶点i则输出0。
【输入样例】

>5 7
1 2
1 3
2 4
3 4
2 3
4 5
4 5

【输出样例】

>1
1
1
2
4

【程序】
```C++
#include<cstdio>
#include<iostream>
#include<queue>
#define MAX 2147483647
#define MIN 0
#define M 500500
#define MOD 100003

using namespace std;

int n,m,s;
struct E{
	int to,next,w;
}e[M];
int tope,head[M];
inline void ae(int x,int y,int z)
{
	tope++;e[tope].to=y;e[tope].next=head[x];head[x]=tope;e[tope].w=z;
}
queue<int>q;
int dis[M],cnt[M];
bool in[M];

inline void spfa(int x)
{
	int topp;
	for(int i=1;i<=n;++i)
	{
		dis[i]=MAX;
	}
	cnt[x]=1;
	q.push(x);dis[x]=0;in[x]=1;
	do
	{
		topp=q.front();
		q.pop();
		in[topp]=0;
		for(int i=head[topp],p=e[i].to;i;i=e[i].next,p=e[i].to)
		{
			if(dis[topp]+e[i].w<dis[p])
			{
				dis[p]=dis[topp]+e[i].w;
				cnt[p]=cnt[topp]%MOD;
				if(!in[p])
				{
					q.push(p);
					in[p]=1;
				}
			}
			else
			{
				if(dis[topp]+e[i].w==dis[p])
					cnt[p]=(cnt[p]+cnt[topp])%MOD;
			}
		}
	}while(!q.empty());
}

int main()
{
	scanf("%d%d",&n,&m);
	int ta,tb;
	for(int i=1;i<=m;++i)
	{
		scanf("%d%d",&ta,&tb);
		ae(ta,tb,1);
		ae(tb,ta,1);
	}
	spfa(1);
	for(int i=1;i<=n;++i)
	{
		if(dis[i]==MAX) printf("0\n");
		else printf("%d\n",cnt[i]);
	}
	return 0;
}
```