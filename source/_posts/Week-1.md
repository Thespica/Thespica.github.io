---
title: Week 1
date: 2023-01-8 01:44:49
tags: Algorithm
categories: SMU Winter 2023
---
# Day 1

题单：[蓝桥杯模拟赛1](https://www.luogu.com.cn/contest/95102#problems) 

今天第一次参加比赛，虽然是很小的校内训练赛，而且可以查东西，但对从未参加过比赛的我而言也很有比赛的感觉。蓝桥杯 OI 赛制，提交后不能看到代码的通过情况，所以我在本地通过样例后再提交代码。我想起我的 luogu 做题数比队里的一些人多，但是排名却没有他们高，只有一个灰色的名字，应该是因为排名参考了提交次数和通过次数，我太不珍惜提交，总是错了再改，导致我的提交次数远大于我的通过次数。今天的蓝桥杯训练赛制甚至没有试错的机会，所以我总结了一些 tips：<!-- more -->

1. 审题清楚。除了题意，还有数据的范围（正负性，浮点还是整数，要不要长整型······），如果有很强的抬杠能力来给自己出反例，程序应该会健壮很多。
2. 本地测试样例。避免出现编译错误，如果返回值不对，往往是出现了非法操作，例如数组越界、除 0；如果样例跑出来的答案不对，则是因为程序的思路不对或者某个部分疏忽了。
3. 把题目都浏览一遍，从简单的入手。题目往往是随机排列的，与难度无关，但是做出来的题目都是等价的，所以先做简单的，过了再说。

此外，出乎我意料的是，我的排名是第四，在 div2 里 Rating 是第一，看到的时候还挺高兴的。但一方面，我告诫自己，其实自己会的算法并不多，今天除了常规的模拟和常识的贪心，几乎没什么算法思想，倒是有一道我没想出的题（[灵能传输](https://www.luogu.com.cn/problem/P8684) ）或许可以称得上是真正的算法题；另一方面，我其实没学什么算法，但是排名都能这么靠前，说明这次小比赛厉害的人不多，说实话有点小失望，我是很乐意和强者同行的，希望只是我短浅的目光暂时没有发现他们。

以下是补题：

## [后缀表达式](https://www.luogu.com.cn/problem/P8683) 

这题错在之前提到的数据范围，题干说了是整数，没有提及正负性，应当默认有正有负，而题目下方的数据约定说得很清楚：范围是正负的。这题得了30分，便是疏忽了正负数。思路：贪心，按照绝对值排序，把正确的符号尽量分给绝对值大的数。

好吧，我为我的轻敌付出了代价——三个半小时的思考和 debug，我必须承认之前并没有理解题意。

1. 首先，`后缀表达式(Reverse Polish notation)`，又称`逆波兰表示法`，是一个专有概念，[wiki中后缀表达式词条](https://zh.wikipedia.org/wiki/逆波兰表示法)  的解释如下：

   > 逆波兰记法中，操作符置于操作数的后面。例如表达“三加四”时，写作“3 4 + ”，而不是“3 + 4”。如果有多个操作符，操作符置于第二个操作数的后面，所以常规中缀记法的“3 - 4 + 5”在逆波兰记法中写作“3 4 - 5 + ”：先3减去4，再加上5。使用逆波兰记法的一个好处是不需要使用括号。例如中缀记法中“3 - 4 * 5”与“（3 - 4）*5”不相同，但后缀记法中前者写做“3 4 5 * - ”，无歧义地表示“3 (4 5 *) -”；后者写做“3 4 - 5 * ”。
   >
   > 逆波兰表达式的解释器一般是基于堆栈的。解释过程一般是：操作数入栈；遇到操作符时，操作数出栈，求值，将结果入栈；当一遍后，栈顶就是表达式的值。因此逆波兰表达式的求值使用堆栈结构很容易实现，并且能很快求值。
   >
   > 注意：逆波兰记法并不是简单的波兰表达式的反转。因为对于不满足交换律的操作符，它的操作数写法仍然是常规顺序，如，波兰记法“/ 6 3”的逆波兰记法是“6 3 /”而不是“3 6 /”；数字的数位写法也是常规顺序。

2. 其次，此题的题意可以转化为：任意使用括号和给出的符号与数组成最大的表达式。

3. 然后，**正号的意义是连接，负号的意义是反转**。只要有一个负号，就可以把所有的负数变成正数。

4. 再然后，考虑一些极端情况：没有正数或负数、没有负号等。

5. 最后，取值范围，包括之前提到的正负数。此外，`int` 类型可以包含 $[-10^9,10^9]$，但是基于整型的运算却可能得到超出整型的结果，所以 sum 要用长整型 `long long`。

以下是 AC 代码：

```cpp
#include <iostream>
#include <cmath> // abs函数头文件
using namespace std;
int main() {
    int n, m, num;
    cin >> n >> m;
    bool has_posi = false;
    bool has_nega = false;
    int max_nega = -1000000000;
    int min_posi = 1000000000;
    long long ans = 0;
    for (int i = 0; i < n + m + 1; i++) {
        cin >> num;
        if (num < 0) {
            has_nega = true;
            if (num > max_nega) {
                max_nega = num;
            }
        } else if (num > 0) {
            has_posi = true;
            if (num < min_posi) {
                min_posi = num;
            }
        } else {
            has_nega = true;
            has_posi = true;
        }
        if (m > 0) {
            ans += abs(num);
        } else {
            ans += num;
        }
    }
    if (m > 0 && (!has_nega || !has_posi)) {
        if (!has_nega && has_posi) {
            ans -= 2 * min_posi;
        } else {
            ans += 2 * max_nega;
        }
    }
    cout << ans;
    return 0;
}
```

$\lambda$ 演算似乎就用到了歧义更少的逆波兰表示法。

## [灵能传输](https://www.luogu.com.cn/problem/P8684) 

胆大心细，这是一道**真正的贪心**。此题有两个关键步骤： 

1. 转化，将数组 $A_n$ 的数值变动问题转化为前缀和 $S_n$ 的排序问题。这一步转化只能说精妙，想到了豁然开朗，没想到百思不解。
2. 寻找合适的贪心策略。一开始，我的想法是固定 `S.end()`，每一位都向前寻找与其差最小数并将其交换到当下位的前一位，并以交换后的这一位开始下一次重复操作。然而我忽略了 `S.begin()` 的绝对值也要算入其中，虽然每一个差项都是贪心的，但开始的特殊项却是一项我没有考虑到的意外。以下是错误代码： 

```cpp
#include <iostream>
#include <vector>
#include <cmath> //abs头文件
#include <algorithm> // swap函数头文件

using namespace std;

int main() {
    int tt;
    cin >> tt;
    while (tt--) {
        int n, num;
        long long dif_max = -1;
        long long sum = 0;
        vector<long long> S;
        cin >> n;
        while (n--) {
            cin >> num;
            sum += num;
            S.push_back(sum);
        }
        for (auto i = S.end(); i > S.begin(); i--) {
            long long dif_min = abs(*(i - 1) - *i);
            auto min_it = i - 1;
            for (auto j = i - 1; j >= S.begin(); j--) {
                if (abs(*i - *j) < dif_min) {
                    dif_min = abs(*i - *j);
                    min_it = j;
                }
            }
            if (min_it != i - 1) {
                swap(*(i - 1), *min_it);
            }
            if (dif_min > dif_max) {
                dif_max = dif_min;
            }
        }
        if (abs(*S.begin()) > dif_max) {
            dif_max = abs(*S.begin());
        }
        cout << dif_max << '\n';
    }
}
```

这与编程艺术背道而驰。把这段代码放在这里是为了提醒自己曾经的代码风格是多么烂，而烂风格的代码是多么难读。混作一团的过程让 debug 变得尤为艰难费时。即使是面向过程，优秀的代码也应当像函数块一样地分条缕析，而非为了微不足道的时间复杂度而使可读性大打折扣（事实上，上面的做法也没有改变时间复杂度）。 

说回贪心策略，为了形式的统一化，在数组前面补出一个 `S[0] = 0`，这样就相当于固定头尾，只算差值的绝对值了。而绝对值的计算，则是先从小到大排好，以两个固定的点中较小的为起点，较大的为终点。贪心策略是图像一定是先排到最小，再拍到最大，再排到终点，像是 $y = -sin(x)$ 在 $[0,-2\pi]$ 的图像。对于要分成两段的部分，先隔一个取一个组成一段，剩下的就是另一段。这里有两个遗留问题：1. 为什么是这样的图像。2. 为什么是隔一个取一个。思路来自：[B 站 y 总的讲解视频](https://www.bilibili.com/video/BV1Mb411t7M1/?spm_id_from=333.880.my_history.page.click&vd_source=03836a37b30756921d327ea531c18b88) 。以下是 AC 代码：

```c++
#include <iostream>
#include <cstdio>
#include <vector>
#include <cmath> //abs头文件
#include <algorithm> // swap,sort函数头文件
using namespace std;
typedef long long LL;
int main() {
    int tt;
    cin >> tt;
    while (tt--) {
        int n;
        scanf("%d", &n);
        vector<LL> S(n + 1);
        S[0] = 0;
        for (int i = 1; i <= n; i++) {
            scanf("%lld", &S[i]);
            S[i] += S[i - 1];
        }
        if(S[0] > S[n]) swap(S[0], S[n]);
        LL S0 = S[0];
        LL Sn = S[n];
        sort(S.begin(), S.end());
        for (int i = 0; i <= n; i++) {
            if (S[i] == S0) {
                S0 = i;
                break;
            }
        }
        for (int i = 0; i <= n; i++) {
            if (S[i] == Sn) {
                Sn = i;
                break;
            }
        }
        vector<bool> exist(n + 1, true);
        vector<LL> sorted(n + 1 );
        int l = 0, r = n;
        for (int i = (int)S0; i >= 0; i -= 2) {
            sorted[l++] = S[i];
            exist[i] = false;
        }
        for (int i = (int)Sn; i <= n; i += 2) {
            sorted[r--] = S[i];
            exist[i] = false;
        }
        for (int i = 0; i <= n; i++) {
            if (exist[i]) {
                sorted[l++] = S[i];
            }
        }
        LL dif_max = 0;
        for (int i = 0; i < n; i++) dif_max = max(dif_max, abs(sorted[i] - sorted[i + 1]));
        cout << dif_max << '\n';
    }
    return 0;
}
```

# Day 3

题单：[SMU Winter 2023 Round #1 (Div.2)](https://www.luogu.com.cn/contest/96294#problems) 

这是一次 ACM 赛制的训练赛，我在通过相同题数的排名里是最低的，因为我罚时最多。所以要提高**写代码的速度**，尽量减少错误的提交。这次比赛中我没有想太多思路上的东西，有几个真正的算法题也没有写，如果都是这样做题，恐怕难有进步吧。

以下是补题：

## [做不完的作业](https://www.luogu.com.cn/problem/P8508) 

一道比较典型的贪心。然而我常常不能克服自己的强迫症，总想往动态规划上面想。其实贪心是很可以锻炼直觉、归谬和证明能力的。这题水水地边想边写搞了好久都没搞出来，诚如 y 总所言：

> 题目有思考难度和代码难度，不要忽视思考的过程。其实到了后面就会发现，思路出来了代码就出来了，做不出题往往都是没想出来。

确实是这样，如果一个人清楚地知道整个问题到底是什么样，每一步该干什么，胸有成竹了，也不至于磨磨蹭蹭半天写不出代码，其实这样的人往往都是没想透问题就开始写。如果连问题都没想明白，即使写出来又有什么用呢？不要把时间浪费在不明所以的地方，对于一道题目，给定适合的思考时间，仔细想想怎么样可以完成，题目的本质是什么，是否可以优化。想出来了就飞快地写，想不出来说明自己还不配，谦虚地看看别人是怎么做的，完全理解之后就自己飞快地写一遍，千万不要再莫名其妙地浪费大把时间了。`做不完的作业`，倒是很符合我的现状。

思路是倒序做任务，再看前面要睡几天觉。写了几天没写出来，算了。以下是卢佳奇的 AC 代码：

```cpp
#include<cstdio>
#define int long long
int n,x,p,q,a,day=1,sleep=0,nw;

inline int read(){
	int x=0;char ch=getchar();
	while(ch<'0'||ch>'9') ch=getchar();
	while(ch>='0'&&ch<='9') x=(x<<1)+(x<<3)+(ch^48),ch=getchar();
	return x;
}
void write(int x){
	if(x>9) write(x/10);
	putchar(x%10+'0');
}

signed main(){
	n=read();x=read();p=read();q=read();
	nw=x;
	while(n--){
		a=read();
		if(nw-a<1||(sleep+(nw-a))*q<day*x*p) sleep+=nw,day++,nw=x;
		int k=day*x*p-(sleep+(nw-a))*q;
		if(k>0) sleep+=k/(x*q-x*p)*nw,day+=k/(x*q-x*p);
		while(nw-a<1||(sleep+(nw-a))*q<day*x*p) sleep+=nw,day++,nw=x;
		nw-=a;
	}
	write(day);
	return 0;
}
```



# Day 5

题单：[SMU Winter 2023 Round #2 (Div.2)](https://codeforces.com/group/L9GOcnr1dm/contest/418722) 

第一次英文的比赛，CF 的题目解释地挺好的，不至于看不懂。做到后面心态有点炸了，一个贪心题用暴力做，结果 TLE 了；还有一题写了一个小时都不对，有一个细节错误没找出来，比赛刚结束就找出来了，属实毁心态。

以下是补题：

## [K. Thermostat](https://codeforces.com/group/L9GOcnr1dm/contest/418722/problem/K) 

分类讨论各种可能的情况，用**卫语句**写可读性会好很多。有一部分情况可以递归，就写成函数了。以下是 AC 代码：

```cpp
#include <iostream>
#include <cmath>
using namespace std;
int work(int l, int r, int x, int a, int b) {
    if (a == b) {
        return 0;
    }
    if (b < l || b > r) {
        return -1;
    }
    if (abs(a - b) >= x) {
        return 1;
    }
    if (abs(l - r) < x) {
        return -1;
    }
    if (b == l) {
        if (abs(a - r) < x) {
            return -1;
        } else {
            return 2;
        }
    }
    if (b == r) {
        if (abs(a - l) < x) {
            return -1;
        } else {
            return 2;
        }
    }
    if (abs(b - l) >= x && abs(b - r) >= x) {
        return 2;
    }
    if (abs(b - l) < x && abs(b - r) < x) {
        return -1;
    }
    if (abs(b - l) >= x) {
        int ans = work(l, r, x, a, l);
        if (ans == -1) {
            return -1;
        } else {
            return 1 + ans;
        }
    }
    if (abs(b - r) >= x) {
        int ans = work(l, r, x, a, r);
        if (ans == -1) {
            return -1;
        } else {
            return 1 + ans;
        }
    }
}

int main() {
    int tt;
    cin >> tt;
    while (tt--) {
        int l, r, x, a, b;
        cin >> l >> r >> x >> a >> b;
        cout << work(l, r, x, a, b) << endl;
    }
    return 0;
}
```

## [F. Binary Inversions](https://codeforces.com/group/L9GOcnr1dm/contest/418722/problem/F) 

### 实现

这题的本质是：对第 $i$ 位执行 $1\rightarrow0$ ，则对整体的改变就是加上 $i$ 左边所有的 1 和减去 $i$ 右边所有的的 0；执行 $0\rightarrow1$ 则相反。因此，只要遍历数组，找出 plus 的最大值，如果大于 0，就加在 sum 上。

### 优化

一个可悲的事实是，上述暴力做法会 TLE。

1. 要进行优化，核心思路是以空间换时间。具体的做法是，开辟一个数组 $s$ 用来记录前缀和，它有两个好处：
   * 利用记忆快速地求出 sum，将 $O(n^2)$ 优化为 $O(n)$；
   * 快速求出第 $i$ 位左边所有的 1 和右边所有的 0，将两个 $O(n)$ 优化为 $O(1)$。
2. 当从 0 转变成 1，$plus=r0-l1$，可以发现，如果如果要这么转，一定是转最左边的 0；同理，如果要从 1 转变成 0 ，一定是转最右边的 1。因此，只要算首个 0 和最后一个 1 的 plus，取最大值即可；
3. 利用 `getchar` 快于 `cin` 和 `scanf` 自己编写输入函数。

以下是 AC 代码：（提示：对于 $plus=r0-l1$ 和 $plus=l1-r0$，都可以在用 $i$、$n$ 和数组 $s$表达后化简）：

```cpp
#include <iostream>
#include <cstdio>
#include <vector>
#include <algorithm>
using namespace std;
int read() {
    char ch;
    while ((ch = getchar()) == ' ' || ch == '\n'){
        continue;
    }
    if (ch == '1') return 1;
    return 0;
}
int main() {
    int tt;
    cin >> tt;
    while (tt--) {
        int n;
        long long sum = 0;
        int first_0_idx = -1;
        int last_1_idx = -1;
        scanf("%d", &n);
        vector<int> a(n, 0) ,s(n, 0);
        for (int i = 0; i < n; i++) {
            a[i] = read();
            s[i] += a[i];
            if (!i) continue;
            s[i] += s[i - 1];
        }
        for (int i = 1; i < n; i++) {
            if (!a[i]) sum += s[i - 1];
        }
        first_0_idx = (int)(find(a.begin(), a.end(), 0) - a.begin());
        int plus = 0;
        if (first_0_idx != -1) {
            plus = max(plus, n - first_0_idx - 1 - s[n - 1]);
        }
        for (int i = n - 1; i > 0; i--) {
            if (a[i]) {
                last_1_idx = i;
                break;
            }
        }
        if (last_1_idx != -1) {
            plus = max(plus, s[last_1_idx - 1] - n + last_1_idx + 1);
        }
        printf("%lld\n", (long long)plus + sum);
    }
    return 0;
}
```

# 总结

其实我是比较反感过于频繁的考试和比赛的，经常学习的效益显然大于经常考试，然而二者花费的时间却可能相同。因为考试和比赛归根结底只是对已有知识的检验和对已有技能的运用，只有学习才能获取更多的知识和理解已有技能、学习更多技能。当然，我并不否定适当考试和比赛的价值。比考试成绩和比赛排名更重要的，是进步。如何进步？补题当然是一种好方法，但如果每次都是在比赛中第一次碰到某个算法，当时想不出来，事后花时间仅仅补这一题，而非对应的算法，那未免也太痛苦了。