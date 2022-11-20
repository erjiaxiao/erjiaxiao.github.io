---
title: Competitive Programming Templates
date: 2022-02-23 09:00:00
categories: Algorithm
index_img: /img/acmicpc.jpg
excerpt: The International Collegiate Programming Contest is an algorithmic programming contest for college students. Teams of three, representing their university, work to solve the most real-world problems.
---

## Introduction

The International Collegiate Programming Contest is an algorithmic programming contest for college students. Teams of three, representing their university, work to solve the most real-world problems, fostering collaboration, creativity, innovation, and the ability to perform under pressure. Since every minute in the match is precious, we could prepare some time-saving templates ahead of time.

![ACM/ICPC](/img/acmicpc.jpg)

## Templates

```
#include <iostream>
#include <iomanip>
#include <cstdio>
#include <cstring>
#include <string>
#include <cmath>
#include <algorithm>
#include <cctype>
#include <ctime>
#include <queue>
#include <stack>
#include <deque>
#include <map>
#include <unordered_map>
#include <set>
#include <unordered_set>
#include <bitset>
using namespace std;
// STL库常用(vector, map, set, pair)
#define pii pair<int, int>
#define pdd pair<double, double>
#define F first
#define S second
//常量
#define PI (acos(-1.0))
#define INF 0x3f3f3f3f
//必加
#ifdef LOCAL
#include <cassert>
#define dbg(...)                                        \
    do                                                  \
    {                                                   \
        cerr << "\033[33;1m" << #__VA_ARGS__ << " -> "; \
        err(__VA_ARGS__);                               \
    } while (0)
void err()
{
    cerr << "\033[39;0m" << endl;
}
template <template <typename...> class T, typename t, typename... A>
void err(T<t> a, A... x)
{
    for (auto v : a)
        cerr << v << ' ';
    cerr << ", ";
    err(x...);
}
template <typename T, typename... A>
void err(T a, A... x)
{
    cerr << a << ' ';
    err(x...);
}
template <typename T>
void err(T *a, int len)
{
    for (int i = 0; i < len; i++)
        cerr << a[i] << ' ';
    err();
}
#else
#define dbg(...)
#define assert(...)
#endif
typedef unsigned long long ull;
typedef long long ll;
typedef double db;
// 快读快输
template <typename T>
inline T read()
{
    T x = 0;
    bool f = 0;
    char c = getchar();
    while (!isdigit(c))
    {
        if (c == '-')
            f = 1;
        c = getchar();
    }
    while (isdigit(c))
    {
        x = (x << 3) + (x << 1) + (c ^ 48);
        c = getchar();
    }
    return f ? ~x + 1 : x;
}
template <typename T>
inline void print(T k)
{
    int num = 0, ch[20];
    if (k == 0)
    {
        putchar('0');
        return;
    }
    (k < 0) && (putchar('-'), k = -k);
    while (k > 0)
        ch[++num] = k % 10, k /= 10;
    while (num)
        putchar(ch[num--] + 48);
}
//图论
const int GRAPHN = 1, GRAPHM = 1;
int h[GRAPHN], e[GRAPHM], ne[GRAPHM], wei[GRAPHM], idx;
void AddEdge(int a, int b) { e[idx] = b, ne[idx] = h[a], h[a] = idx++; }
void AddEdge(int a, int b, int c) { e[idx] = b, ne[idx] = h[a], wei[idx] = c, h[a] = idx++; }
void AddEdge(int h[], int a, int b) { e[idx] = b, ne[idx] = h[a], h[a] = idx++; }
void AddEdge(int h[], int a, int b, int c) { e[idx] = b, ne[idx] = h[a], wei[idx] = c, h[a] = idx++; }
// 数学公式
template <typename T>
inline T gcd(T a, T b) { return b == 0 ? a : gcd(b, a % b); }
template <typename T>
inline T lowbit(T x) { return x & (-x); }
template <typename T>
inline bool mishu(T x) { return x > 0 ? (x & (x - 1)) == 0 : false; }
template <typename T1, typename T2, typename T3>
inline ll q_mul(T1 a, T2 b, T3 p)
{
    ll w = 0;
    while (b)
    {
        if (b & 1)
            w = (w + a) % p;
        b >>= 1;
        a = (a + a) % p;
    }
    return w;
}
template <typename T, typename T2>
inline ll f_mul(T a, T b, T2 p) { return (a * b - (ll)((long double)a / p * b) * p + p) % p; }
template <typename T1, typename T2, typename T3>
inline ll q_pow(T1 a, T2 b, T3 p)
{
    ll w = 1;
    while (b)
    {
        if (b & 1)
            w = (w * a) % p;
        b >>= 1;
        a = (a * a) % p;
    }
    return w;
}
template <typename T1, typename T2, typename T3>
inline ll s_pow(T1 a, T2 b, T3 p)
{
    ll w = 1;
    while (b)
    {
        if (b & 1)
            w = q_mul(w, a, p);
        b >>= 1;
        a = q_mul(a, a, p);
    }
    return w;
}
template <typename T>
inline ll ex_gcd(T a, T b, T &x, T &y)
{
    if (b == 0)
    {
        x = 1, y = 0;
        return (ll)a;
    }
    ll r = ex_gcd(b, a % b, y, x);
    y -= a / b * x;
    return r; /*gcd*/
}
template <typename T1, typename T2>
inline ll com(T1 m, T2 n)
{
    int k = 1;
    ll ans = 1;
    while (k <= n)
    {
        ans = ((m - k + 1) * ans) / k;
        k++;
    }
    return ans;
}
template <typename T>
inline bool isprime(T n)
{
    if (n <= 3)
        return n > 1;
    if (n % 6 != 1 && n % 6 != 5)
        return 0;
    T n_s = floor(sqrt((db)(n)));
    for (int i = 5; i <= n_s; i += 6)
    {
        if (n % i == 0 || n % (i + 2) == 0)
            return 0;
    }
    return 1;
}
/* ----------------------------------------------------------------------------------------------------------------------------------------------------------------- */

const int MAXN = 6e5 + 10;

int n;
struct Node
{
    int x, v;
    Node(int x = 0, int v = 0) : x(x), v(v) {}
    bool operator<(const Node &a) const { return x < a.x; }
} a[MAXN];
const long double eps = 1e-9;
int sgn(long double a) { return a < -eps ? -1 : a > eps ? 1
                                                        : 0; }

long double lM, rM;
void Cal(long double x)
{
    int i = 1;
    lM = 0, rM = 0;
    for (i = 1; i <= n; i++)
        if (a[i].x > x)
            break;
        else
            lM = max(lM, (x - a[i].x) / a[i].v);
    for (; i <= n; i++)
        rM = max(rM, (a[i].x - x) / a[i].v);
}

bool Check(long double mid)
{
    Cal(mid);
    return lM > rM ? false : true;
}

int main()
{
#ifdef LOCAL
    clock_t c1 = clock();
    freopen("D:\\Cpp\\1.in", "r", stdin);
    freopen("D:\\Cpp\\1.out", "w", stdout);
#endif
    //--------------------------------------------
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i].x);
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i].v);
    sort(a + 1, a + 1 + n);
    // for (int i = 1; i <= n; i++)
    // printf("%d", a[i].x);
    long double l = a[1].x, r = a[n].x;
    rM = 1;
    while (sgn(l - r))
    {
        long double mid = (l + r) / 2;
        if (Check(mid))
            l = mid;
        else
            r = mid;
        // printf("%.9lf %.9lf\n", l, lm);
    }
    // printf("%.9lf\n", lM);
    printf("%.9Lf", lM);

//--------------------------------------------
#ifdef LOCAL
    cerr << "Time Used:" << clock() - c1 << "ms" << endl;
#endif
    return 0;
}
```
