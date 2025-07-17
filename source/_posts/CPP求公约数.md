---
title: C++求最大最小公约数
date: 2025-06-29 21:33:00
tags:
---

在 C++ 中，可以使用 **欧几里得算法（辗转相除法）** 高效计算两个数的最大公约数（GCD）。以下是几种实现方式：

------

### **方法 1：递归实现（推荐）**

```c++
#include <iostream>
using namespace std;

int gcd(int a, int b) 
{
    return b == 0 ? a : gcd(b, a % b);
}

int main() 
{
    int a = 24, b = 36;
    cout << "GCD of " << a << " and " << b << " is: " << gcd(a, b) << endl;
    return 0;
}
```

**输出**：

```cmd
GCD of 24 and 36 is: 12
```

### **方法 2：迭代实现**

```c++
#include <iostream>
using namespace std;

int gcd(int a, int b) 
{
    while (b != 0) 
    {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

int main() 
{
    int a = 56, b = 98;
    cout << "GCD of " << a << " and " << b << " is: " << gcd(a, b) << endl;
    return 0;
}
```

**输出**：

```cmd
GCD of 56 and 98 is: 14
```

### **方法 3：使用 C++17 的 `<numeric>` 标准库**

C++17 引入了 `std::gcd`，可以直接调用：

```c++
#include <iostream>
#include <numeric> // 需要包含此头文件
using namespace std;

int main() 
{
    int a = 81, b = 153;
    cout << "GCD of " << a << " and " << b << " is: " << gcd(a, b) << endl;
    return 0;
}
```

**注意**：需编译器支持 C++17 或更高版本（编译时加 `-std=c++17`）。

### **关键点**

1. **欧几里得算法原理**：
   $$
   gcd(a,b)=gcd(b,a\text{ mod } b)
   $$
   直到余数为 0 时，当前的除数即为 GCD。

2. **时间复杂度**：$O(log(min(a,b)))$，效率极高。

3. **处理负数**：
   若输入可能为负数，需先取绝对值：

   ```c++
   int gcd(int a, int b) 
   {
       a = abs(a);
       b = abs(b);
       return b == 0 ? a : gcd(b, a % b);
   }
   ```

### **扩展：计算最小公倍数（LCM）**

利用公式 $LCM(a, b)=\frac{a*b}{gcd(a,b)}$：

```c++
int lcm(int a, int b) 
{
    return (a / gcd(a, b)) * b; // 先除后乘避免溢出
}
```
