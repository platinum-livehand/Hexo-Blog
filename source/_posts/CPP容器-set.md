---
title: CPP容器-set
date: 2025-05-30 11:06:39
tags:
  - 容器
  - C++
  - stl
  - 数据结构
categories: C++基础知识
---

`std::set` 是 C++ 标准库中的关联容器，基于红黑树实现，存储唯一键并按键自动排序。以下是 `std::set` 的 API 参考。

## 1. 构造函数

|             方法             |            描述            | 时间复杂度 |
| :--------------------------: | :------------------------: | :--------: |
|          `set<T> s`          |         创建空 set         |    O(1)    |
|       `set<T> s(comp)`       | 使用自定义比较器创建空 set |    O(1)    |
|    `set<T> s(begin, end)`    |     用迭代器范围初始化     | O(n log n) |
| `set<T> s(begin, end, comp)` | 用迭代器范围和比较器初始化 | O(n log n) |
| `set<T> s(initializer_list)` |     用初始化列表初始化     | O(n log n) |
|    `set<T> s(other_set)`     |        拷贝构造函数        |    O(n)    |
| `set<T> s(move(other_set))`  |        移动构造函数        |    O(1)    |

## 2. 元素访问

|     方法     |                描述                | 时间复杂度 |
| :----------: | :--------------------------------: | :--------: |
| `s.begin()`  |     返回指向第一个元素的迭代器     |    O(1)    |
|  `s.end()`   |        返回指向末尾的迭代器        |    O(1)    |
| `s.rbegin()` | 返回反向迭代器（指向最后一个元素） |    O(1)    |
|  `s.rend()`  | 返回反向迭代器（指向第一个元素前） |    O(1)    |

## 3. 容量操作

|      方法      |           描述           | 时间复杂度 |
| :------------: | :----------------------: | :--------: |
|  `s.empty()`   |       判断是否为空       |    O(1)    |
|   `s.size()`   |       返回元素数量       |    O(1)    |
| `s.max_size()` | 返回可容纳的最大元素数量 |    O(1)    |

## 4. 修改操作

### 4.1 插入元素

|              方法              |          描述           |        时间复杂度        |
| :----------------------------: | :---------------------: | :----------------------: |
|       `s.insert(value)`        |  插入元素（自动去重）   |         O(log n)         |
|    `s.insert(move(value))`     |      移动插入元素       |         O(log n)         |
|     `s.insert(pos, value)`     | 从 pos 开始搜索插入位置 | 平均 O(1)，最坏 O(log n) |
|     `s.insert(begin, end)`     | 插入迭代器范围内的元素  |      O(m log(n+m))       |
|  `s.insert(initializer_list)`  | 插入初始化列表中的元素  |      O(m log(n+m))       |
|      `s.emplace(args...)`      |   就地构造并插入元素    |         O(log n)         |
| `s.emplace_hint(pos, args...)` |   带提示符的就地构造    | 平均 O(1)，最坏 O(log n) |

### 4.2 删除元素

|         方法          |            描述             |        时间复杂度        |
| :-------------------: | :-------------------------: | :----------------------: |
|    `s.erase(pos)`     |    删除迭代器指向的元素     | 平均 O(1)，最坏 O(log n) |
|    `s.erase(key)`     |     删除键为 key 的元素     |         O(log n)         |
| `s.erase(begin, end)` | 删除 [begin,end) 范围的元素 |           O(m)           |
|      `s.clear()`      |        清空所有元素         |           O(n)           |

### 4.3 其他操作

|         方法         |         描述          | 时间复杂度 |
| :------------------: | :-------------------: | :--------: |
| `s.swap(other_set)`  |  交换两个 set 的内容  |    O(1)    |
|  `s.extract(node)`   |   提取节点（C++17）   |    O(1)    |
| `s.merge(other_set)` | 合并两个 set（C++17） | O(n log n) |

## 5. 查找操作

|         方法         |                描述                 | 时间复杂度 |
| :------------------: | :---------------------------------: | :--------: |
|    `s.find(key)`     |         查找键为 key 的元素         |  O(log n)  |
|    `s.count(key)`    |  返回键为 key 的元素数量（0 或 1）  |  O(log n)  |
|  `s.contains(key)`   |       检查是否包含键（C++20）       |  O(log n)  |
| `s.lower_bound(key)` | 返回第一个不小于 key 的元素的迭代器 |  O(log n)  |
| `s.upper_bound(key)` |  返回第一个大于 key 的元素的迭代器  |  O(log n)  |
| `s.equal_range(key)` |      返回键等于 key 的元素范围      |  O(log n)  |

## 6. 比较器相关

|       方法       |     描述     |
| :--------------: | :----------: |
|  `s.key_comp()`  | 返回键比较器 |
| `s.value_comp()` | 返回值比较器 |

## 7. `emplace` 和 `emplace_hint` 详解

`emplace` 系列方法可以直接在 set 内部构造对象，避免临时对象的创建和拷贝：

```c++
struct Person 
{
    Person(std::string n, int a) : name(n), age(a) {}
    std::string name;
    int age;
    
    bool operator<(const Person& other) const 
    {
        return age < other.age; // 按年龄排序
    }
};

std::set<Person> s;
s.emplace("Alice", 25);  // 直接在 set 中构造 Person 对象
s.emplace("Bob", 30);

// 使用 emplace_hint 提高插入效率（如果提示位置正确）
auto hint = s.lower_bound(Person("", 27));
s.emplace_hint(hint, "Charlie", 28);
```

## 8. 性能提示

1. 所有操作都保证 O(log n) 时间复杂度
2. 插入已存在元素不会改变 set
3. 使用 `emplace_hint` 可以提高插入效率（如果提示位置正确）
4. 批量插入比逐个插入更高效（考虑使用 `insert(begin, end)`）

## 9. 完整示例

```c++
#include <iostream>
#include <set>
#include <string>

int main() 
{
    // 初始化
    std::set<std::string> s = {"apple", "banana"};
    
    // 插入元素
    s.insert("orange");
    s.emplace("pear");
    
    // 查找元素
    if (s.find("apple") != s.end()) 
    {
        std::cout << "Found apple!\n";
    }
    
    // 遍历（自动排序）
    for (const auto& fruit : s) 
    {
        std::cout << fruit << " "; // apple banana orange pear
    }
    std::cout << "\n";
    
    // 删除元素
    s.erase("banana");
    
    // 范围查询
    auto [lower, upper] = s.equal_range("o");
    for (auto it = lower; it != upper; ++it) 
    {
        std::cout << *it << " "; // orange
    }
    
    // 自定义比较器
    std::set<int, std::greater<int>> desc_set;
    desc_set.insert({3, 1, 4, 2});
    for (int n : desc_set) 
    {
        std::cout << n << " "; // 4 3 2 1
    }
    
    return 0;
}
```

## 10. 与其他容器比较

|     特性     |  `std::set`  | `std::unordered_set` |    `std::multiset`     |
| :----------: | :----------: | :------------------: | :--------------------: |
| **底层实现** |    红黑树    |        哈希表        |         红黑树         |
| **元素顺序** |   按键排序   |         无序         |        按键排序        |
| **重复元素** |    不允许    |        不允许        |          允许          |
| **查找效率** |   O(log n)   |      平均 O(1)       |        O(log n)        |
| **插入效率** |   O(log n)   |      平均 O(1)       |        O(log n)        |
| **内存使用** |     较高     |         较低         |          较高          |
| **适用场景** | 需要有序存储 |     需要快速查找     | 需要有序存储且允许重复 |

`std::set` 是需要在插入时自动排序且元素唯一的理想选择，如果需要更快的查找速度且不关心顺序，可以考虑 `std::unordered_set`。
