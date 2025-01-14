# 「ALFR Round 3」C 割

## 题目背景

[English Statement](https://www.luogu.com.cn/problem/U517305). You must submit your code at the Chinese version of the statement.

## 题目描述

设 $f(S)$ 表示字符串 $S$ 字典序最大的子序列，给定 $k$，你需要将原字符串 $S$ 分割成 $k$ 段，设第 $i$ 段子串为 $a_i$，则该分割方案的权值为 $\max f(a_i)$，其中 $1\le i\le k$。由于分割方案有很多种，你需要求出所有分割方案中**字典序最小的权值**。

注：这里的权值实际上指的是字符串。

关于子序列的定义：某个序列的子序列是从最初序列通过去除某些元素但不破坏余下元素的相对位置（在前或在后）而形成的新序列。

关于字典序的定义：在字典序中，首先比较两个字符串的第一个字符，如果不同，则第一个字符较小的字符串更小；如果相同，则继续比较下一个字符，依此类推，直到比较完所有字符。如果一个字符串是另一个字符串的前缀，则较短的字符串更小。

## 输入格式

输入共两行。

第一行两个整数 $n,k$，$n$ 表示字符串 $S$ 的长度，$k$ 表示你需要将字符串分割为 $k$ 段。

第二行为一个字符串，即 $S$。

## 输出格式

输出一个字符串表示所有分割方案中字典序最小的权值。

## 样例 #1

### 样例输入 #1

```
7 2
skyaqua
```

### 样例输出 #1

```
y
```

## 提示

### 样例解释
可以将字符串 $S$ 分割成 `sky`、`aqua` 这 $2$ 段，这 $2$ 段的 $f$ 分别为 `y`、`ua`，其中字典序最大的 $f$ 为 `y`，所以该分割方案的权值为 `y`。可以证明 `y` 是所有分割方案中字典序最小的权值。

### 数据范围

| 子任务 | 分值 |              限制               |
| :----: | :--: | :-----------------------------: |
|  $1$   | $10$ |            $n\le 10$            |
|  $2$   | $20$ |           $n\le 10^2$           |
|  $3$   | $20$ |       $n\le 3\times 10^2$       |
|  $4$   | $10$ | 保证字符串 $S$ 中所有字符都相等 |
|  $5$   | $10$ |              $k=1$              |
|  $6$   | $30$ |                -                |

对于 $100\%$ 的数据，$1\le k\le n\le 2\times10^5$，且字符串 $S$ 中的字符均为小写英文字母。

```
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

const int INF = 1e9;

string getMaxSubsequence(const string& s) {
    string result;
    int n = s.size();
    for (int i = 0; i < n; ++i) {
        while (!result.empty() && result.back() < s[i] && result.size() + (n - i - 1) >= n - 1) {
            result.pop_back();
        }
        result.push_back(s[i]);
    }
    return result;
}

int main() {
    int n, k;
    cin >> n >> k;
    string s;
    cin >> s;
    
    vector<vector<string>> dp(k + 1, vector<string>(n + 1, ""));
    
    for (int i = 1; i <= k; ++i) {
        for (int j = 1; j <= n; ++j) {
            string currentSubSeq = getMaxSubsequence(s.substr(j - 1));
            dp[i][j] = currentSubSeq;
        }
    }
    
    cout << dp[k][n] << endl;
    
    return 0;
}

```

