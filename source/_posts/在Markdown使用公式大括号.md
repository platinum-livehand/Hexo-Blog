---
title: 在Markdown使用公式大括号
date: 2025-06-04 17:35:14
tags:
  - Markdown
categories: 问题记录
---

在 Typora 中使用 **Markdown 公式大括号**（如分段函数、方程组等）可以通过 **LaTeX 语法**实现。以下是详细方法：

------

### **1. 基础语法（单行大括号）**

#### **(1) 简单分段函数**

使用 `cases` 环境：

```latex
$$
f(x) = 
\begin{cases} 
x^2, & x \geq 0 \\
-x, & x < 0 
\end{cases}
$$
```

**效果**：
$$
f(x) = 
\begin{cases} 
x^2, & x \geq 0 \\
-x, & x < 0 
\end{cases}
$$

#### **(2) 多条件对齐**

用 `&` 对齐条件和结果：

```latex
$$
P(x) = 
\begin{cases} 
0, & \text{if } x \leq 0 \\
\frac{1}{x}, & \text{if } x > 0 
\end{cases}
$$
```

**效果**：
$$
P(x) = 
\begin{cases} 
0, & \text{if } x \leq 0 \\
\frac{1}{x}, & \text{if } x > 0 
\end{cases}
$$

------

### **2. 复杂大括号（多行公式）**

#### **(1) 方程组（`aligned` 环境）**

```latex
$$
\left\{ 
\begin{aligned} 
x + y &= 5 \\ 
2x - y &= 1 
\end{aligned} 
\right.
$$
```

**效果**：
$$
\left\{ 
\begin{aligned} 
x + y &= 5 \\ 
2x - y &= 1 
\end{aligned} 
\right.
$$

#### **(2) 多行左对齐（`array` 环境）**

```latex
$$
\left\{ 
\begin{array}{l} 
x \in \mathbb{R} \\ 
y \geq 0 \\ 
z \neq 1 
\end{array} 
\right.
$$
```

**效果**：
$$
\left\{ 
\begin{array}{l} 
x \in \mathbb{R} \\ 
y \geq 0 \\ 
z \neq 1 
\end{array} 
\right.
$$

------

### **3. 自动缩放大括号**

用 `\left\{` 和 `\right.` 让大括号自适应高度：

```latex
$$
\left\{ 
\frac{1}{2}, \frac{3}{4}, \ldots, \frac{n}{n+1} 
\right\}
$$
```

**效果**：
$$
\left\{ 
\frac{1}{2}, \frac{3}{4}, \ldots, \frac{n}{n+1} 
\right\}
$$

------

### **4. 注意事项**

1. **Typora 设置**：
   - 确保启用 **内联公式**（`Preferences` → `Markdown` → 勾选 `Inline Math`）。
   - 使用 `$$ ` 包裹公式以进入块模式。
2. **语法高亮**：
   - Typora 会实时渲染 LaTeX，但错误语法可能导致渲染失败（如缺少 `\end{cases}`）。
3. **扩展功能**：
   - 安装 **Pandoc** 或 **MathJax** 插件以支持更复杂的 LaTeX 命令。

------

### **5. 完整示例**

```latex
$$
\mathbf{F}(x) = 
\left\{ 
\begin{array}{cl} 
\int_0^x f(t) \, dt, & x \in [0, 1] \\ 
\sum_{k=0}^n g(k), & x > 1 
\end{array} 
\right.
$$
```

**效果**：
$$
\mathbf{F}(x) = 
\left\{ 
\begin{array}{cl} 
\int_0^x f(t) \, dt, & x \in [0, 1] \\ 
\sum_{k=0}^n g(k), & x > 1 
\end{array} 
\right.
$$

------

通过以上方法，你可以在 Typora 中轻松实现各种公式大括号！
