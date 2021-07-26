---
slug: "psv-03"
title: "PlayStation Vita-03：NoPayStation Browser"
date: 2021-07-26T11:50:17+08:00
tags:
    - Crack
categories:
    - PlayStation Vita
---
1. 下載 [NPS Clients](https://nopaystation.com/)，並放到一個資料夾中。
2. 下載 [pkg2Zip](https://github.com/mmozeiko/pkg2zip/releases)，並將 `pkg2zip.exe` 放到同一個資料夾中。
3. 執行 `NPS_Browser.exe`。
4. 複製以下連結：
    - Games
        - PSV tsv `http://nopaystation.com/tsv/PSV_GAMES.tsv`
        - PSM tsv `http://nopaystation.com/tsv/PSM_GAMES.tsv`
        - PSX tsv `http://nopaystation.com/tsv/PSX_GAMES.tsv`
        - PSP tsv `http://nopaystation.com/tsv/PSP_GAMES.tsv`
        - PS3 tsv `http://nopaystation.com/tsv/PS3_GAMES.tsv`
    - DLCs:
        - PSV tsv `http://nopaystation.com/tsv/PSV_DLCS.tsv`
        - PSP tsv `http://nopaystation.com/tsv/PSP_DLCS.tsv`
        - PS3 tsv `http://nopaystation.com/tsv/PS3_DLCS.tsv`
    - Themes:
        - PSV tsv `http://nopaystation.com/tsv/PSV_THEMES.tsv`
5. 設定 Download and unpack dir: 檔案下載後的位置。
6. 設定 Any pkg dec tool: 路徑為 `pkg2Zip.exe` 的位置。
7. 設定 Your pkg dec params: `-x {pkgFile} "{zRifKey}"`。
8. 設定 HMAC key for updates: `E5E278AA1EE34082A088279C83F9BBC806821C52F2AB5D2B4ABD995450355114`。
9. 設定 CompPack URL: `https://gitlab.com/nopaystation_repos/nps_compati_packs/raw/master/entries.txt`
10. 設定 CompPack Patch URL: `https://gitlab.com/nopaystation_repos/nps_compati_packs/raw/master/entries_patch.txt`
11. 重新啟動 NPS Browser。
12. 下載你要的檔案，並將其放進 PSV 對應路徑中。
13. 進入 Vita Shell，按下三角形按鈕，選擇 Refresh LiveArea