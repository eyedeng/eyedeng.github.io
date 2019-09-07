---
layout:     post                    # 使用的布局（不需要改）
title:      BUA招新.prompt               # 标题 
subtitle:   希望能讲完
date:       2019-09-08              # 时间
author:     eyedeng                      # 作者
header-img: img/alg.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - school
---

## 挑选苹果
1. 首先将第一颗苹果放入口袋中。
2. 从第二颗苹果开始检查，如果正在检查的苹果比口袋中的还大，则将它捡起放入口袋中，同时丢掉原先口袋中的苹果。反之则继续下一颗苹果。直到最后一颗苹果。
3. 最后口袋中的苹果就是所有的苹果中最大的一颗。  
  
_求最大值_
## 抓牌
1. 从第一个元素开始，该元素可以认为已经被排序
2. 取出下一个元素，在已经排序的元素序列中从后向前扫描
3. 如果该元素（已排序）大于新元素，将该元素移到下一位置
4. 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
5. 将新元素插入到该位置后
6. 重复步骤2~5 

_插入排序_
## 打头
伤害值排序
另：100个敌人，3发子弹
_贪心_
## 猜数
想一个100内的数
_二分_
## 最大公约数
```python
def gcd(num1, num2):
	if num2 == 0:
		return num1
	else:
		return gcd(num2, num1%num2)
```
```cpp
int gcd(int num1,int num2)
{
	if(num2 == 0)
		return num1;
	return gcd(num2, num1%num2);
}
```
## ...

[poj submit](http://poj.org/status)
