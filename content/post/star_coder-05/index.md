---
slug: "star_coder-05"
title: "StarCoder2021暑訓：Week04"
date: 2021-08-06T10:42:11+08:00
tags:
    - UVA
categories:
    - StarCoder
---
# 主題
- 動態規劃
## 經典背包模板
### 0/1 背包 & 無限背包
```cpp
const int N = 100, W = 100000;
int cost[N], weight[N], c[W + 1];
void knapsack(int n, int w) {
    memset(c, 0, sizeof(c));
    for (int i = 0; i < n; ++i)
        for (int j = w; j - weight[i] >= 0; --j) // 0/1 背包
        // for (int j = weight[i]; j <= w; ++j) 無限背包
            c[j] = max(c[j], c[j - weight[i]] + cost[i]);
    cout << "最高的價值為" << c[w];
}
```
### 有限背包
```cpp
const int N = 100, W = 100000;
int cost[N], weight[N], number[N], c[W + 1];
void knapsack(int n, int w) {
    for (int i = 0; i < n; ++i) {
        int num = min(number[i], w / weight[i]);
        for (int k = 1; num > 0; k *= 2) {
            if (k > num) k = num;
            num -= k;
            for (int j = w; j >= weight[i] * k; --j)
                c[j] = max(c[j], c[j - weight[i] * k] + cost[i] * k);
        }
    }
    cout << "最高的價值為" << c[w];
}
```
## 經典零錢問題模板
```cpp
int price[5] = {5, 2, 6, 11, 17};
bool c[1000+1]; //int c[1000+1];
void change(int m) {
    memset(c, false, sizeof(c));
    c[0] = true;
    for (int i = 0; i < 5; ++i)
        for (int j = price[i]; j <= m; ++j)
            c[j] ||= c[j-price[i]];
            // c[j] += c[j-price[i]];
    if (c[m]) cout << "湊得到";
    else cout << "湊不到";
    // cout << "湊得價位" << m;
    // cout << "湊法總共" << c[m] << "種";
}
```
## LIS 模板
### DP  
```cpp
const int N = 100;
int s[N], length[N];
int LIS() {
    for (int i=0; i<N; i++) length[i] = 1;
    for (int i=0; i<N; i++)
        for (int j=0; j<i; j++)
            if (s[j] < s[i])
                length[i] = max(length[i], length[j] + 1);
    int l = 0;
    for (int i=0; i<N; i++) l = max(l, length[i]);
    return l;
}
```
### Robinson-Schensted-Knuth Algorithm
- 時間複雜度 O(NlogL) ， N 是序列長度， L 是 LIS 長度。
```cpp
int LIS(vector<int>& s) {
    if (s.size() == 0) return 0;
    vector<int> v;
    v.push_back(s[0]);
    for (int i=1; i<s.size(); ++i) {
        int n = s[i];
        if (n > v.back()) v.push_back(n);
        else *lower_bound(v.begin(), v.end(), n) = n;
    }
    return v.size();
}
```
## LCS 模板
### DP
```cpp
const int N1 = 7, N2 = 5;
int s1[N1+1] = {0, 2, 5, 7, 9, 3, 1, 2};
int s2[N2+1] = {0, 3, 5, 3, 2, 8};
int length[N1+1][N2+1];
int LCS() {
    for (int i=0; i<=N1; i++) length[i][0] = 0;
    for (int j=0; j<=N2; j++) length[0][j] = 0;
    for (int i=1; i<=N1; i++)
        for (int j=1; j<=N2; j++)
            if (s1[i] == s2[j]) length[i][j] = length[i-1][j-1] + 1;
            else length[i][j] = max(length[i-1][j], length[i][j-1]);
    return length[N1][N2];
}
```
### Hunt-Szymanski Algorithm
- LCS 問題，化作 2D LIS 問題，再化作 1D LIS 問題，最後套用 Robinson-Schensted-Knuth Algorithm 。
- 排序所有數對，使用 Counting Sort 。掃描一遍 s2 ，把每個字元的位置紀錄下來。
- 較短的序列當作 s1 ，時間複雜度是 O(Klog(min(N,M)) + R) 。 K 是數對數目， N 和 M 是序列長度， R 是數字範圍。
- K 至多是 NM ，最差情況下比先前的演算法還慢，平均情況下比先前的演算法快上許多。 R 源自 Counting Sort 。
```cpp
int LCS(string &s1, string &s2) {
    vector<int> p[128]; // 假設字元範圍為 0 ~ 127
    for (int i = 0; i < s2.size(); ++i)
        p[s2[i]].push_back(i);
    vector<int> v;
    v.push_back(-1);
    for (int i = 0; i < s1.size(); ++i)
        for (int j = p[s1[i]].size() - 1; j >= 0; --j) {
            int n = p[s1[i]][j];
            if (n > v.back()) v.push_back(n);
            else *lower_bound(v.begin(), v.end(), n) = n;
        }
    return v.size() - 1;
}
```
# 題目
- [Virtual Judge](https://vjudge.net/contest/450622)
- 題目列表與提示
    |題目|題目需求|採用演算法|難度 (for 新手)|
    |--|--|--|:--:|
    |UVa 10130|給物品價值和重量，給全家人每人最大負重，問全家能搬走的最高總價。|經典背包|易|
    |UVa 357|給定幣值種類，問某一個金額存在幾種表示法。|經典零錢問題|易|
    |UVa 10405|給兩個字串，問最長共同子字串。|裸 LCS|易|
    |UVa 369|求 N 個中取 M 個數的總組合數。|組合數公式|易|
    |UVa 10003|給一根棍子上的切斷點，每次切斷的成本為該段長度，問所有切斷點都切斷的最小成本。|區間 DP|易|
    |UVa 103|給一堆 D 維的盒子，問這些盒子一個裝進一個，最多能裝多少個。|DAG 最短路/LIS|中|
    |UVa 10534|一個 Wavio 序列含 2n+1 個數，前半嚴格遞增，後半嚴格遞減。求一個數列中的最長 Wavio 序列。|O(nlogn) LIS|難|
    |UVa 10949|給兩個長度最多 20000 個字元的字串，求最長共同子字串。|因字串很長，需透過 LCS->LIS 轉換，再用快速 LIS 解決。|難|
    |UVa 10032|給一群人的體重，將他們分成兩群（人數最多差一個），並儘可能最小化兩群的體重和差距。|背包變化+狀態壓縮|\\(難^2\\)|
    |UVa 104|給 n 種貨幣彼此間的匯率，找出一種匯率轉換順序能從中套利 1% 以上。|Floyd-Warshall|\\(難^2\\)|

# 參考作法
## A - SuperSale
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        int n, m, c[35], g, ans = 0;
        cin >> n;
        int p[n], w[n];
        for(int i = 0; i < n; ++i)
            cin >> p[i] >> w[i];
        cin >> m;
        for(int i = 0; i < m; ++i) {
            cin >> g;
            memset(c, 0x00, sizeof(c));
            for(int j = 0; j < n; ++j) {
                for(int k = g; k - w[j] >= 0; --k) {
                    c[k] = max(c[k], c[k - w[j]] + p[j]);
                }
            }
            ans += c[g];
        }
        cout << ans << endl;
    }
    return 0;
}
```
## B - Let Me Count The Ways
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int p[5] = {50, 25, 10, 5, 1}, n;
    long long c[30005];
    while(cin >> n) {
        memset(c, 0, sizeof(c));
        c[0] = 1;
        for(int i = 0; i < 5; ++i) {
            for(int j = p[i]; j <= n; ++j) {
                c[j] += c[j - p[i]];
            }
        }
        if(c[n] == 1) cout << "There is only 1 way to produce " << n << " cents change.\n";
        else cout << "There are " << c[n] << " ways to produce " << n << " cents change.\n";
    }
    return 0;
}
```
## 	C - Longest Common Subsequence
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    string s1, s2;
    while(getline(cin, s1)) {
        getline(cin, s2);
        int n1 = s1.length(), n2 = s2.length();
        int dp[n1 + 1][n2 + 1];
        for(int i = 0; i <= n1; ++i) dp[i][0] = 0;
        for(int i = 0; i <= n2; ++i) dp[0][i] = 0;
        for(int i = 0; i < n1; ++i) {
            for(int j = 0; j < n2; ++j) {
                if(s1[i] == s2[j]) dp[i + 1][j + 1] = dp[i][j] + 1;
                else dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j]);
            }
        }
        cout << dp[n1][n2] << endl;
    }
    return 0;
}
```
## 	D - Combinations
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    long long c[105][105] = {0};
    for(int i = 1 ; i <= 100 ; ++i) {
        for(int j = 1 ; j <= i ; ++j) {
            if(i == j) c[i][j] = 1;
            else c[i][j] = c[i - 1][j] * i / (i - j);
        }
    }
    int n, m;
    while(cin >> n >> m && !(n == 0 && m == 0))
        cout << n << " things taken " << m << " at a time is " << c[n][m] << " exactly.\n";
    return 0;
}
```
## E - Cutting Sticks
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int len, n, c[55], dp[55][55];
    while(cin >> len && len) {
        cin >> n;
        c[0] = 0;
        c[++n] = len;
        memset(dp, 0, sizeof(dp));
        for(int i = 1; i < n; ++i) cin >> c[i];
        for(int w = 2; w <= n; ++w) {
            for(int l = 0; l < n - 1; ++l) {
                int r = l + w;
                if(r > n) break;
                dp[l][r] = INF;
                for(int m = l + 1; m < r; ++m)
                    dp[l][r] = min(dp[l][m] + dp[m][r] + c[r] - c[l], dp[l][r]);
            }
        }
        cout << "The minimum cutting is " << dp[0][n] << ".\n";
    }
    return 0;
}
```
## F - Stacking Boxes
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
struct Box {
    int id;
    vector<int> len;
};
bool boxcmp(const Box &a, const Box &b) {
    int sz = a.len.size();
    for(int i = 0; i < sz; ++i) {
        if(a.len[i] < b.len[i]) return true;
        if(a.len[i] > b.len[i]) return false;
    }
    return true;
}
bool isContain(const Box &a, const Box &b) {
    int sz = a.len.size();
    for(int i = 0; i < sz; ++i) {
        if(a.len[i] <= b.len[i]) return false;
    }
    return true;
}
void printBox(const vector<int> &prevNesting, const vector<Box> &box, int lastbox, bool printSpace) {
    if(lastbox == -1) return;
    printBox(prevNesting, box, prevNesting[lastbox], true);
    cout << box[lastbox].id;
    if(printSpace) cout << " ";
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int k, n;
    while(cin >> k >> n) {
        vector<Box> box;
        for(int i = 0; i < k; ++i) {
            int t;
            vector<int> tmp;
            for(int j = 0; j < n; ++j) {
                cin >> t;
                tmp.push_back(t);
            }
            sort(tmp.begin(), tmp.end());
            box.push_back({i + 1, tmp});
        }
        sort(box.begin(), box.end(), boxcmp);
        vector<int> maxNesting(k, 1), prevNesting(k, -1);
        int maxLen = 1, lastbox = 0;
        for(int i = 0; i < k; ++i) {
            for(int j = 0; j < i; ++j) {
                if(isContain(box[i], box[j])) {
                    if(maxNesting[j] + 1 > maxNesting[i]) {
                        maxNesting[i] = maxNesting[j] + 1;
                        prevNesting[i] = j;
                        if(maxNesting[i] > maxLen) {
                            maxLen = maxNesting[i];
                            lastbox = i;
                        }
                    }
                }
            }
        }
        cout << maxLen << endl;
        printBox(prevNesting, box, lastbox, false);
        cout << endl;
    }
    return 0;
}
```
## G - Wavio Sequence
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int n, a[10005], dpI[10005], dpD[10005];

void LIS() {
    if(n == 0) return;
    vector<int> v;
    v.push_back(a[0]);
    dpI[0] = 1;
    for(int i = 1; i < n; ++i) {
        if(a[i] > v.back()) v.push_back(a[i]);
        else *lower_bound(v.begin(), v.end(), a[i]) = a[i];
        dpI[i] = v.size();
    }
}
void LDS() {
    if(n == 0) return;
    vector<int> v;
    v.push_back(a[n - 1]);
    dpD[n - 1] = 1;
    for(int i = n - 2; i >= 0; --i) {
        if(a[i] > v.back()) v.push_back(a[i]);
        else *lower_bound(v.begin(), v.end(), a[i]) = a[i];
        dpD[i] = v.size();
    }
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    while(cin >> n) {
        memset(dpI, 0, sizeof(dpI));
        memset(dpD, 0, sizeof(dpD));
        for(int i = 0; i < n; ++i)
            cin >> a[i];
        LIS();
        LDS();
        int ans = 1;
        for(int i = 0; i < n; ++i)
            ans = max(min(dpI[i], dpD[i]) * 2 - 1, ans);
        cout << ans << endl;
    }
    return 0;
}
```
## H - Kids in a Grid
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
string g[30];
int LCS(string &s1, string &s2) {
    vector<int> p[130];
    for(int i = 0; i < s2.size(); ++i)
        p[s2[i]].push_back(i);
    vector<int> v;
    v.push_back(-1);
    for (int i = 0; i < s1.size(); ++i) {
        for (int j = p[s1[i]].size() - 1; j >= 0; --j) {
            int n = p[s1[i]][j];
            if (n > v.back()) v.push_back(n);
            else *lower_bound(v.begin(), v.end(), n) = n;
        }
    }
    return v.size() - 1;
}
string getStr() {
    string s, path;
    int n, x, y;
    cin >> n >> x >> y;
    cin.ignore(100, '\n');
    getline(cin, path);
    int curX = x - 1, curY = y - 1;
    s += g[curX][curY];
    for(int i = 0; i < n; ++i) {
        if(path[i] == 'N') --curX;
        else if(path[i] == 'E') ++curY;
        else if(path[i] == 'S') ++curX;
        else if(path[i] == 'W') --curY;
        s += g[curX][curY];
    }
    return s;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int T;
    cin >> T;
    for(int i = 1; i <= T; ++i) {
        int row, col;
        cin >> row >> col;
        string str1, str2, tmp;
        for(int j = 0; j < row; ++j) cin >> g[j];
        str1 = getStr();
        str2 = getStr();
        int len, len1 = str1.length(), len2 = str2.length();
        if(len1 > len2) len = LCS(str2, str1);
        else len = LCS(str1, str2);
        cout << "Case " << i << ": " << len1 - len << " " << len2 - len << endl;
    }
    return 0;
}
```
## I - Tug of War
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int f(vector<int> &w, int sum) {
    if(w.size() == 1) return 0;
    sort(w.begin(), w.end());
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        int n, w[105];
        long long dp[22505];
        cin >> n;
        for(int i = 1; i <= n; ++i) cin >> w[i];
        long long sum = accumulate(w + 1, w + n + 1, 0LL);
        memset(dp, 0, sizeof(dp));
        dp[0] = 1;
        for(int i = 1; i <= n; ++i) {
            for(int j = sum / 2; j >= w[i]; j--) {
                dp[j] |= (dp[j - w[i]] << 1);
            }
        }
        if(n & 1) {
            for(int i = sum / 2; i >= 0; --i) {
                long long flag1 = (1LL << (n / 2));
                long long flag2 = (1LL << (n / 2 + 1));
                if((dp[i]&flag1) || (dp[i]&flag2)) {
                    cout << i << " " << sum - i << "\n";
                    break;
                }
            }
        }
        else {
            for(int i = sum / 2; i >= 0; --i) {
                if(dp[i] & (1LL << (n / 2))) {
                    cout << i << " " << sum - i << "\n";
                    break;
                }
            }
        }
        if(T) cout << endl;
    }
    return 0;
}
```
## J - Arbitrage
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
void printPath(vector<vector<vector<int>>> &paths, int t, int i, int j) {
    if(t == 0) {
        cout << i + 1 << " " << j + 1;
        return;
    }
    printPath(paths, t - 1, i, paths[t][i][j]);
    cout << " " << j + 1;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int n;
    while(cin >> n) {
        vector<vector<double>> ct(n, vector<double>(n, 0));
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < n; ++j) {
                if(i == j) ct[i][j] = 1;
                else cin >> ct[i][j];
            }
        }
        vector<vector<vector<double>>> values(n, vector<vector<double>>(n, vector<double>(n)));
        vector<vector<vector<int>>> paths(n, vector<vector<int>>(n, vector<int>(n, -1)));
        values[0] = ct;
        int item = -1, itemT = -1;
        for(int t = 1 ; t < n && item == -1 ; ++t) {
            for(int i = 0 ; i < n && item == -1 ; ++i) {
                for(int j = 0 ; j < n && item == -1 ; ++j) {
                    values[t][i][j] = -1.0;
                    for(int k = 0 ; k < n ; ++k ) {
                        double newRate = values[t - 1][i][k] * ct[k][j];
                        if(newRate > values[t][i][j]) {
                            values[t][i][j] = newRate;
                            paths[t][i][j] = k;
                        }
                    }
                }
                if(values[t][i][i] > 1.01) {
                    item = i;
                    itemT = t;
                    break;
                }
            }
        }
        if(item == -1) cout << "no arbitrage sequence exists\n";
        else {
            printPath(paths, itemT, item, item);
            cout << endl;
        }
    }
    return 0;
}
```