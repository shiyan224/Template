复杂度 $O((n+m) \log m)$
```cpp
struct Edge
{
    int from, to, dist;
    Edge(int u, int v, int d) : from(u), to(v), dist(d) {}
};
struct HeapNode
{
    int dist, u;
    bool operator<(const HeapNode &rhs) const { return dist > rhs.dist; }
};
struct Dijkstra
{
    int n, m;             // 顶点数和边数
    vector<Edge> edges;   // 边列表
    vector<int> G[max_v]; // 每个节点出发的边号（从0开始编号，若题目从1开始编号，则输入时要减一）
    bool vis[max_v];      // 是否已永久标号
    int dis[max_v];       // s到各点的距离
    int p[max_v];         // 最短路中的一条边
    void init(int V)      // 初始化，建立V个顶点的图
    {
        this->n = V;
        for (int i = 0; i < n; ++i)
            G[i].clear(); // 清空邻接表
        edges.clear();    // 清空边表
    }
    void AddEdge(int from, int to, int dist)
    { //无向图需调用两次
        edges.emplace_back(from, to, dist);
        G[from].push_back(edges.size() - 1);
    }
    void dijkstra(int s)
    {
        for (int i = 0; i < n; ++i)
            dis[i] = INF;
        memset(vis, 0, sizeof(vis));
        priority_queue<HeapNode> q;
        q.push({0, s});
        dis[s] = 0;
        while (!q.empty())
        {
            auto x = q.top();
            q.pop();
            int u = x.u;
            if (vis[u])
                continue;
            vis[u] = true;
            for (auto &id : G[u])
            {
                Edge &e = edges[id];
                if (dis[e.to] > dis[u] + e.dist)
                {
                    dis[e.to] = dis[u] + e.dist;
                    p[e.to] = id;
                    q.push({dis[e.to], e.to});
                }
            }
        }
    }
};
```