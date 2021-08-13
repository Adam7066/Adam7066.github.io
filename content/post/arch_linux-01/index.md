---
slug: "arch_linux-01"
title: "Arch Linux-01：系統安裝"
date: 2021-08-11T13:50:32+08:00
tags:
    - Arch Linux
---
# 前言
&emsp;&emsp;在被[吳文元](https://github.com/jw910731/)朋朋洗腦、推坑、直銷 [Arch Linux](https://archlinux.org/) 的各種好之後，我終於也入坑了，而這一系列將紀錄整個的安裝過程，避免未來再踩一次坑。

# 安裝設備
- 筆電：[Zenbook 14 UX433FN](https://www.asus.com/tw/Commercial-Laptops/ASUS-ZenBook-14-UX433FN/)
- 顯卡：
    - GPU0 Intel UHD 620
    - GPU1 Nvidia MX150

# 開始安裝
- 完整官方導覽：[Installation guide](https://wiki.archlinux.org/title/Installation_guide)

## 前置作業
1. 製作開機碟
    - 到 [Arch Linux Download](https://archlinux.org/download/) 下載映像檔 (.iso)
        - [NCHC](https://free.nchc.org.tw/arch/iso/)、[NCTU](http://archlinux.cs.nctu.edu.tw/iso/)
    - 使用 [Rufus](https://rufus.ie/zh_TW/) 寫入隨身碟
2. 壓縮磁碟區
    > 由於我要在一塊已有 Windows 10 的硬碟上安裝 Arch linux，因此得來切磁區。
    - 在 Windows 10 中開啟硬碟管理員
    - 以壓縮磁碟區的方式創造出未分配的空間。

## 安裝系統
1. 插上隨身碟，並以隨身碟開機
2. 檢查硬碟、分配磁區
    > 由於新手上路，這邊我們就分配單一磁區給 `/` 就好。
    > 至於其他像是 `[SWAP]` 之類的，有需要就自行處理。
    - `fdisk -l`: 查看硬碟狀態
    - `cfdisk /dev/nvme0n1` 選擇硬碟(依照情況自行更改名稱)
        - 進去後選取 Free space -> New -> 輸入 Size (例如: 200G) -> Write -> 輸入 yes -> Quit
    - 格式化為 ext4 格式：`mkfs.ext4 /dev/nvme0n1p5` 選擇磁區(依照情況自行更改名稱)
3. 掛載硬碟
    - 掛載 root 分區：`mount /dev/nvme0n1p5 /mnt`
    - 先創建 boot 資料夾：`mkdir /mnt/boot`
    - 掛載 efi 分區：`mount /dev/nvme0n1p1 /mnt/boot` (依照情況自行更改名稱)
        - 這邊我們選擇跟 Windows 10 共用 efi 分區
4. 連接網路，以下為 Wifi 連接方式
    - 輸入 `iwctl`
    - 查詢設備：`device list` (以下依照情況自行更改名稱，這邊以 `wlan0` 為例)
    - 掃描 wifi：`station wlan0 scan` 
    - 列出 wifi：`station wlan0 get-networks`
    - 連接 wifi：`station wlan0 connect Wifi-Name`
    - 有密碼的話輸入密碼
    - 輸入 `exit` 離開
    - 輸入 `ping www.google.com` 檢查是否連接成功
5. 更新系統時間
    - `timedatectl set-ntp true`
    - 可以執行 `timedatectl status` 檢查時間同步服務的狀態。
6. 安裝必要的軟體包
    - 必要：`pacstrap /mnt base linux linux-firmware`
    - 依照需求安裝：`pacstrap /mnt vim iwd man-db man-pages texinfo`
7. 配置系統
    - 生成 fstab 文件
        - `genfstab -U /mnt >> /mnt/etc/fstab`
        - 檢查生成的 fstab 文件：`cat /mnt/etc/fstab`
    - 切換到裝好的系統：`arch-chroot /mnt`
    - 設定時區：`ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime`
    - 同步硬體時鐘：`hwclock --systohc`
    - 在地化
        - `vim /etc/locale.gen`
            - 取消註解 `en_US.UTF-8 UTF-8`、`zh_TW.UTF-8 UTF-8`
        - 生成 locale 訊息：`locale-gen`
        - `vim /etc/locale.conf`
            - 添加設定：`LANG=en_US.UTF-8`
    - 配置網路
        - 創建 hostname 文件：`vim /etc/hostname`
            - 輸入你要的名稱，例如：`myhostname`
        - 修改 hosts：`vim /etc/hosts`，內容如下
            ```
            127.0.0.1	localhost
            ::1		localhost
            127.0.1.1	myhostname.localdomain	myhostname
            ```
    - 設定 DHCP
        - `vim /etc/systemd/network/20-wired.network`，內容如下
            - `enp1s0` 為網卡名稱，請用 `ip a` 自行查看更改
            ```
            [Match]
            Name=enp1s0

            [Network]
            DHCP=yes
            ```
        - 啟用：`systemctl enable --now systemd-networkd.service`
    - 啟用 DNS：`systemctl enable --now systemd-resolved`

    - 修改 root 密碼：`passwd`
8. 開機引導，這邊我們使用 [systemd-boot](https://wiki.archlinux.org/title/systemd-boot)
    - 將 systemd-boot 安裝到 efi 分區：`bootctl --path=/boot install`
    - 手動更新 systemd-boot：`bootctl --path=/boot update`
    - 啟動選單配置
        - `vim /boot/loader/loader.conf`，內容如下
            ```
            default arch-* # arch 那裡應為一字串不用更改
            timeout 10
            console-mode max
            editor no
            ```
        - `vim /boot/loader/entries/arch.conf`，內容如下
            - UUID 請自行填入，可於 `/etc/fstab` 中查看，請選擇 `/` 目錄的。
            ```
            title Arch Linux
            linux /vmlinuz-linux
            initrd /initramfs-linux.img
            options root="UUID=XXX-XXX-XXX" rw
            ```
        - `cp /boot/loader/entries/arch.conf /boot/loader/entries/arch-fallback.conf`
        - `vim /boot/loader/entries/arch-fallback.conf`
            - 將 `/initramfs-linux.img` 更改為 `/initramfs-linux-fallback.img` 即可
9. 重新啟動
    - 退出 chroot 環境：輸入 `exit` 或是按下 `Ctrl+d`
    - 卸載分區：`umount -R /mnt`
    - 重新啟動：`reboot`
    - 記得重開時要移除隨身碟呀

# 補充
- 這台電腦進 BIOS 方法：按住 `F2`，再按電源，直到進入 BIOS 再放開 `F2`
-  在 BIOS 新增開機選項
    1. Boot -> Add New Boot Option
    2. Add boot option：輸入選項名稱
    3. Path for boot option：`\EFI\systemd\systemd-bootx64.efi`
    4. Create -> Ok
- 開機選單消失問題：[Windows_changes_boot_order](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface#Windows_changes_boot_order)
- 字體太小問題
    - [HiDPI - Linux Console](https://wiki.archlinux.org/title/HiDPI#Linux_console)
    - [Linux_console - Fonts](https://wiki.archlinux.org/title/Linux_console#Fonts)