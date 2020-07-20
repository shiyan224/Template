时间复杂度 $O(n)$
```cpp
char s[maxn], Ma[maxn << 1];
int Len[maxn << 1]; //Len[i]表示以字符Ma[i]为中心的最长回文字串的最右字符到Ma[i]的长度
int Manacher(char s[], int len)
{
    int pos = 0;
    Ma[pos++] = '$';              //防止越界
    Ma[pos++] = '#';              //在原字符串的每个相邻两个字符中间插入一个分隔符，同时在首尾也要添加一个分隔符
    for (int i = 0; i < len; ++i) //以便将长度为奇数的回文串和长度为偶数的回文串一起考虑
        Ma[pos++] = s[i], Ma[pos++] = '#';
    Ma[pos] = 0;
    int mx = 0, mid = 0; //mx为之前计算中最长回文子串的右端点的最大值，mid为对应字串的对称轴
    for (int i = 0; i < pos; ++i)
    { //mid*2-i=mid-(i-mid)//如果i>=mx，大于mx的部分我们还没有进行匹配，所以要从mx+1位置开始重新开始匹配
        Len[i] = (i < mx) ? min(Len[mid * 2 - i], mx - i) : 1;
        while (Ma[i + Len[i]] == Ma[i - Len[i]])
            ++Len[i];
        if (Len[i] + i > mx)          //若新计算的回文串右端点位置大于mx
            mx = Len[i] + i, mid = i; //更新mx和mid的值
    }
    int ans = 0;
    for (int i = 0; i < 2 * len + 2; ++i)
        if (Len[i] - 1 > ans)
            ans = Len[i] - 1, pos = i;
    //int l = (pos - ans + 1) / 2 - 1, r = (pos + ans - 1) / 2 - 1;
    //for (int i = l; i <= r; ++i)
    //    printf("%c", s[i]);
    return ans;
}
```