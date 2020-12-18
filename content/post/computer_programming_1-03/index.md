---
slug: "computer_programming_1-03"
title: "程式設計(一)-03：Condition Control"
date: 2020-10-15T20:53:14+08:00
tags:
    - C
categories:
    - 程式設計(一)
---
# Condition Control
## 簡介
- 我們想要讓電腦去做基礎的判斷
    - `if`
    - `switch`

## If
```c
if (condition1) {
    statements;
}
else if (condition2){
    ...
}
else {
    ...
}
```
- 如果條件**不是錯誤**，那麼將會執行大括號裡的敘述
    - 簡而言之，`false` 被定義為 `0`
- `>` -> 大於
- `<` -> 小於
- `>=` -> 大於等於
- `<=` -> 小於等於
- `==` -> 等於
- `!=` -> 不等於
- `&&` -> and
- `||` -> or

## Boolean
- 在 `Cpp` 裡，有個型別稱為 `bool`
    - 它只有兩個值：`true, false`
    - 那麼 `bool` 使用的記憶體大小為何能?( `1 bit or 1 byte` ??)
- 在 `ANSI C` 裡，沒有一個型別為boolean的
- 從 `C99` 開始，有一個標頭檔可以使用，`stdbool.h`

## Switch
- 你可以使用 `if-else` 來做每個條件判斷，但是有時候可能會寫一個巨大的巢狀程式，因此將介紹另一個方法 `switch`
```c
switch (){
    case 1:
        ...
        break;
    ...
    default:
        ...
}
```
- `break`：從此處結束
    - 那麼如果不使用 `break` 呢??
- `default`：如果沒有 `case` 符合，執行這段
## 浮點數比較
```c
#include <stdio.h>
int main() {
    float a = 0.3;
    if (a == 0.3) printf("if01\n");
    else printf("else01\n");
    if (a == 0.3f) printf("if02\n");
    else printf("else02\n");
    return 0;
}
```
- 結果： `else01 if02`
- 請使用 `sizeof()` 查看發生了什麼!(`IEEE 754`)
- 結論：**浮點數的比較是相當危險的!!**