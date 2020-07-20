最长公共子串（Longest Common Substirng）和最长公共子序列（Longest Common Subsequence，LCS）的区别为：
子串是串的一个连续的部分，子序列则是从不改变序列的顺序，而从序列中去掉任意的元素而获得新的序列；
也就是说，子串中字符的位置必须是连续的，子序列则可以不必连续。
```cpp
int dp[maxn][maxn];
int LCS(const string &x, const string &y)
{
    memset(dp, 0, sizeof(dp));
    int n = x.length(), m = y.length();
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < m; ++j)
            if (x[i] == y[j])
                dp[i + 1][j + 1] = dp[i][j] + 1;
            else
                dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j]);
    return dp[n][m];
}
```