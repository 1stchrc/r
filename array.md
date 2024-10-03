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

# [接下来是栈时间](./index.html?page=stack)