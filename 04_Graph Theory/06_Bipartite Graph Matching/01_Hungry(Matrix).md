```cpp
int uN, vN;        //uN是匹配左边的顶点数,vN是匹配右边的顶点数
int g[maxn][maxn]; //邻接矩阵g[i][j] = 1表示i->j的有向边就可以了,是左边向右边的匹配
int linker[maxn];
bool vis[maxn];
bool dfs(int u)
{
    for (int v = 0; v < vN; ++v)
        if (g[u][v] && !vis[v])
        {
            vis[v] = true;
            if (linker[v] == -1 || dfs(linker[v]))
            {
                linker[v] = u;
                return true;
            }
        }
    return false;
}
int hungary(int uN)
{
    int res = 0;
    memset(linker, -1, sizeof(linker));
    for (int u = 0; u < uN; ++u)
    {
        memset(vis, 0, sizeof(vis));
        if (dfs(u))
            ++res;
    }
    return res;
}
```