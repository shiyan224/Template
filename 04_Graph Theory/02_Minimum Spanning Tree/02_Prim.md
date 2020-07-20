```cpp
int cost[max_v][max_v]; //cost[u][v]表示边e={u,v}的权值（不存在的设为INF，已存在的设为0）
struct Prim             //缺点：占用内存过大
{
    int n;           //顶点数
    int dis[max_v];  //从集合X出发的边到每个顶点的最小权值
    bool vis[max_v]; //顶点i是否包含在集合X中
    void init(int V) //顶点从0开始编号，若题目从1开始编号，则输入时要减一
    {
        this->n = V;
        for (int i = 0; i < n; ++i)
            dis[i] = INF, vis[i] = false;
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                cost[i][j] = INF;
    }
    int prim()
    {
        dis[0] = 0;
        int res = 0;
        while (true)
        {
            int v = -1;
            for (int u = 0; u < n; ++u) //从不属于X的顶点中选取从X到其权值最小的顶点
                if (!vis[u] && (v == -1 || dis[u] < dis[v]))
                    v = u;
            if (v == -1)
                break;
            vis[v] = true; //把顶点v加入X
            res += dis[v]; //把边的长度加到结果里
            for (int u = 0; u < n; ++u)
                dis[u] = min(dis[u], cost[v][u]);
        }
        return res;
    }
};
```