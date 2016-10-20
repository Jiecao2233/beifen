---
title: Tarjan（强连通分量）
tags:
  - Tarjan
categories:
  - Tarjan
date: 2016-10-13 21:16:53
---

## 前言
如何判环？用Tarjan啊！多好用啊！（虽然不会写啊）（要不然写这篇博客干啥啊）

<!--more-->

## 1.思想
参考第一道例题程序里的注释

## 2.Tarjan例题之信息传递（noip2015）
【题目链接】
[洛谷【2661】](https://www.luogu.org/problem/show?pid=2661)
【题目描述】
有n个同学（编号为1到n）正在玩一个信息传递的游戏。在游戏里每人都有一个固定的信息传递对象，其中，编号为i的同学的信息传递对象是编号为Ti同学。
游戏开始时，每人都只知道自己的生日。之后每一轮中，所有人会同时将自己当前所知的生日信息告诉各自的信息传递对象（注意：可能有人可以从若干人那里获取信息，但是每人只会把信息告诉一个人，即自己的信息传递对象）。当有人从别人口中得知自己的生日时，游戏结束。请问该游戏一共可以进行几轮？
【输入格式】
输入共2行。
第1行包含1个正整数n表示n个人。
第2行包含n个用空格隔开的正整数T1,T2,……,Tn其中第i个整数Ti示编号为i
的同学的信息传递对象是编号为Ti的同学，Ti≤n且Ti≠i
数据保证游戏一定会结束。
【输出格式】
输出共 1 行，包含 1 个整数，表示游戏一共可以进行多少轮。
【输入样例】

>5
2 4 2 3 1

【输出样例】

>3

【题解】
裸Tarjan。。。
能传递回去就证明有环，能进行的次数就是最小环里的元素个数。
【程序】
```C++
#include<iostream>
#include<cstdio>
#include<algorithm>
#define M 500500

using namespace std;

int n,tmp;

struct E{
	int to,next,w;
}e[M];
int tope,head[M];

int dfn[M],low[M],isinstk[M],stk[M],stktop,dfstop,sov[M],totsov;

inline void ae(int x,int y,int z)
{
	tope++;e[tope].to=y;e[tope].next=head[x];head[x]=tope;e[tope].w=z;
}

void tarjan(int x)
{
    dfn[x]=low[x]=++dfstop;        //将dfn和low值更新为dfs序
    isinstk[x]=1;                  //入栈
    stk[++stktop]=x;               //入栈
    for(int i=head[x],y=e[i].to;i;i=e[i].next,y=e[i].to)   //链式前向星遍历
    {
        if(dfn[y]==0)                      //如果儿子节点的dfn值为0，跑它的tarjan，并更新它的low值
        {
            tarjan(y);
            low[x]=min(low[x],low[y]);     //用low更新low，不要问为什么，记住就行
        }
        else if(isinstk[y])                //如果指向点在栈里，说明找到了一个强连通分量
            low[x]=min(low[x],dfn[y]);     //用dfn更新low，不要问为什么，记住就行
        
    }
    if(dfn[x]==low[x])                //dfn等于low就说明跑回来了，开始退栈
    {
        totsov++; 
        int i;
        do
        {
            i=stk[stktop--];
            sov[totsov]++;
            isinstk[i]=0;
        }while(i!=x);                    //如果用for或者while，起点不会退栈
    }
}

int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;++i)
	{
		scanf("%d",&tmp);
		ae(i,tmp,1);
	}
	for(int i=1;i<=n;++i)
	{
		if(!dfn[i])
			tarjan(i);
	}
	sort(sov+1,sov+totsov+1);
    for(int i=1;i<=totsov;++i)
    {
    	if(sov[i]>1)
        {
            printf("%d",sov[i]);
            break;
        }
    }
	return 0;
}
```
***
## 3.Tarjan例题之上白泽慧音
【题目链接】
[洛谷【1726】](https://www.luogu.org/problem/show?pid=1726)
【题目描述】
在幻想乡，上白泽慧音是以知识渊博闻名的老师。春雪异变导致人间之里的很多道路都被大雪堵塞，使有的学生不能顺利地到达慧音所在的村庄。因此慧音决定换一个能够聚集最多人数的村庄作为新的教学地点。人间之里由N个村庄（编号为1..N）和M条道路组成，道路分为两种一种为单向通行的，一种为双向通行的，分别用1和2来标记。如果存在由村庄A到达村庄B的通路，那么我们认为可以从村庄A到达村庄B，记为(A,B)。当(A,B)和(B,A)同时满足时，我们认为A,B是绝对连通的，记为<A,B>。绝对连通区域是指一个村庄的集合，在这个集合中任意两个村庄X,Y都满足<X,Y>。现在你的任务是，找出最大的绝对连通区域，并将这个绝对连通区域的村庄按编号依次输出。若存在两个最大的，输出字典序最小的，比如当存在1,3,4和2,5,6这两个最大连通区域时，输出的是1,3,4。
【输入格式】
第1行：两个正整数N,M
第2到M+1行：每行三个正整数a,b,t, t = 1表示存在从村庄a到b的单向道路，t = 2表示村庄a,b之间存在双向通行的道路。保证每条道路只出现一次。
【输出格式】
第1行： 1个整数，表示最大的绝对连通区域包含的村庄个数。
第2行：若干个整数，依次输出最大的绝对连通区域所包含的村庄编号。
【输入样例】

>5 5
1 2 1
1 3 2
2 4 2
5 1 2
3 5 1

【输出样例】

>3
1 3 5

【题解】
在图中找出一个最大环，输出它的大小，并按字典序输出其中的元素。
【程序】
```C++
#include<cstdio>
#include<iostream>
#include<algorithm>
#define M 500500

using namespace std;

int n,m; 

struct E{
	int to,next;
}e[M];
int tope,head[M];

int dfn[M],low[M],dfstop,stk[M],stktop,isinstk[M],sov[M],sovtot,maxsov[M];

inline void ae(int x,int y)
{
	tope++;e[tope].to=y;e[tope].next=head[x];head[x]=tope;
}

inline void tarjan(int x)
{
	dfn[x]=low[x]=++dfstop;
	isinstk[x]=1;stk[++stktop]=x;
	for(int i=head[x],y=e[i].to;i;i=e[i].next,y=e[i].to)
	{
		if(!dfn[y])
		{
			tarjan(y);
			low[x]=min(low[x],low[y]);
		}
		else if(isinstk[y])
			low[x]=min(low[x],dfn[y]);
	}
	if(dfn[x]==low[x])
	{
		sovtot++;
		int i=-1;
		do
		{
			i=stk[stktop--];
			sov[sovtot]++;
			maxsov[i]=sovtot;
			isinstk[i]=0;
		}while(i!=x);
	}
}

int main()
{
	scanf("%d%d",&n,&m);
	int ta,tb,tc;
	for(int i=1;i<=m;++i)
	{
		scanf("%d%d%d",&ta,&tb,&tc);
		ae(ta,tb);
		if(tc==2)
		{
			ae(tb,ta);
		}
	}
	for(int i=1;i<=n;++i)
	{
		if(!dfn[i])
			tarjan(i);
	}
	int maxx=0,mpos=0;
	for(int i=1;i<=sovtot;++i)
	{
		if(sov[i]>maxx) maxx=sov[i],mpos=i;
	}
	printf("%d\n",maxx);
	for(int i=1;i<=n;++i)
	{
		if(maxsov[i]==mpos)
			printf("%d ",i);
	}
	return 0;
}
```