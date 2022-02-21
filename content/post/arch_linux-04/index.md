---
slug: "arch_linux-04"
title: "Arch Linux-04：基本軟體安裝"
date: 2021-08-11T22:18:09+08:00
tags:
    - Arch Linux
---
# 前言
&emsp;&emsp;有了[桌面環境](../arch_linux-03)，軟體也不可以缺少吧，因此這裡會紀錄一下用到的軟體的安裝方式

# 教學開始
## Yay
- Yay 的 [Github](https://github.com/Jguer/yay)
```bash
pacman -S --needed base-devel git go
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```
## 中文輸入法
- [中文輸入法 Wiki](https://wiki.archlinux.org/title/Localization_(%E6%AD%A3%E9%AB%94%E4%B8%AD%E6%96%87)/Traditional_Chinese_(%E6%AD%A3%E9%AB%94%E4%B8%AD%E6%96%87)#%E4%B8%AD%E6%96%87%E8%BC%B8%E5%85%A5%E6%B3%95)
- 這邊我選擇了之前在 Ubuntu 用習慣的 [ibus](https://wiki.archlinux.org/title/IBus) ([IBus (简体中文) Wiki](https://wiki.archlinux.org/title/IBus_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)))
    - 安裝注音輸入法：`pacman -S ibus ibus-chewing`
    - `vim /etc/environment`
        ```
        GTK_IM_MODULE=ibus
        QT_IM_MODULE=ibus
        XMODIFIERS=@im=ibus
        ```
    - `vim ~/.xprofile`
        ```
        export CTK_IM_MODULE=ibus
        export XMODIFIERS=@im=ibus
        export QT_IM_MODULE=ibus
        ibus-daemon -drxR
        ```
    - 重新啟動
## Oh My Zsh
- Oh My Zsh 的 [官網](https://ohmyz.sh/)、[Github](https://github.com/ohmyzsh/ohmyzsh)
- 安裝 zsh：`pacman -S zsh`
- 看系統是否裝了 zsh：`cat /etc/shells`
- 切換 Shell 為 zsh：`chsh -s /bin/zsh`
- 安裝 Git 以及 Curl：`pacman -S git curl`
- 安裝 oh-my-zsh：`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
- 配置主題，我選擇的是 [powerlevel10k](https://github.com/romkatv/powerlevel10k)
    - 安裝 Nerd Fonts字體
        ```bash
        # 下載字體
        mkdir -p ~/.local/share/fonts
        cd ~/.local/share/fonts && curl -fLo "Droid Sans Mono for Powerline Nerd Font Complete.otf" https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf
        # 快取字體
        fc-cache -vf ~/.local/share/fonts/
        # 查看是否安裝成功
        fc-list | grep -i droid
        ```
    - 設定終端字體
        - 設定 -> 編輯目前的設定檔... -> 外觀 -> 字型 -> `DroidSansMono Nerd Font`
    - 下載 powerlevel10k：`git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k`
    - 修改主題：`vim ~/.zshrc`
        - 修改 ZSH_THEME 為：`ZSH_THEME="powerlevel10k/powerlevel10k"`
    - 更新配置：`source .zshrc`
- 安裝插件
    - 下載 zsh-autosuggestion：`git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions`
    - 下載 zsh-syntax-highlighting：`git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`
    - 修改 .zshrc：`vim ~/.zshrc`
        ```
        plugins=(
            git
            extract
            archlinux
            colored-man-pages
            zsh-syntax-highlighting
            zsh-autosuggestions
        )
        ```
    - 更新配置：`source .zshrc`
## Vim 配置
- 請查看 [Use Vim as IDE](https://hackmd.io/SWhC4ublSjCBE7EVb6MbEw)
## 藍芽設置
- 使用 [Sony WH-1000XM4](https://www.sony.com.tw/zh/electronics/headband-headphones/wh-1000xm4) 測試可行
    ```bash
    pacman -S bluez bluez-utils
    systemctl start bluetooth.service
    systemctl enable bluetooth.service
    pacman -S pulseaudio-bluetooth
    reboot
    ```
## Asus NumberPad
- asus-touchpad-numpad-driver
    - [GitHub](https://github.com/mohamed-badaoui/asus-touchpad-numpad-driver)
    - [AUR](https://aur.archlinux.org/packages/asus-touchpad-numpad-driver)
    ```bash
    yay -S asus-touchpad-numpad-driver
    ```
## 其他軟體
- [TLP](https://wiki.archlinux.org/title/TLP)
    ```bash
    pacman -S tlp
    systemctl enable --now tlp
    ```
- Google Chrome：`yay -S google-chrome`
- 1password：`yay -S 1password`
- Neofetch： `pacman -S neofetch`
- [Keyring](https://wiki.archlinux.org/title/GNOME/Keyring)：`pacman -S gnome-keyring`
    - Seahorse：`pacman -S seahorse`
- [Visual Studio Code](https://wiki.archlinux.org/title/Visual_Studio_Code)：`yay -S visual-studio-code-bin`
    - 增加在資料夾上右鍵開啟的選項：`vim /usr/share/kservices5/vscodehere.desktop`
        ```
        [Desktop Entry]
        Type=Service
        X-KDE-ServiceTypes=KonqPopupMenu/Plugin
        MimeType=inode/directory;
        X-KDE-Priority=TopLevel
        Actions=openVSCodeHere;
        X-KDE-AuthorizeAction=shell_access

        [Desktop Action openVSCodeHere]
        TryExec=code
        Exec=code %f
        Icon=visual-studio-code

        Name=Open VS Code Here
        Comment=Opens a VS Code Instance in the current folder
        ```
- Discord：`pacman -S discord`
- [LibreOffice](https://wiki.archlinux.org/title/LibreOffice)：`pacman -S libreoffice-still libreoffice-still-zh-tw`
- Hugo-Extended：`yay -S hugo-bin`
- UxPlay：用於 iPad 的螢幕共享
    - 安裝：
        ```bash
        yay -S uxplay-git
        systemctl start avahi-daemon.service
        systemctl enable avahi-daemon.service
        ```
    - 使用：`uxplay`
