---
title: CF992C - Nastya and a Wardrobe
date: 2022-03-11 09:00:00
categories: Algorithm
index_img: /img/codeforces.jpg
excerpt: Math
---

### Problem

[https://codeforces.com/contest/992/problem/C](https://codeforces.com/contest/992/problem/C)

### Reasoning

test

### Solution

```cpp
#include <bits/stdc++.h>

using namespace std;

#define LOCAL
#define MOD 1000000007

typedef long long ll;

ll fastPower(ll s, ll n, ll p)
{
    ll ans = s % MOD;

    while (p != 0)
    {
        if(p&1==1)
            ans = ans * n % MOD;
        n = n * n % MOD;
        p = p >> 1;
    }

    return ans;
}

int main()
{

#ifdef LOCAL
    freopen("data.in", "r", stdin);
    // freopen("data.out", "w", stdout);
#endif

    ll x, k;
    while (cin >> x >> k)
    {
        if(k==0 || x == 0)
            cout << x * 2 % MOD << endl;
        else
        {
            ll round = fastPower(1, 2, k);
            ll last = fastPower(x, 2, k);
            ll start = last + MOD - round + 1;
            cout << (start + last) % MOD << endl;
        }
    }

    return 0;
}
```