---
title: Fast I/O in Competitive Programming
date: 2022-04-20 09:00:00
categories: Algorithm
index_img: /img/fast_io.png
excerpt: In order to avoid the annoying TLE(Time Limit Exceed) problem in competitive programming, it is important to read input as fast as possible so we save valuable time.
---

## Introduction

In competitive programming, it is important to read input as fast as possible so we save valuable time. You must have seen various problem statements saying: "Warning: Large I/O data, be careful with certain languages (though most should be OK if the algorithm is well designed)". The key for such problems is to use Faster I/O techniques. 

## Method 1

It is often recommended to use scanf/printf instead of cin/cout for fast input and output. However, you can still use cin/cout and achieve the same speed as scanf/printf by including the following two lines in your main() function:

```cpp
ios_base::sync_with_stdio(false);
```

It toggles on or off the synchronization of all the C++ standard streams with their corresponding standard C streams if it is called before the program performs its first input or output operation. Adding ```ios_base::sync_with_stdio (false);``` (which is true by default) before any I/O operation avoids this synchronization.

```cpp
cin.tie(NULL);
```

tie() is a method that simply guarantees the flushing of std::cout before std::cin accepts an input. This is useful for interactive console programs which require the console to be updated constantly but also slows down the program for large I/O. The NULL part just returns a NULL pointer.

So your template for competitive programming could look like this:  

```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    return 0;
}
```

It is recommended to use ```cout << "\n";``` instead of ```cout << endl;```. ```endl``` is slower because it forces a flushing stream, which is usually unnecessary. (You’d need to flush if you were writing, say, an interactive progress bar, but not when writing a million lines of data.) So write ```'\n'``` instead of endl.

## Method 2 (Fastest)

If above methods are not fast enough, here is an exclusive code to read and write integers in the fastest way.

```cpp
// 快读快写
inline int read()
{
    int r = 0, w = 1;
    // r是存的这个数的绝对值 w是存的这个数是正数还是负数
    char ch = getchar();         // ch为当前读入的字符
    while (ch < '0' || ch > '9') //如果当前读入的字符不是数字，就一直读入，直到读入的是数字为止
    {
        if (ch == '-')
            w = -1; //如果读到了'-'，就说明这是个负数
        ch = getchar();
    }
    //运行到这里时ch里面一定是个整数
    while (ch >= '0' && ch <= '9') //当读入的字符是数字时一直读入
    {
        r = r * (1 << 3) + r * (1 << 1) + ch - (1 << 4) - (1 << 5); //存入当前这个数字，由于ch是个字符，所以存入的时候要减去字符0的ASCLL码
        // 1<<3是8,1<<1是2,加起来就是10   1<<4是16，1<<5是32，加起来就是0的ASCLL码48
        ch = getchar();
    }
    return r * w;
}

inline void write(int x)
{
    char ch[20];
    int len = 0;
    // ch里面存的是从低到高的数字的ASCII码 len是数组长度
    if (x < 0) //如果x是负数，那么先输出负号，再把x变成正数
    {
        putchar((1 << 5) + (1 << 3) + (1 << 2) + 1);
        //(1<<5)+(1<<3)+(1<<2)+1就是'-'的ASCII码值
        x = ~x + 1; // x=~x+1;就等同于x=-x;
    }
    do //用do while可以防止x=0的特殊情况
    {
        ch[len++] = x % 10 + (1 << 4) + (1 << 5); //取出最低位存到ch数组里面 因为ch是字符数组，所以要加上0的ASCII码
        x /= 10;                                  //砍掉最低位
    } while (x > 0);                              //当x>0的时候每次把x的最低位取出
    for (int i = len - 1; i >= 0; i--)            //因为ch是从低到高存的x的绝对值，又因为要从高到低输出，所以倒着循环ch数组输出
        putchar(ch[i]);                           //输出从高到低的每一位
    return;
}
```
