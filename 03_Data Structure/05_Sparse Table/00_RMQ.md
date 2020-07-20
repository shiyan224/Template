一维RMQ
```cpp
int a[maxn], dp[maxn][20];
void init(int n)
{
    for (int i = 1; i <= n; i++)
    {
        scanf("%d", &a[i]);
        dp[i][0] = a[i];
    }
    for (int j = 1; (1 << j) <= n; j++)
        for (int i = 1; i + (1 << j) - 1 <= n; i++)
            dp[i][j] = max(dp[i][j - 1], dp[i + (1 << (j - 1))][j - 1]);
}
int rmq(int l, int r, int op) // 返回[l,r]最大值
{
    int k = 0; //int k = 31 - __builtin_clz(r - l + 1);
    while ((1 << (k + 1)) <= r - l + 1)
        ++k;
    return max(dp[l][k], dp[r - (1 << k) + 1][k]);
}
```
二维RMQ
```cpp
void init(int n, int m)
{
    for (int i = 0; (1 << i) <= n; i++)
        for (int j = 0; (1 << j) <= m; j++)
        {
            if (i == 0 && j == 0)
                continue;
            for (int row = 1; row + (1 << i) - 1 <= n; row++)
                for (int col = 1; col + (1 << j) - 1 <= m; col++)
                    if (i)
                        dp[row][col][i][j] = max(dp[row][col][i - 1][j], dp[row + (1 << (i - 1))][col][i - 1][j]);
                    else
                        dp[row][col][i][j] = max(dp[row][col][i][j - 1], dp[row][col + (1 << (j - 1))][i][j - 1]);
        }
}
int rmq(int x1, int y1, int x2, int y2)
{
    int kx = 31 - __builtin_clz(x2 - x1 + 1);
    int ky = 31 - __builtin_clz(y2 - y1 + 1);
    int m1 = dp[x1][y1][kx][ky];
    int m2 = dp[x2 - (1 << kx) + 1][y1][kx][ky];
    int m3 = dp[x1][y2 - (1 << ky) + 1][kx][ky];
    int m4 = dp[x2 - (1 << kx) + 1][y2 - (1 << ky) + 1][kx][ky];
    return max({m1, m2, m3, m4});
}
```