加法 乘法 小于号 输出
```cpp
struct bint
{
    int l;
    short int w[505]; //505位
    bint(string x = "")
    {
        memset(w, 0, sizeof(w));
        l = x.length();
        for (int i = 0; i < l; ++i)
            w[i] = x[l - 1 - i] - '0';
    }
    bool operator<(const bint &x) const
    {
        if (l != x.l)
            return l < x.l;
        int i = l - 1;
        while (i >= 0 && w[i] == x.w[i])
            i--;
        return (i >= 0 && w[i] < x.w[i]);
    }
    bint operator+(const bint &x) const
    {
        bint ans;
        ans.l = l > x.l ? l : x.l;
        for (int i = 0; i < ans.l; ++i)
        {
            ans.w[i] += w[i] + x.w[i];
            ans.w[i + 1] += ans.w[i] / 10;
            ans.w[i] = ans.w[i] % 10;
        }
        if (ans.w[ans.l] != 0)
            ans.l++;
        return ans;
    }
    bint operator*(const bint &x) const
    {
        bint res;
        int up, tmp;
        for (int i = 0; i < l; ++i)
        {
            up = 0;
            for (int j = 0; j < x.l; ++j)
            {
                tmp = w[i] * x.w[j] + res.w[i + j] + up;
                res.w[i + j] = tmp % 10;
                up = tmp / 10;
            }
            if (up != 0)
                res.w[i + x.l] = up;
        }
        res.l = l + x.l;
        while (res.w[res.l - 1] == 0 && res.l > 1)
            res.l--;
        return res;
    }
    void print()
    {
        for (int i = l - 1; ~i; --i)
            printf("%d", w[i]);
        puts("");
    }
};

string x, y;
cin >> x >> y;
bint a{x}, b{y};
bint ans = a * b;
ans.print();
```