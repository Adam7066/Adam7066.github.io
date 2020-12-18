---
slug: "computer_programming_1-05"
title: "程式設計(一)-05：Function"
date: 2020-11-21T21:48:47+08:00
tags:
    - C
categories:
    - 程式設計(一)
---
# Function
## 函式
#### double
- `double` 是一種浮點數型別，就像是 `float`
- 就如同它的名字，它使用的記憶體大小為 `float` 的兩倍
- 建議: 當你需要浮點數的話，一律使用 `double`
---
- 到目前為止我們最常使用到的函式為 `printf`
- 我們稱這些函式為 C standard functions (C標準函式)
- 所有的函式都被儲存在libraries中
    - 如果你想要讀書，你需要知道書在哪，然後去圖書館借書
    - 如果你想要使用函式，你需要知道函式在哪，然後include library去使用函式
    - 例如: `stdio.h <-> printf`
- 使用 `math.h` 時，需下編譯參數 `-lm`
---
- 永不重新發明輪子
- 在開發前請先搜尋
---
```c
//原型宣告
Return-Value-Type Function-Name (parameter-Type-list);

Return-Value-Type Function-Name (parameter-list){
    Statements
}
```
- 使用原型宣告並將自訂函式置於main function之後的好處?
    - 不用管function之間的先後順序。
---
#### void
- 沒有型別
- 在這裡，代表不需要回傳值

## 標頭檔 (Header Files)
- 甚麼是header file?
    - 是一個包含函式的原型宣告(prototypes)和其他定義(definitions)的檔案
- 為甚麼我們需要header file?
    - 抽象層
    - 有時我們想保護我們的實作(implementation)
---
```c
#ifndef
#define
...
#endif
```
- 如同我所說，軟體工程師是懶惰的
```c
#pragma once
```

## 如何編譯多個檔案
```c
#static
gcc -c library.c -o library.o
gcc main.c library.o -o main

#dynamic
gcc -shared test.o -o libtest.so
gcc main.c -o main -L. -ltest
#執行
LD_LIBRARY_PATH=. ./main
```
#### Link
- Static link: Static linking is the process of copying **all library modules** used in the program into the **final executable** image.
- Dynamic link: In dynamic linking the names of the external libraries (shared libraries) are placed in the final executable file while **the actual linking takes place at run time** when both executable file and libraries are placed in the memory.
- `.a` 是一堆 `.o` 包在一起

## Random
```c
#include <stdlib.h>
#include <time.h>

srand(time(0));
n = rand % 6 //n: 0 ~ 5
```
- 有安全要求時，請勿使用 `random()`

## Global, Static, Extern Variable
#### global
- 變數的生命週期為整個程式。
- 也可被extern所存取到。
#### static
- 變數只會在程式開始之前分配和初始化一次。
- 在程式終止之前，儲存空間都不會被釋放。
- 加上 `static` 後， `extern`便無法存取了。
#### extern
- 使用外部的變數。

## 遞迴 Recursive
- 遞迴定義如下
    - 遞迴:
        參見「遞迴」。
- 什麼?這個定義什麼也沒有說啊!好吧，改一下:
    - 遞迴:
    如果你還是沒明白遞迴是什麼意思的話，參見「遞迴」。
```c
Return-Value-Type Function-Name ( parameter-list ) {
    if ( Base case ) {
        Return pre-defined value.
    }
    else {
        Call itself with parameter modification.
    }
}
```
- 所有能使用遞迴表達的敘述，皆能以迴圈的方式編寫。
- To iterate is human, to recurse, divine. — L. Peter Deutsch
- 遞迴只應天上有，人間該當用迴圈
- 我的觀點:
    - 如果你找到關係式，遞迴是簡單的。
    - 時常用在虛擬碼(pseudo-code)中。
    - 性能效率可能比迭代差。