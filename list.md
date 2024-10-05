# 线性数据结构之——链表
单向链表在每个存储单元中存储指向下一个存储单元的指针，而双向链表在此基础上会额外存储到上一个存储单元的指针。

## 性能
链表的优越性在于在指定位置插入或删除元素，时间复杂度 $O(1)$。

但是链表的随机访问时间复杂度为 $O(n)$，因为要访问一个元素需要从链表头开始遍历，直到抵达需要访问的元素。

## 链表特有的应用

## [例题：洛谷P1996-约瑟夫问题](https://www.luogu.com.cn/problem/P1996)

## 题目描述

$n$ 个人围成一圈，从第一个人开始报数,数到 $m$ 的人出列，再由下一个人重新从 $1$ 开始报数，数到 $m$ 的人再出圈，依次类推，直到所有的人都出圈，请输出依次出圈人的编号。

**注意：本题和《深入浅出-基础篇》上例题的表述稍有不同。书上表述是给出淘汰 $n-1$ 名小朋友，而该题是全部出圈。**

## 输入格式

输入两个整数 $n,m$。

## 输出格式

输出一行 $n$ 个整数，按顺序输出每个出圈人的编号。

## 样例 #1

### 样例输入 #1

```
10 3
```

### 样例输出 #1

```
3 6 9 2 7 1 8 5 10 4
```

## 提示

$1 \le m, n \le 100$

## 题解
单向链表在删除元素时只需将上一个存储单元的指针指向原本指向的单元的下一个即可。
```cpp
#include<iostream>
using namespace std;

struct lnode{
    int data;
    lnode* next;
};
lnode nodes[100];

int main(){
    int n, m;
    cin >> n >> m;
    for(int i = 0; i < n; ++i){
        nodes[i].data = i + 1;
        nodes[i].next = &nodes[i + 1];
    }
    // 我们可以让链表尾元素的指针指向头元素，因为没人规定不能这样
    nodes[n - 1].next = &nodes[0]; 
    // 删除元素需要修改上一个存储单元，可以通过提前一个元素来实现，但是不这样的话我们可以存储上一个存储单元到本存储单元的指针的指针，我们可以通过这个指针来修改上一个存储单元
    lnode** s = &nodes[n - 1].next; 
    for(int _ = 0; _ < n; ++_){
        // 只位移m - 1次，因为下一次就是删除元素了
        for(int i = 1; i < m; ++i)s = &(*s)->next;
        cout << (*s)->data << ' ';
        *s = (*s)->next;
    }
}
```

## 链式前向星
我们可以有数组索引来代替指针的作用，用这个原理来重写上一题：
```cpp
#include<iostream>
using namespace std;

int dat[100];
int nxt[100];

int main(){
    int n, m;
    cin >> n >> m;
    for(int i = 0; i < n; ++i){
        dat[i] = i + 1;
        nxt[i] = i + 1;
    }
    nxt[n - 1] = 0;
    int s = n - 1;
    for(int _ = 0; _ < n; ++_){
        for(int i = 1; i < m; ++i)s = nxt[s];
        // 还记得数组的[]操作具有交换律吗？这就是它能消除方括号嵌套的情景了，dat[nxt[s]] == nxt[s][dat] == s[nxt][dat]
        cout << s[nxt][dat] << ' ';
        s[nxt] = s[nxt][nxt];
    }
}
```
后面为了方便我会使用链式前向星的写法来写单向链表。

## ST表：将链表化为数组

聪明的小伙伴可能已经发现了，要访问链表的第 $i$ 个元素，相当于对链表头求 $i - 1$ 次 $nxt$ ，而函数组合刚好具有结合律，那么我们可以以类似快速幂的思想，将 $nxt^2$，$nxt^4$, $nxt^8$ 等 $nxt$ 的 $2^n$ 次幂记录下来，从而实现链表的快速随机访问，空间复杂度和时间复杂度均为 $O(nlog(n))$。

但是这样的话就会牺牲链表的增删时间复杂度，所以结论是为什么不直接用数组呢？下面是一道利用了上文思想的例题：

## [例题：洛谷P3865-【模板】ST 表 && RMQ 问题](https://www.luogu.com.cn/problem/P3865)

## 题目背景

这是一道 ST 表经典题——静态区间最大值

**请注意最大数据时限只有 0.8s，数据强度不低，请务必保证你的每次查询复杂度为 $O(1)$。若使用更高时间复杂度算法不保证能通过。**

如果您认为您的代码时间复杂度正确但是 TLE，可以尝试使用快速读入：

```cpp
inline int read()
{
	int x=0,f=1;char ch=getchar();
	while (ch<'0'||ch>'9'){if (ch=='-') f=-1;ch=getchar();}
	while (ch>='0'&&ch<='9'){x=x*10+ch-48;ch=getchar();}
	return x*f;
}
```

函数返回值为读入的第一个整数。

**快速读入作用仅为加快读入，并非强制使用。**

## 题目描述

给定一个长度为 $N$ 的数列，和 $ M $ 次询问，求出每一次询问的区间内数字的最大值。

## 输入格式

第一行包含两个整数 $N,M$，分别表示数列的长度和询问的个数。

第二行包含 $N$ 个整数（记为 $a_i$），依次表示数列的第 $i$ 项。

接下来 $M$ 行，每行包含两个整数 $l_i,r_i$，表示查询的区间为 $[l_i,r_i]$。

## 输出格式

输出包含 $M$ 行，每行一个整数，依次表示每一次询问的结果。

## 样例 #1

### 样例输入 #1

```
8 8
9 3 1 7 5 6 0 8
1 6
1 5
2 7
2 6
1 8
4 8
3 7
1 8
```

### 样例输出 #1

```
9
9
7
7
9
8
7
9
```

## 提示

对于 $30\%$ 的数据，满足 $1\le N,M\le 10$。

对于 $70\%$ 的数据，满足 $1\le N,M\le {10}^5$。

对于 $100\%$ 的数据，满足 $1\le N\le {10}^5$，$1\le M\le 2\times{10}^6$，$a_i\in[0,{10}^9]$，$1\le l_i\le r_i\le N$。

## 题解
对于每个 $i$ 和 $[0, logN]$ 中的 $j$，存储 $a_i$ 到 $a_{i+2^j-1}$ 的最大值，以类似快速幂的方式进行查询即可。
```cpp
#include<iostream>
using namespace std;
int mx[17][(int)1e5];
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    int n, m;
    cin >> n >> m;
    for(int i = 0; i < n; ++i)cin >> mx[0][i];
    for(int i = 1; i < 17; ++i){
        for(int j = 0; j < n; ++j){
            mx[i][j] = mx[i - 1][j];
            if(j + (1 << i - 1) < n)
                mx[i][j] = max(mx[i][j], mx[i - 1][j + (1 << i - 1)]);
        }
    }
    while(m--){
        int l, r; cin >> l >> r;
        --l;
        int ans = 0;
        for(int i = 0; i < 17; ++i){
            if((r - l >> i) & 1){
                ans = max(ans, mx[i][l]);
                l += 1 << i;
            }
        }
        cout << ans << '\n';
    }
}
```

## 碎碎念
实际上用ST表来实现链表随机访问并非没有任何用，以后学习树上算法的时候求 $n$ 次父节点的时候就会用到。

## 可持久化链表

将链表看作一个静态的数据结构，那么它可能为一个空链表，或者一个数据元素和它的剩余链表。

其实链表可以做到时间复杂度和空间复杂度均为 $O(nlogn)$ 的归并排序，当然如果你希望的话其实可以进一步优化空间复杂度。

## [例题：洛谷P1177-【模板】排序](https://www.luogu.com.cn/problem/P1177)

## 题目描述

将读入的 $N$ 个数从小到大排序后输出。

## 输入格式

第一行为一个正整数 $N$。

第二行包含 $N$ 个空格隔开的正整数 $a_i$，为你需要进行排序的数。

## 输出格式

将给定的 $N$ 个数从小到大输出，数之间空格隔开，行末换行且无空格。

## 样例 #1

### 样例输入 #1

```
5
4 2 4 5 1
```

### 样例输出 #1

```
1 2 4 4 5
```

## 提示

对于 $20\%$ 的数据，有 $1 \leq N \leq 10^3$；

对于 $100\%$ 的数据，有 $1 \leq N \leq 10^5$，$1 \le a_i \le 10^9$。

## 题解

利用归并排序算法，将序列的前半部分和后半部分排好序，然后将这两部分合并成一个有序的序列即可。

```cpp
#include<iostream>
using namespace std;
// 本程序用int来表示链表，0表示空链表
int h[(int)1e7]; // h[i]表示i的头元素（若i非空）
int t[(int)1e7]; // t[i]表示i的剩余链表（若i非空）
int nl = 0; // 自增索引
int cons(int hd, int tl){ // 构造链表
    ++nl;
    nl[h] = hd;
    nl[t] = tl;
    return nl;
}
// 这个函数的功能是返回反转的a后面拼接b的结果，不会改变a和b
int rev(int a, int b){ 
    if(!a)return b;
    return rev(a[t], cons(a[h], b));
}
int len(int a){ // 返回链表的长度
    if(!a)return 0;
    return 1 + len(a[t]);
}
// 这个函数的功能是将f设为a的前p个元素组成的链表，r设为剩余链表
void split(int a, int p, int& f, int& r){
    if(!p){
        f = 0;
        r = a;
        return;
    }
    int rf;
    split(a[t], p - 1, rf, r);
    f = cons(a[h], rf);
}
// 这个函数的功能为将两个有序链表a, b合并为一个有序链表，再在前面拼接反转的r
int merge(int a, int b, int r){
    if(!a)return rev(r, b);
    if(!b)return rev(r, a);
    if(a[h] > b[h])return merge(a, b[t], cons(b[h], r));
    return merge(a[t], b, cons(a[h], r));
}
// 归并排序
int merge_sort(int a){
    int l = len(a);
    if(l <= 1)return a;
    int f, r;
    split(a, l / 2, f, r);
    return merge(merge_sort(f), merge_sort(r), 0);
}
int main(){
    int n;
    cin >> n;
    int a = 0;
    while(n--){
        int x; cin >> x;
        a = cons(x, a);
    }
    int b = merge_sort(a);
    for(int i = b; i; i = i[t])cout << i[h] << ' ';
}
```