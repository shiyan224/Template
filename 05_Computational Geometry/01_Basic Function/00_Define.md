```cpp
const double eps = 1e-8;
#define zero(x) ((fabs(x) < eps ? 1 : 0))
#define sgn(x) (fabs(x) < eps ? 0 : ((x) < 0 ? -1 : 1))
struct point
{
    double x, y;
    point(double a = 0, double b = 0) { x = a, y = b; }
    point operator-(const point &b) const { return point(x - b.x, y - b.y); }
    point operator+(const point &b) const { return point(x + b.x, y + b.y); }
    point operator/(const double &k) const { return point(x / k, y / k); }
    // 两点是否重合
    bool operator==(point &b) { return zero(x - b.x) && zero(y - b.y); }
    // 点积(以原点为基准)
    // 用于判断两个向量是否垂直或共线
    double operator*(const point &b) const { return x * b.x + y * b.y; }
    // 叉积(以原点为基准)
    // 用于判断两个向量之间的相对位置（顺时针还是逆时针）
    // 叉积的值为两个向量形成的三角形的有向面积
    double operator^(const point &b) const { return x * b.y - y * b.x; }
    // 绕P点逆时针旋转a弧度后的点
    point rotate(point b, double a)
    {
        double dx, dy;
        (*this - b).split(dx, dy);
        double tx = dx * cos(a) - dy * sin(a);
        double ty = dx * sin(a) + dy * cos(a);
        return point(tx, ty) + b;
    }
    // 点坐标分别赋值到a和b
    void split(double &a, double &b) { a = x, b = y; }
    // 极角排序
    point base() const
    {
        if (x < 0 || (x == 0 && y < 0))
            return point(-x, -y);
        return *this;
    }
    bool operator<(const point &b) const
    {
        point p1 = base(), p2 = b.base();
        return (p1 ^ p2) > 0;
    }
};
struct line
{
    point s, e;
    line() {}
    line(point ss, point ee) { s = ss, e = ee; }
};
```