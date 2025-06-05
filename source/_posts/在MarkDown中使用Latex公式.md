---
title: 在markdown中使用latex公式
date: 2025-06-04 17:35:14
tags:
  - Hexo
  - Markdown
  - Latex
categories: 问题记录

---

# LaTeX 公式完整示例

## 1. 行内公式

这是爱因斯坦质能方程：$E=mc^2$，其中 $E$ 是能量，$m$ 是质量，$c$ 是光速。

## 2. 独行公式

二次方程求根公式：

```latex
$$
x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$
```


$$
x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$

## 3. 多行公式（对齐）

利用 `align` 环境实现公式对齐：

```latex
$$
\begin{align}
f(x) &= (x+1)^2 \\
     &= x^2 + 2x + 1
\end{align}
$$
```


$$
\begin{align}
f(x) &= (x+1)^2 \\
     &= x^2 + 2x + 1
\end{align}
$$

## 4. 矩阵

一个 2x2 矩阵示例：

```latex
$$
\begin{bmatrix}
1 & 2 \\
3 & 4 \\
\end{bmatrix}
$$
```


$$
\begin{bmatrix}
1 & 2 \\
3 & 4 \\
\end{bmatrix}
$$

## 5. 常用数学符号

- 求和：$\sum_{i=1}^n i = \frac{n(n+1)}{2}$
- 积分：$\int_a^b f(x)dx$
- 希腊字母：$\alpha, \beta, \gamma$
- 不等式：$a > b \leq c \neq d$

## 6. 分段函数

用 `cases` 环境定义分段函数：

```latex
$$
f(x) = 
\begin{cases} 
1 & \text{if } x \geq 0 \\
0 & \text{otherwise}
\end{cases}
$$
```


$$
f(x) = 
\begin{cases} 
1 & \text{if } x \geq 0 \\
0 & \text{otherwise}
\end{cases}
$$

---

## 渲染效果说明

1. **行内公式**会嵌入文字中，如 $E=mc^2$。
2. **独行公式**会居中显示，如二次方程求根公式。
3. **多行公式**通过 `&` 对齐，等号位置一致。
4. **矩阵**用 `bmatrix` 环境显示方括号。
5. 符号和希腊字母直接渲染为数学格式。
6. **分段函数**用大括号包裹不同条件。
7. 化学方程式可能需要额外扩展支持（如 Typora）。
