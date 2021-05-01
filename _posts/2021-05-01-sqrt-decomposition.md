---
layout: post
title: Brute-force Tree with SQRT Decomposition
---

[Codeforces Round #199 (Div. 2) E. Xenia and Tree](http://codeforces.com/contest/342/problem/E)

- Three ways to calculate:
    1. Every new red node, we do a BFS to the entire tree and update the distance of each node. The worse case time complexity is $$ O(mn) $$
    2. Every query, we do a BFS to find the nearest red node, also $$ O(mn) $$
    3. Every new red node, we push it into an array. Every query, we calculate distance to every red node and get the minimum. Distance can be calculated with LCA. This is $$ O(mnlog(n)) $$, even worse than 2 since LCA is $$ O(log(n)) $$

- However, we could combine method 1 and 3 with square-root decomposition.
    - Every $$\sqrt{m} $$ updates, we do a BFS with method 1 to update the dist array and clear the red node array. The time complexity is at most $$ O(\sqrt{m}n) $$
    - Every query, we find the minimum distance to the red nodes in the array with method 3, and return the minimum with distance array. The time complexity is at most $$ O(\sqrt{m}log(n)m) $$

```c++

#include <bits/stdc++.h>
using ll = long long;

using namespace std;

int n, m, h;
const int SZ = 100010;
int dep[SZ], dist[SZ];
vector<int> adj[SZ];
int p[SZ][18];

void dfs(int idx, int parent, int d)
{
    p[idx][0] = parent;
    dep[idx] = d;
    for(int i = 1; i < h; i++)
    {
        p[idx][i] = p[p[idx][i - 1]][i - 1];
    }

    for(int nidx : adj[idx])
    {
        if(nidx == parent)
            continue;
        dfs(nidx, idx, d + 1);
    }
}

int get_lca(int a, int b)
{
    if(dep[a] < dep[b])
        swap(a, b);
    
    int diff = dep[a] - dep[b];
    for(int i = h - 1; i >= 0; i--)
    {
        if((1ll << i) <= diff)
        {
            a = p[a][i];
            diff -= (1ll << i);
        }
    }

    if(a == b)
        return a;

    for(int i = h - 1; i >= 0; i--)
    {
        if(p[a][i] != p[b][i])
        {
            a = p[a][i];
            b = p[b][i];
        }
    }

    return p[a][0];
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin >> n >> m;

    memset(dep, 0, sizeof(dep));
    memset(p, 0, sizeof(p));
    memset(dist, 0x3f, sizeof(dist));

    for(int i = 0; i < n - 1; i++)
    {
        int a, b;
        cin >> a >> b;
        adj[a].push_back(b);
        adj[b].push_back(a);
    }

    h = log2(n + 1) + 1;
    dfs(1, 0, 1);

    vector<int> marked = { 1 };
    dist[1] = 0;

    int sqn = sqrt(m);

    for(int i = 0; i < m; i++)
    {
        int t, v;
        cin >> t >> v;
        if(t == 1)
        {
            // update v
            marked.push_back(v);
            if(marked.size() == sqn)
            {
                queue<int> q;
                for(int i : marked)
                {
                    q.push(i);
                    dist[i] = 0;
                }
                marked.clear();

                int val = 0;
                while(!q.empty())
                {
                    int sz = q.size();
                    val++;
                    while(sz--)
                    {
                        auto t = q.front();
                        q.pop();
                        for(auto nt : adj[t])
                        {
                            if(val < dist[nt])
                            {
                                dist[nt] = val;
                                q.push(nt);
                            }
                        }
                    }
                }
            }
        }
        else
        {
            // query v
            int ret = dist[v];
            for(auto tt : marked)
            {
                int lca = get_lca(v, tt);
                int can = dep[v] - dep[lca] + dep[tt] - dep[lca];
                ret = min(ret, can);
            }
            cout << ret << endl;
        }
    }

    return 0;
}

```


