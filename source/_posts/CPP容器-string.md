---
title: CPP容器-string
date: 2025-06-16 10:39:09
tags:
  - 容器
  - C++
  - stl
  - 数据结构
categories: C++基础知识
---

# C++中的string容器详解

`std::string`是C++标准库中用于处理字符串的容器类，它提供了丰富的字符串操作功能，比C风格的字符数组(`char[]`)更安全、更方便。

## 基本特性

1. **动态大小**：string可以动态调整大小，无需手动管理内存
2. **丰富的操作**：提供多种字符串操作方法
3. **安全性**：自动处理内存分配和释放，减少缓冲区溢出风险
4. **兼容性**：可以与C风格字符串互操作

## 头文件

使用string需要包含头文件：

```c++
#include <string>
```

## 构造函数

string提供了多种构造函数：

```c++
std::string s1;               // 默认构造，空字符串
std::string s2("Hello");      // 从C风格字符串构造
std::string s3(s2);           // 拷贝构造
std::string s4(5, 'x');       // 构造包含5个'x'的字符串
std::string s5(s2.begin(), s2.begin()+3); // 从迭代器范围构造
```

## 常用操作

### 赋值操作

```c++
std::string s;
s = "Hello";                  // 赋值
s.assign("World");            // assign方法
s.assign(3, '!');             // 赋值为3个'!'
```

### 访问元素

```c++
char c = s[0];                // 使用下标操作符
c = s.at(1);                  // 使用at方法(会检查边界)
c = s.front();                // 第一个字符
c = s.back();                 // 最后一个字符
```

### 修改操作

```c++
s += " C++";                  // 追加
s.append(" is great");        // append方法
s.push_back('!');             // 追加单个字符
s.insert(5, " amazing");      // 在指定位置插入
s.erase(5, 8);                // 删除从位置5开始的8个字符
s.replace(0, 5, "Hi");        // 替换子串
s.clear();                    // 清空字符串
```

### 容量操作

```c++
int len = s.length();         // 或s.size()
bool empty = s.empty();       // 是否为空
s.reserve(100);               // 预留空间
int cap = s.capacity();       // 当前容量
s.shrink_to_fit();            // 减少容量以适应大小
```

### 字符串比较

```c++
if (s1 == s2) { /* 相等 */ }
if (s1 < s2)  { /* 字典序比较 */ }
if (s1.compare(s2) == 0) { /* 等价于== */ }
```

### 子串操作

```c++
std::string sub = s.substr(6, 5); // 从位置6开始取5个字符
size_t pos = s.find("C++");       // 查找子串位置
pos = s.rfind("C++");             // 从后向前查找
```

### 数值转换

```c++
std::string num = std::to_string(123); // 数值转字符串
int n = std::stoi("456");              // 字符串转int
double d = std::stod("3.14");          // 字符串转double
```

## 迭代器支持

string支持迭代器，可以用于遍历：

```c++
for (auto it = s.begin(); it != s.end(); ++it) 
{
    std::cout << *it;
}

// 或使用范围for循环
for (char c : s) 
{
    std::cout << c;
}
```

## 与C风格字符串互操作

```c++
const char* cstr = s.c_str(); // 获取C风格字符串
const char* data = s.data();   // 获取底层字符数组
```

## 性能考虑

1. string使用动态内存分配，频繁修改可能导致多次重新分配
2. 对于大量字符串操作，预先使用`reserve()`预留空间可以提高性能
3. 短字符串优化(SSO)：许多实现会对短字符串进行优化，避免堆分配

## 现代C++特性

C++11及以后版本为string添加了：

1. 移动语义支持
2. 字面量操作符
3. 更多的构造函数和赋值操作符重载

## 总结

std::string是C++中处理字符串的首选方式，它提供了安全、高效且功能丰富的字符串操作接口。相比C风格字符串，它更安全、更方便，是现代C++程序中处理文本数据的基石。
