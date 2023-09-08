---
slug: "factorio-01"
title: "Factorio-01：使用 LinuxGSM 開伺服器"
date: 2023-09-08T13:57:15+08:00
tags:
    - Factorio
---

# 前情提要
最早玩 Factorio 開 server 是直接使用遊戲內建的方式，然後好友們直接透過 steam 連線進來，但是這樣有個缺點，那就是當自己不玩沒開遊戲時，朋友們也玩不了，因此之後玩時都會開個獨立的 server。

之前有使用過以下方法來開 server 也玩了不少次了：
- 直接在[官網](https://factorio.com/)下載 server file：[Download page](https://factorio.com/download)
- 或是用 Docker 開 server

但最近發現了一個酷東西叫做 [LinuxGSM](https://linuxgsm.com/) 看起來蠻方便的，而且支援不少種遊戲，甚至連有的遊戲需要 SteamCMD 也都幫您處理好了，因此想說來嘗試一下。

# 前置作業
- 開一臺要跑 server 的機器，[規格要求](https://linuxgsm.com/servers/fctrserver/)
- 這邊我開了一臺 ubuntu-23.10 x86-64 的 VM

# 教學開始
## 安裝相關依賴
```bash
sudo dpkg --add-architecture i386; sudo apt update; sudo apt install curl wget file tar bzip2 gzip unzip bsdmainutils python3 util-linux ca-certificates binutils bc jq tmux netcat lib32gcc-s1 lib32stdc++6 xz-utils
```
- 其他環境可參考：[這頁的 Dependencies](https://linuxgsm.com/servers/fctrserver/)

## 安裝 LinuxGSM
1. 新增一個使用者：`sudo adduser fctrserver`
   - 注意!! 不要用文檔上給的範例密碼!!
2. 登入剛剛創出的使用者 `su - fctrserver`
   - 之後可以直接使用這個使用者登入，不用進去在切換
3. 下載 linuxgsm.sh
    ```bash
    wget -O linuxgsm.sh https://linuxgsm.sh && chmod +x linuxgsm.sh && bash linuxgsm.sh fctrserver
    ```
4. 安裝：`./fctrserver install`

## 如何使用
- 運行 server：`./fctrserver start`
- 停止 server：`./fctrserver stop`
- 重啟 server：`./fctrserver restart`
- 輸入指令：`./fctrserver console`
  - 指令可參考 [Factorio Wiki](https://wiki.factorio.com/Console)
- 更新 server：`./fctrserver update`
- 查看 server 狀態：`./fctrserver details`
- 備份 server：`./fctrserver backup`

## 設定 Server
1. mods：把 zip 檔案放到 `~/serverfiles/mods` 裡面
   - `mod-list.json`：記錄了 mod 名稱、是否啟用
2. 地圖：我偏好使用遊戲本身的圖形化方式去建立，完成後再將檔案傳上去 server
   - 地圖檔案放到 `~/serverfiles/saves` 裡面
3. server 設定：修改 `~/serverfiles/data/fctrserver.json` 檔
   - 設定都有註解，就不多解釋
   - 其中一項 `token` 有需要的話，可以從 [官網登入，個人資料頁](https://factorio.com/) 取得
4. RCON 密碼：修改 `~/lgsm/config-lgsm/fctrserver/fctrserver.cfg` 檔案
   - 新增一行 `rconpassword="your-password"`

## 管理員權限
1. 請先確認上方都已設定完，並且 server 也已經啟動
2. 使用 `./fctrserver console` 進入 server console
3. 指令：`promote <player>`
4. 可以發現已經變成 admin 了
5. 補充：
   - server 裡管理員名單位置：`~/serverfiles/server-adminlist.json`
