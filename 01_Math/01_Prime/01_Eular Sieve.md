原理：对于任意合数，必定可以有最小质因子乘以最大因子的分解方式 $O(n)$
因此，对于每个合数，只要用最大因子筛一遍，枚举时只要枚举最小质因子即可。
因为一般有 $prime[i]*prime[i]<=n$ ，所以筛选范围为 $\sqrt{n}$
1e7--665000   1e6--80000   1e5--1e4
```cpp
int prime[cnt]; //prime[0]储存素数的个数，素数下标从1开始
bool heshu[maxn];
void getPrime(int n)
{
    for (int i = 2; i <= n; ++i)
    {
        if (!heshu[i])
            prime[++prime[0]] = i;
        for (int j = 1; j <= prime[0] && i * prime[j] <= n; ++j)
        {
            heshu[i * prime[j]] = true; //找到的素数的倍数不访问
            if (i % prime[j] == 0)
                break;
        }
    }
}
```