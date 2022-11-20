---
title: Range Minimum Query and Sparse Table
date: 2022-03-23 09:00:00
categories: Algorithm
index_img: /img/flammarion_woodcut.jpg
math: true
excerpt: Sparse table concept is used for fast queries on a set of static data (elements do not change). It does preprocessing so that the queries can be answered efficiently.
---

## Introduction

We have an array arr[0 . . . n-1]. We should be able to efficiently find the minimum value from index L (query start) to R (query end) where 0 <= L <= R <= n-1. Consider a situation when there are many range queries as following:

```
Input:  arr[]   = {7, 2, 3, 0, 5, 10, 3, 12, 18};
        query[] = [0, 4], [4, 7], [7, 8]

Output: Minimum of [0, 4] is 0
        Minimum of [4, 7] is 3
        Minimum of [7, 8] is 12
```

This is the **Range Minimum Query** problem. A brute force solution is to run a loop from L to R and find the minimum element in the given range. This solution takes $O(n)$ time to query in the worst case.

## Method 1

A Simple Solution is to create a 2D array lookup[][] where an entry lookup[i][j] stores the minimum value in range arr[i..j]. The minimum of a given range can now be calculated in $O(1)$ time.

![lookup table](https://media.geeksforgeeks.org/wp-content/cdn-uploads/rmqSimple-1.png)

This approach supports queries in $O(1)$, but preprocessing takes $O(n^2)$ time. Also, this approach needs $O(n)$ extra space which may become huge for large input arrays.

## Method 2 (Sparse Table) 

The idea is to precompute a minimum of all subarrays of size $2^j$ where j varies from 0 to $log_n$. Like method 1, we make a lookup table. Here lookup[i][j] contains a minimum of range starting from i and of size $2^j$. For example lookup[0][3] contains a minimum of range [0, 7] (starting with 0 and of size $2^3$).

### Preprocessing

How to fill this lookup table? The idea is simple, fill in a bottom-up manner using previously computed values. 
For example, to find a minimum of range [0, 7], we can use a minimum of the following two.

a) Minimum of range [0, 3]

b) Minimum of range [4, 7]

Based on the above example, below is the formula,

```
// If arr[lookup[0][2]] <=  arr[lookup[4][2]],
// then lookup[0][3] = lookup[0][2]
if arr[lookup[i][j-1]] <= arr[lookup[i+2j-1][j-1]]
   lookup[i][j] = lookup[i][j-1]

// If arr[lookup[0][2]] >  arr[lookup[4][2]],
// then lookup[0][3] = lookup[4][2]
else
   lookup[i][j] = lookup[i+2j-1][j-1]
```

![lookup table](https://media.geeksforgeeks.org/wp-content/cdn-uploads/rmqSparseTable-1.png)

### Query

For any arbitrary range [l, R], we need to use ranges that are in powers of 2. The idea is to use the closest power of 2. We always need to do at most one comparison (compare a minimum of two ranges which are powers of 2). One range starts with L and ends with “L + closest-power-of-2”. The other range ends at R and starts with “R – same-closest-power-of-2 + 1”. For example, if the given range is (2, 10), we compare a minimum of two ranges (2, 9) and (3, 10). 
Based on the above example, below is the formula,

```
// For (2,10), j = floor(Log2(10-2+1)) = 3
j = floor(Log(R-L+1))

// If arr[lookup[0][3]] <=  arr[lookup[3][3]],
// then RMQ(2,10) = lookup[0][3]
if arr[lookup[L][j]] <= arr[lookup[R-(int)pow(2,j)+1][j]]
   RMQ(L, R) = lookup[L][j]

// If arr[lookup[0][3]] >  arr[lookup[3][3]],
// then RMQ(2,10) = lookup[3][3]
else
   RMQ(L, R) = lookup[R-(int)pow(2,j)+1][j]
```

Since we do only one comparison, the time complexity of the query is $O(1)$. So sparse table method supports query operation in $O(1)$ time with $O(n log n)$ preprocessing time and $O(n log n)$ space.
