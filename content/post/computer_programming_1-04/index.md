---
slug: "computer_programming_1-04"
title: "程式設計(一)-04：Loop"
date: 2020-10-18T20:33:18+08:00
tags:
    - C
categories:
    - 程式設計(一)
---
# Loop
    - `while`
    - `for`
    - `do while`
## While Loop
```c
while(條件){
    執行區塊
}
```
- `%.200f` 會發生什麼事? -> 精度不夠沒有意義
- `while(1)` -> 無窮迴圈
## For Loop
```c
for(初始化; 條件; 執行後操作){
    執行區塊
}
```
- `i++` -> Use the current value of i. -> `i = i + 1`
- `++i` -> `i = i + 1` -> Use the new value of i.
- `{}` -> 變數生命週期範圍
- `%4d` ( `%#` ) -> 給最小的位數去顯示
- 在 ANSI C, 變數只能被宣告在函式的開頭，而 Modern C 沒有任何限制
## Do While Loop
```c
do{
    執行區塊
}while(條件)
```
- 三種不同類型的迴圈毫無疑問的都可以互相轉換
    - 除了 `do while` 至少會執行一次
- 大多數來說，如果你知道要執行幾次迴圈的話，會使用 `for`
- `break`：離開當前的區段
- `continue`：跳過剩餘的敘述，直接執行下一次迭代
- 無窮迴圈不是個好東西? -> 不，或許你會需要他，例如：Web server
---
```c
#include <unistd.h>
sleep(sec);
```