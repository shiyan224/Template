时间复杂度 $O(n^3)$ ，空间复杂度 $O(n^2)$
```cpp
struct Floyd // 任意两点间的最短路径
{
    int n;                 //顶点数
    int dis[max_v][max_v]; //dis[u][v]表示边e={u,v}的权值（不存在时设为INF，不过d[i][i]=0）
    void init(int V)
    {
        this->n = V;
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                dis[i][j] = (i == j) ? 0 : INF;
    }
    void AddEdge(int from, int to, int dist) { dis[from][to] = dist; } //无向图需调用两次
    //dis[k][i][j] = min(dis[k-1][i][j], dis[k-1][i][k] + dis[k-1][k][j]);
    void floyd()
    {
        for (int k = 0; k < n; ++k)
            for (int i = 0; i < n; ++i)
                for (int j = 0; j < n; ++j)
                    dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
    }
};
```
如果是一个没有边权的图，把相连的两点间的距离设为dis[i][j]=true，不相连的两点设为dis[i][j]=false
若某一个顶点与其余n-1个顶点都相连，则其排名是确定的
```cpp
bool dis[max_v][max_v];
void floyd(int n) //判断一张图中的任意两点是否相连
{
    for (int k = 0; k < n; ++k)
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                dis[i][j] |= dis[i][k] & dis[k][j];
}
```
最大/最小环（dis[][]全部初始化为INF）
```cpp
void floyd()
{
    for (int k = 0; k < V; ++k)
        for (int i = 0; i < V; ++i)
            for (int j = 0; j < V; ++j)
                dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
    int res = 0;
    for (int i = 0; i < V; ++i)
        res = min(res, dis[i][i]);
    return res;
}
```