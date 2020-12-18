---
slug: "algorithm-01"
title: "演算法-01：蓄水池抽樣法"
date: 2020-12-08T16:02:25+08:00
categories:
    - Algorithm
---
# 蓄水池抽樣法 (Reservoir Sampling)
- 從 N 個樣本中，隨機抽取 K 個樣本，其中 N 非常大(不能將所有數據都放進記憶體或是一個未知數)，而每個被抽出來的機率要相等。

### 定理
該算法保證每個元素以\(k \over n \)的機率被選入蓄水池中。

### 證明
1. 第 i 個元素進入蓄水池的機率為\( k \over i \)，蓄水池內每個元素被替換的機率為\( 1 \over k \)
2. 因此在第 i 輪第 j 個元素被替換的機率為\( {k \over i}\times{1 \over k} = {1 \over i} \)，接下來用 M.I. (數學歸納法)來證明，當 n 次循環結束時每個元素進入蓄水池的機率為\( k \over n \)
3. 假設在 (i-1) 次迭代後，任意一個元素進入 蓄水池的概率為\( k \over i-1 \)。由上面的結論，在第 i 次迭代時，該元素被替換的概率為\( 1 \over i \)， 那麼其不被替換的概率則為\( 1 - {1 \over i} = {i - 1 \over i} \)
4. 故在第 i 次迭代後，該元素在蓄水池內的概率為\( {k \over i-1} \times {i-1 \over i} = {k \over i} \)，歸納結束。
5. 因此當循環結束時，每個元素進入蓄水池的概率為\( k \over n \)，命題得證。

### 例題
- [Leetcode 382.Linked List Random Node](https://leetcode.com/problems/linked-list-random-node/)
- 解法： Cpp
```cpp
class Solution {
    ListNode *p;
public:
    Solution(ListNode* head) {
        p = head;
    }
    int getRandom() {
        int ans = p->val;
        ListNode *t = p->next;
        for(int i=2; t; ++i){
            if(rand()%i == 0) ans = t->val;
            t = t->next;
        }
        return ans;
    }
};
```