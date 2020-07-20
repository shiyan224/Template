```cpp
int a[maxn], lis[maxn], lds[maxn];
int LIS(int n) //返回最长上升子序列长度，序列存在lis[]中（序列下标从1开始）
{
    int len = 1;                 //最长不上升子序列（逆序）
    lis[1] = a[1];               //lds[n] = a[n];
    for (int i = 2; i <= n; ++i) //for (int i = n; i >= 1; --i)
        if (lis[len] <= a[i])
            lis[++len] = a[i];
        else
            *lower_bound(lis + 1, lis + 1 + len, a[i]) = a[i];
    return len;
}
```
简单写法(下标从0开始,只返回长度)
```cpp
int dp[maxn];
int LIS(int n) //返回最长不下降子序列的长度
{
    memset(dp, 0x3f, sizeof(dp)); //返回最长不上升子序列的长度：
    for (int i = 0; i < n; ++i)   //for (int i = n - 1; i >= 0; --i)
        *lower_bound(dp, dp + n, a[i]) = a[i];
    return lower_bound(dp, dp + n, INF) - dp;
}
```