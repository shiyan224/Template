$C_{n}^{m} = \frac{n!}{(n-m)!m!}（m \leq n）$
$1 \leq n,m \leq 10^9, 1<p<10^5且p是素数$
```cpp
//注意如果下标是n+m，则数组大小应为n+m，设为n会越界
const int maxx = min(maxn, maxp);
ll F[maxx];             //F[n]表示n!%p的值
void init(int up, ll p) //up = min(maxn,maxp)
{
    F[0] = 1;
    for (int i = 1; i <= up; ++i)
        F[i] = F[i - 1] * i % p; // % mod
}
ll inv(ll n, ll p) //阶乘n!的逆元
{
    if (n == 1)
        return 1;
    return inv(p % n, p) * (p - p / n) % p;
}
ll lucas(ll n, ll m, ll p)
{
    ll res = 1;
    while (n && m)
    {
        ll a = n % p, b = m % p;
        if (a < b)
            return 0;
        res = res * F[a] % p * inv(F[b] * F[a - b] % p, p) % p;
        n /= p, m /= p;
    }
    return res;
}
```