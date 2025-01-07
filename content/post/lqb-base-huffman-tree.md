---
title: "蓝桥杯 基础练习 Huffuman树"
date: 2019-02-04T22:08:44+08:00
draft: false
author: "Meng小羽"
authorLink: "https://debuginn.com"
authorEmail: "debuginn@icloud.com"
keywords: "蓝桥杯"
comments: true
weight: 0

tags: ["蓝桥杯","algorithm"]
categories: ["algorithm"]


image: "https://webp.debuginn.com/202303241303887.jpg"
imagePreview: "https://webp.debuginn.com/202303241303887.jpg"
---

## 问题描述

Huffman树在编码中有着广泛的应用。在这里，我们只关心Huffman树的构造过程。 

给出一列数`{pi}={p0,p1, …,pn-1}`，用这列数构造Huffman树的过程如下：

1. 找到{pi}中最小的两个数，设为pa和pb，将pa和pb从{pi}中删除掉，然后将它们的和加入到{pi}中。这个过程的费用记为pa+pb。
2. 重复步骤1，直到{pi}中只剩下一个数。

在上面的操作过程中，把所有的费用相加，就得到了构造Huffman树的总费用。

本题任务：对于给定的一个数列，现在请你求出用该数列构造Huffman树的总费用。

例如，对于数列`{pi}={5, 3, 8, 2, 9}`，Huffman树的构造过程如下：

1. 找到{5, 3, 8, 2, 9}中最小的两个数，分别是2和3，从{pi}中删除它们并将和5加入，得到{5, 8, 9, 5}，费用为5。 
2. 找到{5, 8, 9, 5}中最小的两个数，分别是5和5，从{pi}中删除它们并将和10加入，得到{8, 9, 10}，费用为10。
3. 找到{8, 9, 10}中最小的两个数，分别是8和9，从{pi}中删除它们并将和17加入，得到{10, 17}，费用为17。 
4. 找到{10, 17}中最小的两个数，分别是10和17，从{pi}中删除它们并将和27加入，得到{27}，费用为27。 
5. 现在，数列中只剩下一个数27，构造过程结束，总费用为5+10+17+27=59。

## 输入格式

输入的第一行包含一个正整数`n（n<=100）`。

接下来是n个正整数，表示`p0,p1, …,pn-1`，每个数不超过1000。

## 输出格式

输出用这些数构造Huffman树的总费用。

## 样例输入

```shell
5
5 3 8 2 9
```

## 样例输出

59

## C++算法

```c
#include<iostream>
#include<queue>
using namespace std;
priority_queue<int, vector<int>, greater<int> > pq; //构造从小到大的优先队列 
int main() {
  int n;
  cin >> n;
  while (!pq.empty())
    pq.pop();
  int x, s;
  for (int i = 0; i < n; i++) {
    cin >> x;
    pq.push(x); 
  } 
  int sum = 0;
  while (pq.size() > 1) {
    s = pq.top();
    pq.pop();
    s += pq.top();
    pq.pop();
    sum += s;
    pq.push(s);
  }
  cout << sum << endl;
}
```