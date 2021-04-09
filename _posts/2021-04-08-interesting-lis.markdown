---
layout: post
title:  "An Interesting LIS Problem"
---

# [Codeforces Round #277 (Div. 2) E. LIS of Sequence](https://codeforces.com/problemset/problem/486/E)

- For an array $$ a $$, the length of LIS $$ len $$ can be found in $$ O(nlogn) $$ easily
- But here, we also need to figure out:
    1. whether $$ a_i $$ belongs to a LIS
    2. if $$ a_i $$ belongs to a LIS, does it belong to every LIS?

<br/>

- It is not hard to come up with a method to answer 1
    - we can keep tracking the length of LIS ending at $$ a_i $$ as $$ l_i $$, and beginning at $$ a_i $$ as $$ r_i $$.
    - if $$ l_i + r_i - 1 < len $$, then $$ a_i $$ doesn't belong to a LIS
- Now we know if $$ a_i $$ belongs to a LIS, but does it belong to every LIS?
    - An important observation is that, $$ a_i $$ always appears at the same index in LIS! (easy prove. think about it.)
    - Now, if there are multiple $$ a_i $$ have the same index in LIS, then those $$ a_i $$ don't belong to every LIS!

```c++
#include <bits/stdc++.h>
#define ll long long

using namespace std;

vector<ll> get_lis(vector<ll>& v)
{
    ll n = v.size();
    vector<ll> lis;
    vector<ll> left(n);
    for(ll i = 0; i < n; i++)
    {
        auto it = lower_bound(lis.begin(), lis.end(), v[i]);
        if(it == lis.end())
        {
            lis.push_back(v[i]);
            left[i] = lis.size();
        }
        else
        {
            *it = v[i];
            ll idx = it - lis.begin();
            left[i] = idx + 1;
        }
    }
    return left;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    ll n;
    cin >> n;
    vector<ll> v(n);
    for(ll i = 0; i < n; i++)
        cin >> v[i];
    
    auto left = get_lis(v);
    auto rv = v;
    for(auto& i : rv)
        i = -i;
    reverse(rv.begin(), rv.end());
    auto right = get_lis(rv);
    reverse(right.begin(), right.end());

    ll len = *max_element(left.begin(), left.end());
    unordered_map<ll, ll> cnt;
    for(ll i = 0; i < n; i++)
    {
        if(left[i] + right[i] - 1 == len)
        {
            ll idx = left[i] - 1;
            cnt[idx]++;
        }
    }
    
    vector<ll> ret(n);
    for(ll i = 0; i < n; i++)
    {
        if(left[i] + right[i] - 1 < len)
        {
            ret[i] = 1;
        }
        else
        {
            ll idx = left[i] - 1;
            if(cnt[idx] == 1)
                ret[i] = 3;
            else
                ret[i] = 2;
        }
    }

    for(ll i : ret)
        cout << i;
    cout << endl;

    return 0;
}
```

