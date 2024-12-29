# N皇后问题

```
#include <bits/stdc++.h>
using namespace std;

void solve(int n);
void backtrack(vector<string> &scene, int row);
bool isvalid(vector<string> &scene, int row, int col);

int main() {
    int n;
    scanf("%d", &n);
    solve(n);
    return 0;
}

bool isvalid(vector<string> &scene, int row, int col) {
    int n = scene.size();
    //判断是否冲突
    for (int i = 0; i <= row; i++) {
        if (scene[i][col] == 'Q')
            return false;
    }
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (scene[i][j] == 'Q')
            return false;
    }
    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
        if (scene[i][j] == 'Q')
            return false;
    }
    return true;
}

void backtrack(vector<string> &scene, int row) {
    if (row == scene.size()) {
        for (string &s : scene) cout << s << endl;
        cout << endl;
        return;
    }
    for (int col = 0; col < scene.size(); col++) {
        if (isvalid(scene, row, col)) {
            scene[row][col] = 'Q';
            backtrack(scene, row + 1);
            scene[row][col] = 'X';
        }
    }
}

void solve(int n) {
    vector<string> scene(n, string(n, 'X'));
    backtrack(scene, 0);
}
```