---
slug: "python-03"
title: "Python-03：m3u8影片下載"
date: 2020-10-04T11:07:24+08:00
tags:
    - Python
categories:
    - 爬蟲
---
# 簡介
- 現今你時常能在影音媒體網站看到 `.m3u8` 的檔案，以及許多 `.ts` 的分段媒體，本篇將教你如何簡單的下載成 `.mp4` 檔。
- 這篇並不詳加敘述 HLS 之類的觀念，若有興趣深入了解請自行查找資料。

# FFmpeg
- [FFmpeg官網](https://ffmpeg.org/)
- 下載安裝完後，若為 windows 用戶請將 `%ffmpeg%\bin` 的路徑加入環境變數中，並於terminal中執行 `ffmpeg -version` 來查看是否成功加入。
- 下載檔案，直接在 terminal 輸入 `ffmpeg -i m3u8URL -c copy filname.mp4`，即可完成下載。(下面將提供Python的寫法)

# Python
```python
# -*- coding: UTF-8 -*-
import ffmpeg_streaming
from ffmpeg_streaming import Formats

url = 'https://.../index.m3u8'
filename = 'test.mp4'

def ffmpeg_download(input_path, output_path):
    video = ffmpeg_streaming.input(input_path)
    stream = video.stream2file(Formats.h264())
    stream.output(output_path)

if __name__ == "__main__":
    ffmpeg_download(url, './' + filename)
```
- 下一篇將教你如何直接從 m3u8 裡讀取目錄，並使用多線程下載 ts 並合併成 mp4