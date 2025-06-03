---
title: CPP容器-vector
date: 2025-05-29 16:34:36
tags:
  - 容器
  - C++
  - stl
  - 数据结构
categories: C++基础知识
cover: /img/CPP容器-vector/background.png
---

`std::vector` 是 C++ 中最常用的动态数组容器，提供高效的随机访问和动态扩容功能。

------

## 1. 构造函数

|               方法                |                 描述                 | 时间复杂度 |
| :-------------------------------: | :----------------------------------: | :--------: |
|           `vector<T> v`           |            创建空 vector             |    O(1)    |
|         `vector<T> v(n)`          | 创建包含 n 个默认初始化元素的 vector |    O(n)    |
|      `vector<T> v(n, value)`      |    创建包含 n 个 value 的 vector     |    O(n)    |
|     `vector<T> v(begin, end)`     |   用迭代器范围 [begin,end) 初始化    |    O(n)    |
|  `vector<T> v(initializer_list)`  |          用初始化列表初始化          |    O(n)    |
|    `vector<T> v(other_vector)`    |             拷贝构造函数             |    O(n)    |
| `vector<T> v(move(other_vector))` |             移动构造函数             |    O(1)    |

## 2. 元素访问

|    方法     |                   描述                    | 时间复杂度 |
| :---------: | :---------------------------------------: | :--------: |
|   `v[i]`    |       访问第 i 个元素（不检查边界）       |    O(1)    |
|  `v.at(i)`  | 访问第 i 个元素（边界检查，越界抛出异常） |    O(1)    |
| `v.front()` |              访问第一个元素               |    O(1)    |
| `v.back()`  |             访问最后一个元素              |    O(1)    |
| `v.data()`  |          返回指向底层数组的指针           |    O(1)    |

## 3. 容量操作

|        方法         |            描述             | 时间复杂度 |
| :-----------------: | :-------------------------: | :--------: |
|     `v.empty()`     |        判断是否为空         |    O(1)    |
|     `v.size()`      |        返回元素数量         |    O(1)    |
|   `v.max_size()`    |  返回可容纳的最大元素数量   |    O(1)    |
|   `v.capacity()`    |   返回当前分配的存储容量    |    O(1)    |
|   `v.reserve(n)`    | 预留至少容纳 n 个元素的空间 |    O(n)    |
| `v.shrink_to_fit()` |    请求移除未使用的容量     |    O(n)    |

## 4. 修改操作

### 4.1 添加元素

|               方法                |           描述            | 时间复杂度 |
| :-------------------------------: | :-----------------------: | :--------: |
|       `v.push_back(value)`        |  在末尾添加元素（拷贝）   | 平摊 O(1)  |
|    `v.push_back(move(value))`     |  在末尾添加元素（移动）   | 平摊 O(1)  |
|     `v.emplace_back(args...)`     |    在末尾就地构造元素     | 平摊 O(1)  |
|      `v.insert(pos, value)`       | 在 pos 前插入元素（拷贝） |    O(n)    |
|     `v.insert(pos, n, value)`     | 在 pos 前插入 n 个 value  |   O(n+m)   |
|    `v.insert(pos, begin, end)`    |  在 pos 前插入迭代器范围  |   O(n+m)   |
| `v.insert(pos, initializer_list)` |  在 pos 前插入初始化列表  |   O(n+m)   |
|     `v.emplace(pos, args...)`     |   在 pos 前就地构造元素   |    O(n)    |

```c++
// leetcode 循环数组常用
std::vector<int> v1 = {1, 2, 3};
std::vector<int> v2 = {4, 5, 6};

v1.insert(v1.end(), v2.begin(), v2.end());
```

### 4.2 删除元素

|         方法          |            描述             | 时间复杂度 |
| :-------------------: | :-------------------------: | :--------: |
|    `v.pop_back()`     |      删除最后一个元素       |    O(1)    |
|    `v.erase(pos)`     |      删除 pos 处的元素      |    O(n)    |
| `v.erase(begin, end)` | 删除 [begin,end) 范围的元素 |    O(n)    |
|      `v.clear()`      | 清空所有元素（不释放容量）  |    O(n)    |

### 4.3 其他修改

|             方法             |                 描述                 | 时间复杂度 |
| :--------------------------: | :----------------------------------: | :--------: |
|        `v.resize(n)`         |   调整大小为 n，新增元素默认初始化   |    O(n)    |
|     `v.resize(n, value)`     | 调整大小为 n，新增元素初始化为 value |    O(n)    |
|    `v.swap(other_vector)`    |        交换两个 vector 的内容        |    O(1)    |
|     `v.assign(n, value)`     |        替换内容为 n 个 value         |    O(n)    |
|    `v.assign(begin, end)`    |         替换内容为迭代器范围         |    O(n)    |
| `v.assign(initializer_list)` |         替换内容为初始化列表         |    O(n)    |

## 5. 迭代器

|     方法      |                描述                |
| :-----------: | :--------------------------------: |
|  `v.begin()`  |     返回指向第一个元素的迭代器     |
|   `v.end()`   |        返回指向末尾的迭代器        |
| `v.rbegin()`  | 返回反向迭代器（指向最后一个元素） |
|  `v.rend()`   | 返回反向迭代器（指向第一个元素前） |
| `v.cbegin()`  |        const 版本的 begin()        |
|  `v.cend()`   |         const 版本的 end()         |
| `v.crbegin()` |       const 版本的 rbegin()        |
|  `v.crend()`  |        const 版本的 rend()         |

## 6. `emplace` 和 `emplace_back` 详解

`emplace` 和 `emplace_back` 是 C++11 引入的高效构造方法，可以直接在容器内部构造对象，避免不必要的拷贝或移动操作。

### `emplace_back` 示例

```c++
struct Point 
{
    Point(int x, int y) : x(x), y(y) {}
    int x, y;
};

std::vector<Point> v;
v.emplace_back(1, 2);  // 直接在 vector 内存中构造 Point(1, 2)
```

### `emplace` 示例

```c++
std::vector<std::string> v = {"a", "b", "d"};
// 在 "b" 和 "d" 之间插入新字符串
auto it = v.emplace(v.begin() + 2, "c");  // 就地构造 std::string("c")
```

## 7. 性能提示

1. 使用 `reserve()` 预分配空间可以避免多次重新分配
2. `emplace_back` 比 `push_back` 更高效（避免临时对象构造）
3. 删除中间元素会导致元素移动，性能较差
4. 频繁插入/删除时考虑使用 `std::list` 或 `std::deque`

## 8. 完整示例

```c++
#include <iostream>
#include <vector>
#include <string>

int main() 
{
    // 初始化
    std::vector<std::string> words = {"Hello", "World"};
    
    // 添加元素
    words.push_back("from");
    words.emplace_back("C++");  // 比 push_back 更高效
    
    // 插入元素
    words.insert(words.begin() + 2, "beautiful");
    words.emplace(words.begin() + 3, "modern");  // 就地构造
    
    // 访问元素
    std::cout << "First word: " << words.front() << "\n";
    std::cout << "Last word: " << words.back() << "\n";
    
    // 遍历
    for (const auto& word : words) 
    {
        std::cout << word << " ";
    }
    std::cout << "\n";
    
    // 删除元素
    words.pop_back();
    words.erase(words.begin() + 1);
    
    // 容量信息
    std::cout << "Size: " << words.size() << "\n";
    std::cout << "Capacity: " << words.capacity() << "\n";
    
    // 清空
    words.clear();
    
    return 0;
}
```
