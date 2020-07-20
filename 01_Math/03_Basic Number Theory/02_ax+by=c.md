已知a,b,c，求x，使得 $ax \equiv b \pmod {n}$
```cpp
bool solve(ll a, ll b, ll c, ll &x, ll &y)
{
    ll d = exgcd(a, b, x, y);
    if (c % d != 0) //无解
        return false;
    int k = c / d;
    x *= k, y *= k;
    // x = (x % k + k) % k; //最小正整数解
    return true;
}
```