---
slug: "arch_linux-02"
title: "Arch Linux-02：基本設置"
date: 2021-08-11T17:26:15+08:00
tags:
    - Arch Linux
---
# 前言
&emsp;&emsp;在 [Arch Linux-01：系統安裝](../arch_linux-01) 中，已經介紹了如何完整的安裝一個基礎的 Arch Linux 系統，而這部分將去介紹一些基本的設定或是驅動的安裝。

# 教學開始
## 新增使用者
- 由於不應該一直使用 root 帳號，因此我們來新增一個使用者吧。
1. 新增使用者：`useradd -m <user>` ( `<user>` 為你要新增的用戶名稱，請自行更改 )
2. 設置密碼：`passwd <user>`
## Sudo
- [Sudo Wiki](https://wiki.archlinux.org/title/sudo)
- 安裝：`pacman -S sudo`
- 將使用者加入：`usermod -aG wheel <user>`
- 使用 visudo
    - 由於預設使用 vi，但我們之前是用安裝 vim，因此臨時的更改它吧：`EDITOR=vim visudo`
    - 進去後，找到 `%wheel ALL=(ALL) ALL` 這行，然後消掉註解並且儲存離開。
## Xorg
- [Xorg Wiki](https://wiki.archlinux.org/title/xorg)
- 安裝 ：`pacman -S xorg-server`
- [驅動安裝](https://wiki.archlinux.org/title/xorg#Driver_installation)
    - 查看顯卡類型：`lspci -v | grep -A1 -e VGA -e 3D`
    - 安裝對應驅動，請依照情況自行更改
    - 以下為我的筆電的安裝方式 ([參考至此文章](https://blog.sakuya.love/archives/linuxgpu/))
    - 開啟 multilib 倉庫：`vim /etc/pacman.conf`
        - 找到下方兩行，並消掉註解
            ```
            [multilib]
            Include = /etc/pacman.d/mirrorlist
            ```
    - 安裝 mesa 包：`pacman -S mesa lib32-mesa`
    - 若之前已安裝 xf86-video-intel，請卸載掉：`pacman -Rsc xf86-video-intel`
    - nvidia 驅動：`pacman -S nvidia lib32-nvidia-utils`
    - 安装 nvidia-prime：`pacman -S nvidia-prime` ([Prime Wiki](https://wiki.archlinux.org/title/PRIME#PRIME_render_offload))
    - 重新啟動：`reboot`