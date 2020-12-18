---
slug: "python-01"
title: "Python-01：爬蟲-圖片下載"
date: 2020-08-03T00:39:24+08:00
tags:
    - Python
categories:
    - 爬蟲
---
# 簡介
- 這一篇將帶領大家透過Python的爬蟲自動化的下載圖片

# 安裝
- 打開終端機，安裝下列套件
    - `pip install requests`
    - `pip install BeautifulSoup4`
    - `pip install lxml`
    - `pip install selenium`
- 至 Chrome Driver 下載當前電腦中Chrome所對應版本的Driver

# 教學開始
- 首先因為這次的主題為圖片下載，因此我們找了一個擁有大量圖片的漫畫網頁來進行教學。
- 由於這個網頁的圖片是由JavaScript加載的，因此一開始我們使用Selenium來開啟網頁
    ```python
    from selenium import webdriver

    url = 'https://www.ohmanhua.com/13621/1/1.html'
    #使用crx插件
    chop = webdriver.ChromeOptions()
    chop.add_extension('Adblock-Plus_v3.8.4.crx')
    browser = webdriver.Chrome(options = chop)
    browser.implicitly_wait(10)
    browser.get(url)
    ```
    - 當然若不想顯示瀏覽器的視窗可以使用headless模式
    ```python
    chop.add_argument('--headless')  #規避google bug
    chop.add_argument('--disable-gpu')
    ```
- 接下來使用BeautifulSoup4來分析頁面，並取得圖片網址，和共幾張圖片
```python
img_count = int(soup.find('select', {'class': 'mh_select'}).find_all('option')[-1].get('value'))
img_url = soup.find_all('div', {'class': 'mh_comicpic'})[0].find('img').get('src')
if img_url[0] == '/':
    img_url = 'https:' + img_url
m = len(img_url.rsplit('/', 1)[1].split('.')[0])
img_url = img_url.rsplit('/', 1)[0] + '/'
```
- 最後便是下載圖片和關閉瀏覽器
```python
def download_img(img, num):
    r = requests.get(img)
    with open(save_url + str(num+1) + '.jpg', 'wb') as f:
        f.write(r.content)

for i in range(img_count):
    img = img_url + str(i + 1).zfill(m) + '.jpg'
    download_img(img, i)
browser.quit()
```

# 完整程式碼
```python
# -*- coding: UTF-8 -*-
from selenium import webdriver
from bs4 import BeautifulSoup
import requests

def download_img(img, num):
    r = requests.get(img)
    with open(save_url + str(num+1) + '.jpg', 'wb') as f:
        f.write(r.content)
    
if __name__ == "__main__":  
    save_url = './download/'
    url = ''
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
    for i in range(img_count):
        img = img_url + str(i + 1).zfill(m) + '.jpg'
        download_img(img, i)
    browser.quit()
```