如果一个图存在一笔画，则一笔画的路径叫做欧拉路，如果最后又回到起点，那这个路径叫做欧拉回路。
我们定义奇点是指跟这个点相连的边数目有奇数个的点。对于能够一笔画的图，我们有以下两个定理。
定理1：存在欧拉路的条件：图是连通的，有且只有2个奇点。
定理2：存在欧拉回路的条件：图是连通的，有0个奇点。
根据一笔画的两个定理，如果寻找欧拉回路，对任意一个点执行深度优先遍历；
找欧拉路，则对一个奇点执行dfs，时间复杂度为O(m+n)，m为边数，n是点数。
```cpp
int G[maxn][maxn]; //用邻接矩阵存储
int du[maxn];      //记录每个点的度，就是相连边的数目
vector<int> ans;   //记录找到的欧拉路的路径
int V;
inline void init() { memset(G, 0, sizeof(G)), memset(du, 0, sizeof(deg)); }
inline void AddEdge(int u, int v) { ++du[u], ++du[v], ++G[u][v], ++G[v][u]; }
void dfs(int s) //从度数为奇数的点深度优先遍历过程寻找欧拉路 dfs(find_start());
{
    for (int i = 0; i < V; ++i)
        if (G[s][i]) //从任意一个与它相连的点出发
        {
            --G[s][i], --G[i][s];
            dfs(i);
        }
    ans.push_back(s); //记录下路径，若顶点从1开始，输出时要加1
}
int find_start() //寻找度数为奇数的点
{
    int s = 0;                  //如果有奇点，就从奇点开始寻找，这样找到的就是
    for (int i = 0; i < V; ++i) //欧拉路。没有奇点就从任意点开始，
        if (du[i] % 2 == 1)     //这样找到的就是欧拉回路。（因为每一个点都是偶点）
        {
            s = i;
            break; //字典序最小
        }
    return s;
}
```