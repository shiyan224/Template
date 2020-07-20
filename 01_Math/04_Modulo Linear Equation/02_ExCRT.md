```cpp
bool excrt(ll r[], ll m[], ll n, ll &res, ll &M)
{
    ll a, b, c, x, y;
    M = m[0], res = r[0];
    for (int i = 1; i < n; ++i)
    {
        a = M, b = m[i], c = r[i] - res;
        ll d = exgcd(a, b, x, y);
        if (c % d != 0)
            return false;
        x = Mul(x, c / d, b / d); //爆long long，需要快速乘
        res += x * M;
        M = M / d * m[i];
        res %= M;
    }
    res = (res % M + M) % M;
    return true;
}
```