---
title: CPP中常用的STL算法
date: 2025-06-06 15:16:24
tags:
  - C++
  - stl
categories: C++基础知识
---

# C++ 中常用的 STL 算法

STL (Standard Template Library) 提供了大量实用的算法，主要定义在 `<algorithm>` 头文件中。以下是一些最常用的 STL 算法：

## 非修改序列操作

1. **`for_each`** - 对范围内的每个元素应用函数

   ```c++
   std::vector<int> v{1, 2, 3};
   std::for_each(v.begin(), v.end(), [](int i){ std::cout << i << " "; });
   ```

2. **`all_of`/`any_of`/`none_of`** - 检查范围中元素是否满足条件

   ```c++
   bool all_even = std::all_of(v.begin(), v.end(), [](int i){ return i%2 == 0; });
   ```

## 修改序列操作

1. **`copy`/`copy_if`** - 复制元素

   ```c++
   std::vector<int> dest;
   std::copy_if(v.begin(), v.end(), std::back_inserter(dest), [](int i){ return i > 2; });
   ```

2. **`move`** - 移动元素（C++11新增）

   ```c++
   std::vector<std::string> src = {"hello", "world"};
   std::vector<std::string> dest;
   std::move(src.begin(), src.end(), std::back_inserter(dest));
   ```

3. **`fill`/`generate`** - 填充值或生成值

   ```c++
   std::vector<int> v(5);
   std::generate(v.begin(), v.end(), [](){ return rand()%10; });
   ```

4. **`remove`/`remove_if`** - 移除满足条件的元素

   ```c++
   v.erase(std::remove_if(v.begin(), v.end(), [](int i){ return i < 3; }), v.end());
   ```

## 排序和相关操作

1. **`sort`** - 排序

   ```c++
   std::sort(v.begin(), v.end());
   std::sort(v.begin(), v.end(), [](int a, int b){ return a > b; }); // 降序
   ```

2. **`is_sorted`** - 检查是否已排序（C++11新增）

   ```c++
   bool sorted = std::is_sorted(v.begin(), v.end());
   ```

3. **`partial_sort`** - 部分排序

   ```c++
   std::partial_sort(v.begin(), v.begin()+3, v.end()); // 只排序前3个元素
   ```

## 二分搜索操作

1. **`lower_bound`/`upper_bound`** - 返回边界位置

   ```c++
   auto it = std::lower_bound(v.begin(), v.end(), 5); // 第一个不小于5的元素
   ```

2. **`binary_search`** - 二分查找

   ```c++
   bool found = std::binary_search(v.begin(), v.end(), 5);
   ```

## 集合操作

1. `set_union`/`set_intersection`

    - 集合并集/交集

   ```c++
   std::vector<int> v1 = {1,2,3}, v2 = {2,3,4}, result;
   std::set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(result));
   ```

## 堆操作

1. `make_heap`/`push_heap`/`pop_heap`

    - 堆操作

   ```c++
   std::make_heap(v.begin(), v.end());
   v.push_back(10);
   std::push_heap(v.begin(), v.end());
   ```

## 最小/最大操作

1. **`max`** - 返回最大值

   ```c++
   int a = 5, b = 8;
   int max_val = std::max(a, b);  // 返回8
   
   // 使用比较函数
   int max_val_custom = std::max(a, b, [](int x, int y) {
       return abs(x) < abs(y);  // 比较绝对值
   });
   
   // 比较多个值（C++11新增initializer_list支持）
   int max_of_many = std::max({5, 2, 8, 3});  // 返回8
   ```

2. **`min`** - 返回最小值

   ```c++
   int min_val = std::min(a, b);  // 返回5
   
   // 使用比较函数
   int min_val_custom = std::min(a, b, [](int x, int y) {
       return abs(x) < abs(y);  // 比较绝对值
   });
   
   // 比较多个值（C++11新增initializer_list支持）
   int min_of_many = std::min({5, 2, 8, 3});  // 返回2
   ```

3. **`max_element`** - 返回范围内最大元素的位置

   ```c++
   std::vector<int> v{3, 1, 4, 1, 5, 9};
   auto max_it = std::max_element(v.begin(), v.end());
   // *max_it == 9
   ```

4. **`min_element`** - 返回范围内最小元素的位置

   ```c++
   auto min_it = std::min_element(v.begin(), v.end());
   // *min_it == 1
   
   // 使用自定义比较
   auto abs_min_it = std::min_element(v.begin(), v.end(), [](int a, int b) {
       return abs(a) < abs(b);
   });
   ```

## 数值操作

1. **`iota`** - 填充递增序列（C++11新增）

   ```c++
   std::vector<int> v(5);
   std::iota(v.begin(), v.end(), 0); // 0,1,2,3,4
   ```

2. **`accumulate`** - 累加

   ```c++
   int sum = std::accumulate(v.begin(), v.end(), 0);
   ```

3. **`inner_product`** - 内积

   ```c++
   int product = std::inner_product(v1.begin(), v1.end(), v2.begin(), 0);
   ```

这些算法配合C++11的lambda表达式使用，可以极大地简化代码并提高可读性。
