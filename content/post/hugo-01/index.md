---
slug: "hugo-01"
title: "Hugo-01：建立一個Hugo Blog"
date: 2021-02-07T03:22:22+08:00
tags:
    - Hugo
---
# 簡介
- 這篇內容將快速帶你建立一個 Hugo Blog 並將其部屬到 GitHub 上

# 相關連結
- [Hugo](https://gohugo.io/) - Hugo 官網
- [Hugo Themes](https://themes.gohugo.io/) - 選擇自己喜歡的主題
- [Hugo Releases](https://github.com/gohugoio/hugo/releases) - 下載 Hugo 並安裝進電腦

# 教學開始
## 建立 Hugo Blog
1. 安裝 Hugo
    - 這裡以 Ubuntu 為示範
    - 先至 [Hugo Releases](https://github.com/gohugoio/hugo/releases) 下載自己所需的版本
        ```bash
        $ dpkg -i hugo_extended_0.79.0_Linux-64bit.deb # 記得依照檔案自行更改
        ```
2. 創建一個 Hugo Site
    - 這裡創建一個名為 blog
        ```bash
        $ cd ~
        $ hugo new site blog # 可自行修改名稱
        ```
3. 新增主題
    - 這裡我選擇了 [Stack](https://themes.gohugo.io/hugo-theme-stack/) 這個主題
    ```bash
    $ cd ~/blog/
    $ git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
    ```
4. 跟著主題的教學文檔修改 config file
5. 如要建立新文章時
    ```bash
    $ hugo new post/test.md
    ```
    - 此時會在 `content/post/` 下，建立新文章，檔名為 `test.md`
    - `draft`: 草稿
    - `slug`: 此文章的 url (可自行建立)
6. 本機測試
    ```bash
    $ hugo server -D
    ```
    - `-D`: 將會連草稿都顯示出來

## 部署到 GitHub
1. 在 GitHub 上建立一個新的 Repository
    - 名稱為 `"Your account".github.io` ("Your account" 使用自己 GitHub 的名稱)
2. 將剛才建立好的 Blog 上到這個 Repo
    ```bash
    $ cd ~/blog/
    $ git add --all
    $ git commit -m "blog init"
    $ git branch -M main
    $ git remote add origin git@github.com... #這裡使用了 ssh 的方式
    $ git push -u origin main
    ```
3. 使用 GitHub Actions
    - 建立一個一個 workflows 名叫 `hugo_publish.yml`，內容如下 (有些地方須自行依照情況修改)
    ```yml
    name: github pages

    on:
    push:
        branches:    
        - main
        paths: ["content/**", ".github/workflows/hugo_publish.yml", "config.yaml", "layouts/**", "static/**"]

    jobs:
    deploy:
        runs-on: ubuntu-20.04
        steps:
        - uses: actions/checkout@v2
            with:
            submodules: true  # Fetch Hugo themes (true OR recursive)
            fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

        - name: Setup Hugo
            uses: peaceiris/actions-hugo@v2
            with:
            hugo-version: '0.79.0'
            extended: true

        - name: Build
            run: hugo --minify

        - name: Deploy
            uses: peaceiris/actions-gh-pages@v3
            with:
            github_token: ${{ secrets.GITHUB_TOKEN }}
            publish_dir: ./public
            cname: 'blog.smallten.tk'
    ```
4. 修改 Repo 的設定
    - 先找到 GitHub Pages 的地方
    - 將 Source 改成 `Branch: gh-pages`
    - 有 custom domain 的記得填入
    - Enforce HTTPS 打勾
5. 這樣就完成了，之後只要 push 時，便會自動更新網站了