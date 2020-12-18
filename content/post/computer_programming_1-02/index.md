---
slug: "computer_programming_1-02"
title: "程式設計(一)-02：Arithmetic"
date: 2020-10-15T11:19:09+08:00
tags:
    - C
categories:
    - 程式設計(一)
---
# Arithmetic
```c
#include <stdio.h>
int main(){
    int a = 1, b = 2, sum = 0;
    sum = a + b;
    printf("%d", sum);
    return 0;
}
```
- 變數
    - 每個變數都必須有它的型別
    - 在使用變數前必須先宣告它
- 在C裡面， `=` 意思為”指定”，而不是”相等”，指派右邊的數值給左邊的變數
- 一個好習慣，總是初始化變數
- C Spec:
    - C89:If an object that has static storage duration is not initialized explicitly, it is initialized implicitly.
    - C99: If it has arithmetic type, it is initialized to (positive or unsigned) zero.
---
- In C99: `a == (a / b) * b + a % b`
- `printf` 是一個函式去印出格式化字串
    - `%d` -> 有號十進位整數
    - `%f` -> 十進位浮點數
    - `%u` -> 無號十進位整數
    - 當然不只這些
- 小技巧
```c
a += b -> a = a + b
a -= b -> a = a - b
a *= b -> a = a * b
a /= b -> a = a / b
a %= b -> a = a % b
```
---
- 查看記憶體使用大小： `sizeof()`，回傳單位為 `byte` ( `printf("%lu", sizeof());` )
- `#include <stdint.h>`
    - int8_t: 8-bit signed interger
    - int16_t: 16-bit signed interger
    - int32_t: 32-bit signed interger
    - int64_t: 64-bit signed interger
    - uint8_t: 8-bit unsigned interger
    - uint16_t: 16-bit unsigned interger
    - uint32_t: 32-bit unsigned interger
    - uint64_t: 64-bit unsigned interger
---
- 輸入：`scanf("%d", &a);`
    - 至於為甚麼需要 `&`，之後會在指標的章節介紹到
    - `scanf` 是否有回傳值? ( `man 3 scanf` )
---
- 最後也最重要的技能：RTFM and STFG