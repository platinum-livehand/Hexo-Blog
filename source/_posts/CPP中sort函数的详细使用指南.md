---
title: CPP中sort函数的详细使用指南
date: 2025-06-06 15:22:50
tags:
  - C++
  - stl
categories: C++基础知识
---

# C++ `sort` 算法的详细使用指南

`sort` 是 C++ STL 中最常用的排序算法，定义在 `<algorithm>` 头文件中。它使用高效的排序算法（通常是快速排序的变体）对序列进行排序。

## 基本用法

### 1. 默认排序（升序）

```c++
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> nums = {4, 2, 5, 3, 1};
    
    // 默认升序排序
    std::sort(nums.begin(), nums.end());
    
    for (int num : nums) 
    {
        std::cout << num << " ";
    }
    // 输出: 1 2 3 4 5
}
```

### 2. 降序排序

```c++
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> nums = {4, 2, 5, 3, 1};
    
    // 使用 greater<>() 实现降序排序
    std::sort(nums.begin(), nums.end(), std::greater<int>());
    
    for (int num : nums) 
    {
        std::cout << num << " ";
    }
    // 输出: 5 4 3 2 1
}
```

## 自定义比较函数

### 1. 使用函数指针

```c++
#include <algorithm>
#include <vector>
#include <iostream>

bool compare(int a, int b) 
{
    return a > b; // 降序排序
}

int main() {
    std::vector<int> nums = {4, 2, 5, 3, 1};
    
    std::sort(nums.begin(), nums.end(), compare);
    
    for (int num : nums) 
    {
        std::cout << num << " ";
    }
    // 输出: 5 4 3 2 1
}
```

### 2. 使用 Lambda 表达式（C++11及以上）

```c++
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> nums = {4, 2, 5, 3, 1};
    
    // 使用 lambda 表达式自定义排序规则
    std::sort(nums.begin(), nums.end(), [](int a, int b) 
              {
        return a > b; // 降序排序
    });
    
    for (int num : nums) 
    {
        std::cout << num << " ";
    }
    // 输出: 5 4 3 2 1
}
```

## 排序自定义对象

```c++
#include <algorithm>
#include <vector>
#include <iostream>
#include <string>

struct Person 
{
    std::string name;
    int age;
};

int main() 
{
    std::vector<Person> people = 
    {
        {"Alice", 25},
        {"Bob", 20},
        {"Charlie", 30}
    };
    
    // 按年龄升序排序
    std::sort(people.begin(), people.end(), [](const Person& a, const Person& b) 
              {
        return a.age < b.age;
    });
    
    for (const auto& p : people) 
    {
        std::cout << p.name << ": " << p.age << std::endl;
    }
    /* 输出:
    Bob: 20
    Alice: 25
    Charlie: 30
    */
    
    // 按姓名升序排序
    std::sort(people.begin(), people.end(), [](const Person& a, const Person& b) 
              {
        return a.name < b.name;
    });
    
    for (const auto& p : people) 
    {
        std::cout << p.name << ": " << p.age << std::endl;
    }
    /* 输出:
    Alice: 25
    Bob: 20
    Charlie: 30
    */
}
```

## 部分排序

`partial_sort` 可以对部分元素进行排序：

```c++
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> nums = {4, 2, 5, 3, 1, 6, 8, 7};
    
    // 只排序前3个元素，其余元素顺序不确定
    std::partial_sort(nums.begin(), nums.begin() + 3, nums.end());
    
    for (int num : nums) 
    {
        std::cout << num << " ";
    }
    // 可能的输出: 1 2 3 7 5 6 8 4
    // 前3个元素一定是1,2,3，但后面的顺序不确定
}
```

## 稳定排序

`stable_sort` 保持相等元素的相对顺序：

```c++
#include <algorithm>
#include <vector>
#include <iostream>

struct Item 
{
    int value;
    int order;
};

int main() 
{
    std::vector<Item> items = 
    {
        {2, 1}, {1, 2}, {2, 3}, {1, 4}, {3, 5}
    };
    
    // 按value排序，保持相同value的原始顺序
    std::stable_sort(items.begin(), items.end(), [](const Item& a, const Item& b) {
        return a.value < b.value;
    });
    
    for (const auto& item : items) 
    {
        std::cout << "(" << item.value << "," << item.order << ") ";
    }
    // 输出: (1,2) (1,4) (2,1) (2,3) (3,5)
    // 注意相同value的item保持了原始输入顺序
}
```

## 性能注意事项

1. `sort` 的平均时间复杂度为 O(N log N)
2. `stable_sort` 通常比 `sort` 慢一些，因为它需要保持相等元素的顺序
3. 对于小型容器（如少于16个元素），`sort` 可能会使用插入排序

`sort` 是C++中最常用的算法之一，熟练掌握它的使用可以大大提高编程效率。
