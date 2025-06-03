---
title: CPP容器-priority_queue
date: 2025-05-30 09:36:14
tags:
  - 容器
  - C++
  - stl
  - 数据结构
categories: C++基础知识
---

`std::priority_queue` 是 C++ 标准库中的优先队列容器适配器，基于堆数据结构实现，默认提供最大堆功能（最大元素始终位于顶部）。以下是 `std::priority_queue` 的完整 API 参考。

## 1. 构造函数

|                    方法                    |                  描述                   | 时间复杂度 |
| :----------------------------------------: | :-------------------------------------: | :--------: |
|           `priority_queue<T> pq`           | 创建空优先队列（使用 `less<T>` 比较器） |    O(1)    |
| `priority_queue<T, Container, Compare> pq` |          指定底层容器和比较器           |    O(1)    |
|        `priority_queue<T> pq(comp)`        |            使用自定义比较器             |    O(1)    |
|     `priority_queue<T> pq(begin, end)`     |           用迭代器范围初始化            |    O(n)    |
|  `priority_queue<T> pq(begin, end, comp)`  |       用迭代器范围和比较器初始化        |    O(n)    |

## 2. 元素访问

|    方法    |          描述          | 时间复杂度 |
| :--------: | :--------------------: | :--------: |
| `pq.top()` | 访问顶部元素（不删除） |    O(1)    |

## 3. 容量操作

|     方法     |     描述     | 时间复杂度 |
| :----------: | :----------: | :--------: |
| `pq.empty()` | 判断是否为空 |    O(1)    |
| `pq.size()`  | 返回元素数量 |    O(1)    |

## 4. 修改操作

|          方法          |          描述          | 时间复杂度 |
| :--------------------: | :--------------------: | :--------: |
|    `pq.push(value)`    |    插入元素（拷贝）    |  O(log n)  |
| `pq.push(move(value))` |    插入元素（移动）    |  O(log n)  |
| `pq.emplace(args...)`  |   就地构造并插入元素   |  O(log n)  |
|       `pq.pop()`       |      删除顶部元素      |  O(log n)  |
|  `pq.swap(other_pq)`   | 交换两个优先队列的内容 |    O(1)    |

## 5. 底层容器与比较器

`priority_queue` 是容器适配器，默认使用 `vector` 作为底层容器，`less` 作为比较器（最大堆）。可以自定义：

```c++
// 使用最小堆
std::priority_queue<int, std::vector<int>, std::greater<int>> min_pq;

// 自定义比较器
struct Compare 
{
    bool operator()(const T& a, const T& b) 
    {
        return /* 自定义比较逻辑 */;
    }
};
std::priority_queue<T, std::vector<T>, Compare> custom_pq;
```

## 6. `emplace` 方法详解

`emplace` 可以直接在优先队列内部构造对象，避免临时对象的创建和拷贝：

```c++
struct Task 
{
    Task(int p, std::string d) : priority(p), desc(d) {}
    int priority;
    std::string desc;
    
    bool operator<(const Task& other) const 
    {
        return priority < other.priority; // 优先级越高越先出队
    }
};

std::priority_queue<Task> pq;
pq.emplace(3, "Fix bug");  // 直接在堆中构造 Task 对象
pq.emplace(1, "Write docs");
pq.emplace(5, "Urgent hotfix");
```

## 7. 性能提示

1. 插入和删除操作都是 O(log n) 时间复杂度
2. 不支持随机访问迭代器
3. 如果需要频繁访问中间元素，考虑使用 `std::set`
4. 批量初始化比逐个插入更高效（O(n) vs O(n log n)）

## 8. 完整示例

```c#
#include <iostream>
#include <queue>
#include <functional> // for std::greater

int main() 
{
    // 默认最大堆
    std::priority_queue<int> max_pq;
    max_pq.push(3);
    max_pq.push(1);
    max_pq.push(4);
    max_pq.emplace(2);
    
    std::cout << "Max priority queue: ";
    while (!max_pq.empty()) 
    {
        std::cout << max_pq.top() << " "; // 4 3 2 1
        max_pq.pop();
    }
    std::cout << "\n";
    
    // 最小堆
    std::priority_queue<int, std::vector<int>, std::greater<int>> min_pq;
    min_pq.push(3);
    min_pq.push(1);
    min_pq.push(4);
    min_pq.emplace(2);
    
    std::cout << "Min priority queue: ";
    while (!min_pq.empty()) 
    {
        std::cout << min_pq.top() << " "; // 1 2 3 4
        min_pq.pop();
    }
    std::cout << "\n";
    
    // 自定义比较器
    auto cmp = [](int left, int right) { return left % 3 < right % 3; };
    std::priority_queue<int, std::vector<int>, decltype(cmp)> mod_pq(cmp);
    for (int n : {5, 7, 2, 4, 1, 8}) mod_pq.push(n);
    
    std::cout << "Custom priority queue: ";
    while (!mod_pq.empty()) 
    {
        std::cout << mod_pq.top() << " "; // 8 7 5 4 2 1
        mod_pq.pop();
    }
    
    return 0;
}
```

## 9. 与其他容器比较

|     特性     | `std::priority_queue` |  `std::set`   | `std::vector` + `make_heap` |
| :----------: | :-------------------: | :-----------: | :-------------------------: |
| **顶部访问** |         O(1)          | O(1)（begin） |            O(1)             |
|   **插入**   |       O(log n)        |   O(log n)    |    O(log n)（push_heap）    |
| **删除顶部** |       O(log n)        |   O(log n)    |    O(log n)（pop_heap）     |
| **随机访问** |        不支持         |    不支持     |            支持             |
| **中间删除** |        不支持         |   O(log n)    |           不支持            |
| **内存使用** |         较低          |     较高      |            较低             |
| **适用场景** |    只需要顶部访问     | 需要完整排序  |  需要堆操作但需要随机访问   |

`std::priority_queue` 是专门为优先队列场景优化的简单接口，如果不需要完整的排序功能，它比 `std::set` 更高效。
