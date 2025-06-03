---
title: CPP容器-map
date: 2025-05-30 11:17:54
tags:
  - 容器
  - C++
  - stl
  - 数据结构
categories: C++基础知识
---

`std::map` 是 C++ 标准库中的关联容器，基于红黑树实现，存储键值对并按键自动排序。以下是 `std::map` 的 API 参考。

## 1. 构造函数

|              方法              |            描述            | 时间复杂度 |
| :----------------------------: | :------------------------: | :--------: |
|          `map<K,V> m`          |         创建空 map         |    O(1)    |
|       `map<K,V> m(comp)`       | 使用自定义比较器创建空 map |    O(1)    |
|    `map<K,V> m(begin, end)`    |     用迭代器范围初始化     | O(n log n) |
| `map<K,V> m(begin, end, comp)` | 用迭代器范围和比较器初始化 | O(n log n) |
| `map<K,V> m(initializer_list)` |     用初始化列表初始化     | O(n log n) |
|    `map<K,V> m(other_map)`     |        拷贝构造函数        |    O(n)    |
| `map<K,V> m(move(other_map))`  |        移动构造函数        |    O(1)    |

## 2. 元素访问

|     方法     |                描述                | 时间复杂度 |
| :----------: | :--------------------------------: | :--------: |
| `m.at(key)`  |   访问键为 key 的值（边界检查）    |  O(log n)  |
|   `m[key]`   |      访问或插入键为 key 的值       |  O(log n)  |
| `m.begin()`  |     返回指向第一个元素的迭代器     |    O(1)    |
|  `m.end()`   |        返回指向末尾的迭代器        |    O(1)    |
| `m.rbegin()` | 返回反向迭代器（指向最后一个元素） |    O(1)    |
|  `m.rend()`  | 返回反向迭代器（指向第一个元素前） |    O(1)    |

## 3. 容量操作

|      方法      |           描述           | 时间复杂度 |
| :------------: | :----------------------: | :--------: |
|  `m.empty()`   |       判断是否为空       |    O(1)    |
|   `m.size()`   |       返回元素数量       |    O(1)    |
| `m.max_size()` | 返回可容纳的最大元素数量 |    O(1)    |

## 4. 修改操作

### 4.1 插入元素

|               方法                |             描述              |        时间复杂度        |
| :-------------------------------: | :---------------------------: | :----------------------: |
|     `m.insert({key, value})`      |          插入键值对           |         O(log n)         |
| `m.insert(make_pair(key, value))` |          插入键值对           |         O(log n)         |
|   `m.insert(pos, {key, value})`   |    从 pos 开始搜索插入位置    | 平均 O(1)，最坏 O(log n) |
|      `m.insert(begin, end)`       |    插入迭代器范围内的元素     |      O(m log(n+m))       |
|   `m.insert(initializer_list)`    |    插入初始化列表中的元素     |      O(m log(n+m))       |
|       `m.emplace(args...)`        |      就地构造并插入元素       |         O(log n)         |
|  `m.emplace_hint(pos, args...)`   |      带提示符的就地构造       | 平均 O(1)，最坏 O(log n) |
|   `m.try_emplace(key, args...)`   | 仅在键不存在时构造值（C++17） |         O(log n)         |

### 4.2 删除元素

|         方法          |            描述             |        时间复杂度        |
| :-------------------: | :-------------------------: | :----------------------: |
|    `m.erase(pos)`     |    删除迭代器指向的元素     | 平均 O(1)，最坏 O(log n) |
|    `m.erase(key)`     |     删除键为 key 的元素     |         O(log n)         |
| `m.erase(begin, end)` | 删除 [begin,end) 范围的元素 |           O(m)           |
|      `m.clear()`      |        清空所有元素         |           O(n)           |

### 4.3 其他操作

|         方法         |         描述          | 时间复杂度 |
| :------------------: | :-------------------: | :--------: |
| `m.swap(other_map)`  |  交换两个 map 的内容  |    O(1)    |
|  `m.extract(node)`   |   提取节点（C++17）   |    O(1)    |
| `m.merge(other_map)` | 合并两个 map（C++17） | O(n log n) |

## 5. 查找操作

|         方法         |                描述                 | 时间复杂度 |
| :------------------: | :---------------------------------: | :--------: |
|    `m.find(key)`     |         查找键为 key 的元素         |  O(log n)  |
|    `m.count(key)`    |  返回键为 key 的元素数量（0 或 1）  |  O(log n)  |
|  `m.contains(key)`   |       检查是否包含键（C++20）       |  O(log n)  |
| `m.lower_bound(key)` | 返回第一个不小于 key 的元素的迭代器 |  O(log n)  |
| `m.upper_bound(key)` |  返回第一个大于 key 的元素的迭代器  |  O(log n)  |
| `m.equal_range(key)` |      返回键等于 key 的元素范围      |  O(log n)  |

## 6. 比较器相关

|       方法       |     描述     |
| :--------------: | :----------: |
|  `m.key_comp()`  | 返回键比较器 |
| `m.value_comp()` | 返回值比较器 |

## 7. `emplace` 和 `try_emplace` 详解

`emplace` 系列方法可以直接在 map 内部构造对象，避免临时对象的创建和拷贝：

```c++
struct Product 
{
    Product(std::string n, double p) : name(n), price(p) {}
    std::string name;
    double price;
};

std::map<int, Product> inventory;
inventory.emplace(101, Product("Laptop", 999.99));  // 构造临时对象再移动
inventory.emplace(std::piecewise_construct,
                 std::forward_as_tuple(102),
                 std::forward_as_tuple("Phone", 599.99));  // 直接构造

// C++17 try_emplace - 只在键不存在时构造值
auto [iter, inserted] = inventory.try_emplace(101, "Tablet", 299.99);
if (!inserted) 
{
    std::cout << "Product 101 already exists: " << iter->second.name << "\n";
}
```

## 8. 性能提示

1. 所有操作都保证 O(log n) 时间复杂度
2. 使用 `emplace_hint` 可以提高插入效率（如果提示位置正确）
3. `operator[]` 会在键不存在时插入默认值，`at()` 会抛出异常
4. 批量插入比逐个插入更高效（考虑使用 `insert(begin, end)`）

## 9. 完整示例

```c++
#include <iostream>
#include <map>
#include <string>

int main() 
{
    // 初始化
    std::map<std::string, int> scores = 
    {
        {"Alice", 90},
        {"Bob", 85}
    };
    
    // 插入元素
    scores.insert({"Charlie", 88});
    scores.emplace("Dave", 92);
    scores["Eve"] = 95;  // 使用 operator[] 插入
    
    // 访问元素
    std::cout << "Alice's score: " << scores.at("Alice") << "\n";
    std::cout << "Bob's score: " << scores["Bob"] << "\n";
    
    // 查找元素
    if (auto it = scores.find("Frank"); it != scores.end())
    {
        std::cout << "Found Frank with score " << it->second << "\n";
    } 
    else 
    {
        std::cout << "Frank not found\n";
    }
    
    // 遍历（按键自动排序）
    for (const auto& [name, score] : scores) 
    {
        std::cout << name << ": " << score << "\n";
    }
    
    // 范围查询
    auto [lower, upper] = scores.equal_range("C");
    for (auto it = lower; it != upper; ++it) 
    {
        std::cout << it->first << " "; 
    }
    
    // 自定义比较器
    std::map<std::string, int, std::greater<>> desc_scores;
    desc_scores.insert(scores.begin(), scores.end());
    
    return 0;
}
```

## 10. 与其他容器比较

|     特性     |  `std::map`  | `std::unordered_map` |     `std::multimap`      |
| :----------: | :----------: | :------------------: | :----------------------: |
| **底层实现** |    红黑树    |        哈希表        |          红黑树          |
| **元素顺序** |   按键排序   |         无序         |         按键排序         |
|  **重复键**  |    不允许    |        不允许        |           允许           |
| **查找效率** |   O(log n)   |      平均 O(1)       |         O(log n)         |
| **插入效率** |   O(log n)   |      平均 O(1)       |         O(log n)         |
| **内存使用** |     较高     |         较低         |           较高           |
| **适用场景** | 需要有序存储 |     需要快速查找     | 需要有序存储且允许重复键 |

`std::map` 是需要在插入时自动排序且键唯一的理想选择，如果需要更快的查找速度且不关心顺序，可以考虑 `std::unordered_map`。
