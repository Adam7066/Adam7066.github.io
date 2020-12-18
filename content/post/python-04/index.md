---
slug: "python-04"
title: "Python-04：多線程-ts下載並合併成mp4"
date: 2020-10-04T16:36:35+08:00
tags:
    - Python
categories:
    - 爬蟲
---
# 簡介
&emsp;&emsp;這篇將帶你下載m3u8檔並分析出ts的檔案，再透過多線程來加速下載，之後再由FFmpeg合併成mp4。

# Python
## 下載m3u8
```python
m3u8Url = 'https://.../index.m3u8'

def downloadM3u8(url):
    r = requests.get(url)
    with open('./index.m3u8', 'wb') as f:
        f.write(r.content)
```
## 分析m3u8
- 這部份請依照你所取得的m3u8檔進行分析，並將完整的ts檔的url放進tsList即可。
```python
tsList = []
tsCnt = 0

def analyzeM3u8():
    tsList.clear()
    tempUrl = m3u8Url.rsplit('/', 1)[0] + '/'
    with open('./index.m3u8', 'r') as f:
        text = f.read()
    textList = text.split('\n')
    while textList[-1] != '#EXT-X-ENDLIST':
        textList.pop(-1)
    for i in textList:
        if i[0] != '#':
            tsList.append(tempUrl + i)
    global tsCnt 
    tsCnt = len(tsList)
```
## 下載ts檔
```python
q = queue.Queue()

def downloadts():
    global tsList
    while q.qsize() > 0:
        num = q.get()
        r = requests.get(tsList[num])
        with open('./ts/' + str(num+1) + '.ts', 'wb') as f:
            f.write(r.content)

if __name__ == "__main__":
    if not os.path.isdir('./ts/'):
        os.mkdir('./ts/')
    for i in range(tsCnt):
        q.put(i)
    thList = []
    for i in range(tsCnt):
        th = threading.Thread(target=downloadts)
        th.start()
        thList.append(th)
    for th in thList:
        th.join()
    print('下載ts完成')
```
## 合併ts成mp4
```python
def merge(tsCnt, output):
    for i in range(tsCnt):
        with open('./ts/ts.txt', 'a+') as f:
            f.write('file ' + str(i+1) + '.ts\n')
    command = 'ffmpeg -y -f concat -i %s -bsf:a aac_adtstoasc -c copy %s' % ('./ts/ts.txt', output)
    os.system(command)
    print('合併成功')
```
## 移除無用檔案
```python
def remove():
    shutil.rmtree('./ts/')
    os.remove('./index.m3u8')
```
---
## 完整程式碼
```python
# -*- coding: UTF-8 -*-
import requests, os, threading, queue, shutil

m3u8Url = 'https://.../index.m3u8'
tsList = []
tsCnt = 0
q = queue.Queue()

def downloadM3u8(url):
    r = requests.get(url)
    with open('./index.m3u8', 'wb') as f:
        f.write(r.content)

def analyzeM3u8():
    tsList.clear()
    tempUrl = m3u8Url.rsplit('/', 1)[0] + '/'
    with open('./index.m3u8', 'r') as f:
        text = f.read()
    textList = text.split('\n')
    while textList[-1] != '#EXT-X-ENDLIST':
        textList.pop(-1)
    for i in textList:
        if i[0] != '#':
            tsList.append(tempUrl + i)
    global tsCnt 
    tsCnt = len(tsList)

def downloadts():
    global tsList
    while q.qsize() > 0:
        num = q.get()
        r = requests.get(tsList[num])
        with open('./ts/' + str(num+1) + '.ts', 'wb') as f:
            f.write(r.content)

def merge(tsCnt, output):
    for i in range(tsCnt):
        with open('./ts/ts.txt', 'a+') as f:
            f.write('file ' + str(i+1) + '.ts\n')
    command = 'ffmpeg -y -f concat -i %s -bsf:a aac_adtstoasc -c copy %s' % ('./ts/ts.txt', output)
    os.system(command)
    print('合併成功')

def remove():
    shutil.rmtree('./ts/')
    os.remove('./index.m3u8')

if __name__ == "__main__":
    downloadM3u8(m3u8Url)
    analyzeM3u8()
    print('m3u8下載且分析完畢')
    if not os.path.isdir('./ts/'):
        os.mkdir('./ts/')
    for i in range(tsCnt):
        q.put(i)
    thList = []
    for i in range(tsCnt):
        th = threading.Thread(target=downloadts)
        th.start()
        thList.append(th)
    for th in thList:
        th.join()
    print('下載ts完成')
    merge(tsCnt, './test.mp4')
    remove()
```