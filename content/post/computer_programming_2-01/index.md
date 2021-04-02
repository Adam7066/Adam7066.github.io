---
slug: "computer_programming_2-01"
title: "程式設計(二)-01：String"
date: 2021-04-02T14:40:30+08:00
tags:
    - C
categories:
    - 程式設計(二)
---
# 字元
- 在講字串之前我們先來看什麼是字元。
### ASCII
- American Standard Code for Information Interchange.
- 電子通訊的字元編碼標準
- 基於英文字母，ASCII 將 128 個字元編碼成 7 個位元長。
    - 95 個可印字元：A-Z, a-z, 0-9, 標點符號
    - 不可印字元：換行符號
---
- 在電腦中我們使用 8-bit 的記憶體儲存字元。
- 在 C 語言中，使用 `char` 這個型別。
- `%c` -> 輸出字元
- `%x` or `%X` -> 印出 hex or HEX 的值。
---
- 在以前，許多情況下，有些人使用 `unsigned char` 作為 one byte 的資料型別，但現今你應該使用的是 `uint8_t`。
- 請把 `char` 留給字串，盡管事實上對電腦來說都是一樣的。
- 其他編碼：[Big5](https://zh.wikipedia.org/wiki/%E5%A4%A7%E4%BA%94%E7%A2%BC)、[UTF-8](https://zh.wikipedia.org/wiki/UTF-8)

# 字串
- 事實上，字串就是一連串的可印字元。
- 這樣看起來很像陣列對吧? Yes!
---
![](computer_programming_2-01-01.png)
- 在 C 語言中，字串是一個字元指標，並以 `'\0'` 為結尾。
- 常見的錯誤：
    - 沒有分配足夠的空間
    - 輸出一個不包含 `'\0'` 的字串
    - 在 C 的標準中，字元指標是不可修改的，如果要修改字串，必須儲存在字元陣列中。

## 字串處理函式
- 首先，這部分有印象就好，不用記，畢竟到這邊大家應該都有能力自己實作出來吧!
### 字串轉整數
- 很久很久以前
    ```c
    #include <stdlib.h>
    int atoi(const char *nptr);
    long atol(const char *nptr);
    ```
- 現今
    ```c
    #include <stdlib.h>
    // string to double
    double strtod(const char *nptr, char **endptr);
    // string to long int
    long int strtol(const char *nptr, char **endptr, int base);
    ```
### 輸出入函式
- 輸出
```c
#include <stdio.h>
int putchar(int c); // 輸出字元
int puts(const char *s); // 輸出字串
int snprintf(char *restrict buffer, size_t bufsz, const char *restrict format, ... );
```
- 輸入
```c
#include <stdio.h>
int getchar(void); // 輸入字元
char *fgets(char *s, int size, FILE *stream); // 輸入字串
int sscanf(const char *restrict buffer, const char *restrict format, ... );
```
---
- 一些不推薦的函式，永遠不要用他們
    - `char *gets(char *s);`
    - `int sprintf(char *str, const char *format, ...);`
- 有 buffer overflow 的風險
### 字串操作函式
```c
char *strncpy(char *dest, const char *src, size_t n); // 複製字串
char *strncat(char *dest, const char *src, size_t n); // 串接字串
```
---
- 和上述理由一樣，不要使用下方的函式
    - `char *strcpy(char *dest,const char *src);`
    - `char *strcat(char *dest,const char *src);`
### 字串比較函式
```c
#include <string.h>
int strcmp(const char *s1, const char *s2);
int strncmp(const char *s1, const char *s2, size_t n);
```
- 如果s1（或其前n個字節）分別小於、匹配或大於s2，則函數將返回小於、等於或大於零的整數。
### 字串搜尋函式
```c
#include <string.h>
char *strchr(const char *s, int c); // 從頭找字元
char *strrchr(const char *s, int c); // 從尾找字元
// 計算 s 的前綴有多少字元在 accept 中
size_t strspn(const char *s, const char *accept ); 
// 計算 s 的前綴有多少字元不在 reject 中
size_t strcspn(const char *s, const char *reject );
// 回傳在 accept 中的任何字元在 s 字串首次出現位置的指標
char *strpbrk(const char *s, const char *accept );
// 回傳 needle 在 haystack 中首次出現位置的指標
char *strstr(const char *haystack, const char *needle );
// 依照 delim 中的字元分割 str
char *strtok(char *str, const char *delim );
```
#### strtok 範例
```c
char str[] = "the value of pi is 3.14";
char *token = strtok(str, " ");
while(token != NULL) {
    printf("%d: %s\n", i, token);
    token = strtok(NULL, " ");
}
```
- 為何第二次之後呼叫都是以 `NULL` 作為輸入?
    - 因為這是一個 `static` 的變數。如果輸入 `NULL`，該函式將繼續從最後一個位置切割字串。
- 為甚麼我不能直接使用 `const char *str`?
    - 因為 `strtok` 的實作，將會直接操作在輸入的字串上。
### 其他字串函式
```c
#include <string.h>
size_t strlen(const char *s); // 計算字串長度
// 輸出錯誤訊息
char *strerror(int errnum);
void perror(const char *str);
```
- `strerror` 搭配 `errno` 使用 (`#include <errno.h>`)