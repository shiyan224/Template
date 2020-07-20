复杂度： $O(n + m)$
返回y中x的个数
```cpp
int kmp[maxn];                //kmp[i]用于记录当匹配到模式串的第i位之后失配，该跳转到模式串的哪个位置，即next(失配)数组
void initkmp(char x[], int m) //预处理模式串的kmp数组  O(n)
{
    int i = 0, pos = kmp[0] = -1; //前一位、前两位失配了，都只可能将第一位作为新的开头
    while (i < m)                 //自己匹配自己
    {
        while (pos != -1 && x[i] != x[pos]) //如果pos=-1，则回跳至第一个字符，不用再回跳了
            pos = kmp[pos];
        kmp[++i] = ++pos; //i+1位失配后应跳至pos位置
    }
}
int KMP(char x[], int m, char y[], int n) // O(n+m)
{
    initkmp(x, m);
    int i = 0, pos = 0, cnt = 0; //pos表示当前已经匹配完的模式串的最后一位的位置
    while (i < n)                //用模式串和文本串对比
    {
        while (pos != -1 && y[i] != x[pos]) //如果失配，那么就不断地回跳，直到可以继续匹配
            pos = kmp[pos];
        ++i, ++pos;
        if (pos >= m)              //一个模式串匹配成功
            ++cnt, pos = kmp[pos]; //匹配下一个
    }
    return cnt;
}
```