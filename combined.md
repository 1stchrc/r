# 线性数据结构之——数组

“数组是一段连续的内存空间。”

## 功能

数组可以存储若干个相同数据类型的元素，有固定上限，可以通过索引进行查询和修改。对于长度为 $n$ 的数组，其索引为 $[0, n)$ 中的整数。

```cpp
#include<iostream>

int global_array[123]; // 长度为123的全局int数组，最多可以储存123个int元素，占据静态存储空间，初始值为0

int main(){
    double local_array[10]; // 长度为10的局部double数组，占据栈空间，初始值随机
    
    std::cout << sizeof(global_array) << '\n'; // sizeof(int) * 123
    std::cout << sizeof(local_array) << '\n'; // sizeof(double) * 10

    std::cout << global_array[0] << '\n'; // 输出global_array索引为0的元素，此处为0
    std::cout << local_array[0] << '\n'; // 此处的输出为随机值
    
    local_array[0] = 1.0; // 将local_array索引0处的值设为1.0
    for(int i = 1; i < 10; ++i){
        local_array[i] = local_array[i - 1] * i; //递推地生成阶乘序列
    } // 此时对于[0, 10)中的i，local_array[i] == i!
    for(int i = 0; i < 10; ++i){
        std::cout << local_array[i] << ' '; // 输出阶乘序列
    }
    std::cout << '\n';
}
```

## 性能

### 时间复杂度

查询（通过索引）：$O(1)$

修改（通过索引）：$O(1)$

### 空间复杂度 : $O(n)$

## 碎碎念

C++标准库提供了`std::array`和`std::vector`两个对数组的封装，其中`std::array`相比于数组只多了一些类型信息，而`std::vector`则可以动态增加长度，其原理是申请一片更大的内存空间再将原本的内容复制过去，所以其扩容的效率可能不尽如人意，但是可以用它的`reserve`方法来改善。

## 注意事项

访问数组索引范围外的元素造成的后果是不可被预测的，但是通常表现为 $WA$ 或者 $RE$ 。

错误示范：
```cpp
int foo[10];

foo[-1] = 8; // bad!
foo[10] = 9; // also bad!
```

尽量不要将数组命名为`array`，因为`std`命名空间下有个模板类型叫`array`，这会在`using namespace std;`的情况下产生问题。

## Fun Fact

数组的下标访问运算符（`[ ]`）具有交换律：
```cpp
int bar[33];
bar[30] = 233;
std::cout << 30[bar] << '\n'; // 233
```
通常情况下，将索引放在方括号前可以起到激怒同事的效果。

但是在某些情况下这个特性可以被用来避免方括号嵌套，我还是挺喜欢的。

# [例题：洛谷B2089-数组逆序重存放](https://www.luogu.com.cn/problem/B2089)

## 题目描述

将一个数组中的值按逆序重新存放。例如，原来的顺序为 $8,6,5,4,1$。要求改为 $1,4,5,6,8$。

## 输入格式

输入为两行：第一行数组中元素的个数 $n$（$1 \lt n \le 100)$，第二行是 $n$ 个整数，每两个整数之间用空格分隔。

## 输出格式

输出为一行：输出逆序后数组的整数，每两个整数之间用空格分隔。

## 样例 #1

### 样例输入 #1

```
5
8 6 5 4 1
```

### 样例输出 #1

```
1 4 5 6 8
```

## 题解
这题做不出来的话还是别打算法竞赛了...
#### 代码：
```cpp
#include<iostream>
using namespace std;
int arr[100];
int main(){
    int n;
    cin >> n;
    for(int i = 0; i < n; ++i)cin >> arr[i];
    for(int i = n - 1; i >= 0; --i)cout << arr[i] << ' ';
}
```

# 线性数据结构之——栈

栈是后进先出的线性表，支持`push`，`pop`和`top`操作。

`push`操作为在栈顶上加入一个元素，成为新的栈顶。

`pop`操作为将栈顶元素从栈中移除，此时原先栈顶下面的元素成为新的栈顶。

`top`操作为获取栈顶元素。

## [例题：洛谷B3614-【模板】栈](https://www.luogu.com.cn/problem/B3614)

## 题目描述

请你实现一个栈（stack），支持如下操作：
- `push(x)`：向栈中加入一个数 $x$。
- `pop()`：将栈顶弹出。如果此时栈为空则不进行弹出操作，输出 `Empty`。
- `query()`：输出栈顶元素，如果此时栈为空则输出 `Anguei!`。
- `size()`：输出此时栈内元素个数。

## 输入格式

**本题单测试点内有多组数据**。  
输入第一行是一个整数 $T$，表示数据组数。对于每组数据，格式如下：  
每组数据第一行是一个整数，表示操作的次数 $n$。  
接下来 $n$ 行，每行首先由一个字符串，为 `push`，`pop`，`query` 和 `size` 之一。若为 `push`，则其后有一个整数 $x$，表示要被加入的数，$x$ 和字符串之间用空格隔开；若不是 `push`，则本行没有其它内容。

## 输出格式

对于每组数据，按照「题目描述」中的要求依次输出。每次输出占一行。

## 样例 #1

### 样例输入 #1

```
2
5
push 2
query
size
pop
query
3
pop
query
size
```

### 样例输出 #1

```
2
1
Anguei!
Empty
Anguei!
0
```

## 提示

### 样例 1 解释
对于第二组数据，始终为空，所以 `pop` 和 `query` 均需要输出对应字符串。栈的 size 为 0。

### 数据规模与约定

对于全部的测试点，保证 $1 \leq T, n\leq 10^6$，且单个测试点内的 $n$ 之和不超过 $10^6$，即 $\sum n \leq 10^6$。保证 $0 \leq x \lt 2^{64}$。

### 提示
- 请注意大量数据读入对程序效率造成的影响。
- 因为一开始数据造错了，请注意输出的 `Empty` 不含叹号，`Anguei!` 含有叹号。

## 题解

考虑利用数组来实现栈。

维护栈中元素的个数$sz$，`push x`操作将数组索引$sz$处的元素设为`x`再将$sz$增加 $1$，`pop`操作将$sz$减去 $1$，`top`操作获取数组索引$sz-1$ 处的元素。
#### 代码：
```cpp
#include<iostream>
#include<string>
using namespace std;
using ll = unsigned long long; // 相当于typedef unsigned long long ll;
ll arr[(int)1e6]; // 由于操作次数不超过1e6，所以栈中元素个数不会超过1e6，所以可以将数组长度设为1e6

int main(){
    ios::sync_with_stdio(0); cin.tie(0); // 增加读入速度
    int T; cin >> T;
    while(T--){
        int n; cin >> n;
        int sz = 0;
        while(n--){
            string opt;
            cin >> opt;
            if(opt == "push"){
                cin >> arr[sz++];
                continue;
            }
            if(opt == "pop"){
                if(sz == 0)
                    cout << "Empty\n";
                else --sz;
                continue;
            }
            if(opt == "query"){
                if(sz == 0)
                    cout << "Anguei!\n";
                else cout << arr[sz - 1] << '\n';
                continue;
            } // 只有四种操作，所以剩下的一种一定是size
            cout << sz << '\n';
        }
    }
}
```

## 性能
### 时间复杂度
`push`: $O(1)$

`pop`: $O(1)$

`top`: $O(1)$
### 空间复杂度：$O(n)$ （$n$ 为最大元素个数）

## 碎碎念
C++标准库提供了`std::stack`作为栈容器，作为一个没什么用的小知识。

# 应用

## [例题：洛谷P10473-表达式计算4](https://www.luogu.com.cn/problem/P10473)

## 题目描述

给出一个表达式，其中运算符仅包含 +，-，*，/，^，要求求出表达式的最终值。

数据可能会出现括号情况，还有可能出现多余括号情况。

数据保证不会出现超过 int 范围的数据，数据可能会出现负数情况。

## 输入格式

仅一行一个字符串，即为表达式。

## 输出格式

仅一行，既为表达式算出的结果。

## 样例 #1

### 样例输入 #1

```
(2+2)^(1+1)
```

### 样例输出 #1

```
16
```

## 提示

表达式总长度不超过 $30$。

## 题解

考虑记录表达式暂时未参与计算的部分，而参与计算的部分则在第一时间被计算。

比如说在处理 $1+2*3+4$ 时，从左往右读，读取到 $1+2$ 时此时我们不能确定这个 $1+2$ 是否该被计算，这取决于下一个运算符的优先级，所以我们发现当下一个运算符的优先级低于当前运算符时，我们应当立刻计算当前运算符。当我们读取到 $1+2*3+...$ 时，可以知道应当计算 $2*3$，此时记录成为 $1+6+...$，我们可以规定加法为左结合的，那么此时可以继续计算 $1+6$，记录变为 $7+...$。不难发现未参与计算的部分的结合方式必然是形如 $a+(b-(c*(d/...)))$ 形式的，或者说右结合。

所以我们可以分别为操作数和运算符开两个栈，读取到操作数就将数压入栈，读取到运算符就根据当前栈顶运算符的优先级来决定是否该进行计算。

对于括号，我们可以在读取到左括号的时候往运算符栈里压入一个特殊符号（后面的代码使用左括号），当栈顶为该符号时对其的处理类似于对栈为空时的处理，防止其左边的运算符被计算，直到读入右括号，此时将最后一个左括号右边的部分全部计算，并让左括号出栈。

对于负号，我们可以将其当作前缀函数来处理，当我们根据上下文确定读入的字符不可能是运算符时读入了负号，就将其压入前缀函数栈，当有操作数入栈但前缀函数不为空时，就一直对其进行前缀函数栈顶的操作。

对于前缀函数和括号的情况的处理与运算符类似，也是利用特殊符号来防止其左边的部分被计算。
#### 代码：
```cpp
#include<iostream>
using namespace std;
int fexp(int a, int b){ //快速幂
    int r = 1;
    for(int i = 0; i < 32; ++i){
        if((b >> i) & 1)r *= a;
        a *= a;
    }
    return r;
}
int priority(char opt){ // 运算符优先级
    switch(opt){
        case '+': return 1;
        case '-': return 1;
        case '*': return 2;
        case '/': return 2;
        case '^': return 3;
    }
    return 0; 
}
bool islcombine(char opt){ // 运算符是否为左结合
    switch(opt){
        case '+': return true;
        case '-': return true;
        case '*': return true;
        case '/': return true;
        case '^': return false;
    }
    return false; 
}
int opts[31], nums[31], funs[31], nopts, nnums, nfuns;
void eval_opt(){ // 计算运算符
    int b = nums[--nnums];
    int a = nums[--nnums]; // 由于栈是后进先出，所以先pop出来的放在运算符右侧
    char opt = opts[--nopts];
    switch(opt){
        case '+':
            nums[nnums++] = a + b;
            break;
        case '-':
            nums[nnums++] = a - b;
            break;
        case '*':
            nums[nnums++] = a * b;
            break;
        case '/':
            nums[nnums++] = a / b;
            break;
        case '^':
            nums[nnums++] = fexp(a, b);
            break;
    }
}
void eval_fun(){ // 计算前缀函数
    int a = nums[--nnums];
    char f = funs[--nfuns];
    switch (f){
        case '-':
            nums[nnums++] = -a;
            break;
    }
}
int main(){
    bool time4opt = false; // 表示字符可能为运算符
    int curnum; // 表示正在读取还未入栈的操作数
    bool exnum = false; // 表示是否有正在读取还未入栈的操作数
    char c;
    while(cin >> c){
        if(time4opt){ // 若读入字符可能是运算符
            if(exnum){ // 若存在正在读取还未入栈的操作数
                if(c >= '0' && c <= '9'){ // 若读入字符为数字
                    curnum = curnum * 10 + (c - '0'); // 在未入栈的操作数后添加一位
                    continue; // 进入到读取下一个符号的逻辑
                } // 若读入字符不为数字
                nums[nnums++] = curnum; // 将操作数入栈
                exnum = false; // 此时没有未入栈的操作数
            }
            // 操作数入栈后或者括号被计算后计算前缀函数，直到前缀函数栈空或遇到左括号
            while (nfuns != 0 && funs[nfuns - 1] != '(')eval_fun();
            if(c == ')'){ // 若读入字符为右括号
                --nfuns; // 此时前缀函数栈顶必定为左括号，让其出栈
                // 计算运算符直到左括号
                while(opts[nopts - 1] != '(')eval_opt();
                --nopts; // 让左括号出栈
                continue; // 进入读取下一个字符的逻辑
            } // 若字符为运算符
            // 如果读入运算符和栈顶运算符的优先级以及结合性满足条件，那么进行计算
            while(nopts != 0 && 
            (priority(c) < priority(opts[nopts - 1]) || 
            islcombine(opts[nopts - 1]) && priority(c) == priority(opts[nopts - 1])))eval_opt();
            opts[nopts++] = c; // 将读入运算符入栈
            time4opt = false; // 此时下一个字符不可能是运算符
        }else{ // 若字符不可能为运算符
            if(c >= '0' && c <= '9'){ // 若字符为数字
                curnum = c - '0'; // 读入操作数的第一位
                time4opt = true; // 此时下一个字符可能是运算符
                exnum = true; // 现在存在正在读入的操作数
                continue; // 进入处理下一个字符的逻辑
            }
            // 若字符为左括号那么在运算符栈中压入左括号
            if(c == '(')opts[nopts++] = '(';
            // 此时字符可能为前缀函数（负号）或者左括号，两种情况均将其压入前缀函数栈即可
            funs[nfuns++] = c;
        }
    }// 表达式读取完毕
    // 此时可能有未入栈的操作数
    if(exnum)nums[nnums++] = curnum;
    // 同理可能有未计算的前缀函数
    while(nfuns != 0)eval_fun();
    // 同理可能有未计算的运算符
    while(nopts != 0)eval_opt();
    // 此时操作数栈会剩下一个数，就是最终的结果
    cout << nums[0];
}
```

## 碎碎念
聪明的你可能已经发现了，上文实际上是在利用栈来表达式树上进行深度优先搜索。众所周知，深度优先搜索的通常实现方式是函数的递归调用，而C/C++的多层函数调用实际上是通过函数调用栈维护的，也就是说栈的一个重要应用是深度优先搜索，但是C/C++把函数调用抽象地太好了以至于在大部分深度优先搜索的情景你都不需要手动地维护一个栈。

## [例题：洛谷P5788-【模板】单调栈](https://www.luogu.com.cn/problem/P5788)

## 题目背景

模板题，无背景。  

2019.12.12 更新数据，放宽时限，现在不再卡常了。

## 题目描述

给出项数为 $n$ 的整数数列 $a_{1 ... n}$。

定义函数 $f(i)$ 代表数列中第 $i$ 个元素之后第一个大于 $a_i$ 的元素的**下标**，即 $f(i)=\min_{i<j\leq n, a_j > a_i} \{j\}$。若不存在，则 $f(i)=0$。

试求出 $f(1... n)$。

## 输入格式

第一行一个正整数 $n$。

第二行 $n$ 个正整数 $a_{1... n}$。

## 输出格式

一行 $n$ 个整数表示 $f(1), f(2), ..., f(n)$ 的值。

## 样例 #1

### 样例输入 #1

```
5
1 4 2 3 5
```

### 样例输出 #1

```
2 5 4 5 0
```

## 提示

【数据规模与约定】

对于 $30\%$ 的数据，$n\leq 100$；

对于 $60\%$ 的数据，$n\leq 5 \times 10^3$ ；

对于 $100\%$ 的数据，$1 \le n\leq 3\times 10^6$，$1\leq a_i\leq 10^9$。

## 题解
考虑记录前 $k$ 个数中 $f$ 的值大于等于 $k$ 或者为 $0$ 的数，对于这些数中小于 $a_{k+1}$ 的数，那么我们就可以得到它们对应的 $f$ 为 $k+1$。但是这样每次新添第 $k+1$ 个数都得遍历一遍最坏情况下有 $k$ 个数的容器，时间复杂度为 $O(n^2)$ ，不可接受。

注意到前 $k$ 个数中 $f$ 的值大于等于 $k$ 或为 $0$ 的数中下标较小的值必定较大，否则它的 $f$ 值就会是它后面的一个具有更大的值的下标了，所以我们可以检查这其中下标最大的数是否小于 $a_{k+1}$，如果小于那么我们就得到了它的 $f$ 值，再移除这个数，然后重复这个过程直到下标最大的的数大于等于 $a_{k+1}$，再将第 $k+1$ 个数插入容器，我们就可以得到前 $k+1$ 个数的结果。

我们可以用栈来进行维护，只要让每次`push`的下标大于栈中的所有下标，那么这个栈的栈顶的下标就是最大的，也就是说我们只要按照下标顺序`push`即可。分析时间复杂度可以发现每一个数最多被`push`一次，`pop`一次，并进行一次比较，以及用于终止循环的一次判空或者比较，所以时间复杂度为 $O(n)$，可以接受。

#### 代码：
```cpp
#include<iostream>
using namespace std;
int stk[3 * (int)1e6], sz, f[1 + 3 * (int)1e6], arr[1 + 3 * (int)1e6];
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    int n; cin >> n;
    for(int i = 1; i <= n; ++i){
        cin >> arr[i];
        while(sz != 0 && arr[stk[sz - 1]] < arr[i])f[stk[--sz]] = i;
        stk[sz++] = i;
    }
    for(int i = 1; i <= n; ++i)cout << f[i] << ' ';
}
```
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