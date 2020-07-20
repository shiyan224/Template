适用于正负整数
```cpp
template <class T>
inline bool input(T &res)
{
    char c;
    int sgn;
    if (c = getchar(), c == EOF)
        return false; //EOF
    while (c != '-' && (c < '0' || c > '9'))
        c = getchar();
    sgn = (c == '-') ? -1 : 1;
    res = (c == '-') ? 0 : (c - '0');
    while (c = getchar(), c >= '0' && c <= '9')
        res = res * 10 + (c - '0');
    res *= sgn;
    return true;
}
template <class T>
inline void output(T x)
{
    if (x < 0)
        x = -x, putchar('-');
    if (x > 9)
        output(x / 10);
    putchar(x % 10 + '0');
}
```