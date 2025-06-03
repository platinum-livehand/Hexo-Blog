---
title: CPP容器-deque
date: 2025-05-30 10:30:11
tags:
  - 容器
  - C++
  - stl
  - 数据结构
categories: C++基础知识
---

`std::deque`（双端队列）是 C++ 标准库中的双向开口的动态数组，结合了 `vector` 和 `list` 的优点，支持高效的首尾插入删除操作。以下是 `std::deque` 的 API 参考。

## 1. 构造函数

|              方法               |                描述                 | 时间复杂度 |
| :-----------------------------: | :---------------------------------: | :--------: |
|          `deque<T> d`           |            创建空 deque             |    O(1)    |
|         `deque<T> d(n)`         | 创建包含 n 个默认初始化元素的 deque |    O(n)    |
|     `deque<T> d(n, value)`      |    创建包含 n 个 value 的 deque     |    O(n)    |
|    `deque<T> d(begin, end)`     |   用迭代器范围 [begin,end) 初始化   |    O(n)    |
| `deque<T> d(initializer_list)`  |         用初始化列表初始化          |    O(n)    |
|    `deque<T> d(other_deque)`    |            拷贝构造函数             |    O(n)    |
| `deque<T> d(move(other_deque))` |            移动构造函数             |    O(1)    |

## 2. 元素访问

|    方法     |                   描述                    | 时间复杂度 |
| :---------: | :---------------------------------------: | :--------: |
|   `d[i]`    |       访问第 i 个元素（不检查边界）       |    O(1)    |
|  `d.at(i)`  | 访问第 i 个元素（边界检查，越界抛出异常） |    O(1)    |
| `d.front()` |              访问第一个元素               |    O(1)    |
| `d.back()`  |             访问最后一个元素              |    O(1)    |

## 3. 容量操作

|         方法         |                 描述                 | 时间复杂度 |
| :------------------: | :----------------------------------: | :--------: |
|     `d.empty()`      |             判断是否为空             |    O(1)    |
|      `d.size()`      |             返回元素数量             |    O(1)    |
|    `d.max_size()`    |       返回可容纳的最大元素数量       |    O(1)    |
|    `d.resize(n)`     |   调整大小为 n，新增元素默认初始化   |    O(n)    |
| `d.resize(n, value)` | 调整大小为 n，新增元素初始化为 value |    O(n)    |
| `d.shrink_to_fit()`  |         请求移除未使用的容量         |    O(n)    |

## 4. 修改操作

### 4.1 添加元素

|               方法                |           描述            | 时间复杂度 |
| :-------------------------------: | :-----------------------: | :--------: |
|       `d.push_back(value)`        |  在末尾添加元素（拷贝）   | 平摊 O(1)  |
|    `d.push_back(move(value))`     |  在末尾添加元素（移动）   | 平摊 O(1)  |
|     `d.emplace_back(args...)`     |    在末尾就地构造元素     | 平摊 O(1)  |
|       `d.push_front(value)`       |  在开头添加元素（拷贝）   | 平摊 O(1)  |
|    `d.push_front(move(value))`    |  在开头添加元素（移动）   | 平摊 O(1)  |
|    `d.emplace_front(args...)`     |    在开头就地构造元素     | 平摊 O(1)  |
|      `d.insert(pos, value)`       | 在 pos 前插入元素（拷贝） |    O(n)    |
|     `d.insert(pos, n, value)`     | 在 pos 前插入 n 个 value  |   O(n+m)   |
|    `d.insert(pos, begin, end)`    |  在 pos 前插入迭代器范围  |   O(n+m)   |
| `d.insert(pos, initializer_list)` |  在 pos 前插入初始化列表  |   O(n+m)   |
|     `d.emplace(pos, args...)`     |   在 pos 前就地构造元素   |    O(n)    |

### 4.2 删除元素

|         方法          |            描述             | 时间复杂度 |
| :-------------------: | :-------------------------: | :--------: |
|    `d.pop_back()`     |      删除最后一个元素       |    O(1)    |
|    `d.pop_front()`    |       删除第一个元素        |    O(1)    |
|    `d.erase(pos)`     |      删除 pos 处的元素      |    O(n)    |
| `d.erase(begin, end)` | 删除 [begin,end) 范围的元素 |    O(n)    |
|      `d.clear()`      | 清空所有元素（不释放容量）  |    O(n)    |

### 4.3 其他修改

|             方法             |         描述          | 时间复杂度 |
| :--------------------------: | :-------------------: | :--------: |
|    `d.swap(other_deque)`     | 交换两个 deque 的内容 |    O(1)    |
|     `d.assign(n, value)`     | 替换内容为 n 个 value |    O(n)    |
|    `d.assign(begin, end)`    | 替换内容为迭代器范围  |    O(n)    |
| `d.assign(initializer_list)` | 替换内容为初始化列表  |    O(n)    |

## 5. 迭代器

|     方法      |                描述                |
| :-----------: | :--------------------------------: |
|  `d.begin()`  |     返回指向第一个元素的迭代器     |
|   `d.end()`   |        返回指向末尾的迭代器        |
| `d.rbegin()`  | 返回反向迭代器（指向最后一个元素） |
|  `d.rend()`   | 返回反向迭代器（指向第一个元素前） |
| `d.cbegin()`  |        const 版本的 begin()        |
|  `d.cend()`   |         const 版本的 end()         |
| `d.crbegin()` |       const 版本的 rbegin()        |
|  `d.crend()`  |        const 版本的 rend()         |

## 6. `emplace` 和 `emplace_back/front` 详解

`deque` 的 emplace 系列方法与 `vector` 类似，可以直接在容器内部构造对象。

### `emplace_back` 示例

```c++
struct Point 
{
    Point(int x, int y) : x(x), y(y) {}
    int x, y;
};

std::deque<Point> d;
d.emplace_back(1, 2);  // 直接在 deque 末尾构造 Point(1, 2)
```

### `emplace_front` 示例

```c++
d.emplace_front(0, 0); // 在 deque 开头构造 Point(0, 0)
```

### `emplace` 示例

```c++
auto it = d.emplace(d.begin() + 1, 3, 4); // 在第二个位置构造 Point(3, 4)
```

## 7. 性能提示

1. 首尾插入/删除效率高（平摊 O(1)），中间操作效率低（O(n)）
2. 不需要 `reserve()` 方法（deque 采用分块存储，自动管理内存）
3. 随机访问比 `list` 快（O(1)），但比 `vector` 稍慢
4. 内存使用通常比 `vector` 高（因为分块存储的额外开销）

## 8. 完整示例

```c++
#include <iostream>
#include <deque>
#include <string>

int main() 
{
    // 初始化
    std::deque<std::string> d = {"Hello", "World"};
    
    // 添加元素
    d.push_back("from");
    d.emplace_back("C++");
    d.push_front("Say");
    d.emplace_front(">>>");
    
    // 插入元素
    d.insert(d.begin() + 2, "beautiful");
    d.emplace(d.begin() + 3, "modern");
    
    // 访问元素
    std::cout << "First element: " << d.front() << "\n";
    std::cout << "Last element: " << d.back() << "\n";
    std::cout << "Third element: " << d[2] << "\n";
    
    // 遍历
    for (const auto& word : d) {
        std::cout << word << " ";
    }
    std::cout << "\n";
    
    // 删除元素
    d.pop_back();
    d.pop_front();
    d.erase(d.begin() + 1);
    
    // 容量信息
    std::cout << "Size: " << d.size() << "\n";
    
    // 清空
    d.clear();
    
    return 0;
}
```

## 9. deque 与 vector 对比

|     特性     |   `std::deque`   |  `std::vector`   |
| :----------: | :--------------: | :--------------: |
| **存储结构** |   分块连续存储   |   单块连续存储   |
| **首部插入** |       O(1)       |       O(n)       |
| **尾部插入** |       O(1)       |    平摊 O(1)     |
| **中间插入** |       O(n)       |       O(n)       |
| **随机访问** |   O(1)（稍慢）   |   O(1)（更快）   |
| **内存使用** | 较高（分块开销） |       较低       |
| **扩容方式** |   按需分配新块   | 重新分配整个数组 |
| **适用场景** | 需要频繁首尾操作 | 需要高效随机访问 |

`std::deque` 是需要在首尾高效插入删除时的理想选择，而 `std::vector` 更适合需要频繁随机访问的场景。
