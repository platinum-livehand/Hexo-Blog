---
title: CPP实现十大排序算法
date: 2025-05-29 17:32:45
tags: 
  - C++
  - 排序算法
  - leetcode
categories: 数据结构与算法
cover: /img/CPP实现十大排序算法/background.png
---

以下是 **C++ 实现的十大排序算法** 及其详细介绍，包括 **时间复杂度、空间复杂度、稳定性、适用场景** 等。

------

## **1. 冒泡排序（Bubble Sort）**

**思想**：重复比较相邻元素，如果顺序错误就交换，直到没有交换发生。
​**​特点​**​：简单但效率低，适合小规模数据。

```c++
void bubbleSort(vector<int>& arr) 
{
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) 
    {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; j++) 
        {
            if (arr[j] > arr[j + 1]) 
            {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped) break; // 提前终止
    }
}
```

**复杂度分析**：

- **时间复杂度**：
  - 最好 `O(n)`（已排序）
  - 平均 `O(n²)`
  - 最坏 `O(n²)`
- **空间复杂度**：`O(1)`（原地排序）
- **稳定性**：✅ 稳定

------

## **2. 选择排序（Selection Sort）**

**思想**：每次选择最小的元素放到已排序部分的末尾。
​**​特点​**​：简单但效率低，不稳定。

```c++
void selectionSort(vector<int>& arr) 
{
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) 
    {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) 
        {
            if (arr[j] < arr[min_idx]) 
            {
                min_idx = j;
            }
        }
        swap(arr[i], arr[min_idx]);
    }
}
```

**复杂度分析**：

- **时间复杂度**：`O(n²)`（始终如此）
- **空间复杂度**：`O(1)`
- **稳定性**：❌ 不稳定（交换可能改变相同元素的顺序）

------

## **3. 插入排序（Insertion Sort）**

**思想**：将未排序部分的元素插入到已排序部分的正确位置。
​**​特点​**​：对小规模或基本有序的数据高效。

```c++
void insertionSort(vector<int>& arr) 
{
    int n = arr.size();
    for (int i = 1; i < n; i++) 
    {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) 
        {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

**复杂度分析**：

- **时间复杂度**：
  - 最好 `O(n)`（已排序）
  - 平均 `O(n²)`
  - 最坏 `O(n²)`
- **空间复杂度**：`O(1)`
- **稳定性**：✅ 稳定

------

## **4. 希尔排序（Shell Sort）**

**思想**：改进的插入排序，通过分组排序减少逆序对。
​**​特点​**​：比 `O(n²)` 快，但不稳定。

```c++
void shellSort(vector<int>& arr) 
{
    int n = arr.size();
    for (int gap = n / 2; gap > 0; gap /= 2) 
    {
        for (int i = gap; i < n; i++) 
        {
            int temp = arr[i];
            int j;
            for (j = i; j >= gap && arr[j - gap] > temp; j -= gap) 
            {
                arr[j] = arr[j - gap];
            }
            arr[j] = temp;
        }
    }
}
```

**复杂度分析**：

- **时间复杂度**：`O(n log n)` ~ `O(n²)`（取决于 `gap` 序列）
- **空间复杂度**：`O(1)`
- **稳定性**：❌ 不稳定

------

## **5. 归并排序（Merge Sort）**

**思想**：分治法，递归排序后合并。
​**​特点​**​：稳定且高效，但需要额外空间。

```c++
void merge(vector<int>& arr, int l, int m, int r) 
{
    vector<int> temp(r - l + 1);
    int i = l, j = m + 1, k = 0;
    while (i <= m && j <= r) 
    {
        if (arr[i] <= arr[j]) temp[k++] = arr[i++];
        else temp[k++] = arr[j++];
    }
    while (i <= m) temp[k++] = arr[i++];
    while (j <= r) temp[k++] = arr[j++];
    for (int p = 0; p < k; p++) arr[l + p] = temp[p];
}

void mergeSort(vector<int>& arr, int l, int r) 
{
    if (l >= r) return;
    int m = l + (r - l) / 2;
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);
    merge(arr, l, m, r);
}
```

**复杂度分析**：

- **时间复杂度**：`O(n log n)`（始终如此）
- **空间复杂度**：`O(n)`（需要额外数组）
- **稳定性**：✅ 稳定

------

## **6. 快速排序（Quick Sort）**

**思想**：分治法，选一个 `pivot`，将小的放左边，大的放右边。
​**​特点​**​：平均最快，但不稳定。

```c++
int partition(vector<int>& arr, int low, int high) 
{
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) 
    {
        if (arr[j] < pivot) 
        {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) 
{
    if (low < high) 
    {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

**复杂度分析**：

- **时间复杂度**：
  - 最好 `O(n log n)`
  - 平均 `O(n log n)`
  - 最坏 `O(n²)`（已排序时）
- **空间复杂度**：`O(log n)`（递归栈）
- **稳定性**：❌ 不稳定

------

## **7. 堆排序（Heap Sort）**

**思想**：利用堆结构（大顶堆/小顶堆）进行排序。
​**​特点​**​：原地排序，但不稳定。

```c++
void heapify(vector<int>& arr, int n, int i) 
{
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    if (left < n && arr[left] > arr[largest]) largest = left;
    if (right < n && arr[right] > arr[largest]) largest = right;
    if (largest != i) 
    {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) 
{
    int n = arr.size();
    for (int i = n / 2 - 1; i >= 0; i--) 
    {
        heapify(arr, n, i);
    }
    for (int i = n - 1; i > 0; i--) 
    {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```

**复杂度分析**：

- **时间复杂度**：`O(n log n)`（始终如此）
- **空间复杂度**：`O(1)`
- **稳定性**：❌ 不稳定

------

## **8. 计数排序（Counting Sort）**

**思想**：统计每个元素的出现次数，然后按顺序填充。
​**​特点​**​：适用于小范围整数排序。

```c++
void countingSort(vector<int>& arr) {
    if (arr.empty()) return;
    int max_val = *max_element(arr.begin(), arr.end());
    int min_val = *min_element(arr.begin(), arr.end());
    int range = max_val - min_val + 1;
    vector<int> count(range), output(arr.size());
    for (int num : arr) count[num - min_val]++;
    for (int i = 1; i < range; i++) count[i] += count[i - 1];
    for (int i = arr.size() - 1; i >= 0; i--) 
    {
        output[count[arr[i] - min_val] - 1] = arr[i];
        count[arr[i] - min_val]--;
    }
    arr = output;
}
```

**复杂度分析**：

- **时间复杂度**：`O(n + k)`（`k` 是数据范围）
- **空间复杂度**：`O(n + k)`
- **稳定性**：✅ 稳定

------

## **9. 桶排序（Bucket Sort）**

**思想**：将数据分到多个桶，每个桶单独排序后合并。
​**​特点​**​：适用于均匀分布的数据。

```c++
void bucketSort(vector<float>& arr) 
{
    int n = arr.size();
    vector<vector<float>> buckets(n);
    for (float num : arr) 
    {
        int idx = num * n;
        buckets[idx].push_back(num);
    }
    for (auto& bucket : buckets) 
    {
        sort(bucket.begin(), bucket.end());
    }
    int idx = 0;
    for (auto& bucket : buckets) 
    {
        for (float num : bucket) 
        {
            arr[idx++] = num;
        }
    }
}
```

**复杂度分析**：

- **时间复杂度**：
  - 最好 `O(n + k)`（`k` 是桶的数量）
  - 最坏 `O(n²)`（所有数据在一个桶）
- **空间复杂度**：`O(n + k)`
- **稳定性**：✅ 稳定（取决于桶内排序算法）

------

## **10. 基数排序（Radix Sort）**

**思想**：按位排序（从低位到高位）。
​**​特点​**​：适用于整数或字符串排序。

```c++
void countingSortRadix(vector<int>& arr, int exp) 
{
    int n = arr.size();
    vector<int> output(n), count(10);
    for (int num : arr) count[(num / exp) % 10]++;
    for (int i = 1; i < 10; i++) count[i] += count[i - 1];
    for (int i = n - 1; i >= 0; i--) 
    {
        output[count[(arr[i] / exp) % 10] - 1] = arr[i];
        count[(arr[i] / exp) % 10]--;
    }
    arr = output;
}

void radixSort(vector<int>& arr) {
    int max_val = *max_element(arr.begin(), arr.end());
    for (int exp = 1; max_val / exp > 0; exp *= 10) {
        countingSortRadix(arr, exp);
    }
}
```

**复杂度分析**：

- **时间复杂度**：`O(d(n + k))`（`d` 是位数，`k` 是基数）
- **空间复杂度**：`O(n + k)`
- **稳定性**：✅ 稳定

------

## **总结对比**

|   排序算法   |     平均时间复杂度     |   最好情况    |   最坏情况    | 空间复杂度 | 稳定性 |     适用场景     |
| :----------: | :--------------------: | :-----------: | :-----------: | :--------: | :----: | :--------------: |
| **冒泡排序** |        `O(n²)`         |    `O(n)`     |    `O(n²)`    |   `O(1)`   |   ✅    |    小规模数据    |
| **选择排序** |        `O(n²)`         |    `O(n²)`    |    `O(n²)`    |   `O(1)`   |   ❌    |    简单但低效    |
| **插入排序** |        `O(n²)`         |    `O(n)`     |    `O(n²)`    |   `O(1)`   |   ✅    | 小规模或基本有序 |
| **希尔排序** | `O(n log n)` ~ `O(n²)` | `O(n log n)`  |    `O(n²)`    |   `O(1)`   |   ❌    |   中等规模数据   |
| **归并排序** |      `O(n log n)`      | `O(n log n)`  | `O(n log n)`  |   `O(n)`   |   ✅    |  大规模稳定排序  |
| **快速排序** |      `O(n log n)`      | `O(n log n)`  |    `O(n²)`    | `O(log n)` |   ❌    |  大规模高效排序  |
|  **堆排序**  |      `O(n log n)`      | `O(n log n)`  | `O(n log n)`  |   `O(1)`   |   ❌    |     原地排序     |
| **计数排序** |       `O(n + k)`       |  `O(n + k)`   |  `O(n + k)`   | `O(n + k)` |   ✅    |    小范围整数    |
|  **桶排序**  |       `O(n + k)`       |  `O(n + k)`   |    `O(n²)`    | `O(n + k)` |   ✅    |   均匀分布数据   |
| **基数排序** |     `O(d(n + k))`      | `O(d(n + k))` | `O(d(n + k))` | `O(n + k)` |   ✅    |   整数或字符串   |
