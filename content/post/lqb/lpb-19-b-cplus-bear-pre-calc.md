---
title: "蓝桥杯 2017年省赛C++B组题3 承压计算"
date: 2019-03-19T21:55:14+08:00
draft: false
author: "Meng小羽"
authorLink: "https://debuginn.com"
authorEmail: "debuginn@icloud.com"
keywords: "蓝桥杯"
comments: true
tags: ["蓝桥杯","algorithm"]
categories: ["algorithm"]
image: "https://webp.debuginn.com/202303241303887.jpg"
---

X星球的高科技实验室中整齐地堆放着某批珍贵金属原料。

每块金属原料的外形、尺寸完全一致，但重量不同。

金属材料被严格地堆放成金字塔形。

```shell
                             7 
                            5 8 
                           7 8 8 
                          9 2 7 2 
                         8 1 4 9 1 
                        8 1 8 8 4 1 
                       7 9 6 1 4 5 4 
                      5 6 5 5 6 9 5 6 
                     5 5 4 7 9 3 5 5 1 
                    7 5 7 9 7 4 7 3 3 1 
                   4 6 4 5 5 8 8 3 2 4 3 
                  1 1 3 3 1 6 6 5 5 4 4 2 
                 9 9 9 2 1 9 1 9 2 9 5 7 9 
                4 3 3 7 7 9 3 6 1 3 8 8 3 7 
               3 6 8 1 5 3 9 5 8 3 8 1 8 3 3 
              8 3 2 3 3 5 5 8 5 4 2 8 6 7 6 9 
             8 1 8 1 8 4 6 2 2 1 7 9 4 2 3 3 4 
            2 8 4 2 2 9 9 2 8 3 4 9 6 3 9 4 6 9 
           7 9 7 4 9 7 6 6 2 8 9 4 1 8 1 7 2 1 6 
          9 2 8 6 4 2 7 9 5 4 1 2 5 1 7 3 9 8 3 3 
         5 2 1 6 7 9 3 2 8 9 5 5 6 6 6 2 1 8 7 9 9 
        6 7 1 8 8 7 5 3 6 5 4 7 3 4 6 7 8 1 3 2 7 4 
       2 2 6 3 5 3 4 9 2 4 5 7 6 6 3 2 7 2 4 8 5 5 4 
      7 4 4 5 8 3 3 8 1 8 6 3 2 1 6 2 6 4 6 3 8 2 9 6 
     1 2 4 1 3 3 5 3 4 9 6 3 8 6 5 9 1 5 3 2 6 8 8 5 3 
    2 2 7 9 3 3 2 8 6 9 8 4 4 9 5 8 2 6 3 4 8 4 9 3 8 8 
   7 7 7 9 7 5 2 7 9 2 5 1 9 2 6 5 3 9 3 5 7 3 5 4 2 8 9 
  7 7 6 6 8 7 5 5 8 2 4 7 7 4 7 2 6 9 2 1 8 2 9 8 5 7 3 6 
 5 9 4 5 5 7 5 5 6 3 5 3 9 5 8 9 5 4 1 2 6 1 4 3 5 3 2 4 1 
X X X X X X X X X X X X X X X X X X X X X X X X X X X X X X 
```

其中的数字代表金属块的重量（计量单位较大）。
最下一层的X代表30台极高精度的电子秤。

假设每块原料的重量都十分精确地平均落在下方的两个金属块上，
最后，所有的金属块的重量都严格精确地平分落在最底层的电子秤上。
电子秤的计量单位很小，所以显示的数字很大。

工作人员发现，其中读数最小的电子秤的示数为：2086458231

请你推算出：读数最大的电子秤的示数为多少？

注意：需要提交的是一个整数，不要填写任何多余的内容。

## 格式化金字塔

将金字塔转化为二维数组形式，见下图：

```shell
{7},
{5,8},
{7,8,8},
{9,2,7,2},
{8,1,4,9,1},
{8,1,8,8,4,1},
{7,9,6,1,4,5,4},
{5,6,5,5,6,9,5,6},
{5,5,4,7,9,3,5,5,1},
{7,5,7,9,7,4,7,3,3,1},
{4,6,4,5,5,8,8,3,2,4,3},
{1,1,3,3,1,6,6,5,5,4,4,2},
{9,9,9,2,1,9,1,9,2,9,5,7,9},
{4,3,3,7,7,9,3,6,1,3,8,8,3,7},
{3,6,8,1,5,3,9,5,8,3,8,1,8,3,3},
{8,3,2,3,3,5,5,8,5,4,2,8,6,7,6,9},
{8,1,8,1,8,4,6,2,2,1,7,9,4,2,3,3,4},
{2,8,4,2,2,9,9,2,8,3,4,9,6,3,9,4,6,9},
{7,9,7,4,9,7,6,6,2,8,9,4,1,8,1,7,2,1,6},
{9,2,8,6,4,2,7,9,5,4,1,2,5,1,7,3,9,8,3,3},
{5,2,1,6,7,9,3,2,8,9,5,5,6,6,6,2,1,8,7,9,9},
{6,7,1,8,8,7,5,3,6,5,4,7,3,4,6,7,8,1,3,2,7,4},
{2,2,6,3,5,3,4,9,2,4,5,7,6,6,3,2,7,2,4,8,5,5,4},
{7,4,4,5,8,3,3,8,1,8,6,3,2,1,6,2,6,4,6,3,8,2,9,6},
{1,2,4,1,3,3,5,3,4,9,6,3,8,6,5,9,1,5,3,2,6,8,8,5,3},
{2,2,7,9,3,3,2,8,6,9,8,4,4,9,5,8,2,6,3,4,8,4,9,3,8,8},
{7,7,7,9,7,5,2,7,9,2,5,1,9,2,6,5,3,9,3,5,7,3,5,4,2,8,9},
{7,7,6,6,8,7,5,5,8,2,4,7,7,4,7,2,6,9,2,1,8,2,9,8,5,7,3,6},
{5,9,4,5,5,7,5,5,6,3,5,3,9,5,8,9,5,4,1,2,6,1,4,3,5,3,2,4,1}
```

## 解题算法

```c
#include "iostream"
#include "algorithm"
double a[30][30]{
	{7},
	{5,8},
	{7,8,8},
	{9,2,7,2},
	{8,1,4,9,1},
	{8,1,8,8,4,1},
	{7,9,6,1,4,5,4},
	{5,6,5,5,6,9,5,6},
	{5,5,4,7,9,3,5,5,1},
	{7,5,7,9,7,4,7,3,3,1},
	{4,6,4,5,5,8,8,3,2,4,3},
	{1,1,3,3,1,6,6,5,5,4,4,2},
	{9,9,9,2,1,9,1,9,2,9,5,7,9},
	{4,3,3,7,7,9,3,6,1,3,8,8,3,7},
	{3,6,8,1,5,3,9,5,8,3,8,1,8,3,3},
	{8,3,2,3,3,5,5,8,5,4,2,8,6,7,6,9},
	{8,1,8,1,8,4,6,2,2,1,7,9,4,2,3,3,4},
	{2,8,4,2,2,9,9,2,8,3,4,9,6,3,9,4,6,9},
	{7,9,7,4,9,7,6,6,2,8,9,4,1,8,1,7,2,1,6},
	{9,2,8,6,4,2,7,9,5,4,1,2,5,1,7,3,9,8,3,3},
	{5,2,1,6,7,9,3,2,8,9,5,5,6,6,6,2,1,8,7,9,9},
	{6,7,1,8,8,7,5,3,6,5,4,7,3,4,6,7,8,1,3,2,7,4},
	{2,2,6,3,5,3,4,9,2,4,5,7,6,6,3,2,7,2,4,8,5,5,4},
	{7,4,4,5,8,3,3,8,1,8,6,3,2,1,6,2,6,4,6,3,8,2,9,6},
	{1,2,4,1,3,3,5,3,4,9,6,3,8,6,5,9,1,5,3,2,6,8,8,5,3},
	{2,2,7,9,3,3,2,8,6,9,8,4,4,9,5,8,2,6,3,4,8,4,9,3,8,8},
	{7,7,7,9,7,5,2,7,9,2,5,1,9,2,6,5,3,9,3,5,7,3,5,4,2,8,9},
	{7,7,6,6,8,7,5,5,8,2,4,7,7,4,7,2,6,9,2,1,8,2,9,8,5,7,3,6},
	{5,9,4,5,5,7,5,5,6,3,5,3,9,5,8,9,5,4,1,2,6,1,4,3,5,3,2,4,1}
};

int main(){
	int i, j;
	double max=0, min=9999999;
	double result;
	for(i=1; i<=29; i++){
		for(j=0; j<=i; j++){
			//如果为首尾两个数值话，直接自身/2运算 
			if(j==0){
				a[i][j] += a[i-1][0]/2.0;
			}else{
			//正常两个数值进行向下除法加法运算 
				a[i][j] += a[i-1][j-1]/2.0 + a[i-1][j]/2.0;
			}
		}
	} 
	for(i=0; i<=29; i++){
		if(a[29][i]<min){
			min = a[29][i];
		}
		if(a[29][i]>max){
			max = a[29][i];
		}
	} 
	printf("%lf\n",2086458231/min*max);
	
	return 0;
}
```

## 题解答案

72665192664
