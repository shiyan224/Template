Heavy-Light Decomposition
轻重链/长链剖分
```cpp
int temp[maxn];
int sumv[maxn << 2], maxv[maxn << 2], lazy[maxn << 2];
//build函数中：if (l == r) { sumv[rt]/maxv[rt] = temp[l]; return; }

struct HLD
{
    int n, dfs_clock;
    int sz[maxn], top[maxn], son[maxn], dep[maxn], fa[maxn], id[maxn];
    vector<int> G[maxn];
    // vector<pair<PII, int>> edges; 维护边权时，将其下放为儿子结点的点权
    void init(int n)
    {
        this->n = n, memset(son, -1, sizeof(son)), dfs_clock = 0;
        for (int i = 0; i <= n; i++)
            G[i].clear();
    }
    void add_edge(int u, int v) { G[u].push_back(v), G[v].push_back(u); }
    void dfs(int u, int f, int d) //u当前节点，f父亲，d深度
    {                             //标记每个点的深度、父亲、每个非叶子节点的子树大小
        dep[u] = d, fa[u] = f, sz[u] = 1;
        for (auto &v : G[u])
        {
            if (v == f) //若为父亲则continue
                continue;
            dfs(v, u, d + 1); //dfs其儿子
            sz[u] += sz[v];   //把它的儿子数加到它身上
            if (son[u] == -1 || sz[v] > sz[son[u]])
                son[u] = v; //标记每个非叶子节点的重儿子编号
        }
    }
    void link(int u, int t)              //u当前节点，t(top)当前链的最顶端的节点
    {                                    //这个点所在链的顶端
        top[u] = t, id[u] = ++dfs_clock; //标记每个点的新编号
        if (son[u] == -1)                //如果没有儿子则返回
            return;
        link(son[u], t); //按先处理重儿子，再处理轻儿子的顺序递归处理
        for (auto &v : G[u])
            if (v != son[u] && v != fa[u])
                link(v, v); //对于每一个轻儿子都有一条从它自己开始的链
    }
    void build_tree() // 数据结构相关操作，一般使用线段树或者树状数组
    {
        for (int i = 1; i <= n; ++i)
            scanf("%d", &temp[id[i]]);
        build(1, n, 1);
    }
    int query_path(int u, int v) //更新只需把query函数换成update函数
    {
        int res = 0;
        while (top[u] != top[v]) //当两个点不在同一条链上
        {
            if (dep[top[u]] < dep[top[v]]) //把u点改为所在链顶端的深度更深的那个点
                swap(u, v);
            res = sum / max / min(res, query(id[top[u]], id[u], 1, n, 1)); //res加上u点到u所在链顶端 这一段区间的点权和
            u = fa[top[u]];                                                //把u跳到u所在链顶端的那个点的上面一个点
        }                                                                  //直到两个点处于一条链上
        if (dep[u] > dep[v])                                               //把u点改为深度更深的那个点
            swap(u, v);
        res = sum / max / min(res, query(id[u], id[v], 1, n, 1)); //这时再加上此时两个点的区间和即可
        /* 边权
        if (u == v) return ret;
        if (dep[u] > dep[v]) swap(u, v);
        ret += query(id[son[u]], id[v]);
        */
        return res;
    }
    int query_son(int u) //查询以u为根节点的子树
    {
        return query(id[u], id[u] + sz[u] - 1, 1, n, 1);
    }
};

HLD T;
T.init(n);
/* 输入边信息 */
T.dfs(root, -1, 1);
T.link(root, root);
T.build_tree(); //建树在最后
```