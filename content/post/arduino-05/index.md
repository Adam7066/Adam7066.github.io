---
slug: "arduino-05"
title: "Arduino-05：HC-05 AT命令"
date: 2020-05-10T21:56:45+08:00
tags:
    - Arduino
---
# 簡介
- 這篇內容將教大家透過Arduino的序列埠設定HC-05的AT命令

# 硬體
- Arduino Uno * 1
- HC-05藍芽模組 * 1

# 教學
## 腳位連接
|Arduino|HC-05|
|----|----|
|5V|VCC|
|GND|GND|
|8|TX|
|9|RX|

## 程式碼
```cpp
#include <SoftwareSerial.h>

SoftwareSerial BT(8, 9);
char val;

void setup() {
    Serial.begin(9600);
    BT.begin(38400);
}

void loop() {
    if(Serial.available()){
        val = Serial.read();
        BT.print(val);
    }
    if(BT.available()){
        val = BT.read();
        Serial.print(val);
    }
}
```
1. 連接腳位，並上傳程式碼，最後給HC-05供電前，先按住上面的按鈕，再提供電源，燈號將變成約兩秒一閃，及表示進入了AT命令模式
2. 接下來打開序列埠監控視窗，將設定調成"9600 baud"和"NL與CR"，最後依需求輸入以下命令即可
- AT -> 顯示OK表示連接成功
- 查看韌體版本 -> AT+VERSION
- 查看名稱 -> AT+NAME?
- 修改名稱 -> AT+NAME=你要的名字
- 查看密碼 -> AT+PSWD?
- 修改密碼 -> AT+PSWD=你要的密碼