---
title: Combinations of Max-Heap and Min-Heap
date: 2022-05-13 09:00:00
categories: Algorithm
index_img: /img/heap_combination.png
excerpt: Heap is usually used to find the maximum or the minimum in a number array efficiently. Combinations of Max-Heap and Min-Heap could achieve more advanced searching goals.
---

## Problem

Black Box is a primitive database. It can store an array of integers, and there is a special variable $i$. At the beginning, the Black Box is empty. And $i=0$. This Black Box has to process a bunch of commands.

There are only two operations:

- `ADD(x)`: put $x$ into Black Box;

- `GET`: add $1$ to $i$, and output the smallest $i$ number in the Black Box.

Remember: the smallest $i$ number is the $i$th element of the numbers in the Black Box sorted from small to large.

Let's demonstrate with 11 operations. (As shown in the table below)

| No. | Operation    | $i$ | Database             | Output |
| :-: | :----------- | :-: | -------------------- | :----: |
|  1  | `ADD(3)`     | $0$ | $3$                  |   /    |
|  2  | `GET`        | $1$ | $3$                  |  $3$   |
|  3  | `ADD(1)`     | $1$ | $1,3$                |   /    |
|  4  | `GET`        | $2$ | $1,3$                |  $3$   |
|  5  | `ADD(-4)`    | $2$ | $-4,1,3$             |   /    |
|  6  | `ADD(2)`     | $2$ | $-4,1,2,3$           |   /    |
|  7  | `ADD(8)`     | $2$ | $-4,1,2,3,8$         |   /    |
|  8  | `ADD(-1000)` | $2$ | $-1000,-4,1,2,3,8$   |   /    |
|  9  | `GET`        | $3$ | $-1000,-4,1,2,3,8$   |  $1$   |
| 10  | `GET`        | $4$ | $-1000,-4,1,2,3,8$   |  $2$   |
| 11  | `ADD(2)`     | $4$ | $-1000,-4,1,2,2,3,8$ |   /    |

There are $m$ of `ADD` operations and $n$ of `GET` operations. Now use two integer arrays to represent the command string:

1. $a_1,a_2,\cdots,a_m$: elements that will be put into the Black Box. For example, $a=[3,1,-4,2,8,-1000,2]$ in the above example.

2. $u_1,u_2,\cdots,u_n$: means that a `GET` operation will appear after the $u_i$th element is put into the Black Box. For example $u=[1,2,6,6]$ in the above example.

### Input Format

The first line contains two integers $m$ and $n$, indicating the number of elements and the number of `GET` operations.

The second line has a total of $m$ integers, and the $i$ integer from left to right is $a_i$, separated by spaces.

The third line has a total of $n$ integers, and the $i$ integer from left to right is $u_i$, separated by spaces.

### Output Format

Output the string obtained by Black Box according to the operations, one number per line.

### Example Input

```
7 4
3 1 -4 2 8 -1000 2
1 2 6 6
```

### Example Output

```
3
3
1
2
```

## Solution

For tracking the top minimal values of a number array, we could utilize both max-heap and min-heap. Specifically, the max-heap stores the top $k$ minimal values of the array, while the min-heap stores the rest value. The important detail here is $k$ is set to $1$ initially and increases by $1$ after every operation `GET`. So the root of the max-heap could keep the $kth$ minimal value of the array which is expected every time it operates `GET`.

## Code

```cpp
#include <bits/stdc++.h>

using namespace std;

#define LOCAL

class MinHeap
{

public:
    vector<int> vec;

    void add(int x)
    {
        vec.push_back(x);

        int curr = vec.size() - 1, par;
        while (curr != 0)
        {
            par = (curr - 1) / 2;
            if (vec[par] <= vec[curr])
                return;
            if (vec[par] > vec[curr])
            {
                int tmp = vec[curr];
                vec[curr] = vec[par];
                vec[par] = tmp;
            }
            curr = par;
        }
    }

    int pop()
    {
        int popping = vec[0];
        vec[0] = vec[vec.size() - 1];
        vec.pop_back();

        int curr = 0, next;
        while (true)
        {
            int child1 = curr * 2 + 1;
            int child2 = curr * 2 + 2;

            if (child1 >= vec.size() && child2 >= vec.size())
                break;
            else if (child1 < vec.size() && child2 < vec.size())
            {
                if (vec[child1] < vec[child2])
                    next = child1;
                else
                    next = child2;
            }
            else if (child1 < vec.size())
                next = child1;
            else
                next = child2;

            if (vec[curr] <= vec[next])
                break;
            else
            {
                int tmp = vec[next];
                vec[next] = vec[curr];
                vec[curr] = tmp;
            }
            curr = next;
        }

        return popping;
    }
};

class MaxHeap
{

public:
    vector<int> vec;

    void add(int x)
    {
        vec.push_back(x);

        int curr = vec.size() - 1, par;
        while (curr != 0)
        {
            par = (curr - 1) / 2;
            if (vec[par] >= vec[curr])
                return;
            if (vec[par] < vec[curr])
            {
                int tmp = vec[curr];
                vec[curr] = vec[par];
                vec[par] = tmp;
            }
            curr = par;
        }
    }

    int pop()
    {
        int popping = vec[0];
        vec[0] = vec[vec.size() - 1];
        vec.pop_back();

        int curr = 0, next;
        while (true)
        {
            int child1 = curr * 2 + 1;
            int child2 = curr * 2 + 2;

            if (child1 >= vec.size() && child2 >= vec.size())
                break;
            else if (child1 < vec.size() && child2 < vec.size())
            {
                if (vec[child1] > vec[child2])
                    next = child1;
                else
                    next = child2;
            }
            else if (child1 < vec.size())
                next = child1;
            else
                next = child2;

            if (vec[curr] >= vec[next])
                break;
            else
            {
                int tmp = vec[next];
                vec[next] = vec[curr];
                vec[curr] = tmp;
            }
            curr = next;
        }

        return popping;
    }
};

int main()
{

#ifdef LOCAL
    freopen("data.in", "r", stdin);
#endif

    int m, n, tmp;
    vector<int> numbers, thresholds;

    cin >> m >> n;
    while (m--)
    {
        cin >> tmp;
        numbers.push_back(tmp);
    }
    while (n--)
    {
        cin >> tmp;
        thresholds.push_back(tmp);
    }

    MaxHeap maxheap;
    MinHeap minheap;

    int volumn = 1;
    int i = 0;
    for (int t : thresholds)
    {
        while (i < t)
        {

            if (maxheap.vec.size() < volumn)
                maxheap.add(numbers[i]);
            else
            {
                minheap.add(numbers[i]);
                while (minheap.vec[0] < maxheap.vec[0])
                {
                    int n1 = minheap.pop();
                    int n2 = maxheap.pop();
                    minheap.add(n2);
                    maxheap.add(n1);
                }
            }
            i++;
        }

        cout << maxheap.vec[0] << endl;

        volumn++;
        if(minheap.vec.size()!=0)
            maxheap.add(minheap.pop());
    }
}
```
