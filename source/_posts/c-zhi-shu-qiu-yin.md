---
title: C语言-求质因数
date: 2017-07-03 00:24:23
categories: C
tags:
- C语言
- 数学
---

语言求质因数

输入[2, 100000]之间的数字.
会求得质因数,返回结果为
n=n1xn2xn3xn3xn4

数学学的菜^^

<!-- more -->

```
#include <stdio.h>
/*
这个函数是用来获取当前最小素数 
*/
int ss(int n) {
	int x ;
	int ret =0;
	for (x = 2 ; x <= n; x++){
		if (n % x == 0) {
			ret = x;
			break;
		}
	}
	return ret;
} 
int main(){	
	int x;
	scanf("%d", &x);
	if (x >= 2 && x <= 100000){
	int y = 2;
	int i;
	printf("%d=",x );
	for (i = 0; x >1; i++){
		if (x % y == 0) {
			// 如果当前的合数可以被当前素数整除 
			x = x / y;
		} else {
			/* 
			如果当前合数不能被素数整除, 我们就调用函数
			获取能够整除当前合数最小的素数. 
			*/ 
			y = ss(x);
			x = x / y;
		}
		printf("%d",y );
		if(x > 1) {
			printf("x");
		}
	}
}
	return 0;
}
```