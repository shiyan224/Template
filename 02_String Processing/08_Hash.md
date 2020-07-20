用哈希法判断一个字符串是否是另一个字符串的字串？


设字符串 $S=s_1s_2...s_m$，定义哈希函数 $$H(S)=(s_1 * b^{m-1} + s_2 * b^{m-2} + s_3 * b^{m-3} +...+ s_m * b^0) mod h$$ 其中b是基数，相当于把字符串看作b进制数
则 $H(S[k+1...k+m])=(H(S[k...k+m-1])*b - s_k * b^m + s_{k+m}) mod h$
```cpp
typedef unsigned long long ull;
const ull seed_Pool[] = {146527, 19260817};
const ull mod_Pool[] = {1000000007, 998244353};
ull seed = seed_Pool[0], mod = mod_Pool[0];
ull p[maxLen], h[maxLen];

void init(int n)
{
    p[0] = 1;
    for (int i = 1; i <= n; ++i)
        p[i] = p[i - 1] * seed % mod;
}
void Hash(const string &s)
{
    int n = s.length();
    h[0] = 0;
    for (int i = 1; i <= n; ++i)
        h[i] = (h[i - 1] * seed % mod + s[i - 1]) % mod;
}
//获取[l,r)子串的哈希值
ull get(int l, int r)
{
    return (h[r] - h[l] * p[r - l] % mod + mod) % mod;
}
//获取以l开始长度为m的哈希值
ull substr(int l, int m) { return get(l, l + m); }
```