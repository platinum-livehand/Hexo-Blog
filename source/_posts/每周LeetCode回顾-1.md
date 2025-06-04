---
title: 每周LeetCode回顾-1
date: 2025-06-02 20:06:35
tags:
  - leetcode
  - C++
categories: leetcode
---

## [456. 132模式](https://leetcode.cn/problems/132-pattern/)

### 思路：

从后往前遍历，维护一个**单调递减**的栈，同时使用 `k` 记录所有出栈元素的最大值。

```c++
class Solution {
public:
    bool find132pattern(vector<int>& nums) 
    {
        // 获取数组大小
        int n = nums.size();
        // 单调栈
        stack<int> stk;
        // 存储 k
        int k = INT_MIN;
        // 利用单调栈使得一直满足 j < k 
        for(int i = n-1; i >= 0; --i)
        {
            // 发现 k > i 的情况直接返回 true
            if(k > nums[i]) return true;
            while(!stk.empty() && stk.top() < nums[i])
            {
                // 贪心，每次挑选尽量大的值作为 k
                k = max(stk.top(), k);
                stk.pop();
            }
            stk.emplace(nums[i]);
        }
        // 没有满足 132 模式的情况
        return false;
    }
};
```

- **时间复杂度：O(n)**
- **空间复杂度：O(n)**

## [189. 轮转数组](https://leetcode.cn/problems/rotate-array/)

### 思路：

![](./img/每周LeetCode回顾-1/轮转数组.png)

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) 
    {
        // 数组大小
        int n = nums.size();
        // 对 k 取模
        k %= n;
        // 整体反转
        reverse(nums.begin(), nums.end());
        // 反转前半部分
        reverse(nums.begin(), nums.begin()+k);
        // 反转后半部分
        reverse(nums.begin()+k, nums.end());
    }
};
```

- **时间复杂度：O(n)**
- **空间复杂度：O(1)**

## [73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes)

### 思路：

使用与行列数量相等的数组记录置零信息。

```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) 
    {
        // 行数
        int m = matrix.size();
        // 列数
        int n = matrix[0].size();
        // 记录置零位置
        vector<int> cols(n);
        vector<int> rows(m);
        // 遍历矩阵，寻找置零位置
        for(int i = 0; i < m; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                if(matrix[i][j] == 0)
                {
                    rows[i] = 1;
                    cols[j] = 1;
                }
            }
        }
        // 遍历矩阵将记录的行和列置零
        for(int i = 0; i < m; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                if(rows[i] == 1 || cols[j] == 1)
                {
                    matrix[i][j] = 0;
                }
            }
        }
    }
};
```

- **时间复杂度**：**O(m × n)**
- **空间复杂度**：**O(m + n)**

## [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self)

### 思路：

`answer[i]` 等于 `nums` 中除了 `nums[i]` 之外其余各元素的乘积。换句话说，如果知道了 `i` 左边所有数的乘积，以及 `i` 右边所有数的乘积，就可以算出 `answer[i]`。

于是：

- 定义 `pre[i]`表示从 `nums[0]` 到 `nums[i−1]` 的乘积。

- 定义 `suf[i]` 表示从 `nums[i+1]` 到 `nums[n−1]` 的乘积。

我们可以先计算出从 `nums[0]` 到 `nums[i−2]` 的乘积 `pre[i−1]`，再乘上 `nums[i−1]`，就得到了 `pre[i]`，即
$$
pre[i]=pre[i−1]⋅nums[i−1]
$$
同理有

$$
suf[i]=suf[i+1]⋅nums[i+1]
$$
初始值：`pre[0]=suf[n−1]=1`。按照上文的定义，`pre[0]` 和 `suf[n−1]` 都是空子数组的元素乘积，我们规定这是 1，因为 1 乘以任何数 `x` 都等于 `x`，这样可以方便递推计算 `pre[1]`，`suf[n−2]` 等。

算出 `pre` 数组和 `suf` 数组后，有

$$
answer[i]=pre[i]⋅suf[i]
$$

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) 
    {
        int n = nums.size();
        // 前缀乘积（不包含本元素）
        vector<int> preMulti(n, 1);
        // 后缀乘积（不包含本元素）
        vector<int> posMulti(n, 1);
        // 计算前缀乘积
        for(int i = 1; i < n; ++i)
        {
            preMulti[i] = preMulti[i-1]*nums[i-1];
        }
        // 计算后缀乘积
        for(int j = n-2; j >= 0; --j)
        {
            posMulti[j] = posMulti[j+1]*nums[j+1];
        }
        // 结果
        vector<int> ans(n);
        for(int i = 0; i < n; ++i)
        {
            // 前缀乘积 与 后缀乘积相乘
            ans[i] = posMulti[i] * preMulti[i];
        }
        return ans;
    }
};
```

- **时间复杂度**：**O(n)**
- **空间复杂度**：**O(n)**

## [2134. 最少交换次数来组合所有的 1 II](https://leetcode.cn/problems/minimum-swaps-to-group-all-1s-together-ii/)

### 思路：

将问题转化为 **定长滑动窗口内 0 的最小个数**

```c++
class Solution {
public:
    int minSwaps(vector<int>& nums) 
    {
        // 数组大小
        int n = nums.size(), k = 0;
        // 计算 1 的个数（窗口长度）
        for(auto& num : nums)
        {
            if(num == 1) k++;
        }
        // 拼接环形数组
        nums.insert(nums.end(), nums.begin(), nums.begin()+k);

        int ans = INT_MAX;
        // 记录 0 和 1 的个数
        vector<int> hash(2);
        // 利用定长滑动窗口记录窗口中 0 的个数即可
        for(int i = 0; i < nums.size(); ++i)
        {
            // 进入窗口
            hash[nums[i]]++;
            // 小于窗口长度
            if(i < k) continue;
            // 退出窗口
            hash[nums[i-k]]--;
            // 找到 0 最少的窗口
            ans = min(ans, hash[0]);
        }
        return ans;
    }
};
```

- **时间复杂度**：**O(n)**
- **空间复杂度**：**O(n)**

## [1297. 子串的最大出现次数](https://leetcode.cn/problems/maximum-number-of-occurrences-of-a-substring/)

### 思路：

只需统计长度为 minSize 的子串，而不需要统计长度为 maxSize 的字串，因为 "abc" 肯定会覆盖 "a"，"ab"，即长的肯定会覆盖短的，只需要考虑最短的即可。

```c++
class Solution {
public:
    int maxFreq(string s, int maxLetters, int minSize, int maxSize) 
    {
        int n = s.size(), ans = 0;
        // 统计子串出现的次数
        unordered_map<string, int> cnt;
        for(int i = 0; i < n-minSize+1; ++i)
        {
            // 获取子串（定长滑动窗口）
            string subString = s.substr(i, minSize);
            // 利用哈希集合统计不同字母的个数
            unordered_set<int> hash_set(subString.begin(), subString.end());
            // 如果满足不同字母数目小于等于 maxLetters
            if(hash_set.size() <= maxLetters)
            {
                // 当前子串个数++
                cnt[subString]++;
                // 尝试更新最长子串
                ans = max(ans, cnt[subString]);
            }
        }    
        return ans;
    }
};
```

- **时间复杂度**：**O(n*minSize)**
- **空间复杂度**：**O(n*minSize)**

## [2389. 和有限的最长子序列](https://leetcode.cn/problems/longest-subsequence-with-limited-sum)

### 思路：

- 贪心：由于元素和有上限，为了能让子序列尽量长，子序列中的元素值越小越好。

对于本题来说，元素在数组中的位置是无关紧要的（因为我们计算的是元素和），所以可以排序了。把 `nums` 从小到大排序后，再从小到大选择尽量多的元素（相当于选择一个前缀），使这些元素的和不超过询问值。

- 既然求的是前缀的元素和（前缀和），那么干脆把每个前缀和都算出来。做法是递推：前 `i` 个数的元素和，等于前 `i−1` 个数的元素和，加上第 `i` 个数的值。

例如 `[4,5,2,1]` 排序后为 `[1,2,4,5]`，从左到右递推计算前缀和，得到 `[1,3,7,12]`。

- 由于 `nums[i]` 都是正整数，前缀和是严格单调递增的，这样就能在前缀和上使用二分查找：找到大于 `queries[i]` 的第一个数的下标，由于下标是从 `0` 开始的，这个数的下标正好就是前缀和小于等于 `queries[i]` 的最长前缀的长度。


例如在 `[1,3,7,12]` 二分查找大于 `3` 的第一个数（`7`），得到下标 `2`，这正好就是前缀和小于等于 `3` 的最长前缀长度。对应到 `nums` 中，就是选择了 `2` 个数（`1` 和 `2`）作为子序列中的元素。

```c++
class Solution {
public:
    vector<int> answerQueries(vector<int>& nums, vector<int>& queries) 
    {
        int n = nums.size(), m = queries.size();

        // 由于题目只要求子序列，所以可以对原数组进行排序
        sort(nums.begin(), nums.end());

        // 计算元素前缀和
        // 可以使用 C++ 函数 partial_sum(nums.begin(), nums.end()) 原地求前缀和
        vector<int> preSum(n);
        preSum[0] = nums[0];
        for(int i = 1; i < n; ++i)
        {
            preSum[i] = preSum[i-1]+nums[i];
        }    

        vector<int> ans;
        for(int i = 0; i < m; ++i)
        {
            // lower_bound 找到第一个 <= 的元素
            // upper_bound 找到第一个 < 的元素，会覆盖所有 <= 的元素
            auto it = upper_bound(preSum.begin(), preSum.end(), queries[i]);
            // 计算元素个数
            ans.emplace_back(it-preSum.begin());
        }

        return ans;
    }
};
```

## [2140. 解决智力问题](https://leetcode.cn/problems/solving-questions-with-brainpower)

### 思路：

#### **一、寻找子问题**

在`示例1`中，我们要解决的问题（原问题）是：

- **剩余问题的下标为 `[0, 3]`**，计算从这些问题中可以获得的最大分数。

**讨论 `questions[0]` 选或不选的情况**：

- 如果不选 `questions[0]`子问题变为：剩余问题的下标为 `[1, 3]`，计算从这些问题中可以获得的最大分数。
- 如果选，接下来的 `2` 个问题（`brainpower[0] = 2`）都不能选，子问题变为：剩余问题的下标为 `[3, 3]`，计算从这些问题中可以获得的最大分数。

------

#### **二、状态定义与递推公式**

根据上面的讨论，定义 `dp(i)` 表示：剩余问题的下标为 `[i, n-1]` 时，能获得的最大分数（`n` 是 `questions` 的长度）。

**讨论 `questions[i]` 选或不选的情况**：

- 如果不选，子问题为 `dp(i + 1)`（剩余问题下标 `[i+1, n-1]`）。
- 如果选，跳过 `brainpower[i]` 个问题，子问题为 `dp(i + brainpower[i] + 1)`（剩余问题下标 `[i + brainpower[i] + 1, n-1]`），并累加当前分数 `points[i]`。

**递推公式**：
$$
dp(i)=max(dp(i+1),dp(i+brainpower[i]+1)+points[i])
$$

```c++
class Solution {
public:
    long long mostPoints(vector<vector<int>>& questions) 
    {
        int n = questions.size();
        // 剩余问题的下标为 [i,n-1] 时，能获得的最大分数
        vector<long long> dp(n+1);
        for(int i = n-1; i >= 0; --i)
        {
            // dp[i+1]：不选questions[i]
            // dp[i+questions[i][1]+1]：选questions[i]
            dp[i] = max(dp[i+1], dp[min(i+questions[i][1]+1, n)]+questions[i][0]);
        }
        return dp[0];
    }
};
```

## [53. 最大数组和](https://leetcode.cn/problems/maximum-subarray/)

### 思路 1：前缀和 + 贪心

由于子数组的元素和等于两个前缀和的差，所以求出 `nums` 的前缀和，问题就变成 [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/) 了。本题子数组不能为空，相当于一定要交易一次。

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) 
    {
        int n = nums.size();

        // 计算前缀和
        vector<int> preSum(n);
        preSum[0] = nums[0];
        for(int i = 1; i < n; ++i)
        {
            preSum[i] = preSum[i-1] + nums[i];
        }

        int ans = preSum[0];
        int localMinimum = 0; // 最小值
        for(auto& value : preSum)
        {
            // 更新最大的数组和
            ans = max(ans, value-localMinimum);
            // 更新最小值
            localMinimum = min(localMinimum, value);
        }

        return ans;
    }
};
```

### 思路 2：动态规划

#### **动态规划定义**

定义 `dp[i]` 表示以 `nums[i]` 结尾的最大子数组和。

**状态转移方程**

分两种情况讨论：

- `nums[i]` 单独成子数组 `dp[i] = nums[i]`。
- `nums[i]` 与前面的子数组拼接 `dp[i] = dp[i-1] + nums[i]`。

综合两种情况，取最大值：
$$
f(x) = 
\begin{cases} 
nums[i], & i = 0 \\
max(dp[i-1],0)+nums[i], & i \geq 0 
\end{cases}
$$
