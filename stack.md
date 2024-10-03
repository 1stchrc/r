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

比如说在处理 $1+2*3+4$ 时，从左往右读，读取到 $1+2$ 时此时我们不能确定这个 $1+2$ 是否该被计算，这取决于下一个运算符的优先级，所以我们发现当下一个运算符的优先级低于当前运算符时，我们应当立刻计算当前运算符。当我们读取到 $1+2*3+$ 时，可以知道应当计算 $2*3$，此时记录成为 $1+6+$，我们可以规定加法为左结合的，那么此时可以继续计算 $1+6$，记录变为 $7+$。

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
聪明的你可能已经发现了，栈可以被用来计算表达式的特性稍加利用就可以用来模拟程序行为，没错，C/C++的函数调用确实是靠栈来实现的，所以栈可以用来写深度优先搜索。

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