# Light OJ 1074 Extended Traffic（floyd找负环）
[vj链接](https://vjudge.net/problem/LightOJ-1074)

## Solution
我发现网上此题好像没有 floyd 的题解，那么我就来写一发。

使用 floyd 判负环时，我们可以跑一遍 floyd，然后遍历所有的点，如果此点到自己的距离小于 0，说明其一定在负环内，那我们就以此点作为起点，dfs 染色所有负环内的点（可参考 spfa 解法）

注意这里 inf 的值比初始化 dis 数组时的值要小一些，因为松弛时有可能出现 0x3f3f3f3f + (一个小于0的值)，此时我们如果使用 0x3f3f3f3f 来作为无穷大就会出现原本无法到达的点变成能够到达

## 代码

```cpp
#include<stdio.h>
#include<string.h>
#include<algorithm>
#include<iostream>
#include<vector>
#include<map>
#include<cmath>
#include<string>
#include<queue>
#include<set>
//#define int long long
using namespace std;
typedef pair<int, int> pii;
typedef pair<double, int> pdi;
typedef double dd;
typedef long long ll;
const int MAXN = 210;
const int MAXM = 100010;
const dd eps = 1e-6;
const int inf = 0x2f3f3f3f;

int n, m;
int dis[MAXN][MAXN];
vector<int> mp[MAXN];
bool col[MAXN];
int a[MAXN];

void dfs(int now)
{
    col[now] = 1;
    for(int i : mp[now])
        if (!col[i])
            dfs(i);
}

int main()
{
    int t;
    cin >> t;
    int cnt = 0;
    while(t--)
    {
        cin >> n;
        memset(dis, 0x3f, sizeof(dis));
        memset(col, 0, sizeof(col));
        for (int i = 1; i <= n; i++)
            cin >> a[i], dis[i][i] = 0,mp[i].clear();
        cin >> m;
        for (int i = 1; i <= m;i++)
        {
            int u, v;
            cin >> u >> v;
            dis[u][v] = min((a[v] - a[u]) * (a[v] - a[u]) * (a[v] - a[u]), dis[u][v]);
            mp[u].push_back(v);
        }
        for (int k = 1; k <= n;k++)
        {
            for (int i = 1; i <= n;i++)
            {
                for (int j = 1; j <= n;j++)
                {
                    // if(dis[k][j] == inf || dis[i][k] == inf)
                    //     continue;
                    dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
                }
            }
        }
        for (int i = 1; i <= n;i++)
        {
            if(!col[i] && dis[i][i] < 0)
                dfs(i);
        }
        int q;
        cin >> q;
        printf("Case %d:\n", ++cnt);
        while(q--)
        {
            int x;
            cin >> x;
            if(col[x] || dis[1][x] < 3 || dis[1][x] >= inf)
                printf("?\n");
            else
                printf("%d\n", dis[1][x]);
        }
    }
}
```

