# Hanio塔问题（输出每一步操作）

```
#include <bits/stdc++.h>
using namespace std;

void solve_hanio(int n, char S, char T, char E);

int main() {
    int n;
    cin >> n;
    solve_hanio(n, 'A', 'B', 'C');
    return 0;
}

//一共有从小到大n个不同大小盘，放在S塔上，欲转移到E塔上，中间临时塔为T塔
//先将n-1阶移到T塔，再将底盘移到
void solve_hanio(int n, char S, char T, char E) {
    if (n == 1) {
        printf("%c -> %c, %d\n", S, E, 1);
        return;
    }
    solve_hanio(n - 1, S, E, T);
    printf("%c -> %c, %d\n", S, E, n);
    solve_hanio(n - 1, T, S, E);
}
```

