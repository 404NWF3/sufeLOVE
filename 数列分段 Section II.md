# 数列分段 Section II

## 题目描述

对于给定的一个长度为 $N$ 的正整数数列 $A_{1\sim N}$，现要将其分成 $M$（$M\leq N$）段，并要求每段连续，且每段和的最大值最小。

关于最大值最小：

例如一数列 $4\ 2\ 4\ 5\ 1$ 要分成 $3$ 段。

将其如下分段：

$$
[4\ 2][4\ 5][1]
$$

第一段和为 $6$，第 $2$ 段和为 $9$，第 $3$ 段和为 $1$，和最大值为 $9$。

将其如下分段：

$$
[4][2\ 4][5\ 1]
$$

第一段和为 $4$，第 $2$ 段和为 $6$，第 $3$ 段和为 $6$，和最大值为 $6$。

并且无论如何分段，最大值不会小于 $6$。

所以可以得到要将数列 $4\ 2\ 4\ 5\ 1$ 要分成 $3$ 段，每段和的最大值最小为 $6$。

## 输入格式

第 $1$ 行包含两个正整数 $N,M$。  

第 $2$ 行包含 $N$ 个空格隔开的非负整数 $A_i$，含义如题目所述。

## 输出格式

一个正整数，即每段和最大值最小为多少。

## 样例 #1

### 样例输入 #1

```
5 3
4 2 4 5 1
```

### 样例输出 #1

```
6
```

## 提示

对于 $20\%$ 的数据，$N\leq 10$。

对于 $40\%$ 的数据，$N\leq 1000$。

对于 $100\%$ 的数据，$1\leq N\leq 10^5$，$M\leq N$，$A_i < 10^8$， 答案不超过 $10^9$。



### 贪心+二分查找

本题核心为二分查找每段和最小值。如果可以分不多于$M$段使得当前值$mid$减小，则缩小范围，否则扩大范围；

```cpp
#include <bits/stdc++.h>
using namespace std;

#define maxN 100001
int arr[maxN], n, maximum = INT_MIN, sum, m;

bool judge(int);

int main() {
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
        maximum = max(maximum, arr[i]);
        sum += arr[i];
    }
    int l = maximum, r = sum, mid;
    while (l < r) {
        mid = l + (r - l) / 2;
        if (judge(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    cout << mid << endl;

    return 0;
}

bool judge(int mid) {
    int prefix = 0, clap = 1;
    for (int i = 0; i < n; i++) {
        if (prefix + arr[i] > mid) {
            clap++;
            if (clap > m) return false;
            prefix = arr[i];
        } else if (prefix + arr[i] <= mid) {
            prefix += arr[i];
        }
        //else if (prefix + arr[i] == mid) {
        //     if (i != n - 1) clap++;
        //     if (clap > m) return false;
        //     prefix = 0;
        // }
    }
    return true;
}
```

