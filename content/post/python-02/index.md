---
slug: "python-02"
title: "Python-02：多線程-加速圖片下載"
date: 2020-08-03T00:56:43+08:00
tags:
    - Python
categories:
    - 爬蟲
---
# 簡介
- 接續上一篇Python-01：爬蟲-圖片下載的內容，這篇文章將帶領大家使用多線程來加速圖片的下載

# 教學開始
- 首先我們先看個簡單的多線程範例
```python
import time, threading

# 子執行緒的工作函數
def job():
  for i in range(5):
    print("Child thread:", i)
    time.sleep(1)
# 建立一個子執行緒
t = threading.Thread(target = job)
# 執行該子執行緒
t.start()
# 主執行緒繼續執行自己的工作
for i in range(3):
  print("Main thread:", i)
  time.sleep(1)
# 等待 t 這個子執行緒結束
t.join()
print("Done.")
```
```python
import time, threading

# 子執行緒的工作函數
def job(num):
  print("Thread", num)
  time.sleep(1)
# 建立 5 個子執行緒
threads = []
for i in range(5):
  threads.append(threading.Thread(target = job, args = (i,)))
  threads[i].start()
# 主執行緒繼續執行自己的工作

# 等待所有子執行緒結束
for i in range(5):
  threads[i].join()
print("Done.")
```
- 接下來我們修改[Python-01：爬蟲-圖片下載](../python-01)中下載圖片那部分的程式碼
```python
th_list = []
for i in range(img_count):
    img = img_url + str(i + 1).zfill(m) + '.jpg'
    th = threading.Thread(target=download_img, args=(img, i))
    th.start()
    th_list.append(th)
for th in th_list:
    th.join()
```

# 完整程式碼
```python
# -*- coding: UTF-8 -*-
from selenium import webdriver
from bs4 import BeautifulSoup
import requests, threading

def download_img(img, num):
    r = requests.get(img)
    with open(save_url + str(num+1) + '.jpg', 'wb') as f:
        f.write(r.content)
    
if __name__ == "__main__":  
    save_url = './test/'
    url = 'https://www.ohmanhua.com/13621/1/1.html'
    chop = webdriver.ChromeOptions()
    chop.add_extension('Adblock-Plus_v3.8.4.crx')
    browser = webdriver.Chrome(options = chop)
    browser.implicitly_wait(10)
    browser.get(url)
    soup = BeautifulSoup(browser.page_source, 'lxml')
    img_count = int(soup.find('select', {'class': 'mh_select'}).find_all('option')[-1].get('value'))
    img_url = soup.find_all('div', {'class': 'mh_comicpic'})[0].find('img').get('src')
    if img_url[0] == '/':
        img_url = 'https:' + img_url
    m = len(img_url.rsplit('/', 1)[1].split('.')[0])
    img_url = img_url.rsplit('/', 1)[0] + '/'
    th_list = []
    for i in range(img_count):
        img = img_url + str(i + 1).zfill(m) + '.jpg'
        th = threading.Thread(target=download_img, args=(img, i))
        th.start()
        th_list.append(th)
    for th in th_list:
        th.join()
    browser.quit()
```