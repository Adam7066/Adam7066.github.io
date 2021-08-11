---
slug: "arch_linux-03"
title: "Arch Linux-03：KDE"
date: 2021-08-11T21:55:25+08:00
tags:
    - Arch Linux
---
# 前言
&emsp;&emsp;在安裝完了[系統](../arch_linux-01)，也設定了[基本配置](../arch_linux-02)後，想當然爾，該來安裝一個桌面環境了吧，因此接下來將告訴你怎麼安裝 [KDE](https://wiki.archlinux.org/title/KDE)。
# 教學開始
## 基本安裝
1. 安裝 KDE
    - [Arch Linux 的 meta package 與 package group](https://kaiiiz.github.io/blog/arch-meta-package/)
    - 安裝 Plasma：`pacman -S plasma-meta`
    - 安裝 KDE applications：`pacman -S kde-applications`
2. 安裝顯示管理器
    - 這邊我們將使用 [SDDM](https://wiki.archlinux.org/title/SDDM)：`pacman -S sddm`
    - 設定自動啟動：`systemctl enable sddm.service`
3. 安裝終端模擬器
    - 這邊我們使用 konsole：`pacman -S konsole`
    - 喜歡 kitty 的也可安裝：`pacman -S kitty`
4. 安裝文件管理器
    - 這邊我們使用 Dolphin：`pacman -S dolphin`
5. 一些基本軟體
    - 中文字體：`pacman -S adobe-source-han-sans-tw-fonts`
6. 重新啟動：`reboot`
## KDE 美化
- 切換系統語言
    - 系統設定 -> 區域設定 -> 
        - 語言 -> 增加語言... -> 繁體中文 -> 設為預設 -> 套用
        - Formats -> 區域 -> 選擇 台灣 - 繁體中文 (zh_TW)
- 調整顯示設定
    - 系統設定 -> 顯示與螢幕 -> 
        - 顯示設定 -> 全域縮放比例 -> 125%
        - 組合器 ->
            - 縮放方式：平滑
            - 成像後端介面：OpenGL 3.1
            - Latency：Prefer smoother animations
- 更換系統主題
    - 系統設定 -> 外觀 ->
        - Global Theme -> 取得新全域主題 -> `Layan look and feel theme`
        - Icons -> 取得新圖示 -> `Tela dark`
- 更換桌布
    - 在桌面右鍵 -> Configure Desktop and Wallpaper... ->
    - 桌布 -> 取得新桌布... -> 桌布型態：影像 -> `HD NO LOGO PL1591`
- Konsole 美化
    - 設定 -> 
        - 顯示工具列 -> 取消 主工具列、Session Toolbar 的勾選
        - 編輯目前設定檔 -> 外觀 -> 配色與字形 -> 取得更多 -> `Sweet`