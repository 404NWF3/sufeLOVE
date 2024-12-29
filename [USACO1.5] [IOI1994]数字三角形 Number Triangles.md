# [USACO1.5] [IOI1994]数字三角形 Number Triangles

## 题目描述

观察下面的数字金字塔。


写一个程序来查找从最高点到底部任意处结束的路径，使路径经过数字的和最大。每一步可以走到左下方的点也可以到达右下方的点。

![](https://cdn.luogu.com.cn/upload/image_hosting/95pzs0ne.png)

在上面的样例中，从 $7 \to 3 \to 8 \to 7 \to 5$ 的路径产生了最大权值。

## 输入格式

第一个行一个正整数 $r$ ,表示行的数目。

后面每行为这个数字金字塔特定行包含的整数。

## 输出格式

单独的一行,包含那个可能得到的最大的和。

## 样例 #1

### 样例输入 #1

```
5
7
3 8
8 1 0
2 7 4 4
4 5 2 6 5
```

### 样例输出 #1

```
30
```

## 提示

【数据范围】  
对于 $100\%$ 的数据，$1\le r \le 1000$，所有输入在 $[0,100]$ 范围内。

题目翻译来自NOCOW。

USACO Training Section 1.5

IOI1994 Day1T1



### DP

#### bottom-up DP

```
#include <bits/stdc++.h>
using namespace std;

//dp_table(i, j)为第i行第j个元素
int dp_table[1001][1001], n;

int main() {
    //一共有n层
    cin >> n;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= i; j++) {
            cin >> dp_table[i][j];
        }
    }

    for (int f = n - 1; f >= 0; f--) {
        if (f == n - 1) continue;
        for (int j = 0; j <= f; j++) {
            dp_table[f][j] += max(dp_table[f + 1][j], dp_table[f + 1][j + 1]);
        }
    }

    cout << dp_table[0][0] << endl;

    return 0;
}
```

