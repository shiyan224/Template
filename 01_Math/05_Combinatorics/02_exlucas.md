$C_{n}^{m} = \frac{n!}{(n-m)!m!}（m \leq n）$
$1 <= n,m <= 1e18, 2<=p<1e6, 不保证p是素数$
```cpp
ll calc(ll n, ll pi, ll pk)
{
    if (n == 0)
        return 1;
    ll res = 1;
    for (ll i = 2; i <= pk; ++i)
        if (i % pi)
            res = res * i % pk;
    res = mod_pow(res, n / pk, pk);
    for (ll i = 2; i <= n % pk; ++i)
        if (i % pi)
            res = res * i % pk;
    return res * calc(n / pi, pi, pk) % pk;
}
ll multilucas(ll n, ll m, ll pi, ll pk) //pi为质数，返回C(n,m)%pk,(pk为pi的多次方)
{
    ll x = calc(n, pi, pk), y = calc(m, pi, pk), z = calc(n - m, pi, pk);
    ll cnt = 0;
    for (ll i = n; i; i /= pi)
        cnt += i / pi;
    for (ll i = m; i; i /= pi)
        cnt -= i / pi;
    for (ll i = n - m; i; i /= pi)
        cnt -= i / pi;
    ll res = x * inv(y, pk) % pk * inv(z, pk) % pk * mod_pow(pi, cnt, pk) % pk;
    return res;
}
ll exlucas(ll n, ll m, ll p) //返回C(n,m)%p,m>n情况下返回-1
{
    if (m > n)
        return -1; //组合式没意义
    ll id = p, res = 0;
    for (ll i = 2; i <= id; ++i)
    {
        if (id % i == 0)
        {
            ll pk = 1;
            while (id % i == 0)
                pk *= i, id /= i;
            res = (res + multilucas(n, m, i, pk) * (p / pk) % p * inv(p / pk, pk) % p) % p; //中国剩余定理合并
        }
    }
    return res;
}
```