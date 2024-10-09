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

### 最大公约数（欧几里得算法）

通过对两数辗转相除并取余，最后得到的最大公约数一定也是余数的因数。
想象有A、B两个箱子，把A放进B，留下的空隙就是这一次的余数。
再想象和空隙同体积的临时箱子C1，用C1填满A，最后留下空隙C2。
依次类推，最后会留下一个“最小单位体积”的箱子Cn，也就是两数的最大公因数。
特别的，当两数互质时，最大公因数是1。

递归伪代码：

```text "Greatest Common Divisor"
gcd(a, b) = gcd(b, a % b)
```

```c++ title = "Greatest Common Divisor"
int gcd(int a, int b) {
    while (b != 0) {
        int remainder = a % b;
        a = b;
        b = remainder;
    }
    return a;
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
