---
slug: "arch_linux-05"
title: "Arch Linux-05：Howdy 臉部辨識"
date: 2022-04-05T04:55:18+08:00
tags:
    - Arch Linux
---
# 前言
- Windows 提供了 Windows Hello 來進行臉部辨識的身份認證，但在使用 Arch Linux 時總是要自己手動輸入密碼覺得十分地麻煩，畢竟筆電都有紅外線鏡頭模組幹麻不用它，因此搜尋了一下，找到了 [Howdy](https://github.com/boltgolt/howdy) 這個東東。
# 相關連結
- [Howdy - Github](https://github.com/boltgolt/howdy)
- [Howdy - Arch_Wiki](https://wiki.archlinux.org/title/Howdy)
- [Howdy - AUR](https://aur.archlinux.org/packages/howdy)

# 教學開始
## 安裝
- AUR: `yay -S howdy`
## 設定
### PAM
- 為了讓 Howdy 對用戶進行身份驗證，必須在可能要使用 Howdy 的任何 PAM 設定檔的一開始添加下面這行
    - `auth sufficient pam_python.so /lib/security/howdy/pam.py`
    - 相關檔案可在 `/etc/pam.d/` 中找到
- 登入驗證：須修改這三個檔案，加入上述內容
    ```bash
    vim /etc/pam.d/sddm
    vim /etc/pam.d/kde
    vim /etc/pam.d/system-local-login
    ```
### Howdy
1. 查看鏡頭：`v4l2-ctl --list-devices`
    - 選擇對應的鏡頭路徑：例如我紅外線的鏡頭就為 `/dev/video2`
2. 修改資料夾權限：`chmod -R 755 /lib/security/howdy`
3. 設定 Howdy
    - `vim /lib/security/howdy/config.ini`
        - 設定鏡頭，路徑自行更換
        ```
        # The path of the device to capture frames from
        # Should be set automatically by an installer if your distro has one
        device_path = /dev/video2
        ```
        - 關閉 snapshots
        ```
        [snapshots]
        capture_failed = false
        capture_successful = false
        ```
4. 新增用戶臉部模型：`sudo howdy -U smallten add`
    - smallten 為當前使用者名稱，自行更改
5. 測試 Howdy：`sudo howdy -U smallten test`
    - smallten 為當前使用者名稱，自行更改