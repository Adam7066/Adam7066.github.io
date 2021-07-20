---
slug: "star_coder-02"
title: "StarCoder2021暑訓：Week01"
date: 2021-07-15T00:07:54+08:00
tags:
    - UVA
categories:
    - StarCoder
---
# 主題
- 搜尋、排序、貪心

# 題目
- [Virtual Judge](https://vjudge.net/contest/446298)

# 題解
## A - Flip Sort
- 題目說明：
    - 給一堆數字，輸出要交換(只能相鄰交換)多少次，才能由小到大排好。
- 解題思路：
    - Bubble sort
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    int main() {
        int n, a[1005];
        while(cin >> n) {
            int ans = 0;
            for(int i = 0; i < n; ++i) cin >> a[i];
            for(int i = 0; i < n; ++i) {
                for(int j = n - 1; j > i; --j) {
                    if(a[j] < a[j - 1]) {
                        swap(a[j], a[j - 1]);
                        ++ans;
                    }
                }
            }
            cout << "Minimum exchange operations : " << ans << '\n';
        }
        return 0;
    }
    ```

## B - Age Sort
- 題目說明：
    - 給一堆數字，由小到大排序。
- 解題思路：
    - std::sort()
    - 小心 Presentation error
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    int main() {
        ios::sync_with_stdio(0);
        cin.tie(0);
        int n;
        bool flag;
        while (cin >> n && n) {
            int a[n];
            for(int i = 0; i < n; ++i) cin >> a[i];
            sort(a, a + n);
            flag = false;
            for(auto i : a) {
                if(flag) cout << ' ';
                cout << i;
                flag = true;
            }
            cout << endl;
        }
        return 0;
    }
    ```

## C - Conformity
- 題目說明：
    - 輸出最多人選擇課的程組合的人數，若是最多的有多個，將人數相加。
- 解題思路：
    - 將每行的課程排序後，再丟入 map 中統計。
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    int main() {
        ios::sync_with_stdio(0);
        cin.tie(0);
        int n;
        while(cin >> n && n) {
            int maxNum = 0, total = 0;
            map<string, int> msi;
            vector<string> vs;
            string input, line;
            while(n--) {
                line.clear();
                vs.clear();
                for(int i = 0; i < 5; ++i) {
                    cin >> input;
                    vs.push_back(input);
                }
                sort(vs.begin(), vs.end());
                for(int i = 0; i < 5; ++i) line += vs[i];
                ++msi[line];
            }
            for(auto it = msi.begin(); it != msi.end(); ++it)
                if(it->second > maxNum) maxNum = it -> second;
            for(auto it = msi.begin(); it != msi.end(); ++it)
                if(it->second == maxNum) total += maxNum;
            cout << total << endl;
        }
        return 0;
    }
    ```

## D - Guessing Game
- 題目說明：
    - 依照題目模擬，看有沒有說謊。
- 解題思路：
    - 模擬操作，最後看 right on 的值有沒有在區間中。
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    int main() {
        int n;
        while(scanf("%d ", &n) != EOF && n != 0) {
            int s = 1, e = 10;
            string str;
            while(getline(cin, str) && str != "right on") {
                if(str == "too high") e = min(e, n - 1);
                else if(str == "too low") s = max(s, n + 1);
                scanf("%d ", &n);
            }
            if(n >= s && n <= e) cout << "Stan may be honest\n";
            else cout << "Stan is dishonest\n";
        }
        return 0;
    }
    ```

## E - Ancient Cipher
- 題目說明：
    - 判斷第一行字串能不能依照題目的轉換方式變成第二行字串。
- 解題思路：
    - 先紀錄彼此字母出現的次數並排序，再比對排序後數值是否相同即可。
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    int main() {
        string s1, s2;
        while(cin >> s1 >> s2) {
            int a[26] = {0}, b[26] = {0};
            for(auto i : s1) ++a[i - 'A'];
            for(auto i : s2) ++b[i - 'A'];
            sort(a, a + 26);
            sort(b, b + 26);
            bool flag = true;
            for(int i = 0; i < 26; ++i) {
                if(a[i] != b[i]) {
                    cout << "NO\n";
                    flag = false;
                    break;
                }
            }
            if(flag) cout << "YES\n";
        }
        return 0;
    }
    ```

## F - Bridge Hands
- 題目說明：
    - 依照給定方位的下一個人開始給牌，最後輸出要依照順序排列。
- 解題思路：
    - 就模擬發牌並排序吧。
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    using pcc = pair<char, char>;
    char c;
    vector<pcc> v[4];
    bool cmp(pcc a, pcc b) {
        int ta[2], tb[2];
        char cc[4] = {'C', 'D', 'S', 'H'};
        char ct[13] = {'2', '3', '4', '5', '6', '7', '8', '9', 'T', 'J', 'Q', 'K', 'A'};
        for(int i = 0; i < 4; ++i) {
            if(a.first == cc[i]) ta[0] = i;
            if(b.first == cc[i]) tb[0] = i;
        }
        for(int i = 0; i < 13; ++i) {
            if(a.second == ct[i]) ta[1] = i;
            if(b.second == ct[i]) tb[1] = i;
        }
        if(ta[0] < tb[0]) return true;
        else if(ta[0] > tb[0]) return false;
        else {
            if(ta[1] < tb[1]) return true;
            else return false;
        }
    }
    void sortCard() {
        for(int i = 0; i < 4; ++i) {
            sort(v[i].begin(), v[i].end(), cmp);
        }
    }
    void printCard() {
        char oc[4] = {'S', 'W', 'N', 'E'};
        char cc[4] = {'N', 'E', 'S', 'W'};
        int ic[4][4] = {{1, 2, 3, 0}, {0, 1, 2, 3}, {3, 0, 1, 2}, {2, 3, 0, 1}};
        int t;
        for(int i = 0; i < 4; ++i) {
            if(cc[i] == c) {
                t = i;
                break;
            }
        }
        for(int i = 0; i < 4; ++i) {
            cout << oc[i] << ":";
            for(auto it : v[ic[t][i]])
                cout << " " << it.first << it.second;
            cout << endl;
        }
    }
    int main() {
        while(cin >> c && c != '#') {
            string s;
            int temp = 0;
            for(int i = 0; i < 4; ++i) v[i].clear();
            for(int i = 0; i < 2; ++i) {
                cin >> s;
                int len = s.size();
                for(int j = 0; j < len; j += 2) {
                    v[temp % 4].push_back({s[j], s[j + 1]});
                    ++temp;
                }
            }
            sortCard();
            printCard();
        }
        return 0;
    }
    ```

## G - Vito's Family
- 題目說明：
    - 選出一個數，並計算彼此距離和最小的值。
- 解題思路：
    - 中位數算距離和。
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    int main() {
        int T;
        cin >> T;
        while(T--) {
            int n;
            cin >> n;
            int a[n];
            for(int i = 0; i < n; ++i) cin >> a[i];
            sort(a, a + n);
            int mid = a[n / 2], ans = 0;
            for(int i = 0; i < n; ++i) ans += abs(a[i] - mid);
            cout << ans << endl;
        }
        return 0;
    }
    ```

## H - Shoemaker's Problem
- 題目說明：
    - 找出能使罰金最少的工作順序。
- 解題思路：
    - 貪心法
    - 依照 (罰金 / 時間) 的值，大到小排序。
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    using pdi = pair<double, int>;
    bool cmp(pdi a, pdi b) {
        return a.first > b.first;
    }
    int main() {
        int T;
        cin >> T;
        while(T--) {
            int n;
            cin >> n;
            vector<pdi> v;
            for(int i = 1; i <= n; ++i) {
                int a, b;
                cin >> a >> b;
                v.push_back({(b * 1.0 / a), i});
            }
            sort(v.begin(), v.end(), cmp);
            bool flag = false;
            for(auto it : v) {
                if(flag) cout << ' ';
                cout << it.second;
                flag = true;
            }
            cout << endl;
            if(T) cout << endl;
        }
        return 0;
    }
    ```

## I - The Bus Driver Problem
- 題目說明：
    - 公車工作安排，輸出雇主最少需要支付的加班費。
- 解題思路：
    - 先依照早晚排序，頭尾配對(最大配最小)，最後計算加班費的金額。
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    int main() {
        int n, d, r;
        while(cin >> n >> d >> r && n && d && r) {
            int morning[n], night[n];
            for(int i = 0; i < n; ++i) cin >> morning[i];
            for(int i = 0; i < n; ++i) cin >> night[i];
            sort(morning, morning + n);
            sort(night, night + n);
            int ans = 0;
            for(int i = 0; i < n; ++i) {
                if(morning[i] + night[n - i - 1] > d)
                    ans += ((morning[i] + night[n - i - 1] - d) * r);
            }
            cout << ans << endl;
        }
        return 0;
    }
    ```

## J - Watering Grass
- 題目說明：
    - 計算最少需要多少個噴水頭才能覆蓋整個區域，若都不行輸出 -1。
- 解題思路：
    - 先轉換成一維。
    - 貪心法
    - 區間覆蓋
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    using pdd = pair<double, double>;
    bool cmp(pdd a, pdd b) {
        if(a.first == b.first) return a.second > b.second;
        else return a.first < b.first;
    }
    int main() {
        int n;
        double l, w;
        while(cin >> n >> l >> w) {
            w /= 2;
            double p, r;
            vector<pdd> v;
            for(int i = 0; i < n; ++i) {
                cin >> p >> r;
                if(r > w) {
                    double dd = sqrt(r * r - w * w);
                    v.push_back({p - dd, p + dd});
                }
            }
            sort(v.begin(), v.end(), cmp);
            int ans = 0;
            double right = 0.0;
            for(int i = 0; i < v.size(); ++i) {
                if(v[i].first > right) break;
                for(int j = i + 1; j < v.size() && v[j].first <= right; ++j) {
                    if(v[j].second > v[i].second) i = j;
                }
                ++ans;
                right = v[i].second;
                if(right >= l) break;
            }
            if(right >= l) cout << ans << endl;
            else cout << "-1\n";
        }
        return 0;
    }
    ```

## K - Ultra-QuickSort
- 題目說明：
    - 輸出最少要交換的數量。
- 解題思路：
    - 用 merge sort 求逆序數對。
    - 大佬們要用 BIT 去解也行。
- 程式碼：
    ```cpp
    #include <bits/stdc++.h>
    using namespace std;
    long long ans = 0;
    long long a[500005];
    void mergeSort(long long *arr, int len) {
        if(len <= 1) return;
        int leftLen = len / 2, rightLen = len - leftLen;
        long long *leftArr = arr, *rightArr = arr + leftLen;
        mergeSort(leftArr, leftLen);
        mergeSort(rightArr, rightLen);
        static long long tmp[500005];
        long long tmpLen = 0, l = 0, r = 0;
        while(l < leftLen && r < rightLen) {
            if(leftArr[l] < rightArr[r]) tmp[tmpLen++] = leftArr[l++];
            else {
                tmp[tmpLen++] = rightArr[r++];
                ans += leftLen - l;
            }
        }
        while(l < leftLen) tmp[tmpLen++] = leftArr[l++];
        while(r < rightLen) tmp[tmpLen++] = rightArr[r++];
        for(int i = 0; i < tmpLen; ++i) arr[i] = tmp[i];
    }
    int main() {
        int n;
        while(cin >> n && n) {
            ans = 0;
            for(int i = 0; i < n; ++i) cin >> a[i];
            mergeSort(a, n);
            cout << ans << endl;
        }
        return 0;
    }
    ```
