```cpp
int mat[10][10], s[10];       //输入矩阵和辅助数组
int n, m;                     //矩阵的行、列
int MaxSeqSum(int a[], int n) //序列a和长度n，最大子段和
{
    int sum = 0, cur = 0;
    for (int i = 0; i < n; ++i)
        cur += a[i], sum = max(cur, sum), cur = max(0, cur);
    return sum;
}
int MaxSeqMatix() //最大子矩阵
{
    int res = 0;
    for (int i = 0; i < n; ++i)
    {
        memset(s, 0, sizeof(s));
        for (int j = i; j < n; ++j)
        {
            for (int k = 0; k < m; ++k)
                s[k] += mat[j][k];
            res = max(MaxSeqSum(s, m), res);
        }
    }
    return res;
}
```