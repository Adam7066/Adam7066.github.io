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
|三|圖、狀態搜尋、拓樸排序、尤拉路|[Link](../star_coder-04)|
|四|動態規劃|[Link](../star_coder-05)|
|五|最小生成樹|[Link](../star_coder-06)|
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
    |併查集 disjoint set|有效率地合併兩個集合、有效率地查詢兩個元素是否屬於同一集合|無。[模板](../star_coder-03/#併查集模板)|
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
## 第三週 - 圖、狀態搜尋、拓樸排序、尤拉路
- 線上教材
    |教材|說明|
    |--|--|
    |[師大碼賽客：狀態搜尋](https://drive.google.com/file/d/1J4BtOA5YqNyHE98rqk_zQgi6VNxYK4Xy/view?usp=sharing)|仲軒學長的教學講義（手把手教學與題目解說）|
    |[師大碼賽客：基礎圖論](https://drive.google.com/file/d/1sFEu6V7Sk5RpT01SSH4P7RBOdYHXPioe/view?usp=sharing)|健愷學長的教學講義（有拓樸序和尤拉路）|
    |[台大資訊之芽：圖](https://www.csie.ntu.edu.tw/~sprout/algo2019/ppt_pdf/week04/graph.pdf)|圖的實作/搜尋/二分圖判定|
    |[建中培訓 (第 1/2/5 節)](https://tioj.ck.tp.edu.tw/uploads/attachment/5/13/3.pdf)|第5節有拓樸排序|
    |[成大培訓 (單元 4)：圖/DFS/BFS](https://nckuacm.github.io/2019/)|有一些練習題|
- 線上影片
    |影片|說明|
    |--|--|
    |[台大陳縕儂老師：圖](https://youtu.be/kQu6JS4HWbs)|40 分鐘學圖的概念、實作與一筆畫問題|
    |[台大陳縕儂老師：BFS](https://youtu.be/l8VG83k3b6g)|60 分鐘（主要講證明）|
    |[台大陳縕儂老師：DFS/連通/拓樸排序](https://youtu.be/F_20YOiLKBI)|60 分鐘|
- 演算法視覺化
    |演算法|說明|
    |--|--|
    |[BFS](https://www.cs.usfca.edu/~galles/visualization/BFS.html)|搭配樹狀圖/陣列實作/串列實作：推薦！|
    |[DFS](https://www.cs.usfca.edu/~galles/visualization/DFS.html)|搭配樹狀圖/陣列實作/串列實作：推薦！|
    |[連通 (connected component)](https://www.cs.usfca.edu/~galles/visualization/ConnectedComponent.html)|搭配樹狀圖/陣列實作/串列實作|
    |[拓樸排序](https://www.cs.usfca.edu/~galles/visualization/TopoSortDFS.html)|搭配樹狀圖/陣列實作/串列實作|
## 第四週 - 動態規劃
- 線上教材
    |教材|說明|
    |--|--|
    |[師大碼賽客：基礎 DP](https://drive.google.com/file/d/1i2jVK5B-mlC12hx_qAGMnYd0Q0eYLzMy/view?usp=sharing)|品新學長的教學講義（涵蓋重要經典題型與題解）|
    |[台大資訊之芽：DP 概念](https://www.csie.ntu.edu.tw/~sprout/algo2019/ppt_pdf/week07/dynamic_programming_1.pdf)、[LIS/LCS](https://www.csie.ntu.edu.tw/~sprout/algo2019/ppt_pdf/week07/dynamic_programming_2_1.pdf)、[零錢/背包](https://www.csie.ntu.edu.tw/~sprout/algo2019/ppt_pdf/week09/dynamic_programming_2_2.pdf)|三段講義|
    |[成大培訓 (單元 9)：DP (背包/LIS/LCS)](https://nckuacm.github.io/2019/)|有一些練習題|
    |[sa072686 的筆記](https://hackmd.io/@sa072686/DP?type=view)|有很多習題與解答。|
- 線上影片
    |影片|說明|
    |--|--|
    |[台大陳縕儂老師：DP 概念](https://youtu.be/Ovh1Pgrxc-o)|20 分鐘影片：以費氏數列解釋 DP 概念|
    |[台大陳縕儂老師：矩陣連乘 (區間 DP)](https://youtu.be/baurdahvESA)|30 分鐘|
- 演算法視覺化
    |演算法|說明|
    |--|--|
    |[LIS](https://algorithm-visualizer.org/dynamic-programming/longest-increasing-subsequence)|O(n2) 程式步進動畫|
    |[LCS](https://algorithm-visualizer.org/dynamic-programming/longest-common-subsequence)|O(n2) 程式步進動畫|
    |[背包](https://algorithm-visualizer.org/dynamic-programming/knapsack-problem)|O(NW) 程式步進動畫|

## 第五週 - 最小生成樹
- 線上教材
    |教材|說明|
    |--|--|
    |[師大碼賽客：MST](https://drive.google.com/file/d/1GRIOvPBGcm2655oochxr6sgtNiRvFdft/view?usp=sharing)|智鈞學長的教學講義與題解（本集也包含拓撲排序和尤拉路）|
    |[成大培訓 (單元 11)：Graph](https://nckuacm.github.io/2019/)|速成、有一些練習題|
    |[演算法筆記：生成樹](http://web.ntnu.edu.tw/~algo/SpanningTree.html)|師大資工最有名的個人網站XD|
- 線上影片
    |影片|說明|
    |--|--|
    |[資訊之芽：最小生成樹](https://youtu.be/NXfNZa3rKD0)|30分鐘|
- 演算法視覺化
    |演算法|說明|
    |--|--|
    |[Kruskal’s](https://www.cs.usfca.edu/~galles/visualization/Kruskal.html)|搭配樹狀圖/陣列實作/串列實作：推薦！|
    |[Kruskal’s](https://algorithm-visualizer.org/greedy/kruskals-minimum-spanning-tree)|搭配程式碼|
    |[Prim’s](https://www.cs.usfca.edu/~galles/visualization/Prim.html)|搭配樹狀圖/陣列實作/串列實作：推薦！|
    |[Prim’s](https://algorithm-visualizer.org/greedy/prims-minimum-spanning-tree)|搭配程式碼|