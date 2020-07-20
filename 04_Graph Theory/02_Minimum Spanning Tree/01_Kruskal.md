```cpp
struct Edge
{
    int from, to, dist;
    Edge(int u, int v, int d) : from(u), to(v), dist(d) {}
    bool operator<(const Edge &rhs) const { return dist < rhs.dist; }
};
struct Kruskal
{
    int n;
    int fa[max_v];
    vector<Edge> G;
    void init(int V)
    {
        this->n = V;
        for (int i = 0; i < n; ++i)
            fa[i] = i;
        G.clear();
    }
    int find(int x) { return x == fa[x] ? x : fa[x] = find(fa[x]); }
    void unite(int x, int y) { fa[find(x)] = find(y); }
    bool same(int x, int y) { return find(x) == find(y); }
    void AddEdge(int from, int to, int dist)
    {
        G.push_back(Edge{from, to, dist});
    }
    int kruskal() //不存在最小生成树时返回-1
    {
        sort(G.begin(), G.end()); //按照edge.cost的顺序从小到大排列
        int res = 0, cnt = 0;
        for (auto &e : G)
        {
            if (!same(e.from, e.to))
            {
                unite(e.from, e.to);
                ++cnt;
                res += e.dist; //res = sum/max/min(res,e.dist);
            }
        }
        return cnt == n - 1 ? res : -1; //最小生成树的边必为顶点数减一
    }
};
```
最大生成树
```cpp
bool operator<(const Edge &rhs) const { return dist > rhs.dist; }
```
从顶点1出发到达顶点n，求所有路径中边最小长度最大的
```cpp
for (int i = 0; i < G.size(); ++i)
{
    Edge &e = G[i];
    unite(e.from, e.to);
    if (same(0, n - 1))
        return e.dist;
}
```
求所有路径中边最大长度最小的，只需再修改重载小于号函数
```cpp
bool operator<(const Edge &rhs) const { return dist > rhs.dist; }
```