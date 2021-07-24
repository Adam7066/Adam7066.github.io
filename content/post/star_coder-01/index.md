---
slug: "star_coder-01"
title: "StarCoder2021暑訓"
date: 2021-07-14T22:52:11+08:00
categories:
    - StarCoder
---
# 簡介
|週次|主題|題目|
|:--:|:--:|:--:|
|一|搜尋、排序、貪心|[Link](../star_coder-02/)|
|二|STL、併查集|[Link](../star_coder-03/)|
|三|||
|四|||
|五|||
|六|||
|七|||
|八|||

# 學習資源
## 第一週 - 搜尋、排序、貪心
- 線上教材
    |教材|說明|
    |--|--|
    |[師大碼賽客：排序/貪心/二分搜](https://drive.google.com/file/d/1_7Ch2G52jH8fHqlO_Atmjp9smAnvq9R4/view?usp=sharing)|子緯學長的教學講義（詳盡的新手入門）|
    |[北一女培訓：排序](https://web.fg.tp.edu.tw/~tfgcsblog/blog/wp-content/uploads/2010/12/Training-2-Sorting-All.pdf)|六種排序法的程式與簡介|
    |[建中培訓 (第4/6/7節)](https://tioj.ck.tp.edu.tw/uploads/attachment/5/12/1_2.pdf)|4.排序STL/6.貪心/7.二分搜|
    |[台大資訊之芽：貪心](https://www.csie.ntu.edu.tw/~sprout/algo2019/ppt_pdf/week05/greedy.pdf)|貪心法與理論/Huffman Tree|
    |[成大競程培訓 (單元5/6)](https://nckuacm.github.io/2019/)|5.二分搜/6.三種排序|
- 線上影片
    |影片|說明|
    |--|--|
    |[台大孔令傑老師：二分搜尋法](https://youtu.be/QVj81KR-_Hk)|7 分鐘學會二分搜尋法|
    |[看舞蹈學排序法](https://www.youtube.com/user/AlgoRythmics/videos)|以舞蹈呈現各種排序法的運作過程|
    |[台大陳縕儂老師：貪心](https://youtu.be/Q6VyP6P4ukA)|50 分鐘的正規演算法課程 (CLRS課本)|
- 演算法視覺化
    |演算法||
    |--|--|
    |[線性搜尋與二分搜尋](https://www.cs.usfca.edu/~galles/visualization/Search.html)|以動畫呈現兩種搜尋法的運作過程|
    |[排序](https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html)|以動畫呈現六種排序演算法的運作過程|
## 第二週 - STL、併查集
- 內容大綱
    |資料結構|簡要說明|C++ 內建|
    |--|--|--|
    |陣列|在記憶體中連續，支援隨機存取以及 O(1) 插入尾端|std::vector|
    |字串|以 ‘\0’ 結尾的字元陣列|std::string|
    |串列 linked list|支援 O(1) 的插入與刪除|std::list|
    |堆疊 stack|支援後進先出 (LIFO) 的存取模式|std::stack|
    |佇列 queue|支援先進先出 (FIFO) 的存取模式|std::queue|
    |堆積 heap (優先隊列 priority_queue)|支援 O(logN) 的插入和 O(logN) 取出最大/小值|std::priority_queue|
    |併查集 disjoint set|有效率地合併兩個集合、有效率地查詢兩個元素是否屬於同一集合|無。[模板](../star_coder-03/)|
    |平衡搜尋樹|記錄 (鍵, 值) 對應關係，支援 O(logN) 的插入和查詢|std::map|
    |平衡搜尋樹|實現「集合」，支援 O(logN) 的插入和查詢|std::set|
- 線上教材
    |教材|說明|
    |--|--|
    |[師大碼賽客：基礎資料結構/STL](https://drive.google.com/file/d/1XoAMrDslzOPR6THMBpW1Xt5xfc_9hnaA/view?usp=sharing)|品新學長的教學講義（詳盡的STL語法示範與題目解說）|
    |[板中培訓：STL](https://drive.google.com/file/d/1Gir6DKICVljhpfzW1_Q4Rhs81riPyIiW/view)|STL語法|
    |[建中培訓 (第3節)](https://tioj.ck.tp.edu.tw/uploads/attachment/5/12/1_2.pdf)|STL語法|
    |[北一女培訓：樹/二元樹/Heap/BST](http://web.fg.tp.edu.tw/~tfgcsblog/blog/wp-content/uploads/2010/12/%E6%A8%B9%E7%8B%80%E7%B5%90%E6%A7%8BTreeHeap.ppt)|樹狀結構投影片|
    |[北一女培訓：併查集(disjoint set)](http://web.fg.tp.edu.tw/~tfgcsblog/blog/wp-content/uploads/2016/07/06_1_%E9%80%B2%E9%9A%8E%E8%B3%87%E6%96%99%E7%B5%90%E6%A7%8B_Disjoint_Set.pptx)|併查集投影片|
    |[成大競程培訓 (單元2/3/4)](https://nckuacm.github.io/2019/)|資結/STL/樹/圖|
- 演算法視覺化
    |資料結構|說明|
    |--|--|
    |[堆積 (heap)](https://www.cs.usfca.edu/~galles/visualization/Heap.html)|最小堆積的插入與取值動畫（圖形結構與陣列內容）：推薦！|
    |[併查集](https://www.cs.usfca.edu/~galles/visualization/DisjointSets.html)|併查集的 union/find 操作動畫（圖形結構與陣列內容）：推薦！|