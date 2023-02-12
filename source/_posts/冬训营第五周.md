---
title: 冬训营第五周
date: 2023-02-06 17:45:12
tags:
categories: SMU 冬训营 2023
---
# Day 1

题单：[蓝桥杯模拟赛 3](https://www.luogu.com.cn/contest/100614#problems) 

无论题目如何，一定要专注地思考下去，不要划水，不要摆烂。所有的思考都不会白费，正是这些思考让学习算法得以锻炼思维，而不是仅仅记住一个又一个的模板。

> 在你意识到问题的本质之前，所有尝试都是必要的，没有哪条弯路是白走的。

<!--more-->

## [砝码称重](https://www.luogu.com.cn/problem/P8742) 

### 建模

动态规划，与经典的 01 背包问题相似，每一个砝码有放、不放、放反面三种情况。以下基于一维的滚动数组。

此处有一个问题：从前往后推还是从后往前推？前推后：基于已有组合推出新的组合；后推前：对于未知组合，判断其需要的前置组合是否存在。

更深一步，无论选取那种推法，都是为了避免**一个砝码重复取**的问题。

前推后：

* 正推：刚推出的组合可能会在后续的循环中被当作「已有组合」再次使用，重复取了；
* 逆推：对于要减去的砝码，同理会重复取。

后推前：

* 正推：同前推后的正推；
* 逆推：同前推后的逆推。

这······难道一维数组注定取重吗？问题的根源不在前推后还是后推前，而是全都采取了错误的处理方法——取正和取负同时处理。我们将取正和取负分开算：

先取正，与 01 背包相同。前推后和后推前都是**逆向遍历**；

后取负
- 如果本来就取了该砝码，那么得到的就是不算该物品的组合，应当是原本就有的，无影响；
- 如果本来没取该砝码，那么就可以减了，为了防止减过的组合再减，应当**正向遍历**；

### 优化

除了正负分开计算，该问题与 01 背包的另一个不同之处在于：01 背包的 `dp` 数组中存放的是整型的价值；而该问题的 `dp` 数组中存放的是布尔类型的存在性。因此，利用位运算，`dp` 数组能够以一个二进制数的形式储存；

以下是实现代码：

```cpp
#include <iostream>
#include <bitset>
#include <vector>
using namespace std;
bitset<100005> exist;
int main() {
    exist.set(0); // 将第 0 位设为默认的 true
    int N;
    cin >> N;
    vector<int> weight(N);
    for (int i = 0; i < N; i++) {
        cin >> weight[i];
        exist |= exist << weight[i]; // 取正
    }
    for (int i = 0; i < N; i++) {
        exist |= exist >> weight[i]; // 取负
    }
    cout << exist.count() - 1; // 统计 1 的数量并减去第 0 位的 1
}
```

## [左孩子右兄弟](https://www.luogu.com.cn/problem/P8744) 

### 建模

树形动态规划，设 `dp[i]` 意为第 `i` 个节点的最大高度加 1。对于任意节点 `i`，只需关注子节点数量 `len[i]` 和各子节点 `son[i][j]` 的最大子节点高度 `dp[i]`，得出状态转移方程：

$$
dp[i]=len[i]+max_{j=0}^{len[j]-1}dp[son[i][j]]
$$

就我目前所学而言，动态规划无非是「记忆化搜索」，具有「记忆化」作用的状态转移方程已经推出，还有树形动态规划特有的「搜索」。

由于树本身的结构具有递推性，因此基于递归的深度优先搜索就十分适合树的遍历。

以下是 AC 代码：

```cpp
#include <iostream>
#include <vector>
using namespace std;
vector<int> son[(int)1e5 + 5];
int len[(int)1e5 + 5];
int dp[(int)1e5 + 5];
int n;
void DFS(int k) {
    if (len[k] == 0) {
        dp[k] = 1;
        return;
    }
    int max_son = 0;
    for (auto i : son[k]) {
        DFS(i);
        max_son = max_son < dp[i] ? dp[i] : max_son;
    }
    dp[k] = len[k] + max_son;
}
int main() {
    cin >> n;
    int fa;
    for (int i = 2; i <= n; i++) {
        cin >> fa;
        len[fa]++;
        son[fa].push_back(i);
    }
    DFS(1);
    cout << dp[1] - 1;
}
```

# Day 3

题单：[SMU Winter 2023 Round #11 (Div.2)](https://codeforces.com/group/L9GOcnr1dm/contest/425500) 

有一道题教会我这样一件事：不要一个劲儿地想本质、优化，尤其是你想不到的时候。也许有时暴力模拟就是正解，多维度的解法既是保底，也有助于看清算法与算法之间的关系。

# Day 5

题单：[SMU Winter 2023 Round #12 (Div.2)](https://codeforces.com/group/L9GOcnr1dm/contests) 

## [KK 与卡牌](https://codeforces.com/group/L9GOcnr1dm/contest/425928/problem/D) 

### 建模

「对象」的思想是如此优雅。基于游戏规则，可以创建一个名为 `Card` 的类型，并且该类型的对象可以进行严格的大小比较。只需要定义其比较方式（即重载 `<`），就可以调用内置的排序函数 `sort` 和二分查找函数之一的 `upper_bound`，从而达到类似数与数组比较的效果。

先排序，后二分查找。

注意：名字的字典序小的卡片优先级更高。因此先看点数，越大越好；点数相同，再看名字，越小越好。

以下是 AC 代码：

```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;
struct Card {
    string name;
    float power{}; // 初始化，防止内存泄漏
} kk_card[100005];

bool operator<(const Card &a, const Card &b) {
    if (a.power == b.power) {
        return a.name > b.name;
    } else {
        return a.power < b.power;
    }
}

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    int n, q;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> kk_card[i].name >> kk_card[i].power;
    }
    sort(kk_card, kk_card + n);
    cin >> q;
    Card bm;
    for (int i = 0; i < q; i++) {
        cin >> bm.name >> bm.power;
        int ans = n - (int) (upper_bound(kk_card, kk_card + n, bm) - kk_card);
        cout << ans << '\n';
    }
    return 0;
}
```

# 总结

我喜欢原始而具体的问题，却讨厌基于一大堆理论的问题。其实所谓理论与体系，都是为了解决原始问题所建立的抽象。既然喜欢原始问题，就应该去追根溯源，而非抱怨理论过于繁琐。具体与抽象之间的关系多么奥妙。

珍惜地思考吧，数据结构和算法实在有趣。