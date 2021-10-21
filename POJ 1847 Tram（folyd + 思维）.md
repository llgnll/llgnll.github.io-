# POJ 1847 Tram（folyd + 思维）
[vj链接](https://vjudge.net/problem/POJ-1847)

## Solution
乍一看可能会觉得跑最短路时需要改路径的值，但我们仔细思考后会发现，我们在最短路径上走过的点一定不会再走回来，如果有回头路，那么我们完全可以不走到前面的点。因此只需要按题意建边权为 0 和 1 的边即可。

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
const int inf = 0x3f3f3f3f;

int n, a, b;
int dis[MAXN][MAXN];

int main()
{
    memset(dis, 0x3f, sizeof(dis));
    cin >> n >> a >> b;
    for (int i = 1; i <= n;i++)
    {
        dis[i][i] = 0;
        int k;
        cin >> k;
        for (int j = 1; j <= k;j++)
        {
            int v;
            cin >> v;
            if(j == 1)
                dis[i][v] = 0;
            else
                dis[i][v] = 1;
        }
    }
    for (int k = 1; k <= n;k++)
    {
        for (int i = 1; i <= n;i++)
        {
            for (int j = 1; j <= n;j++)
            {
                dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
            }
        }
    }
    printf("%d", dis[a][b] == inf ? -1 : dis[a][b]);
}
```

