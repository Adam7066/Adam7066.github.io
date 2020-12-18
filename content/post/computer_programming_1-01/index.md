---
slug: "computer_programming_1-01"
title: "程式設計(一)-01：Your first program"
date: 2020-10-04T11:00:48+08:00
tags:
    - C
categories:
    - 程式設計(一)
---
# Hello World
```c
#include <stdio.h>
//Your first code.
int main(){
    printf("Hello World\n");
    return 0;
}
```
- main是每個C程式的進入點，我們稱它為main function(主函式)
- `int` 及 `return` 是C裡面的Keywords
    - int代表這個函式將會回傳一個整數
    - 每個函式都應該有一個回傳值
- 每個敘述的結尾都應該要有 `;`
- `printf` 是一個會顯示格式化字串的函式
- `\n` -> 換行
- `\t` -> tab
- `\\` -> \
- `\"` -> “
- `#` 的那一行是C的預處理器並且不需要;結尾
- `stdio.h` -> standard input / output header(標準輸出/輸入標頭檔)
- 註解 -> 是給開發者看的
    - `//Your code` -> 單行
    - `/*Your code*/` -> 多行
---
- 使用編譯器將程式碼編譯成組合語言，再由組譯器組議成機械碼或可執行的二進制檔
- IDE -> Integrated Development Environment，不是編譯器
- gcc是最受歡迎的C編譯器之一(不完全對!!因為它不只做了編譯的動作…)
- 一些基本的Linux的操作指令
    - `man` -> 不會就問那個男人吧，男人不會就Google
    - `ls`
    - `cd`
    - `rm`
    - `pwd`
- `gcc main.c` 一些參數
    - `-o`
    - `-v`
    - `-g`
    - `-Wall -Wextra`
    - `-O2` or `-Og`

# Makefile
```makefile
all:
    gcc main.c -o main
clean:
    rm -rf main
```
- `make` 將執行all下的指令
- 中間縮排應為Tab而不是Space
- `make clean`
- 預設可執行的檔名為 `makefile，Makefile，GNUmakefile`，若為其他可下 `-f` 的參數