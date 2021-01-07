---
slug: "computer_programming_1-07"
title: "程式設計(一)-07：Array"
date: 2020-12-07T19:38:14+08:00
tags:
    - C
categories:
    - 程式設計(一)
---
# Array
- 陣列是一種可以儲存大量相同型別資料的方法。
- 連續的記憶體位置。
- 永遠從0開始
    - `int32_t a[10]` -> `a[0] ~ a[9]`
- 計數變數 i 的型別可以宣告為 `size_t`，它是一個無號的整數型別。
- 初始化
    - `int32_t a[5] = {0, 0, 0, 0, 0};`
    - `int32_t a[5] = {0};`
- 存取陣列元素使用 `variable[index]`
- 專業說明：電腦將找到第一個元素的地址，然後根據索引移動記憶體位置以訪問數據。
- 事實上一維陣列可以處理所有情況，至於多維陣列只是給人類方便閱讀的。
---
# define
- 是遇處理指令，不是C的詞(statement)
- 我們可以使用 `#define` 去做巨集(MACRO)
    - 當開發時NACRO有些像function，然而對電腦而言他們是不同的。
    - 當遇到MACRO，編譯器將簡單的依定義替換掉程式碼。
    - 函式擁有自己的標記。
# 基本排序
#### 氣泡排序法
```c
for(int i = 0; i < n; ++i) {
    for(int j = i; j < n; ++j) {
        if(a[j] < a[i]) {
            a[i] = a[i] ^ a[j];
            a[j] = a[i] ^ a[j];
            a[i] = a[i] ^ a[j];
        }
    }
}
```
#### qsort
```c
#include <stdlib.c>
int cmp(const void *a, const void *b) {
    return (*(int*)a - *(int*)b);
}
qsort(a, n, sizeof(int), cmp);
```

# 傳陣列至函式
```c
int f(int [][n], int);

f(a, 10);

int f(int a[][n], int size){

}
```
- 除了第一個[]外，剩下的都必須要給大小。(電腦才能計算偏移量)
- 為甚麼要給size? 因為傳過去的只是陣列的記憶體起始位置而已。
- 在函式中依然會改到本身的值。

# const
- constant
- read-only
# 可變長度陣列
- Variable Length Array
- 雖然有些編譯器支援了以下寫法(C99之後)，但有些依然不支援
```c
int n = 5;
int a[n] = {0};
```
- 但你應該使用 `malloc`
    - 準確來說，你應該使用 `calloc`，而不是 `malloc。
- 我的建議：當考試的時候不要使用這功能( `a[n]` )，因為你不知道編譯器的版本。