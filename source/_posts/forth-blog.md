---
title: 动态规划（dp）
tags: 算法
date: 2025-03-20
top_img: transparent
comments: false
---

# 动态规划

## 核心原理

### 重叠子问题

在递归分解问题时，相同子问题会被多次重复计算。例如，斐波那契数列中计算 F(5)需要多次计算 F(3)，动态规划通过缓存子问题解（如数组或哈希表）避免重复计算

### 最优子结构

问题的最优解可以由其子问题的最优解组合而成。例如，最短路径问题中，从 A 到 B 的最短路径必然包含中间点 C 的最短路径

### 状态转移方程

定义状态之间的关系式，明确如何从子问题推导出当前问题的解。
例如，0-1 背包问题的状态转移方程为：

```cpp
dp[i][j] = max(dp[i-1][j], dp[i-1][j-w[i]] + v[i])
```

## 实现步骤

1.定义状态

2.初始化边界条件

3.推导状态转移方程

4.计算顺序与填表

## 实现方式

1.自底向上迭代（推荐）​
通过循环逐步填充状态表，适合线性问题。例如，计算斐波那契数列：

```cpp
int fib(int n) {
    if (n <= 1) return n;
    int dp[n+1];
    dp[0] = 0, dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}
```

2.自顶向下递归（记忆化）​
通过递归分解问题，并用缓存避免重复计算。例如：

```cpp
vector<int> memo(n+1, -1);
int fib(int n) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    memo[n] = fib(n-1) + fib(n-2);
    return memo[n];
}
```

3.空间优化（滚动数组）​
对于某些问题（如背包问题），可将二维数组优化为一维。例如，0-1 背包的滚动数组实现：

```cpp
int knapsack(vector<int>& w, vector<int>& v, int C) {
    vector<int> dp(C+1, 0);
    for (int i = 0; i < w.size(); i++) {
        for (int j = C; j >= w[i]; j--) {  // 逆序遍历避免覆盖
            dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
        }
    }
    return dp[C];
}
```

## 经典问题

1.爬楼梯问题

2.0-1 背包问题

3.最长公共子序列（LCS）
状态转移方程为：

```cpp
dp[i][j] = (s1[i] == s2[j]) ? dp[i-1][j-1] + 1 : max(dp[i-1][j], dp[i][j-1])
```
