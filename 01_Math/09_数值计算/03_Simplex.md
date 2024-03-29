输入矩阵$a$描述线性规划的标准形式。\\\\
$a$为$m+1$行$n+1$列，其中行$0 \sim m-1$为不等式，行$m$为目标函数（最大化）。\\\\
列$0 \sim n-1$为变量$0 \sim n-1$的系数，列$n$为常数项。\\\\
约束为$a_{i, 0}x_0 + a_{i, 1}x_1 + \cdots \le a_{i, n}$，目标为$\max(a_{m, 0}x_0 + a_{m, 1}x_1 + \cdots + a_{m, n - 1}x_{n - 1} - a_{m, n})$\\\\
注意：变量均有非负约束$x[i] \ge 0$
```cpp
const int maxm = 500; // 约束数目上限
const int maxn = 500; // 变量数目上限
const double INF = 1e100;
const double eps = 1e-10;
struct Simplex
{
    int n;                // 变量个数
    int m;                // 约束个数
    double a[maxm][maxn]; // 输入矩阵
    int B[maxm], N[maxn]; // 算法辅助变量
    void pivot(int r, int c)
    {
        swap(N[c], B[r]);
        a[r][c] = 1 / a[r][c];
        for (int j = 0; j <= n; j++)
            if (j != c) a[r][j] *= a[r][c];
        for (int i = 0; i <= m; i++)
            if (i != r)
            {
                for (int j = 0; j <= n; j++)
                    if (j != c) a[i][j] -= a[i][c] * a[r][j];
                a[i][c] = -a[i][c] * a[r][c];
            }
    }
    bool feasible()
    {
        for (;;)
        {
            int r, c;
            double p = INF;
            for (int i = 0; i < m; i++)
                if (a[i][n] < p) p = a[r = i][n];
            if (p > -eps) return true;
            p = 0;
            for (int i = 0; i < n; i++)
                if (a[r][i] < p) p = a[r][c = i];
            if (p > -eps) return false;
            p = a[r][n] / a[r][c];
            for (int i = r + 1; i < m; i++)
                if (a[i][c] > eps)
                {
                    double v = a[i][n] / a[i][c];
                    if (v < p) r = i, p = v;
                }
            pivot(r, c);
        }
    }
    // 解有界返回1，无解返回0，无界返回-1。b[i]为x[i]的值，ret为目标函数的值
    int simplex(int n, int m, double x[maxn], double& ret)
    {
        this->n = n, this->m = m;
        for (int i = 0; i < n; i++) N[i] = i;
        for (int i = 0; i < m; i++) B[i] = n + i;
        if (!feasible()) return 0;
        for (;;)
        {
            int r, c;
            double p = 0;
            for (int i = 0; i < n; i++)
                if (a[m][i] > p) p = a[m][c = i];
            if (p < eps)
            {
                for (int i = 0; i < n; i++)
                    if (N[i] < n) x[N[i]] = 0;
                for (int i = 0; i < m; i++)
                    if (B[i] < n) x[B[i]] = a[i][n];
                ret = -a[m][n];
                return 1;
            }
            p = INF;
            for (int i = 0; i < m; i++)
                if (a[i][c] > eps)
                {
                    double v = a[i][n] / a[i][c];
                    if (v < p) r = i, p = v;
                }
            if (p == INF) return -1;
            pivot(r, c);
        }
    }
};
```