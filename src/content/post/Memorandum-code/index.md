---
title: "[备忘录]简单算法的实现"
publishDate: "6 October 2024"
updatedDate: "6 October 2024"
description: "记录一些简单算法的实现，以备日后参考"
tags: ["备忘录","image"]
coverImage:
    src: "./komari-cover.png"
    alt: "Komari"
---

## 数学运算

### 回文数与数字反转

``` c++ title = "palindrome number"
int reverseNumber(int x) {
    int reversed = 0;
    while (x != 0) {
        reversed = reversed * 10 + x % 10;
        x /= 10;
    }
    return reversed;
}


bool isPalindrome(int x) {
    int x_reverse = reverseNumber(x);
    if (x == x_reverse) {
        return true;
    };
    return false;
}
```

### 比较大小（三元运算符）

``` c++ title = "compare size"
void compareSize(int a, int b) {
    max = (a > b) ? a : b;  // If (a > b) returns True, then return a as value.
    min = (a < b) ? a : b;
    // (expression) ? do A : do B;
    // If expression is True, then do A, otherwise do B.
}

```

### 判断素数

``` c++ title = "isPrimenumber"
bool isPrimenumber(int x) {
    for (int i = 2; i <= sqrt(x); i++) {    // i <= sqrt(x)
        if (x % i == 0) {
            return false;
        };
    };
    return true;
}
```

## 简单算法

### 冒泡排序

每次遍历时，最后一个数一定是该次遍历最大的数，因此下次遍历时即可略过。

```c++ title = "bubble sort"
void bubbleSort(vector<int>& v) {
    for (int i = 0; i <= v.size() - 2; i++) {
        for (int j = 0; j <= v.size() - 2 - i; j++) {
            if (v[j] > v[j + 1]) {
                swap(v[j], v[j + 1]);
            };
        };
    };
};

```
