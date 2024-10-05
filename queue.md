# 线性数据结构之——队列
队列是先进先出的线性表。

区别于栈，队列的`push`会将元素添加至队尾，`pop`操作则会删除队首的元素，同时也需要有`front`操作来获取队首的元素。

# [例题：洛谷P1443-马的遍历](https://www.luogu.com.cn/problem/P1443)

## 题目描述

有一个 $n \times m$ 的棋盘，在某个点 $(x, y)$ 上有一个马，要求你计算出马到达棋盘上任意一个点最少要走几步。

## 输入格式

输入只有一行四个整数，分别为 $n, m, x, y$。

## 输出格式

一个 $n \times m$ 的矩阵，代表马到达某个点最少要走几步（不能到达则输出 $-1$）。

## 样例 #1

### 样例输入 #1

```
3 3 1 1
```

### 样例输出 #1

```
0    3    2    
3    -1   1    
2    1    4
```

## 提示

### 数据规模与约定

对于全部的测试点，保证 $1 \leq x \leq n \leq 400$，$1 \leq y \leq m \leq 400$。

# 题解

考虑广度优先搜索，只要在搜索的时候避开之前已到达的点，那么到达任意点的步数都会是最少的。利用队列可以实现广度优先搜索。

```cpp
#include<iostream>
#include<queue> // C++标准库中的队列容器
using namespace std;
int ans[404][404]; // 上下左右预留两格为了边界条件

int main(){
    int n, m;
    cin >> n >> m;
    for(int i = 2; i < n + 2; ++i){
        fill(&ans[i][2], &ans[i][m + 2], -1); // 将棋盘格子初始值设为-1
    }
    int x, y;
    cin >> x >> y;
    // 由于预留了两格，所以实际坐标会比原坐标多1
    ans[x + 1][y + 1] = 0;
    queue<pair<int, int>> q;
    q.push(make_pair(x + 1, y + 1));
    while(!q.empty()){
        auto p = q.front();
        q.pop();
        int x = p.first, y = p.second, v = ans[x][y] + 1;
        // 利用lambda表达式减少重复代码，注意q的捕获方式应为引用（或者把q写成全局变量也行）
        auto procp = [v, &q](int x, int y){
            if(ans[x][y] != -1)return;
            ans[x][y] = v;
            q.push(make_pair(x, y));
        };
        procp(x + 1, y + 2);
        procp(x + 2, y + 1);
        procp(x - 1, y + 2);
        procp(x - 2, y + 1);
        procp(x + 1, y - 2);
        procp(x + 2, y - 1);
        procp(x - 1, y - 2);
        procp(x - 2, y - 1);
    }
    for(int i = 2; i < n + 2; ++i){
        for(int j = 2; j < m + 2; ++j){
            cout << ans[i][j] << ' ';
        }
        cout << '\n';
    }
}
```

# 队列变体之——双端队列
某种意义上我们可以说栈是只能`pop`队尾元素的队列，而队列是只能`push`到栈底的栈，集队列和栈的特点于一体，既能在队首`push`，也能在队尾`push`，既能在队首`pop`，也能在队尾`pop`，这样的数据结构就是双端队列。

# [例题：洛谷P1886-滑动窗口 /【模板】单调队列](https://www.luogu.com.cn/problem/P1886)

## 题目描述

有一个长为 $n$ 的序列 $a$，以及一个大小为 $k$ 的窗口。现在这个从左边开始向右滑动，每次滑动一个单位，求出每次滑动后窗口中的最大值和最小值。

例如，对于序列 $[1,3,-1,-3,5,3,6,7]$ 以及 $k = 3$，有如下过程：
|窗口位置|最小值|最大值|
|---|:---:|:---:|
|`[1   3  -1] -3   5   3   6   7 `|`-1`|`3`|
|` 1  [3  -1  -3]  5   3   6   7 `|`-3`|`3`|
|` 1   3 [-1  -3   5]  3   6   7 `|`-3`|`5`|
|` 1   3  -1 [-3   5   3]  6   7 `|`-3`|`5`|
|` 1   3  -1  -3  [5   3   6]  7 `|`3`|`6`|
|` 1   3  -1  -3   5  [3   6   7]`|`3`|`7`|

## 输入格式

输入一共有两行，第一行有两个正整数 $n,k$。
第二行 $n$ 个整数，表示序列 $a$

## 输出格式

输出共两行，第一行为每次窗口滑动的最小值   
第二行为每次窗口滑动的最大值

## 样例 #1

### 样例输入 #1

```
8 3
1 3 -1 -3 5 3 6 7
```

### 样例输出 #1

```
-1 -3 -3 -3 3 3
3 3 5 5 6 7
```

## 提示

【数据范围】    
对于 $50\%$ 的数据，$1 \le n \le 10^5$；  
对于 $100\%$ 的数据，$1\le k \le n \le 10^6$，$a_i \in [-2^{31},2^{31})$。
## 题解
考虑求最大值，因为求最大值和求最小值是相似的。

类似于上一节的[单调栈](https://www.luogu.com.cn/problem/P5788)，在滑动窗口的时候如果发现某个数右侧第一个大于等于某个数的数的下标小于等于窗口右端点，那么这个数在接下来的窗口滑动过程中对于窗口最大值将没有任何贡献，因为大于等于它的一个数将更晚离开窗口。

所以我们可以沿用[单调栈](https://www.luogu.com.cn/problem/P5788)的思路，窗口向右滑动时以类似的方式维护一个单调的双端队列，至于它是双端队列而不是栈是因为我们需要把窗口左侧离开窗口的数`pop`出队列，而队首的数就是窗口的最大值。

```cpp
#include<iostream>
#include<deque> // C++标准库的双端队列容器
#include<ranges>
using namespace std;
int arr[(int)1e6];

int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    int n, k;
    cin >> n >> k;
    // 并没有什么用的小知识之<ranges>里的std::views::iota(b, e)可以生成[b, e)的序列，而且这是C++20才有的特性
    for(int i : views::iota(0, n))cin >> arr[i];
    deque<int> dq;
    auto push = [&dq](int i, auto cmp){
        while(!dq.empty() && cmp(arr[i], arr[dq.back()])){
            dq.pop_back();
        }
        dq.push_back(i);
    };
    auto prans = [n, k, &push, &dq](auto cmp){
        dq.clear();
        for(int i : views::iota(0, k))push(i, cmp);
        cout << arr[dq.front()] << ' ';
        for(int i : views::iota(0, n - k)){
            push(i + k, cmp);
            if(i >= dq.front())dq.pop_front();
            cout << arr[dq.front()] << ' ';
        }
        cout << '\n';
    };
    prans(less_equal{});
    prans(greater_equal{});
}
```

# [最后也是最糟糕的线性数据结构会是谁呢？](./index.html?page=list)