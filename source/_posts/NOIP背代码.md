---
title: NOIP背代码
tags:
  - noip
categories:
  - noip
date: 2016-10-16 21:33:39
---

## 前言
这些代码实在没空去仔细理解了，就背一下吧。
写的有相关博客的以链接形式存在。

<!--more-->

## 快速幂
[快速幂(含于矩乘中)](/2016/10/31/矩阵乘法-（矩阵）快速幂-斐波那契/#1-了解一下快速幂)

## 最短路

### SPFA
[SPFA](/2016/10/16/SPFA/)

### Dijkstra
```C++
#include<iostream>
#include<cstdio>
#include<queue>
#include<climits>
#define M 1000000
using namespace std;
struct Edge
{
	int to,next,w;
}e[M];
int head[M],tope;
inline void ae(int x,int y,int z)
{
	tope++;e[tope].next=head[x];e[tope].to=y;e[tope].w=z;head[x]=tope;
}
struct Node
{
	int x,d;
	inline bool operator<(const Node &a) const
	{
		return d>a.d;
	}
};
inline Node make_node(int x,int y)
{
	Node a;
	a.x=x;a.d=y;
	return a;
}
priority_queue<Node>pig;
int n,m;
int dis[M];
inline void dijs_buhuipinle(int x)
{
	for(int i=1;i<=n;++i)
		dis[i]=0x7fffffff;
	pig.push(make_node(x,0));
	while(!pig.empty())
	{
		Node ce=pig.top();pig.pop();
		if(dis[ce.x]<0x7fffffff) continue;
		dis[ce.x]=ce.d;
		for(int i=head[ce.x];i;i=e[i].next)
			pig.push(make_node(e[i].to,ce.d+e[i].w));
	}
}
int s;
int main()
{
	scanf("%d%d%d",&n,&m,&s);
	int ta,tb,tc;
	for(int i=1;i<=m;++i)
	{
		scanf("%d%d%d",&ta,&tb,&tc);
		ae(ta,tb,tc);
	}
	dijs_buhuipinle(s);
	for(int i=1;i<=n;++i)
		printf("%d ",dis[i]);
	return 0;
}
```

## KMP
[KMP](/2016/10/17/KMP/)

## 线段树
[线段树](/2016/10/16/线段树/)

## Tarjan
[Tarjan](/2016/10/13/Tarjan（强连通分量）/)

## 背包问题

### 01背包
二维：
```C++
for(int i=1;i<=n;i++)
  for(int v=0;v<=m;v++)
  {
  	if(v-w[i]>=0) f[i][v]=max(f[i-1][v],f[i-1][v-w[i]]+c[i]);
  	else f[i][v]=f[i-1][v];
  }
```
一维：
```C++
for(int i=1;i<=n;i++)
  for(int v=m;v>=w[i];v--)
    f[v]=max(f[v],f[v-w[i]]+c[i]);
```

### 完全背包
优化：拆成$w[i]\*2^k$ , $c[i]\*2^k$ 的若干件（其中$w[i]\*2^k$<V）

二维：
```C++
for(int i=1;i<=n;i++)
  for(int v=0;v<=m;v++)
  {
  	if(v-w[i]>=0) f[i][v]=max(f[i-1][v],f[i][v-w[i]]+c[i]);
  	else f[i][v]=f[i-1][v];
  } 
```
一维：
```C++
for(int i=1;i<=n;i++)
  for(int v=w[i];v<=m;v++)
    f[v]=max(f[v],f[v-w[i]]+c[i]);
```

### 多重背包
优化：拆成1,2,4,…, $2^(k-1)$ , $n[i]-2^k+1$ （其中k为满足$n[i]-x^k+1$>0的最大整数)

二维：
```C++
for(int i=1;i<=n;i++)
  for(int v=0;v<=m;v++)
    for(int k=0;k<=num[i];k++)
    {
    	f[i][v]=max(f[i][v],f[i-1][v]);
  	    if(v-k*w[i]>=0) f[i][v]=max(f[i][v],f[i-1][v-k*w[i]]+k*c[i]);
    }
```
一维：
```C++
for(int i=1;i<=n;i++)
  for(int v=m;v>=0;v--)
    for(int k=0;k<=num[i];k++)
    {
        if(v-k*w[i]<0) break;
        f[v]=max(f[v],f[v-k*w[i]]+k*c[i]);
    }
```

## 并查集
```C++
int find(int x)
{
	if(fa[x]==x) return x;
	else fa[x]=find(fa[x]);
}

//路径压缩
int find(int x)
{
	if(x!=fa[x])
		fa[x]=find(fa[x]);
	return fa[x];
}

inline void un(int x,int y)
{
	fa[find(x)]=find(y);
}
```

## 最小生成树
```C++
#include<cstdio>
#include<iostream>
#include<algorithm>
#define M 500500
#define MAX 2147483647

using namespace std;

int n,m;
int fa[M];
struct E{
	int to,from,w;
}e[M];
int tope,ans;
inline void ae(int x,int y,int z)
{
	tope++;e[tope].to=y;e[tope].from=x;e[tope].w=z;
}

int cmp(E x,E y)
{
	return x.w<y.w;
}

inline int find(int x)
{
	if(x!=fa[x])
		fa[x]=find(fa[x]);
	return fa[x];
}

inline void un(int x,int y)
{
	fa[find(x)]=find(y);
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;++i)
	{
		fa[i]=i;
	}
	int ta,tb,tc;
	for(int i=1;i<=m;++i)
	{
		scanf("%d%d%d",&ta,&tb,&tc);
		ae(ta,tb,tc);
	}
	sort(e+1,e+tope+1,cmp);
	for(int i=1;i<=tope;++i)
	{
		if(find(e[i].to)!=find(e[i].from))
		{
			un(e[i].to,e[i].from);
			ans+=e[i].w;
		}
	}
	printf("%d",ans);
	return 0;
}
```

## gcd
```C++
int gcd(int a,int b)
{
    return b==0?a:gcd(b,a%b);
}
```

