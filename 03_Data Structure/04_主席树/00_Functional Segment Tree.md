静态查询区间第 $k$ 小的值
```cpp
#define Lson l, m, lson[x], lson[y]
#define Rson m + 1, r, rson[x], rson[y]
const int maxn = ;
int a[maxn], rt[maxn];
int cnt;
int lson[maxn << 5], rson[maxn << 5], sum[maxn << 5];
void update(int pos, int l, int r, int &x, int y)
{
    lson[++cnt] = lson[y], rson[cnt] = rson[y], sum[cnt] = sum[y] + 1, x = cnt;
    if (l == r)
        return;
    int m = (l + r) >> 1;
    if (pos <= m)
        update(pos, Lson);
    else
        update(pos, Rson);
}
int query(int l, int r, int x, int y, int k)
{
    if (l == r)
        return l;
    int m = (l + r) >> 1;
    int s = sum[lson[y]] - sum[lson[x]]; //左子树的个数
    if (s >= k)
        return query(Lson, k);
    else
        return query(Rson, k - s);
}
int kth(int l, int r, int k, int n)
{
    return v[query(1, n, rt[l - 1], rt[r], k) - 1];
}
void init()
{
    v.clear(), cnt = 0;
    rt[0] = sum[0] = 0;
}

for (int i = 1; i <= n; ++i)
    scanf("%d", a + i), v.push_back(a[i]);
/* ---  离散化  ---- */
for (int i = 1; i <= n; ++i)
    update(getID(a[i]), 1, n, rt[i], rt[i - 1]);
res = kth(l, r, k, n);
```