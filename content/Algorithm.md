## 模拟

### 1. 所有三角形

https://www.acwing.com/problem/content/5167/

- 每一个1都有三个边
- 对于每一个1，判断左右是否也有1，如果有则减掉一条边
- 对于奇数位的1，判断上 | 下是否有1，如果有也要减掉一条边

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 200010;

int n;
int a[N], b[N];

int main()
{
	cin >> n;
	
	int res = 0;
	
	for (int i = 1; i <= n; i ++)
	{
		cin >> a[i];
		if (a[i] == 1) res += 3;
	}
	
	for (int i = 1; i <= n; i ++)
	{
		cin >> b[i];
		if (b[i] == 1) res += 3;
	}
	
	for (int i = 1; i <= n; i ++)
	{
		if (a[i])
		{
			if (a[i - 1]) res --;
			if (a[i + 1]) res --;
			if (i % 2 == 1)
			{
				if (b[i] == 1) res --;
			}
		}
	}
	
	for (int i = 1; i <= n; i ++)
	{
		if (b[i])
		{
			if (b[i - 1]) res --;
			if (b[i + 1]) res --;
			if (i % 2 == 1)
			{
				if (a[i] == 1) res --;
			}
		}
	}
	
	cout << res << endl;
	
	return 0;
}
```

## DFS

### 1. 机器人的运动范围

https://www.acwing.com/problem/content/22/

```c++
class Solution {
public:
    int res = 0;
    
    int movingCount(int threshold, int rows, int cols)
    {
        if (!rows || !cols) return 0;
        vector<vector<int>> g(rows, vector<int>(cols, 0));
        vector<vector<bool>> vis(rows, vector<bool>(cols, false));
        dfs(g, vis, 0, 0, threshold);
        return res;
    }
    
    void dfs(vector<vector<int>>& g, vector<vector<bool>>& vis, int x, int y, int threshold)
    {
        vis[x][y] = true;
        res ++;
        
        int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
        for (int k = 0; k < 4; k ++)
        {
            int i = x + dx[k], j = y + dy[k];
            if (i < 0 || i >= int(g.size()) || j < 0 || j >= int(g[0].size()) || vis[i][j] || cnt(i, j) > threshold) continue;
            dfs(g, vis, i, j, threshold);
        }
    }
    
    int cnt(int x, int y)
    {
        int sum = 0;
        while (x) sum += x % 10, x /= 10;
        while (y) sum += y % 10, y /= 10;
        return sum;
    }
};
```

### 2. CCC单词搜索

https://www.acwing.com/problem/content/5168/

> 搜索逻辑：分为正十字与斜十字

> 更新答案逻辑：需要进行两个条件的约数，一个是是否匹配到了最后一个字母，一个是转弯次数不超过一次

> 转弯判断逻辑：
>   1. 首先不能是起点开始的
>   2. 对于正十字：如果next的行&列都与pre的行和列不相等，就算转弯
>   3. 对于斜十字：如果next的行|列有和pre相等的，就算转弯

```cpp
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 110;

string s;
int m, n;
char g[N][N];
int res;

// 正十字，a，b为之前的位置，x，y为当前的位置，now为当前待匹配的字母位，cnt为转弯次数
void dfs1(int a, int b, int x, int y, int now, int cnt)
{
	if (g[x][y] != s[now]) return;
	
	if (now == s.size() - 1)
	{
		if (cnt <= 1) res++; 
		return;
	}
	
	int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
	
	for (int k = 0; k < 4; k ++)
	{
		int i = x + dx[k], j = y + dy[k];
		if (x < 0 || x >= m || y < 0 || y >= n) continue;
		
		// 判断是否转弯（now不是起点 且 pre和next行列均不相等） 
		if (a != -1 && b != -1 && a != i && b != j) dfs1(x, y, i, j, now + 1, cnt + 1);
		else dfs1(x, y, i, j, now + 1, cnt);
	}
}

// 斜十字
void dfs2(int a, int b, int x, int y, int now, int cnt)
{
	if (g[x][y] != s[now]) return;
	
	if (now == s.size() - 1)
	{
		if (cnt <= 1) res++; 
		return;
	}
	
	int dx[] = {-1, -1, 1, 1}, dy[] = {-1, 1, 1, -1};
	
	for (int k = 0; k < 4; k ++)
	{
		int i = x + dx[k], j = y + dy[k];
		if (x < 0 || x >= m || y < 0 || y >= n) continue;
		
		// 判断是否转弯（now不是起点 且 不在同一对角线） 
		if (a != -1 && b != -1 && (a == i || b == j)) dfs2(x, y, i, j, now + 1, cnt + 1);
		else dfs2(x, y, i, j, now + 1, cnt);
	}
}


int main()
{
	cin >> s;
	cin >> m >> n;
	
	for (int i = 0; i < m; i ++)
		for (int j = 0; j < n; j ++)
			cin >> g[i][j];
	
	for (int i = 0; i < m; i ++)
		for (int j = 0; j < n; j ++)
			dfs1(-1, -1, i, j, 0, 0);
	
	for (int i = 0; i < m; i ++)
		for (int j = 0; j < n; j ++)
			dfs2(-1, -1, i, j, 0, 0);
	
	cout << res << "\n";
	
	return 0;
}
```



## DP

### 1.对称山脉

https://www.acwing.com/problem/content/5169/

模拟，时间复杂度 $O(n^3)$

```cpp
#include <iostream>
#include <cmath>

using namespace std;

const int N = 5010;

int n;
int a[N];

int main()
{
	cin >> n;
	
	for (int i = 1; i <= n; i ++)
		cin >> a[i];
		
	// 枚举区间长度
	for (int len = 1; len <= n; len ++)
	{
		int res = 2e9;
		// 枚举相应长度的所有区间
		for (int i = 1, j = i + len - 1; j <= n; i ++, j ++)
		{
		    // 计算区间的不对称值
			int l = i, r = j;
			int sum = 0;
			while (l < r)
			{
				sum += abs(a[l] - a[r]);
				l ++ , r --;
			}
			res = min(res, sum);
		}
		cout << res << ' ';
	}
		
	return 0;
}
```

dp优化，时间复杂度 $O(n^2)$

```cpp
#include <iostream>
#include <cmath>
#include <cstring>

using namespace std;

const int N = 5010;

int n;
int a[N];

int dp[N][N]; // dp[i][j] 表示第 i 到 j 的不对称值
int res[N];   // res[len] 表示长度为 len 的山脉的最小不对称值 

int main()
{
	cin >> n;
	
	for (int i = 1; i <= n; i ++)
		cin >> a[i];
		
	memset(res, 0x3f, sizeof res);
	
	// 长度为 1 的情况
	res[1] = 0; 
	
	// 长度为 2 的情况
	for (int i = 1, j = i + 1; j <= n; i ++, j ++)
	{
		dp[i][j] = abs(a[i] - a[j]);
		res[2] = min(res[2], dp[i][j]);
	}
	
	// 长度 >= 3 的情况 
	for (int len = 3; len <= n; len ++)
	{
		for (int i = 1, j = i + len - 1; j <= n; i ++, j ++)
		{
			dp[i][j] = dp[i + 1][j - 1] + abs(a[i] - a[j]);
			res[len] = min(res[len], dp[i][j]);
		}
	}
	
	for (int i = 1; i <= n; i ++)
		cout << res[i] << ' ';
	
	return 0;
}
```