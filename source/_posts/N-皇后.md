---
title: N 皇后
date: 2023-01-31 16:51:57
tags: DFS
categories: Algorithm
---

# 建模

不妨以树的抽象结构来思考所有可能的情况：由于每一行最多且必须放一个皇后，因此一行一行地放不仅简化了步骤，更方便按行枚举；又由于 N 皇后是不确定的，使用递归便于控制循环。<!--more-->

# 剪枝优化

「剪枝」一词十分贴切，剪的是树的一个节点，剪下的是这个节点后面所有的枝。如果某一个位置冲突了，后面的行必定冲突，直接跳出以减少时间的浪费。开一个一维数组存放每层的皇后的列，基于前面皇后的位置，编写一个用于判断某位置是否与前面冲突的函数。

# 代码实现

一开始自作聪明，想着走过的路不必再走，每一层从 `queen[row]` 开始。然而这是错误的，换一个角度看：每一层都来自上一层的递归，既然才递过来，后面的路都是没有走过的，因此都要从头走。之前的错误在于走到最后不退回去，循环变成一次性的了。以下是代码：

```cpp
#include <iostream>
#include <vector>
#include <cmath>

#define MAXN 17
using namespace std;
int N;
long long sum;
vector<int> queen(MAXN, 0);

bool conflict(int row, int cow) {
    if (row == 0) return false;
    for (int i = 0; i < row; i++) {
        if (queen[i] == cow || row - i == abs(cow - queen[i])) {
            return true;
        }
    }
    return false;
}

void DFS(int row) {
    if (row >= N) {
        sum++;
        return;
    }
    for (int i = 0; i < N; i++) {
        if (!conflict(row, i)) {
            queen[row] = i;
            DFS(row + 1);
        }
    }
}

int main() {
    cin >> N;
    DFS(0);
    cout << sum;
    return 0;
}
```

此外，如果追求更高的速度，可以开一个二维数组记录冲突情况，随用随取，避免了 `conflict()` 函数的运算时间。如果对性能有极致的追求，位运算也大有用武之地。我嘛，就先咕咕咕了。