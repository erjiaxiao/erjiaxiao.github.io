---
title: CF235A - LCM Challenge
date: 2022-03-05 09:00:00
categories: Algorithm
index_img: /img/codeforces.jpg
excerpt: Number Theory
---

### Problem

[https://codeforces.com/contest/235/problem/A](https://codeforces.com/contest/235/problem/A)

### Reasoning

test

### Solution

```cpp
#include <iostream>
#include <iomanip>
#include <map>
#include <vector>
#include <string>
#include <algorithm>
#include <cmath>

using namespace std;

typedef long long ll;

ll gcd(ll a, ll b) { return !b ? a : gcd(b, a % b); }
ll lcm(ll a, ll b) { return a * b / gcd(a, b); }

int main()
{

#ifdef LOCAL
    freopen("data.in", "r", stdin);
    // freopen("data.out", "w", stdout);
#endif

    ll n;
    while (cin >> n)
    {
        if(n==1)
            cout << 1 << endl;
        
        else if (n==2)
            cout << 2 << endl;

        else if (n%2==1)
            cout << n * (n - 1) * (n - 2) << endl;

        else
        {
            ll ans = (n - 1) * (n - 2) * (n - 3);
            for (int i = 2; i < 7; i++)            
            {
                ans = max(ans, lcm(n * (n - 1), n - i));
            }
            cout << ans << endl;
        }
    }

    return 0;
}
```