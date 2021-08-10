---
slug: "star_coder-06"
title: "StarCoder2021暑訓：Week05"
date: 2021-08-10T23:50:17+08:00
tags:
    - UVA
    - UVL
categories:
    - StarCoder
---
# 主題
- 最小生成樹
## MST 模板
### Kruskal's Algorithm
- 時間複雜度 O(ElogE)
```cpp
struct edge {
    int u, v, w;
    bool operator < (const edge &r) const {
        return w < r.w;
    }
};
int vn, en; // vertex num, edge num
vector<edge> ve;
vector<int> dsu;
int Find(int x) {
    if(x == dsu[x]) return x;
    return dsu[x] = x;
}
bool Union(int x, int y) {
    int a = Find(x), b = Find(y);
    if(a != b) {
        dsu[a] = b;
        return true;
    }
    return false;
}
int Kruskal() {
    for(int i = 0; i < vn; ++i) dsu.push_back(i);
    sort(ve.begin(), ve.end());
    int cnt = 0, ans = 0;
    for(int i = 0; i < en && cnt < vn; ++i) {
        if(Union(ve[i].u, ve[i].v)) {
            ans += ve[i].w;
            ++cnt;
        }
    }
    return ans;
}
```
### Prim's Algorithm
- 時間複雜度 O(VlogE)
```cpp
struct edge {
    int idx, w;
    bool operator < (const edge &r) const {
        return w > r.w;
    }
};
int prim() {
    int vn; // vertex num
    vector<edge> adj[vn];
    int ans = 0;
    priority_queue<edge> pq;
    vector<bool> vis(vn, false);
    vis[0] = true;
    for(auto v : adj[0]) pq.emplace(v);
    while(!pq.empty()) {
        edge mn = pq.top();
        pq.pop();
        if(vis[mn.idx]) continue;
        vis[mn.idx] = true;
        ans += mn.w;
        for(auto v : adj[mn.idx]) pq.emplace(v);
    }
}
```
# 題目
- [Virtual Judge](https://vjudge.net/contest/452051)
- 題目列表與提示
    |題目|題目需求|難度|
    |--|--|--|
    |UVa 10034|給定平面上各點座標，求最小生成樹的邊權總和。(裸)|模板題|
    |UVL 7001|讀懂題目的三項性質，便知道題意為求最小生成樹。|模板題|
    |UVa 1208|讀懂題目便知題意。|模板題|
    |UVa 10369|讀懂題目，自行思考和最小生成樹演算法的關係。|小變化|
    |UVL 6437|讀懂題目，自行思考該作什麼樣的小修改。|小變化|
    |UVa 534|讀懂題目，自行思考和最小生成樹演算法的關係。(最小瓶頸路)|經典變化|
    |Uva 11733|讀懂題目，自行思考和最小生成樹演算法的關係。|小變化|
    |Uva 1395|給定圖，對該圖所有生成樹計算最大邊權減最小邊權，輸出該差距之最小值。|小變化|
    |Uva 1151|給定圖以及 q 組已知購買成本的子網路，問連通所有點的最小成本。|小變化|

# 參考作法
## A - Freckles
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int n;
struct Point {
    int group;
    double x, y;
} p[105];
struct Line {
    int aIdx, bIdx;
    double len;
};
double pD(const Point &a, const Point &b) {
    double dx = a.x - b.x, dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}
bool cmpLen(const Line &a, const Line &b) {
    return a.len < b.len;
}
bool isMSTEnd() {
    for(int i = 0; i < n - 1; ++i)
        if(p[i].group != p[i + 1].group) return false;
    return true;
}
void Union(int a, int b) {
    int minG = min(a, b);
    for(int i = 0; i < n; ++i) {
        if(p[i].group == a || p[i].group == b)
            p[i].group = minG;
    }
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
        cin >> n;
        for(int i = 0; i < n; ++i) {
            p[i].group = i;
            cin >> p[i].x >> p[i].y;
        }
        vector<Line> lines;
        for(int i = 0; i < n; ++i) {
            for(int j = i + 1; j < n; ++j) {
                lines.push_back({i, j, pD(p[i], p[j])});
            }
        }
        sort(lines.begin(), lines.end(), cmpLen);
        double ans = 0.0;
        for(int i = 0; !isMSTEnd(); ++i) {
            if(p[lines[i].aIdx].group == p[lines[i].bIdx].group)
                continue;
            ans += lines[i].len;
            Union(p[lines[i].aIdx].group, p[lines[i].bIdx].group);
        }
        cout << fixed << setprecision(2) << ans << endl;
        if(T) cout << endl;
    }
    return 0;
}
```
## B - Bus Problem
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int n, m;
long long sumO, sumM;
vector<int> dsu;
struct edge {
    int s, e, v;
};
vector<edge> ve;
bool cmp(const edge &a, const edge &b) {
    return a.v < b.v;
}
int Find(int x) {
    if(dsu[x] == x) return x;
    return dsu[x] = Find(dsu[x]);
}
int merge(int x, int y, int t) {
    int a = Find(x), b = Find(y);
    if(a != b) {
        dsu[a] = b;
        sumM += ve[t].v;
    }
}
int main() {
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        cin >> n >> m;
        sumO = sumM = 0;
        dsu.clear();
        ve.clear();
        for(int i = 0; i < m; ++i) {
            int a, b, d;
            cin >> a >> b >> d;
            ve.push_back({a, b, d});
            sumO += d;
        }
        for(int i = 0; i < n; ++i) dsu.push_back(i);
        sort(ve.begin(), ve.end(), cmp);
        for(int i = 0; i < m; ++i) {
            merge(ve[i].s, ve[i].e, i);
        }
        cout << sumO - sumM << endl;
    }
    return 0;
}
```
## C - Oreon
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
int n;
struct edge {
    int u, v, w;
};
vector<edge> g;
vector<int> dsu;
bool cmp(const edge &a, const edge &b) {
    if(a.w == b.w){
        if(a.u == b.u) return a.v < b.v;
        else return a.u < b.u;
    }
    return a.w < b.w;
}
int Find(int x) {
    if(x == dsu[x]) return x;
    return dsu[x] = Find(dsu[x]);
}
void Union(int x, int y) {
    int a = Find(x), b = Find(y);
    if(a != b) dsu[a] = b;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    int T;
    cin >> T;
    for(int ca = 1; ca <= T; ++ca) {
        cout << "Case " << ca << ":\n";
        cin >> n;
        g.clear();
        cin.ignore(100, '\n');
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < n; ++j) {
                int t;
                char c;
                scanf("%d%c", &t, &c);
                if(i == j || t == 0) continue;
                g.push_back({i, j, t});
            }
        }
        sort(g.begin(), g.end(), cmp);
        dsu.clear();
        for(int i = 0; i < n; ++i)
            dsu.push_back(i);
        int sz = g.size();
        for(int i = 0; i < sz; ++i) {
            int a = Find(g[i].u), b = Find(g[i].v);
            if(a != b) {
                Union(a, b);
                cout << (char)('A' + g[i].u) << "-" << (char)('A' + g[i].v) << " " << g[i].w << endl;
            }
        }
    }
    return 0;
}
```
## D - Arctic Network
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
struct Point {
    int x, y;
};
vector<Point> vp;
struct edge {
    int u, v;
    double len;
};
vector<edge> ve;
vector<int> dsu;
bool cmpVE(const edge &a, const edge &b) {
    return a.len < b.len;
}
int Find(int x) {
    if(x == dsu[x]) return x;
    return dsu[x] = Find(dsu[x]);
}
bool Union(int x, int y) {
    int a = Find(x), b = Find(y);
    if(a == b)return false;
    dsu[a] = b;
    return true;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        int s, p;
        cin >> s >> p;
        vp.clear();
        ve.clear();
        dsu.clear();
        for(int i = 0; i < p; ++i) {
            int x, y;
            cin >> x >> y;
            vp.push_back({x, y});
            dsu.push_back(i);
        }
        for(int i = 0; i < p; ++i) {
            for(int j = i + 1; j < p; ++j) {
                int dx = vp[i].x - vp[j].x, dy = vp[i].y - vp[j].y;
                ve.push_back({i, j, sqrt(dx * dx + dy * dy)});
            }
        }
        sort(ve.begin(), ve.end(), cmpVE);
        vector<double> ans;
        for(int i = 0; ans.size() < p - 1; ++i) {
            if(!Union(ve[i].u, ve[i].v)) continue;
            ans.push_back(ve[i].len);
        }
        cout << fixed << setprecision(2) << ans[p - 1 - s] << endl;
    }
    return 0;
}
```
## E - Power Plant
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
struct node {
    int x, y, z;
} a[500010];
int fa[100010], n, m, k, ans, len = 0;
bool vis[100010];
int getf(int x) {
    if (x == fa[x]) return x;
    return fa[x] = getf(fa[x]);
}
bool cmp(node x, node y) {
    return x.z < y.z;
}
int main() {
    int T, ll = 0;;
    scanf("%d", &T);
    while (T--) {
        memset(vis, 0, sizeof(vis));
        ll++;
        len = ans = 0;
        int xx;
        scanf("%d%d%d", &n, &m, &k);
        for (int i = 1; i <= k; i++) scanf("%d", &xx), vis[xx] = 1;
        for (int i = 1; i <= m; i++) {
            ++len;
            scanf("%d%d%d", &a[len].x, &a[len].y, &a[len].z);
            len++;
            a[len].x = a[len - 1].y;
            a[len].y = a[len - 1].x;
            a[len].z = a[len - 1].z;
        }
        for (int i = 1; i <= len; i++) {
            if (vis[a[i].x]) a[i].x = n + 1;
            if (vis[a[i].y]) a[i].y = n + 1;
        }
        sort(a + 1, a + len + 1, cmp);
        for (int i = 1; i <= n + 1; i++) fa[i] = i;
        int num = 0;
        for (int i = 1; i <= len; i++) {
            int x = getf(a[i].x), y = getf(a[i].y);
            if (x != y) {
                fa[x] = y;
                ans += a[i].z;
                num++;
                if (num == n) break;
            }
        }
        printf("Case #%d: %d\n", ll, ans);
    }
    return 0;
}
```
## F - Frogger
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
struct node {
    int x, y;
};
vector<node> vn;
double dis[205][205];
int main() {
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    USE_CPPIO();
    int n, cnt = 1;
    while(cin >> n && n) {
        vn.clear();
        for(int i = 0; i < n; ++i) {
            int a, b;
            cin >> a >> b;
            vn.push_back({a, b});
        }
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < n; ++j) {
                int dx = vn[i].x - vn[j].x, dy = vn[i].y - vn[j].y;
                dis[i][j] = sqrt(dx * dx + dy * dy);
            }
        }
        for(int k = 0; k < n; ++k) {
            for(int i = 0; i < n; ++i) {
                for(int j = 0; j < n; ++j) {
                    dis[i][j] = min(dis[i][j], max(dis[i][k], dis[k][j]));
                }
            }
        }
        cout << "Scenario #" << cnt++ << endl;
        cout << "Frog Distance = " << fixed << setprecision(3) << dis[0][1] << endl << endl;
    }
    return 0;
}
```
## G - Airports
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
struct node {
    int u, v, w;
    bool operator < (const node &r) const {
        return w < r.w;
    }
};
vector<node> vn;
vector<int> dsu;
int Find(int x) {
    if(x == dsu[x]) return x;
    return dsu[x] = Find(dsu[x]);
}
bool Union(int x, int y) {
    int a = Find(x), b = Find(y);
    if(a != b) {
        dsu[a] = b;
        return true;
    }
    return false;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    USE_CPPIO();
    int T;
    cin >> T;
    for(int ca = 1; ca <= T; ++ca) {
        int n, m, cost;
        vn.clear();
        dsu.clear();
        cin >> n >> m >> cost;
        for(int i = 0; i <= n; ++i) dsu.push_back(i);
        for(int i = 0; i < m; ++i) {
            int a, b, c;
            cin >> a >> b >> c;
            vn.push_back({a, b, c});
        }
        sort(vn.begin(), vn.end());
        int sum = 0;
        for(int i = 0; i < m; ++i) {
            if(vn[i].w >= cost) continue;
            if(Union(vn[i].u, vn[i].v))
                sum += vn[i].w;
        }
        int cnt = 0;
        for(int i = 1; i <= n; ++i) {
            if(i == dsu[i]) ++cnt;
        }
        cout << "Case #" << ca << ": " << sum + cost*cnt << " " << cnt << endl;
    }
    return 0;
}
```
## H - Slim Span
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
struct node {
    int u, v, w;
    bool operator < (const node &r) const {
        return w < r.w;
    }
};
vector<node> vn;
vector<int> dsu;
int Find(int x) {
    if(x == dsu[x]) return x;
    return dsu[x] = Find(dsu[x]);
}
bool Union(int x, int y) {
    int a = Find(x), b = Find(y);
    if(a != b) {
        dsu[a] = b;
        return true;
    }
    return false;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    USE_CPPIO();
    int n, m;
    while(cin >> n >> m && (n || m)) {
        vn.clear();
        for(int i = 0; i < m; ++i) {
            int a, b, c;
            cin >> a >> b >> c;
            vn.push_back({a, b, c});
        }
        sort(vn.begin(), vn.end());
        int ans = INF;
        for(int i = 0; i < m; ++i) {
            dsu.clear();
            for(int i = 0; i <= n; ++i) dsu.push_back(i);
            int cnt = n - 1;
            for(int j = i; j < m; ++j) {
                if(Union(vn[j].u, vn[j].v)) --cnt;
                if(cnt == 0) {
                    ans = min(ans, vn[j].w - vn[i].w);
                    break;
                }
            }
        }
        if(ans == INF) cout << "-1\n";
        else cout << ans << endl;
    }
    return 0;
}
```
## I - Buy or Build
```cpp
#include <bits/stdc++.h>
using namespace std;
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
struct point {
    int x, y;
};
struct buy {
    int m, ci;
    vector<int> a;
};
struct node {
    int u, v, w;
    bool operator < (const node &r) const {
        return w < r.w;
    }
};
vector<point> vp;
vector<buy> vb;
vector<node> vn, vt;
vector<int> dsu;
int Find(int x) {
    if(x == dsu[x]) return x;
    return dsu[x] = Find(dsu[x]);
}
bool Union(int x, int y) {
    int a = Find(x), b = Find(y);
    if(a != b) {
        dsu[a] = b;
        return true;
    }
    return false;
}
int ksu(int t, vector<node> &vn, vector<node> &used) {
    if(t == 1) return 0;
    int m = vn.size(), ans = 0;
    used.clear();
    for(int i = 0; i < m; ++i) {
        if(Union(vn[i].u, vn[i].v)) {
            ans += vn[i].w;
            used.push_back(vn[i]);
            if(--t == 1) break;
        }
    }
    return ans;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        vp.clear();
        vb.clear();
        vn.clear();
        dsu.clear();
        int n, q;
        cin >> n >> q;
        for(int i = 0; i < q; ++i) {
            buy tmp;
            cin >> tmp.m >> tmp.ci;
            for(int j = 0; j < tmp.m; ++j) {
                int t;
                cin >> t;
                tmp.a.push_back(t - 1);
            }
            vb.push_back(tmp);
        }
        for(int i = 0; i < n; ++i) {
            int x, y;
            cin >> x >> y;
            vp.push_back({x, y});
        }
        for(int i = 0; i < n; ++i) {
            for(int j = i + 1; j < n; ++j) {
                int dx = vp[i].x - vp[j].x, dy = vp[i].y - vp[j].y;
                vn.push_back({i, j, dx * dx + dy * dy});
            }
        }
        sort(vn.begin(), vn.end());
        for(int i = 0; i < n; ++i) dsu.push_back(i);
        int ans = ksu(n, vn, vt);
        for(int mask = 0; mask < (1 << q); ++mask) {
            dsu.clear();
            for(int i = 0; i < n; ++i) dsu.push_back(i);
            int cnt = n, c = 0;
            for(int i = 0; i < q; ++i) {
                if(mask & (1 << i)) {
                    c += vb[i].ci;
                    for(int j = 1; j < vb[i].a.size(); j++) {
                        if(Union(vb[i].a[0], vb[i].a[j]))
                            --cnt;
                    }
                }
            }
            vector<node> vd;
            ans = min(ans, c + ksu(cnt, vt, vd));
        }
        cout << ans << endl;
        if(T) cout << endl;
    }
    return 0;
}
```