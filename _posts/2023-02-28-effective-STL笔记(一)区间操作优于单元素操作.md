---
tags: C++ STL 
---

# 写在前面



# 辅助函数

## 打印数组

方便查看结果, 而不是每次都用循环输出. 下面这个函数可以在Clang和gcc上运行. 但是需要针对每一种容器写一次, 比较麻烦

```cpp
template <typename T>
ostream &operator<<(ostream &os, const vector<T> &v) {
    for (T i : v) os << i << " ";
    return os << endl;
}
```

这个函数只能在gcc下运行, 但是不需要为每一种容器分别重载`<<`, 比较方便

```cpp
template <typename T, template <typename> class C>
ostream &operator<<(ostream &os, const C<T> &v) {
    for (T i : v) os << i << " ";
    return os << endl;
}
```



# 关于区间操作的讨论



## 与单元素操作的区别

```cpp
#include <iostream>
#include <vector>
using namespace std;

template <typename T>
ostream &operator<<(ostream &os, const vector<T> &v) {
    for (T i : v) os << i << " ";
    return os << endl;
}

vector<int> v1{}, v2{1, 2, 3, 4, 5};

void t1() {
    // 最好的方法
    v1.assign(v2.begin() + v2.size() / 2, v2.end());
}

void t2() {
    for (vector<int>::const_iterator ci = v2.begin() + v2.size() / 2;
         ci != v2.end(); ++ci)
        v1.emplace_back(*ci);
}

void t3() {
    // 使用STL算法, 事实上copy算法内部还是用到了循环
    copy(v2.begin() + v2.size() / 2, v2.end(), back_inserter(v1));
}

// 几乎所有通过插入迭代器限定目标区间的copy调用,
// 都可以转换为利用区间成员函数的调用. 如下:
void t4() {
    // 区间插入
    v1.insert(v1.end(), v2.begin() + v2.size() / 2, v2.end());
}


int main(int argc, char const *argv[]) {
    cout << "v1: " << v1;
    cout << "v2: " << v2;
    cout << "after assign: \n";
    // t1();
    // t2();
    t3();
    // t4();
    cout << "v1: " << v1;
    cout << "v2: " << v2;
    /*
v1:
v2: 1 2 3 4 5
after assign:
v1: 3 4 5
v2: 1 2 3 4 5
*/
    return 0;
}
```





# 准则

## 区间创建

```cpp
void t1() {
    vector<int> v1{1, 2, 3, 4, 5};
    vector<int> v2(v1.begin() + 1, v1.begin() + 4);
    cout << v2; // 2 3 4 
}
```





## 区间插入

这也是文中主要讨论的方法

```cpp
void t2() {
    vector<int> v1{1, 2, 3, 4, 5};
    vector<int> v2{10, 11, 12};
    // 成员函数层面
    v1.insert(v1.begin() + 1, v2.begin(), v2.end());
    cout << v1; // 1 10 11 12 2 3 4 5
}
```



## 区间赋值

只能从头开始赋值. 

```cpp
void t4() {
    // 区间赋值
    vector<int> v1{1, 2, 3, 4, 5};
    vector<int> v2{10, 11, 12};
    // 成员函数层面
    v1.assign(v2.begin(), v2.end() - 1);
    cout << v1; // 10 11

    v1.assign({9, 8, 7}); // 初始化列表
    cout << v1;           // 9 8 7

    v1.assign(10, 9); // count 个 value
    cout << v1;       // 9 9 9 9 9 9 9 9 9 9
}
```







## 区间删除



### 序列式容器

```cpp
void t3() {
    // 区间删除
    vector<int> v1{1, 2, 3, 4, 5};
    v1.erase(v1.begin() + 1, v1.begin() + 4);
    cout << v1; // 1 5
}
```



### 关联式容器

```cpp
void t3_2() {
    // 区间删除: 序列式容器
    set<int> s1{1, 2, 3, 4, 5};
    set<int>::const_iterator first = s1.find(1), last = s1.find(3);
    s1.erase(first, last); // erase: [first, last)
    cout << s1;            // 3 4 5
}
```

