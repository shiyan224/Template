复杂度： $O(NM)$
```cpp
struct Edge
{
    int from, to, dist;
    Edge(int u, int v, int d) : from(u), to(v), dist(d) {}
};
struct BellmanFord
{
    int n, m;             // 顶点数和边数
    vector<Edge> edges;   // 边列表
    vector<int> G[max_v]; // 每个节点出发的边编号（从0开始编号，若题目从1开始编号，则输入时要减一）
    bool inq[max_v];      // 是否在队列中
    int dis[max_v];       // s到各个点的距离
    int p[max_v];         // 最短路中的上一条弧
    int cnt[max_v];       // 进队次数
    void init(int V)      // 初始化，建立V个顶点的图
    {
        this->n = V;
        for (int i = 0; i < V; i++)
            G[i].clear();
        edges.clear();
    }
    void AddEdge(int from, int to, int dist) //无向图需调用两次
    {
        edges.emplace_back(from, to, dist);
        G[from].push_back(edges.size() - 1);
    }
    bool bellmanford(int s) //如果返回false则存在负圈
    {
        queue<int> q;
        memset(inq, 0, sizeof(inq));
        memset(cnt, 0, sizeof(cnt));
        for (int i = 0; i < n; i++)
            dis[i] = INF;
        q.push(s);
        dis[s] = 0, inq[s] = true;
        while (!q.empty())
        {
            int u = q.front();
            q.pop();
            inq[u] = false;
            for (auto &id : G[u])
            {
                Edge &e = edges[id];
                if (dis[u] < INF && dis[e.to] > dis[u] + e.dist)
                {
                    dis[e.to] = dis[u] + e.dist;
                    p[e.to] = id;
                    if (!inq[e.to])
                    {
                        q.push(e.to);
                        inq[e.to] = true;
                        if (++cnt[e.to] > n)
                            return false;
                    }
                }
            }
        }
        return true;
    }
};
```