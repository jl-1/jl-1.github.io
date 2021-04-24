---
layout: post
title: Interval and Group of Points 
---

[Codeforces Round #216 (Div. 2) E. Valera and Queries](https://codeforces.com/problemset/problem/369/E)

- A simplified version of the problem is to assume we have only one query. Suppose the query has $$ k $$ elements, then the problem can be solved in $$ O(n + k) $$.
    - we sort all intervals, and keep two pointers to beginning of the intervals and the query points.
    1. If the query point is on the LHS of the interval, we move to the next point
    2. If the query point is on the RHS of the interval, we move to the next interval
    3. If the query point is in the interval, we increment the result, and move to the next interval, because each interval is only supposed to be counted once.
- However, in this problem we have multiple queries. Although the total count of query points are on the order of 1e5, the number of queries is also on the order of 1e5. (This constraints imply that we may want to combine all query points together)
    - If we process each query one-by-one, the worse case complexity is $$ O(mn) $$, which will be TLE
    - We could put all query points into a priority queue and process them one by one. This seems to transform the problem back to the single query case, but it's not! The above mentioned case 1 and 2 works fine. However, if the point lies inside the interval, we can no longer advance the interval. Since we have multiple queries, each interval can be counted by multiple queries!

<br/>

- To deal with interval problems, BIT or Segment Tree can be a good tool
    - Intervals have two end points, we can only sort the by one end
    - What we can do is to sort the intervals by left point, and take them into account one by one
    - During the process, we store the information of the right points in BIT
    - With this strategy, we could answer such queries offline: ***how many intervals lines between point A and point B?***

<br/>

```c++
#include <bits/stdc++.h>
#define ll long long

using namespace std;

const int SZ = 1e6 + 10;
vector<pair<int, int>> check_point[SZ];

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n, m;
    cin >> n >> m;
    vector<pair<int, int>> v(n);
    for(int i = 0; i < n; i++)
        cin >> v[i].first >> v[i].second;
    
    vector<vector<int>> q(m);
    for(int i = 0; i < m; i++)
    {
        int cnt, num;
        cin >> cnt;
        for(int j = 0; j < cnt; j++)
        {
            cin >> num;
            q[i].push_back(num);
        }
    }

    sort(v.begin(), v.end());
    vector<int> ret(m, 0);
    
    BIT bit(SZ);
    for(int i = 0; i < m; i++)
    {
        for(int j = 0; j < q[i].size(); j++)
        {
            if(j == 0)
            {
                check_point[0].push_back({q[i][j], i});
            }
            else
            {
                check_point[q[i][j - 1] + 1].push_back({q[i][j], i});
            }
        }
        check_point[q[i].back() + 1].push_back({SZ - 1, i});
    }

    int p = n - 1;
    for(int i = SZ - 1; i >= 0; i--)
    {
        while(p >= 0 && v[p].first == i)
        {
            bit.add(v[p].second, 1);
            p--;
        }

        for(auto [j, idx] : check_point[i])
        {
            int cnt = 0;
            if(j - 1 >= 0)
                cnt = bit.sum(j - 1);
            ret[idx] += cnt;
        }
    }
    
    for(int i : ret)
    {
        cout << n - i << endl;
    }
    
    return 0;
}
```
