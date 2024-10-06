---
title: "[备忘录]简单算法的实现"
publishDate: "6 October 2024"
updatedDate: "6 October 2024"
description: "记录一些简单算法的实现，以备日后参考"
tags: ["备忘录"]
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
