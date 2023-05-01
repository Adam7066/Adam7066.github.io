---
slug: "caddy-01"
title: "Caddy-01：第一次架站就上手"
date: 2023-04-28T10:45:52+08:00
tags:
    - Caddy
---
# 簡介
- [Caddy Server](https://caddyserver.com) 是一個開源的，使用 Golang 編寫，支援 HTTP/2 的 Web 伺服器端。
- Caddy 一個顯著的特性是預設啟用 HTTPS。
- 是第一個無需額外組態即可提供 HTTPS 特性的 Web 伺服器。

# 前言
- 一開始我是想說直接裝在 server 上，但由於我 DNS、SSL 簽證是透過 [Cloudflare](https://www.cloudflare.com/zh-tw/) 來處理的，這就得需要使用到 [xcaddy](https://github.com/caddyserver/xcaddy)。
- 但我一直處理不好，所以最後還是跑去使用 Docker 的方式了。

# 教學開始
## 前置作業
### 安裝 Docker
- 請在 Server 上安裝 [Docker](https://www.docker.com)，這邊就不多做介紹了。
### 安裝 Docker Compose
- 請在 Server 上安裝 [Docker Compose](https://docs.docker.com/compose/install/)，這邊就不多做介紹了。
### 設定 DNS
- 到 Cloudflare 將你的網域設定到你的 Server IP。
### 取得 Token
- 到 Cloudflare -> 右上角用戶 icon -> 我的設定檔 -> API 權杖 -> 建立 Token

## 目錄結構
```
caddy/
    |-- Caddyfile
    |-- Dockerfile
    |-- docker-compose.yaml
    |-- site/
        |-- web/
            |-- index.html
```
## 檔案內容
### Caddyfile
```caddyfile
smallten.me {
    tls {
        dns cloudflare your_token
    }
    root * ./web
    file_server
    encode gzip
    try_files {path}  /index.html # Required by Vue Router HTML5 mode
}
```
- 更多內容可以參考 [caddyfile](https://caddyserver.com/docs/caddyfile)
### Dockerfile
```dockerfile
FROM caddy:builder AS builder
RUN xcaddy build --with github.com/caddy-dns/cloudflare

FROM caddy:latest
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
```
### docker-compose.yaml
```yaml
version: "3"

services:
  caddy:
    build: .
    restart: unless-stopped
    ports:
      - 80:80
      - 443:443
      - 443:443/udp
    volumes:
      - $PWD/Caddyfile:/etc/caddy/Caddyfile
      - $PWD/site:/srv
      - caddy_data:/data
      - caddy_config:/config

volumes:
  caddy_data:
    external: true
  caddy_config:
```
