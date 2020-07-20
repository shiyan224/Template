```cpp
typedef long long ll;
ll Mul(ll a, ll b, ll mod)
{
    if (mod <= 1000000000)
        return a * b % mod;
    else if (mod <= 1000000000000LL)
        return (((a * (b >> 20) % mod) << 20) + (a * (b & ((1 << 20) - 1)))) % mod;
    else
    {
        ll d = (ll)floor(a * (long double)b / mod + 0.5);
        ll res = (a * b - d * mod) % mod;
        if (res < 0)
            res += mod;
        return res;
    }
}

bool Miller_Rabin(ll n, int s) //s >= 8
{
    if (n == 2)
        return true;
    if (n < 2 || !(n & 1))
        return false;
    int t = 0;
    ll u = n - 1;
    while ((u & 1) == 0)
        ++t, u >>= 1;
    for (int i = 0; i < s; ++i)
    {
        ll a = rand() % (n - 1) + 1;
        ll x = mod_pow(a, u, n);
        for (int j = 0; j < t; ++j)
        {
            ll y = Mul(x, x, n);
            if (y == 1 && x != 1 && x != n - 1)
                return false;
            x = y;
        }
        if (x != 1)
            return false;
    }
    return true;
}
```