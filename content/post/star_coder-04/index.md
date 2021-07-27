---
slug: "star_coder-04"
title: "StarCoder2021暑訓：Week03"
date: 2021-07-28T05:48:25+08:00
tags:
    - UVA
    - SPOJ
categories:
    - StarCoder
---
# 主題
- 圖、狀態搜尋、拓樸排序、尤拉路
# 題目
- [Virtual Judge](https://vjudge.net/contest/448943)
- 題目列表與提示
    |題目|題目需求|採用演算法|基本題|
    |--|--|--|:--:|
    |UVa 10004|無向圖的兩色著色問題|DFS/BFS 均可|V|
    |UVa 10959|求無向無權圖上每一點和一指定點的最短距離|BFS|V|
    |UVa 572|求二維地圖上的連通塊數量|DFS/BFS 均可|V|
    |UVa 441|給定 k 個數，由小到大列出所有包含其中 6 個數的遞增數列|DFS||
    |UVa 567|求無向無權圖上指定兩點間的最短距離|BFS (或用後面會學到的 Floyd-Warshall 演算法)|V|
    |UVa 10926|給有向無環圖，求最大一棵樹的節點數減1|DFS/BFS 均可|| 
    |SPOJ PT07Z|求數直徑（經典題）|DFS/BFS 均可|| 
    |UVa 10603|倒水問題（給三個水瓶，倒出指定水量）|BFS 變型 (帶權最短路)|| 
    |UVa 10305|給定 n 個工作的兩兩先後關係，輸出任一個合法的工作完成順序。|拓樸排序|V|
    |UVa 1423|給定一數列中 Sij = a[i]+…+a[j] 的正負號，輸出一組符合正負號關係的數列。(有趣，值得思考！)|拓樸排序||
    |UVa 302|給定一個無向圖和指定起點，列印尤拉路。|尤拉路|| 
    |UVa 10441|給定一堆字串，問如何將它們頭尾相連串起來。|尤拉路||

# 參考作法
## A - Bicoloring
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
using namespace std;
bool a[205][205];
int p[205];
bool dfs(int x, int n, int color) {
    if(p[x]) {
        if(p[x] != color) return false;
        else return true;
    }
    p[x] = color;
    bool ans = true;
    for(int i = x + 1; i < n; ++i) {
        if(a[x][i]) ans = dfs(i, n, (color == 1) ? 2 : 1);
    }
    return ans;
}
int main() {
    USE_CPPIO();
    int n, e;
    while(cin >> n && n) {
        cin >> e;
        memset(a, false, sizeof(a));
        memset(p, 0x00, sizeof(p));
        int u, v;
        for(int i = 0; i < e; ++i) {
            cin >> u >> v;
            a[u][v] = a[v][u] = true;
        }
        if(dfs(0, n, 1)) cout << "BICOLORABLE.\n";
        else cout << "NOT BICOLORABLE.\n";
    }
    return 0;
}
```
## B - The Party, Part I
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
using namespace std;
bool a[1005][1005];
int d[1005], n, m;
void bfs() {
    for(int i = 0; i < n; ++i) d[i] = n;
    d[0] = 0;
    queue<int> q;
    q.push(0);
    while (!q.empty()) {
        int now = q.front();
        q.pop();
        for(int i = 0; i < n; ++i) {
            if(a[now][i] && d[i] > d[now] + 1) {
                d[i] = d[now] + 1;
                q.push(i);
            }
        }
    }
}
int main() {
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        memset(a, false, sizeof(a));
        cin >> n >> m;
        int u, v;
        for(int i = 0; i < m; ++i) {
            cin >> u >> v;
            a[u][v] = a[v][u] = true;
        }
        bfs();
        for(int i = 1; i < n; ++i)
            cout << d[i] << "\n";
        if(T) cout << "\n";
    }
    return 0;
}
```
## C - Oil Deposits
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
int n, m, ans;
char g[105][105];
int d[8][2] = {{1, 1}, {1, 0}, {1, -1}, {0, -1}, {-1, -1}, {-1, 0}, {-1, 1}, {0, 1}};
void dfs(int x, int y) {
    g[x][y] = '*';
    for(int i = 0; i < 8; ++i) {
        int dx = x + d[i][0], dy = y + d[i][1];
        if(dx >= 0 && dx < n && dy >= 0 && dy < m && g[dx][dy] == '@') {
            dfs(dx, dy);
        }
    }
}
int main() {
    USE_CPPIO();
    while(cin >> n >> m && n && m) {
        ans = 0;
        string s;
        for(int i = 0; i < n; ++i) {
            cin >> s;
            for(int j = 0; j < m; ++j) g[i][j] = s[j];
        }
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < m; ++j) {
                if(g[i][j] == '@') {
                    dfs(i, j);
                    ++ans;
                }
            }
        }
        cout << ans << endl;
    }
    return 0;
}
```
## D - Lotto
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
int n, a[15], ans[15];
bool u[15];
void dfs(int x, int t) {
    if(t == 6) {
        for(int i = 0; i < 5; ++i)
            cout << ans[i]<<' ';
        cout << ans[5] << "\n";
        return;
    }
    for(int i = x; i < n; ++i) {
        if(!u[i]) {
            u[i] = true;
            ans[t] = a[i];
            dfs(i + 1, t + 1);
            u[i] = false;
        }
    }
}
int main() {
    USE_CPPIO();
    int cnt = 0;
    while(cin >> n && n) {
        if(cnt++) cout << "\n";
        memset(u, false, sizeof(u));
        for(int i = 0; i < n; ++i) cin >> a[i];
        dfs(0, 0);
    }
    return 0;
}
```
## E - Risk
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
int main() {
    int cnt = 0, g[21][21], n;
    while(cin >> n) {
        memset(g, INF, sizeof(g));
        for(int i = 1; i <= 19; ++i) {
            int a;
            for(int j = 0; j < n; ++j) {
                cin >> a;
                g[i][a] = g[a][i] = 1;
            }
            if(i < 19) cin >> n;
        }
        for(int k = 1; k <= 20; ++k) {
            for(int i = 1; i <= 20; ++i) {
                for(int j = 1; j <= 20; ++j) {
                    g[i][j] = min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }
        cout << "Test Set #" << ++cnt << "\n";
        cin >> n;
        for(int i = 0; i < n; ++i) {
            int a, b;
            cin >> a >> b;
            printf("%2d to %2d: %d\n", a, b, g[a][b]);
        }
        cout << "\n";
    }
    return 0;
}
```
## F - How Many Dependencies?
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
int n, a[105];
vector<int> g[105];
int dfs(int x) {
    if(a[x] != -1) return a[x];
    int t = 0;
    for(auto i : g[x])
        t = max(t, dfs(i));
    return a[x] = t + 1;
}
int main() {
    USE_CPPIO();
    int n;
    while (cin >> n && n) {
        for(int i = 0; i < 105; ++i) g[i].clear();
        memset(a, -1, sizeof(a));
        for(int i = 1; i <= n; ++i) {
            int t, b;
            cin >> t;
            while(t--) {
                cin >> b;
                g[i].push_back(b);
            }
        }
        int ans = 1, most = 0;
        for(int i = 1; i <= n; ++i) {
            int t = dfs(i);
            if(t > most) {
                most = t;
                ans = i;
            }
        }
        cout << ans << endl;
    }
    return 0;
}
```
## G - Longest path in a tree
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
int n, ans = 0;
vector<int> g[10005];
bool vis[10005];
int dfs(int x, int y) {
    int ans1 = 0, ans2 = 0;
    for(auto i : g[x]) {
        if(i == y) continue;
        int t = dfs(i, x);
        if(t > ans1) {
            ans2 = ans1;
            ans1 = t;
        }
        else if(t > ans2) {
            ans2 = t;
        }
    }
    if(ans1 + ans2 > ans) ans = ans1 + ans2;
    return ans1 + 1;
}
int main() {
    USE_CPPIO();
    cin >> n;
    for(int i = 1; i < n; ++i) {
        int u, v;
        cin >> u >> v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    dfs(1, 0);
    cout << ans << endl;
    return 0;
}
```
## H - Fill
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
struct node {
    int v[3], dist;
    bool operator<(const node &r) const {
        return dist > r.dist;
    }
};
int main() {
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        int cup[3], d, ans[205];
        bool vis[205][205];
        cin >> cup[0] >> cup[1] >> cup[2] >> d;
        memset(vis, false, sizeof(vis));
        memset(ans, -1, sizeof(ans));
        priority_queue<node> pq;
        pq.push({0, 0, cup[2], 0});
        vis[0][0] = true;
        while(!pq.empty()) {
            node now = pq.top();
            pq.pop();
            for(int i = 0; i < 3; ++i) {
                int t = now.v[i];
                if(ans[t] < 0 || now.dist < ans[t])
                    ans[t] = now.dist;
            }
            if(ans[d] > 0) break;
            for(int i = 0; i < 3; ++i) {
                for(int j = 0; j < 3; ++j) {
                    if(i != j) {
                        if(now.v[i] == 0 || now.v[j] == cup[j]) continue;
                        int amount = min(cup[j], now.v[i] + now.v[j]) - now.v[j];
                        node tmp = now;
                        tmp.dist = now.dist + amount;
                        tmp.v[i] -= amount;
                        tmp.v[j] += amount;
                        if(!vis[tmp.v[0]][tmp.v[1]]) {
                            vis[tmp.v[0]][tmp.v[1]] = true;
                            pq.push(tmp);
                        }
                    }
                }
            }
        }
        while(d >= 0) {
            if(ans[d] >= 0) {
                cout << ans[d] << " " << d << endl;
                break;
            }
            --d;
        }
    }
    return 0;
}
```
## I - Ordering Tasks
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
int main() {
    USE_CPPIO();
    int n, m;
    while(cin >> n >> m) {
        if(n == 0 && m == 0) break;
        int indeg[105] = {0}, cnt = 0;
        vector<int> g[101];
        for(int i = 0; i < m; ++i) {
            int u, v;
            cin >> u >> v;
            g[u].push_back(v);
            ++indeg[v];
        }
        for(int i = 1; i <= n; ++i) {
            if(indeg[i] == 0) {
                if(cnt < n) cout << i << " ";
                ++cnt;
                indeg[i] = -1;
                for(auto j : g[i]) --indeg[j];
            }
            if(cnt == n) break;
            else if(i == n) i = 0;
        }
        cout << endl;
    }
    return 0;
}
```
## J - Guess
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
int n, indeg[15], a[15];
string s;
vector<int> g[15];
void addEdge(int u, int v) {
    ++indeg[v];
    g[u].push_back(v);
}
int main() {
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        cin >> n >> s;
        memset(indeg, 0, sizeof(indeg));
        for(int i = 0; i <= n; ++i) g[i].clear();
        int pos = 0;
        for(int i = 0; i <= n; ++i) {
            for(int j = i + 1; j <= n; ++j) {
                if(s[pos] == '+') addEdge(i, j);
                else if(s[pos] == '-') addEdge(j, i);
                ++pos;
            }
        }
        int d = 0;
        queue<int> q;
        for(int i = 0; i <= n; ++i)
            if(indeg[i] == 0) q.push(i);
        while(!q.empty()) {
            int sz = q.size();
            for(int i = 0; i < sz; ++i) {
                int f = q.front();
                q.pop();
                a[f] = d;
                for(auto j : g[f]) {
                    --indeg[j];
                    if(indeg[j] == 0) q.push(j);
                }
            }
            ++d;
        }
        for(int i = 1; i <= n; ++i)
            cout << a[i] - a[i - 1] << " \n"[i == n];
    }
    return 0;
}
```
## K - John's trip
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
int g[50][2010], cnt[50], nRoads, nPoints, start;
bool vis[2010];
stack<int> st;
void init() {
    nRoads = nPoints = 0;
    while(!st.empty()) st.pop();
    memset(g, 0, sizeof(g));
    memset(cnt, 0, sizeof(cnt));
    memset(vis, false, sizeof(vis));
}
void euler(int u) {
    for(int v = 1; v <= nRoads; ++v) {
        if(!vis[v] && g[u][v]) {
            vis[v] = true;
            euler(g[u][v]);
            st.push(v);
        }
    }
}
void solve() {
    bool flag = false;
    for(int i = 1; i < 50; ++i) {
        if(cnt[i] % 2) {
            flag = true;
            break;
        }
    }
    if(flag) cout << "Round trip does not exist.\n";
    else {
        euler(start);
        bool ff = false;
        while(!st.empty()) {
            if(ff) cout << " ";
            cout << st.top();
            st.pop();
            ff = true;
        }
        cout << "\n";
    }
    cout << "\n";
}
int main() {
    USE_CPPIO();
    int x, y, z, t = 0;
    bool flag = false;
    init();
    while(cin >> x >> y) {
        ++t;
        if(x == 0 && y == 0) {
            if(flag) break;
            flag = true;
            t = 0;
            solve();
            init();
            continue;
        }
        flag = false;
        cin >> z;
        if(t == 1) start = min(x, y);
        nRoads = max(nRoads, z);
        nPoints = max(nPoints, max(x, y));
        g[x][z] = y;
        g[y][z] = x;
        ++cnt[x];
        ++cnt[y];
    }
    return 0;
}
```
## L - Catenyms
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
int n, dsu[26], cnt, tot, indeg[26], outdeg[26];
bool vis[26], used[26][1005];
vector<string> g[26], ans;
void init() {
    cnt = 1, tot = 0;
    memset(indeg, 0, sizeof(indeg));
    memset(outdeg, 0, sizeof(outdeg));
    memset(vis, false, sizeof(vis));
    memset(used, false, sizeof(used));
    for(int i = 0; i < 26; ++i) {
        g[i].clear();
        dsu[i] = i;
    }
    ans.clear();
}
int Find(int x) {
    if(dsu[x] == x) return x;
    return dsu[x] = Find(dsu[x]);
}
void euler(int u) {
    for(int i = 0; i < g[u].size(); ++i) {
        int v = g[u][i][g[u][i].length() - 1] - 'a';
        if(!used[u][i]) {
            used[u][i] = true;
            euler(v);
            ans.push_back(g[u][i]);
        }
    }
}
bool solve() {
    if(cnt != tot) return false;
    int t = INF, odd = 0, q;
    for(int i = 0; i < 26; ++i) {
        if(g[i].size()) t = min(t, i);
        if(outdeg[i] - indeg[i] == 1) {
            ++odd;
            q = i;
        }
        else if(indeg[i] - outdeg[i] == 1) ++odd;
        else if(indeg[i] != outdeg[i]) return false;
        if(odd > 2) return false;
    }
    if(!odd) euler(t);
    else euler(q);
    for(int i = ans.size() - 1; i > 0; --i) cout << ans[i] << ".";
    cout << ans[0] << endl;
    return true;
}
int main() {
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        init();
        cin >> n;
        for(int i = 0; i < n; ++i) {
            string s;
            cin >> s;
            int u = s[0] - 'a', v = s[s.length() - 1] - 'a';
            if(!vis[u]) {
                vis[u] = true;
                ++tot;
            }
            if(!vis[v]) {
                vis[v] = true;
                ++tot;
            }
            ++indeg[v];
            ++outdeg[u];
            g[u].push_back(s);
            int x = Find(u), y = Find(v);
            if(x != y) {
                dsu[x] = y;
                ++cnt;
            }
        }
        for(int i = 0; i < 26; ++i) sort(g[i].begin(), g[i].end());
        if(!solve()) cout << "***\n";
    }
    return 0;
}
```