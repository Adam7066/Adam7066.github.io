---
slug: "star_coder-03"
title: "StarCoder2021暑訓：Week02"
date: 2021-07-24T08:26:57+08:00
tags:
    - UVA
categories:
    - StarCoder
---
# 主題
- STL、併查集
## 併查集模板
```cpp
int dsu[MAX_N];
void init(int num) {
    for(int i = 0; i <= num; ++i)
        dsu[i] = i;
}
int Find(int x) {
    if(x == dsu[x]) return x;
    return dsu[x] = Find(dsu[x]);
}
void Union(int x, int y) {
    int a = Find(x), b = Find(y);
    if(a != b) dsu[a] = b;
}
```

# 題目
- [Virtual Judge](https://vjudge.net/contest/447620)
- 題目列表與提示
    |題目|題目需求|採用結構|優先練習|
    |--|--|--|:--:|
    |UVa 673|括號匹配與 LIFO 操作|std::stack|v|
    |UVa 442|括號匹配與 LIFO 操作|std::stack||
    |UVa 12100|遍歷和 FIFO 操作|std::queue (加上 std::priority_queue 效率更高)|v|
    |UVa 245|取出第 n 個以及插入頭端|std::list / std::deque / std::vector||
    |UVa 1203|插入與取出最小值|std::priority_queue|v|
    |UVa 11995|模擬 stack, queue, priority_queue|std::stack, std::queue, std::priority_queue|v|
    |UVa 10583|標準併查集操作|disjoint set|v|
    |UVa 11987|併查集的變化題(值得思考)|disjoint set|| 
    |UVa 1665|判斷連通塊數|disjoint set|| 
    |UVa 230|字串排序與搜尋|std::map / std::set|v|
    |UVa 1592|字串比較（將字串轉成數值以加快比較）|std::map||

# 參考作法
## A - Parentheses Balance
```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin >> n;
    cin.ignore(100, '\n');
    while(n--) {
        string s;
        getline(cin, s);
        stack<char> st;
        for(auto &i : s) {
            if(i == ']') {
                if(!st.empty() && st.top() == '[') st.pop();
                else st.emplace(i);
            }
            else if(i == ')') {
                if(!st.empty() && st.top() == '(') st.pop();
                else st.emplace(i);
            }
            else st.emplace(i);
        }
        cout << (st.empty() ? "Yes" : "No") << '\n';
    }
    return 0;
}
```

## B - Matrix Chain Multiplication
```cpp
#include <bits/stdc++.h>
using namespace std;
struct Matrix {
    int row, col;
};
int main() {
    int n, errCnt = -1;
    cin >> n;
    map<char, Matrix> ma;
    for(int i = 0; i < n; ++i) {
        char name;
        Matrix m;
        cin >> name >> m.row >> m.col;
        ma[name] = m;
    }
    string s;
    while(cin >> s) {
        int multiCnt = 0;
        stack<Matrix> st;
        for(int i = 0; i < s.length(); ++i) {
            if(s[i] == '(') continue;
            if(s[i] != ')') {
                st.push(ma[s[i]]);
                continue;
            }
            Matrix b = st.top();
            st.pop();
            Matrix a = st.top();
            st.pop();
            if(a.col != b.row) {
                multiCnt = errCnt;
                break;
            }
            multiCnt += a.row * b.row * b.col;
            st.push({a.row, b.col});
        }
        if(multiCnt == errCnt) cout << "error\n";
        else cout << multiCnt << endl;
    }
    return 0;
}
```

## C - Printer Queue
```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int T;
    cin >> T;
    while(T--) {
        int n, m;
        cin >> n >> m;
        deque<pair<int, int>> dq;
        int t[10] = {0};
        for(int i = 0; i < n; ++i) {
            int a;
            cin >> a;
            dq.emplace_back(a, i);
            ++t[a];
        }
        int cnt = 0;
        int lev, idx;
        while(true) {
            tie(lev, idx) = dq.front();
            int canPop = 1;
            for(int i = lev + 1; i < 10; ++i) canPop &= (t[i] == 0);
            if(canPop) {
                ++cnt;
                --t[lev];
                if(idx == m) break;
                dq.pop_front();
            }
            else {
                dq.push_back(dq.front());
                dq.pop_front();
            }
        }
        cout << cnt << '\n';
    }
    return 0;
}
```

## D - Uncompress
```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    vector<string> vs;
    string buf;
    while (getline(cin, buf) && buf[0] != '0') {
        string s;
        for (int i = 0; buf[i]; ++ i) {
            if (isalpha(buf[i])) {
                s = "";
                while (isalpha(buf[i]))
                    s.insert(s.end(), buf[i ++]);
                vs.push_back(s);
                --i;
                cout << s;
            }
            else if (isdigit(buf[i])) {
                int value = 0;
                while (isdigit(buf[i]))
                    value = value * 10 + buf[i ++] - '0';
                s = vs[vs.size() - value];
                vs.erase(vs.end() - value);
                vs.push_back(s);
                --i;
                cout << s;
            }
            else printf("%c", buf[i]);
        }
        puts("");
    }
    return 0;
}
```

## E - Argus
```cpp
#include <bits/stdc++.h>
using namespace std;
using tiii = tuple<int, int, int>; // time, query_num, period
int main() {
    priority_queue<tiii, vector<tiii>, greater<tiii>> pq;
    string s;
    int q, p, cur, n;
    while(cin >> s && s != "#" && cin >> q >> p)
        pq.push(make_tuple(p, q, p));
    cin >> n;
    while(n--) {
        tie(cur, q, p) = pq.top();
        pq.pop();
        cout << q << '\n';
        pq.push(make_tuple(cur + p, q, p));
    }
    return 0;
}
```

## F - I Can Guess the Data Structure!
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
using namespace std;
int main() {
    USE_CPPIO();
    int n;
    while(cin >> n) {
        bool flag[3];
        memset(flag, true, sizeof(flag));
        stack<int> st;
        queue<int> qu;
        priority_queue<int> pq;
        int a, b;
        for(int i = 0; i < n; ++i) {
            cin >> a >> b;
            if(a == 1) {
                st.push(b);
                qu.push(b);
                pq.push(b);
            }
            else if(a == 2) {
                if(flag[0]) {
                    if(!st.empty() && st.top() == b) st.pop();
                    else flag[0] = false;
                }
                if(flag[1]) {
                    if(!qu.empty() && qu.front() == b) qu.pop();
                    else flag[1] = false;
                }
                if(flag[2]) {
                    if(!pq.empty() && pq.top() == b) pq.pop();
                    else flag[2] = false;
                }
            }
        }
        if((flag[0] && flag[1])||(flag[1] && flag[2])||(flag[2] && flag[0]))
            cout<<"not sure\n";
        else if(flag[0]) cout<<"stack\n";
        else if(flag[1]) cout<<"queue\n";
        else if(flag[2]) cout<<"priority queue\n";
        else cout<<"impossible\n";
    }
    return 0;
}
```

## G - Ubiquitous Religions
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
using namespace std;
int dsu[50005];
void init(int n) {
    for(int i = 0; i < n; ++i)
        dsu[i] = i;
}
int Find(int x) {
    if(x == dsu[x]) return x;
    return dsu[x] = Find(dsu[x]);
}
bool Union(int x, int y) {
    int a = Find(x);
    int b = Find(y);
    if(a == b) return false;
    dsu[a] = b;
    return true;
}
int main() {
    USE_CPPIO();
    int n, m, Case = 1;
    while(cin >> n >> m && n && m) {
        init(n);
        int cnt = n;
        for(int i = 0; i < m; ++i) {
            int a, b;
            cin >> a >> b;
            if(Union(a - 1, b - 1)) --cnt;
        }
        cout << "Case " << Case++ << ": " << cnt << endl;
    }
    return 0;
}
```

## H - Almost Union-Find
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
using namespace std;
int dsu[200005], num[200005], sum[200005];
void init(int n) {
    for(int i = 0; i <= n; ++i) {
        dsu[i] = dsu[i + n] = i + n;
        sum[i] = sum[i + n] = i;
        num[i] = num[i + n] = 1;
    }
}
int Find(int x) {
    if(x == dsu[x]) return x;
    return dsu[x] = Find(dsu[x]);
}
int main() {
    USE_CPPIO();
    int n, m;
    while(cin >> n >> m) {
        init(n);
        for(int i = 0; i < m; ++i) {
            int t, a, b;
            cin >> t >> a;
            if(t == 1) {
                cin >> b;
                int x = Find(a);
                int y = Find(b);
                if(x == y) continue;
                dsu[x] = y;
                num[y] += num[x];
                sum[y] += sum[x];
            }
            else if(t == 2) {
                cin >> b;
                int x = Find(a);
                int y = Find(b);
                if(x == y) continue;
                dsu[a] = y;
                --num[x];
                ++num[y];
                sum[x] -= a;
                sum[y] += a;
            }
            else if(t == 3) {
                int x = Find(a);
                cout << num[x] << " " << sum[x] << "\n";
            }
        }
    }
    return 0;
}
```

## I - Islands
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
using namespace std;
int n, m, total, dsu[1000010];
int dx[4] = {0, 0, 1, -1};
int dy[4] = {1, -1, 0, 0};
struct Point {
    int x, y, id, height;
};
vector<Point> vp;
bool cmp(Point a, Point b) {
    return a.height > b.height;
}
int Find(int t) {
    if(t == -1) return -1;
    if(dsu[t] == t) return t;
    return dsu[t] = Find(dsu[t]);
}
void add(int idx) {
    int id = vp[idx].id;
    dsu[id] = id;
    ++total;
    for(int i = 0; i < 4; ++i) {
        int x = vp[idx].x + dx[i];
        int y = vp[idx].y + dy[i];
        if(x >= 0 && x < n && y >= 0 && y < m) {
            int id2 = x * m + y;
            int root_id2 = Find(id2);
            if(root_id2 == -1) continue;
            if(root_id2 != id) {
                --total;
                dsu[root_id2] = id;
            }
        }
    }
}
int main() {
    USE_CPPIO();
    int T;
    cin >> T;
    while(T--) {
        cin >> n >> m;
        vp.clear();
        vector<int> h;
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < m; ++j) {
                int t;
                cin >> t;
                Point p = {i, j, i * m + j, t};
                vp.push_back(p);
                dsu[i * m + j] = -1;
            }
        }
        sort(vp.begin(), vp.end(), cmp);
        int q;
        cin >> q;
        for(int i = 0; i < q; ++i) {
            int t;
            cin >> t;
            h.push_back(t);
        }
        int idx = 0;
        total = 0;
        for(int i = h.size() - 1; i >= 0; --i) {
            while(idx < vp.size() && h[i] < vp[idx].height)
                add(idx++);
            h[i] = total;
        }
        for(auto i : h) cout << i << " ";
        cout << endl;
    }
    return 0;
}
```

## J - Borrowers
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
using namespace std;
map<string, string> mss;
struct ReturnBook {
    string title;
    string author;
};
bool cmp(string b1, string b2) {
    string a1 = mss[b1];
    string a2 = mss[b2];
    if(a1 != a2) return a1 < a2;
    return b1 < b2;
}
bool cmp_return(ReturnBook b1, ReturnBook b2) {
    if(b1.author != b2.author) return b1.author < b2.author;
    return b1.title < b2.title;
}
int main() {
    USE_CPPIO();
    string s;
    vector<string> shelve;
    vector<ReturnBook> vRB;
    while(getline(cin, s)) {
        if(s == "END") break;
        int idx = s.find("\" by");
        string title = s.substr(0, idx + 1);
        string author = s.substr(idx + 4);
        mss[title] = author;
        shelve.push_back(title);
    }
    sort(shelve.begin(), shelve.end(), cmp);
    while(getline(cin, s)) {
        if(s == "END") break;
        if(s == "SHELVE") {
            sort(vRB.begin(), vRB.end(), cmp_return);
            for(int i = 0; i < vRB.size(); ++i) {
                auto it = lower_bound(shelve.begin(), shelve.end(), vRB[i].title, cmp);
                if(it == shelve.begin()) cout << "Put " << vRB[i].title << " first" << endl;
                else {
                    int t = it - shelve.begin() - 1;
                    cout << "Put " << vRB[i].title << " after " << shelve[t] << endl;
                }
                shelve.insert(it, vRB[i].title);
            }
            vRB.clear();
            cout << "END\n";
        }
        else {
            int idx = s.find("\"");
            string title = s.substr(idx);
            if(s[0] == 'B') {
                auto it = lower_bound(shelve.begin(), shelve.end(), title, cmp);
                shelve.erase(it);
            }
            else vRB.push_back({title, mss[title]});
        }
    }
    return 0;
}
```

## K - Database
```cpp
#include <bits/stdc++.h>
#define USE_CPPIO() ios_base::sync_with_stdio(0); cin.tie(0)
using namespace std;
map<string, int> ID;
int idx = 1;
int StrID(string str) {
    if(ID[str]) return ID[str];
    return ID[str] = idx++;
}
int main() {
    USE_CPPIO();
    int row, col;
    while(cin >> row >> col) {
        cin.get();
        int IDtable[row + 2][col + 2];
        string table[row + 2][col + 2], tmp;
        for(int i = 1; i <= row; ++i) {
            getline(cin, tmp);
            char *pch = strtok((char*)tmp.c_str(), ",");
            int j = 1;
            while(pch) {
                table[i][j++] = pch;
                pch = strtok(NULL, ",");
            }
        }
        for(int i = 1; i <= row; ++i) {
            for(int j = 1; j <= col; ++j) {
                IDtable[i][j] = StrID(table[i][j]);
            }
        }
        bool flag = true;
        for(int i = 1; i <= col && flag; ++i) {
            for(int j = i + 1; j <= col && flag; ++j) {
                map<int, int> mii;
                for(int k = 1; k <= row && flag; ++k) {
                    int idx = IDtable[k][i] * 100000 + IDtable[k][j];
                    if(mii.count(idx)) {
                        cout << "NO" << endl;
                        cout << mii[idx] << " " << k << endl;
                        cout << i << " " << j << endl;
                        flag = false;
                    }
                    else mii[idx] = k;
                }
            }
        }
        if(flag) cout << "YES" << endl;
    }
    return 0;
}
```