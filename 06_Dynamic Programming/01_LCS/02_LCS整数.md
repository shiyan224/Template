给出1~n的两个排列P1和P2，求它们的最长公共子序列。
```cpp
int a[maxn], b[maxn], idx[maxn];
int dp[maxn];
int n;

cin >> a[i];
idx[a[i]] = i;

cin >> b[i];
b[i] = idx[b[i]];
```