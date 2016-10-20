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
```C++
int power(int a,int b,int P)
{
    int re=1;
    for(;b;b>>=1,a=a*a%P)
        if(b&1) re=re*a%P;
    return re;
}
```

## 最短路

### SPFA
```C++
memset(ds,0x7f,sizeof(ds));//不用memset，for一遍
ds[S]=0; vis[S]=1; q.push(S);
while(!q.empty())
{
    int x=q.front(); q.pop(); vis[x]=0;
    for(int i=0;i<to[x].size();i++)
    {
        int y=to[x][i],c=cost[x][i];
        if(ds[y]>ds[x]+c)
        {
            ds[y]=ds[x]+c;
            if(!vis[y])
            {
                vis[y]=1; q.push(y);
            }
        }
    }
}
```

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
```C++
for(int i=1;i<=n;i++)
  for(int v=0;v<=m;v++)
  {
  	if(v-w[i]>=0) f[i][v]=max(f[i-1][v],f[i-1][v-w[i]]+c[i]);
  	else f[i][v]=f[i-1][v];
  }
  
for(int i=1;i<=n;i++)
  for(int v=m;v>=w[i];v--)
    f[v]=max(f[v],f[v-w[i]]+c[i]);
```

### 完全背包
优化：拆成$w[i]\*2^k$ , $c[i]\*2^k$ 的若干件（其中$w[i]\*2^k$<V）

```C++
for(int i=1;i<=n;i++)
  for(int v=0;v<=m;v++)
  {
  	if(v-w[i]>=0) f[i][v]=max(f[i-1][v],f[i][v-w[i]]+c[i]);
  	else f[i][v]=f[i-1][v];
  } 
  
for(int i=1;i<=n;i++)
  for(int v=w[i];v<=m;v++)
    f[v]=max(f[v],f[v-w[i]]+c[i]);
```

### 多重背包
优化：拆成1,2,4,…, $2^(k-1)$ , $n[i]-2^k+1$ （其中k为满足$n[i]-x^k+1$>0的最大整数)
```C++
for(int i=1;i<=n;i++)
  for(int v=0;v<=m;v++)
    for(int k=0;k<=num[i];k++)
    {
    	f[i][v]=max(f[i][v],f[i-1][v]);
  	    if(v-k*w[i]>=0) f[i][v]=max(f[i][v],f[i-1][v-k*w[i]]+k*c[i]);
    }
    
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

void un(int x,int y)
{
	fa[find(x)]=find(y);
}
```

## gcd
```C++
int gcd(int a,int b)
{
    return b==0?a:gcd(b,a%b);
}
```

