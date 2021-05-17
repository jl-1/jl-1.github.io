---
layout: post
title: A Combinatorics Problem 
---

[Educational Codeforces Round 109 (Rated for Div. 2) E. Assimilation IV](https://codeforces.com/contest/1525/problem/E)

- I need to practise combinatorics more
- Since it's expected value, we can look at each point individually
    - What's is the probability that this point get covered by one the the $$ n $$ city
    - Then add the expected value of each point together
    - Note that, we need to do modular inverse. Therefore, we add the numerator first. Denominator is just $$ \prod_{i=1}^{n}inv[i] $$ or $$ inv(fact[n]) $$
- Now, how to calculate the individual expected value? Key observation is that $$ d $$ is in range $$ [1, n + 1] $$
    - Distance $$ n + 1 $$ will never be covered, therefore we only care about $$ [1, n] $$
    - The problem becomes count the number of combinations satisfy the condition that certain element is less than the given value.
    - Sort the array first and the solution becomes trivial. Do more combinatorics. This should be really obvious!

```c++
#include <bits/stdc++.h>
using ll = long long;
using ull = unsigned long long;

using namespace std;

const ll mod = 998244353;
ll n, m;
ll fact[21];

ll solve(vector<ll>& nums)
{
    ll sz = nums.size();
    ll ret = 1;
    for(ll i = 0; i < sz; i++)
    {
        ret = (ret * (nums[i] - i - 1)) % mod;
    }
    ret = ret * fact[n - sz] % mod;
    ret = (fact[n] - ret) % mod;
    if(ret < 0)
        ret += mod;
    return ret;
}

ll power(ll x, ll y) 
{ 
    ll ret = 1; 
   
    while (y > 0) 
    { 
        if (y % 2 == 1) 
            ret = ret * x % mod; 
        y = y / 2;
        x = x * x % mod;
    } 
    return ret; 
}

ll inv(ll num)
{
    return power(num, mod - 2);
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin >> n >> m;
    vector<vector<ll>> v(n, vector<ll>(m));
    for(ll i = 0; i < n; i++)
    {
        for(ll j = 0; j < m; j++)
        {
            cin >> v[i][j];
        }
    }

    fact[0] = 1;
    for(ll i = 1; i <= 20; i++)
        fact[i] = i * fact[i - 1] % mod;

    ll ret = 0;
    for(ll j = 0; j < m; j++)
    {
        vector<ll> nums;
        for(ll i = 0; i < n; i++)
        {
            if(v[i][j] <= n)
                nums.push_back(v[i][j]);
        }
        sort(nums.begin(), nums.end()); 
        ret = (ret + solve(nums)) % mod;
    }

    ret = (ull)ret * inv(fact[n]) % mod;
    cout << ret << endl;

    return 0;
}

``` 
