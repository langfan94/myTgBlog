---
title: two-eggs
date: 2020-03-20 18:07:38
tags: C++, javascript
---

# Two Egg Problem

> http://datagenetics.com/blog/july22012/index.html

C++
``` C++
#include <iostream>
#include <vector>
#include <string>
#include<algorithm>

using namespace std;

const int Floors = 25;
const int Eggs = 5;

// max{ fn{egg - 1, dropFloor} fn{egg, floor - dropFloor} }

// 一共egg个鸡蛋, floor层, 从dropFloor层扔, dropFloor 取值 1 - n
int drop(int egg, int floor) {
    if (egg == 0 || floor == 0) {
        printf("egg %d and floor %d: %d \n", egg, floor, 0);
        return 0;
    }

    if (egg == 1) {
        printf("egg %d and floor %d: %d \n", egg, floor, floor);
        return floor;
    }

    if (floor == 1) {
        printf("egg %d and floor %d: %d \n", egg, floor, 1);
        return 1;
    }

    int minRes = INT_MAX;
    for (int i = 1; i < floor; i++)
    {
        int curRes = max(drop(egg - 1, i - 1), drop(egg, floor - i));
        if (minRes > curRes) {
            minRes = curRes;
        }
    }

    printf("egg %d and floor %d: %d \n", egg, floor, minRes + 1);
    return minRes + 1;
} 

int main()
{
    int msg = drop(Eggs, Floors);

    cout << msg << endl;
}
```
运行时间: 6.8512s



javascript

```javascript
const Floors = 25;
const Eggs = 5;

// max{ fn{egg - 1, dropFloor} fn{egg, floor - dropFloor} }

// 一共egg个鸡蛋, floor层, 从dropFloor层扔
function drop(egg, floor) {
    if (egg == 0 || floor == 0) {
        return 0;
    }

    if (egg == 1) {
        return floor;
    }

    if (floor == 1) {
        return 1;
    }

    let minRes = Infinity;
    for (let i = 1; i < floor; i++)
    {
        let curRes = Math.max(drop(egg - 1, i - 1), drop(egg, floor - i));
        if (minRes > curRes) {
            minRes = curRes;
        }
    }

    return minRes + 1;
}

console.log(drop(Eggs, Floors));
```
运行时间: 6.022s

思路：F层E个鸡蛋 可以拆解成 假设从k层仍一个鸡蛋 Max{ k层E-1个鸡蛋, F-k层E个鸡蛋 }

k为最优解 可以用遍历的方式 k从 1 - F 层取值 Min{ (1 - F) 层扔一个鸡蛋 } 带入上面 递归计算。