---
layout:     post                    # 使用的布局（不需要改）
title:      Catalan数               # 标题 
subtitle:    #副标题
date:       2019-08-03              # 时间
author:     eyedeng                      # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - algorithm
---

$C_{n+1}= \sum_{k=0}^{n}C_{k}C_{n-k}$

```cpp
#define M 35
ll C[M+1];
void catalan()
{
    C[0]=C[1]=1;
    for(int i=2; i<=M; i++)
    {
        C[i]=0;
        for(int j=0; j<i; j++)
            C[i] += C[j]*C[i-j-1];
    }
}
```
