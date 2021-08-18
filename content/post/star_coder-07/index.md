---
slug: "star_coder-07"
title: "StarCoder2021暑訓：Week06"
date: 2021-08-18T16:02:47+08:00
tags:
    - UVA
categories:
    - StarCoder
---
# 主題
- 最短路徑
## 最短路徑模板
### Dijkstra’s
- 不能有負邊
```cpp
int vn; // vertex num
struct Edge {
    int idx, w;
    bool operator < (const Edge &r) const {
        return w > r.w;
    }
};
vector<Edge> adj[maxv];
void dijkstra(int s) {
    vector<bool> vis(vn, false);
    vector<int> dist(vn, INF);
    dist[s] = 0;
    priority_queue<Edge> pq;
    pq.emplace(0, s);
    while(!pq.empty()) {
        int u = pq.top().idx;
        pq.pop();
        if(vis[u]) continue;
        vis[u] = true;
        for(auto v : adj[u]) {
            if(dist[v.idx] > dist[u] + v.w) {
                dist[v.idx] = dist[u] + v.w;
                pq.emplace(dist[v.idx], v.idx);
            }
        }
    }
}
```

### Bellman-Ford
- 可以檢查是否有環
```cpp
int vn; // vertex num
struct Edge {
    int idx, w;
};
vector<Edge> adj[maxv];
bool bellmanford(int s) { //return true if negative cycle exists;
    int cnt = 0;
    bool update = false;
    vector<int> dist(vn, INF);
    dist[s] = 0;
    do {
        if(++cnt == vn) return true;
        update = false;
        for(int u = 0; u < vn; ++u) {
            if(dist[u] == INF) continue;
            for(auto v : adj[u]) {
                if(dist[v.idx] > dist[u] + v.w) {
                    update = true;
                    dist[v.idx] = dist[u] + v.w;
                }
            }
        }
    } while(update);
    return false;
}
```

### Floyd-Warshall
```cpp
int dist[MAX_V][MAX_V];
for(int k = 0; k < vn; ++k)
    for(int i = 0; i < vn; ++i)
        for(int j = 0; j < vn; ++j)
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
```

# 題目
- [Virtual Judge](https://vjudge.net/contest/453684)
- 題目列表與提示
    |題目|題目需求|難度|
    |--|--|--|
    |UVa 929|給一個二維迷宮，求左上走到右下的最少成本（最短路徑）。|單源模板題|
    |UVa 10986|給一個無向帶權圖，求指定兩點間的最短路徑長。|單源模板題|
    |UVa 10000|給一個有向無環圖，求指定兩點間的最長路徑。|多種作法|
    |UVa 558|給定一個有向帶權圖（黑洞通道為邊），問是否存在負環（可回到無限遠的過去）。|BF|
    |UVa 10801|給定電梯系統與移動時間，若轉換電梯需 60 秒，問去某樓的最短時間。|單源小變化|
    |UVa 821|給一個無向無權圖，求所有兩點間最短路徑長的平均值。|FW 模板題|
    |UVa 10048|給一個無向帶權圖，求指定兩點間路徑上最大邊權的最小值。|FW 變化|
    |UVa 658|給一堆修補程式的安裝前後狀態，問最少要安裝多少次修補程式可以修好所有 bug。|變化題|
    |UVa 11374|給經濟線和商業線兩套捷運路線圖的行駛時間，在商業線只能搭一段的限制下，求指定兩點間的最快路線。|變化題|
    |UVa 10917|題目不長，自己讀讀看。|變化題|

# 參考作法
## A - Number Maze
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using tiii = tuple<int, int, int>;
int d[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        int n, m;
        cin >> n >> m;
        vector<vector<int>> g(n, vector<int>(m, 0));
        vector<vector<int>> val(n, vector<int>(m, -1));
        vector<vector<bool>> vis(n, vector<bool>(m, false));
        priority_queue<tiii, vector<tiii>, greater<tiii>> pq;
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < m; ++j)
                cin >> g[i][j];
        val[0][0] = g[0][0];
        pq.push(make_tuple(g[0][0], 0, 0));
        while(!pq.empty()) {
            int v, x, y;
            tie(v, x, y) = pq.top();
            pq.pop();
            vis[x][y] = true;
            for(int i = 0; i < 4; ++i) {
                int nx = x + d[i][0], ny = y + d[i][1];
                if(nx >= 0 && nx < n && ny >= 0 && ny < m && !vis[nx][ny]) {
                    int tmp = v + g[nx][ny];
                    if(val[nx][ny] == -1 || val[nx][ny] > tmp) {
                        val[nx][ny] = tmp;
                        pq.push(make_tuple(tmp, nx, ny));
                    }
                }
            }
        }
        cout << val[n - 1][m - 1] << endl;
    }
    return 0;
}
```
## B - Sending email
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using pii = pair<int, int>;
using plli = pair<long long, int>;
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int T;
    cin >> T;
    for(int ca = 1; ca <= T; ++ca) {
        cout << "Case #" << ca << ": ";
        int n, m, s, t;
        cin >> n >> m >> s >> t;
        vector<pii> g[n];
        vector<long long> val(n, INF);
        vector<bool> vis(n, false);
        for(int i = 0; i < m; ++i) {
            int a, b, c;
            cin >> a >> b >> c;
            g[a].push_back({b, c});
            g[b].push_back({a, c});
        }
        priority_queue<plli, vector<plli>, greater<plli>> pq;
        val[s] = 0;
        pq.push({0, s});
        while(!pq.empty()) {
            int v = pq.top().first, x = pq.top().second;
            pq.pop();
            if(val[x] < v) continue;
            for(auto it : g[x]) {
                if(val[it.first] > val[x] + it.second) {
                    val[it.first] = val[x] + it.second;
                    pq.push({val[it.first], it.first});
                }
            }
        }
        if(val[t] == INF) cout << "unreachable\n";
        else cout << val[t] << endl;
    }
    return 0;
}
```
## C - Longest Paths
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
struct edge {
    int u, v;
};
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int n, cnt = 0;
    while(cin >> n && n) {
        int s;
        cin >> s;
        int p, q;
        vector<edge> g;
        vector<int> val(n + 1, INF);
        while(cin >> p >> q && !(p == 0 && q == 0))
            g.push_back({p, q});
        val[s] = 0;
        for(int i = 0; i < n; ++i) {
            for(auto it : g) {
                if(val[it.v] > val[it.u] - 1 && val[it.u] != INF)
                    val[it.v] = val[it.u] - 1;
            }
        }
        int en, ans = 0;
        for(int i = 1; i <= n; ++i) {
            if(val[i] < ans) {
                ans = val[i];
                en = i;
            }
        }
        cout << "Case " << ++cnt << ": The longest path from " << s << " has length " << -1 * ans << ", finishing at " << en << ".\n\n";
    }
    return 0;
}
```
## D - Wormholes
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int n, m;
struct edge {
    int u, v, w;
};
bool Bellman_Ford(vector<edge> &ve, vector<int> val) {
    for(int i = 0; i < n; ++i) {
        bool flag = false;
        for(int j = 0; j < m; ++j) {
            if(val[ve[j].v] > val[ve[j].u] + ve[j].w && val[ve[j].u] != INF) {
                val[ve[j].v] = val[ve[j].u] + ve[j].w;
                flag = true;
            }
        }
        if(!flag) return true;
        if(i == n - 1 && flag) return false;
    }
    return false;
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
        cin >> n >> m;
        vector<edge> ve;
        vector<int> val(n, INF);
        for(int i = 0; i < m; ++i) {
            int a, b, c;
            cin >> a >> b >> c;
            ve.push_back({a, b, c});
        }
        val[0] = 0;
        if(Bellman_Ford(ve, val)) cout << "not possible\n";
        else cout << "possible\n";
    }
    return 0;
}
```
## E - Lift Hopping
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
    int n, tar;
    while(cin >> n >> tar) {
        vector<int> vt(n), va;
        vector<vector<int>> w(100, vector<int>(100, INF));
        for(int i = 0; i < n; ++i) cin >> vt[i];
        cin.ignore(100, '\n');
        for(int i = 0; i < n; ++i) {
            va.clear();
            string s;
            getline(cin, s);
            stringstream ss(s);
            int tmp;
            while(ss >> tmp) va.push_back(tmp);
            for(int j = 0; j < va.size(); ++j) {
                for(int k = j + 1; k < va.size(); ++k) {
                    int dis = abs(va[j] - va[k]) * vt[i];
                    if(dis < w[va[j]][va[k]])
                        w[va[j]][va[k]] = w[va[k]][va[j]] = dis;
                }
            }
        }
        vector<bool> vis(n, false);
        vector<int> d(100, INF);
        d[0] = 0;
        for(int i = 0; i < 100; ++i) {
            int x, m = INF;
            for(int y = 0; y < 100; ++y) {
                if(!vis[y] && d[y] < m) {
                    m = d[y];
                    x = y;
                }
            }
            vis[x] = true;
            for(int y = 0; y < 100; ++y) {
                if(d[y] > d[x] + w[x][y] + 60)
                    d[y] = d[x] + w[x][y] + 60;
            }
        }
        if(d[tar] == INF) cout << "IMPOSSIBLE\n";
        else {
            if(tar == 0) cout << "0\n";
            else cout << d[tar] - 60 << endl;
        }
    }
    return 0;
}
```
## F - Page Hopping
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
map<int, int> m;
void memNum(int n) {
    if(m.find(n) == m.end())
        m[n] = m.size();
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int ca = 1, x, y;
    while(cin >> x >> y && !(x == 0 && y == 0)) {
        m.clear();
        vector<vector<int>> g(110, vector<int>(110, INF));
        for(int i = 0; i < 110; ++i) g[i][i] = 0;
        memNum(x);
        memNum(y);
        g[m[x]][m[y]] = 1;
        while(cin >> x >> y && !(x == 0 && y == 0)) {
            memNum(x);
            memNum(y);
            g[m[x]][m[y]] = 1;
        }
        int cnt = 0, sz = m.size();
        for(int k = 0; k < sz; ++k) {
            for(int i = 0; i < sz; ++i) {
                for(int j = 0; j < sz; ++j) {
                    g[i][j] = min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }
        for(int i = 0; i < sz; ++i) {
            for(int j = 0; j < sz; ++j) {
                if(g[i][j] != INF) cnt += g[i][j];
            }
        }
        cout << "Case " << ca++ << ": average length between pages = ";
        cout << fixed << setprecision(3) << (double)cnt / (sz * (sz - 1)) << " clicks\n";
    }
    return 0;
}
```
## G - Audiophobia
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
    int c, s, q, ca = 1;
    while(cin >> c >> s >> q && !(c == 0 && s == 0 && q == 0)) {
        vector<vector<int>> path(1005, vector<int>(1005, INF));
        int c1, c2, d;
        for(int i = 0; i < s; ++i) {
            cin >> c1 >> c2 >> d;
            path[c1][c2] = path[c2][c1] = d;
        }
        for(int k = 1; k <= c; ++k) {
            for(int i = 1; i <= c; ++i) {
                for(int j = 1; j <= c; ++j) {
                    path[i][j] = min(path[i][j], max(path[i][k], path[k][j]));
                }
            }
        }
        if(ca > 1) cout << endl;
        cout << "Case #" << ca++ << endl;
        for(int i = 0; i < q; ++i) {
            cin >> c1 >> c2;
            if(path[c1][c2] != INF) cout << path[c1][c2] << endl;
            else cout << "no path\n";
        }
    }
    return 0;
}
```
## H - It's not a Bug, it's a Feature!
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
#define maxn (1<<20) + 10
#define maxm 110
int n, m, minTime;
bool vis[maxn];
struct node {
    int time;
    int a1, a2, b1, b2;
} a[maxm];
struct pqNode {
    int u, d;
    bool operator < (const pqNode &r) const {
        return d > r.d;
    }
};
bool dijkstra() {
    priority_queue<pqNode> pq;
    pq.push({(1 << n) - 1, 0});
    while(!pq.empty()) {
        pqNode x = pq.top();
        pq.pop();
        if(x.u == 0) {
            minTime = x.d;
            return true;
        }
        if(vis[x.u]) continue;
        vis[x.u] = true;
        for(int i = 0; i < m; ++i) {
            if((x.u & a[i].a1) == a[i].a1 && (~x.u & a[i].a2) == a[i].a2) {
                int t = x.u | a[i].b1;
                t = t & ~a[i].b2;
                pq.push({t, x.d + a[i].time});
            }
        }
    }
    return false;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    USE_CPPIO();
    int ca = 1;
    while(cin >> n >> m && !(n == 0 && m == 0)) {
        cout << "Product " << ca++ << endl;
        memset(vis, false, sizeof(vis));
        for(int i = 0; i <= m; ++i)
            a[i].time = a[i].a1 = a[i].a2 = a[i].b1 = a[i].b2 = 0;
        for(int i = 0; i < m; ++i) {
            int t;
            string s1, s2;
            cin >> t >> s1 >> s2;
            for(int j = 0; j < n; ++j) {
                a[i].time = t;
                if(s1[n - 1 - j] == '+') a[i].a1 = (a[i].a1 | (1 << j));
                if(s1[n - 1 - j] == '-') a[i].a2 = (a[i].a2 | (1 << j));
                if(s2[n - 1 - j] == '+') a[i].b1 = (a[i].b1 | (1 << j));
                if(s2[n - 1 - j] == '-') a[i].b2 = (a[i].b2 | (1 << j));
            }
        }
        if(dijkstra()) cout << "Fastest sequence takes " << minTime << " seconds.\n";
        else cout << "Bugs cannot be fixed.\n";
        cout << endl;
    }
    return 0;
}
```
## I - Airport Express
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
#define maxn 505
struct edge {
    int u, v, dist;
};
struct node {
    int d, u;
    bool operator < (const node &r) const {
        return d > r.d;
    }
};
vector<edge> ve;
vector<int> g[maxn], path1[maxn], path2[maxn];
bool done[maxn];
int d[maxn], p[maxn], d1[maxn], d2[maxn];
int ca = 0, n, s, e, m, k;
void dijkstra(int s, int *dist, vector<int> *paths) {
    priority_queue<node> pq;
    for(int i = 0; i < n; ++i) d[i] = INF;
    d[s] = 0;
    memset(done, 0, sizeof(done));
    pq.push({0, s});
    while(!pq.empty()) {
        node x = pq.top();
        pq.pop();
        if(done[x.u]) continue;
        done[x.u] = true;
        for(int i = 0; i < g[x.u].size(); ++i) {
            edge &e = ve[g[x.u][i]];
            if(d[e.v] > d[x.u] + e.dist) {
                d[e.v] = d[x.u] + e.dist;
                p[e.v] = g[x.u][i];
                pq.push({d[e.v], e.v});
            }
        }
    }
    for(int i = 0; i < n; ++i) {
        dist[i] = d[i];
        paths[i].clear();
        int t = i;
        paths[i].push_back(t);
        while(t != s) {
            paths[i].push_back(ve[p[t]].u);
            t = ve[p[t]].u;
        }
        reverse(paths[i].begin(), paths[i].end());
    }
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    USE_CPPIO();
    while(cin >> n >> s >> e >> m) {
        for(int i = 0; i < n; ++i) g[i].clear();
        ve.clear();
        --s;
        --e;
        for(int i = 0; i < m; ++i) {
            int a, b, c;
            cin >> a >> b >> c;
            --a;
            --b;
            ve.push_back({a, b, c});
            g[a].push_back(ve.size()-1);
            ve.push_back({b, a, c});
            g[b].push_back(ve.size()-1);
        }
        dijkstra(s, d1, path1);
        dijkstra(e, d2, path2);
        int ans = d1[e];
        vector<int> pp = path1[e];
        int midpoint = -1;
        cin >> k;
        for(int i = 0; i < k; ++i) {
            int a, b, c;
            cin >> a >> b >> c;
            --a;
            --b;
            for(int j = 0; j < 2; ++j) {
                if(d1[a] + d2[b] + c < ans) {
                    ans = d1[a] + d2[b] + c;
                    pp = path1[a];
                    for(int p = path2[b].size() - 1; p >= 0; --p)
                        pp.push_back(path2[b][p]);
                    midpoint = a;
                }
                swap(a, b);
            }
        }
        if(ca++ != 0) cout << endl;
        for(int i = 0; i < pp.size() - 1; ++i) cout << pp[i] + 1 << " ";
        cout << e + 1 << endl;
        if(midpoint == -1) cout << "Ticket Not Used\n";
        else cout << midpoint + 1 << endl;
        cout << ans << endl;
    }
    return 0;
}
```
## J - Walk Through the Forest
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
vector<pair<int, int>> link[1001];
vector<int> rebuild[1001];
int distant[1001];
int dp[1001];
bool vis[1001];
int n, m;
struct node {
    int dis, index;
    bool operator < (const node &r) const {
        return dis > r.dis;
    }
};
int dfs(int now) {
    if(now == 1) return 1;
    if(dp[now] == -1) {
        dp[now] = 0;
        for(auto i : rebuild[now])
            dp[now] += dfs(i);
    }
    return dp[now];
}
void dijkstra() {
    priority_queue<node> pq;
    pq.push({0, 1});
    int cur;
    distant[1] = 0;
    while(!pq.empty()) {
        cur = pq.top().index;
        pq.pop();
        if(vis[cur]) continue;
        vis[cur] = 1;
        for(auto i : link[cur]) {
            int next = i.first;
            int dis = i.second;
            if(distant[next] > distant[cur] + dis) {
                distant[next] = distant[cur] + dis;
                pq.push({distant[next], next});
            }
        }
    }
}
void build() {
    memset(dp, -1, sizeof(dp));
    int next, dis;
    for(int i = 0; i < n; ++i) {
        rebuild[i].clear();
        for(auto j : link[i])
            if(distant[i] > distant[j.first])
                rebuild[i].emplace_back(j.first);
    }
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    USE_CPPIO();
    while(cin >> n >> m && n) {
        memset(link, 0, sizeof(link));
        memset(vis, 0, sizeof(vis));
        for(int i = 0; i < n; ++i)
            distant[i] = INF;
        while(m--) {
            int a, b, c;
            cin >> a >> b >> c;
            link[--a].push_back({--b, c});
            link[b].push_back({a, c});
        }
        dijkstra();
        build();
        cout << dfs(0) << '\n';
    }
    return 0;
}
```