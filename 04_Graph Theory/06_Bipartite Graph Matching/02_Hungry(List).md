一般地，可行性+唯一性的匹配问题可用二分图解
O(nm)
```cpp
int uN, vN; //左边顶点数、右边顶点数 u → v
vector<int> G[max_u];
//调用hungary算法后，表示右边顶点v与左边顶点linker[v]相连
//linker[v]=-1表示没有点与v相连
int linker[max_v];
bool vis[max_v];
inline void addedge(int u, int v) //u → v（二分图只需调用一次，从左边顶点连向右边顶点）
{
    G[u].push_back(v);
}
bool dfs(int u)
{
    for (auto &v : G[u])
    {
        if (!vis[v])
        {
            vis[v] = true;
            if (linker[v] == -1 || dfs(linker[v]))
            {
                linker[v] = u;
                return true;
            }
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
        memset(vis, false, sizeof(vis));
        if (dfs(u))
            ++res;
    }
    return res;
}
```