## 模拟

### 1. 所有三角形

https://www.acwing.com/problem/content/5167/

> - 每一个1都有三个边
> - 对于每一个1，判断左右是否也有1，如果有则减掉一条边
> - 对于奇数位的1，判断上 | 下是否有1，如果有也要减掉一条边

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

### 2. XOR Palindromes

https://codeforces.com/contest/1867/problem/B

> 题意：给定一个二进制字符串s长度为n，现在需要构造一个长度为n+1的字符串t，对于t中的每一位，即0<=i<=n的t[i]，如果将s中的数字0变为1或1变为0一共i次后，s成为了一个回文串，那么t[i]就是1，如果成为不了回文串就是0
>
> 输出：给出最终构造的t字符串
>
> 注意点：==对于回文问题，多考虑双指针算法==
>
> 思路：想要在改变了x（0<=x<=n）个数位后成为一个回文串，那么就要先统计当前的字符串s的回文情况。假如对称位置相同，那么可以不消耗改变次数或者消耗两次改变次数，假如对称位置不同，那么就必须要消耗一次改变次数。同时需要特判的是假如原串的字符数为奇数，那么中间的位置可以提供一个缓冲的机会，即假如被前面的两种情况消耗掉改变次数之后，还剩一次改变次数，那么就可以改变中间的字符。当然了假如改变完了中间的字符之后还是多了改变次数，那么就无法使得原串为回文串了。
>
> 时间复杂度：$O(n\log n)$

```c++
#include <iostream>
using namespace std;

int main()
{
	int T;
	cin >> T;
	
	while (T--)
	{
		int n; cin >> n;
		string s; cin >> s;
		
         // 判断原串是否为奇数个字符
		bool odd = false;
		if (n % 2) odd = true;
		
         // 双指针统计原串的对应情况
		int l = 0, r = n - 1;
		int same = 0, dif = 0;
		while (l < r)
		{
			if (s[l] == s[r]) same ++;
			else dif ++;
			l ++, r --;
		}
		
		// 对当前s的x个数位进行转换数字 
		for (int x = 0; x <= n; x ++)
             // 不同的对应位是一定要改变的
			if (x < dif) cout << 0;
			else
			{
				int aft = x - dif; // aft为消耗掉不同对应位次数后剩余的改变次数
				for (int i = 2 * same; i >= 0; i -= 2) // 时间复杂度为log n
					if (aft >= i)
					{
						aft -= i;
						break;
					}
				
                  // 此时为消耗掉不同对应位和相同对应位次数之后的剩余改变次数
				if (aft > 1) cout << 0;
				else if (aft == 1)
				{
					if (odd) cout << 1;
					else cout << 0;
				}
				else cout << 1;
			}
		
		cout << endl;
	}
	
	return 0;
}
```



## 哈希表

### 1. 分组

https://www.acwing.com/problem/content/5182/

> 存储不想同组和想同组的人员信息：存入数组，数据类型为一对字符串
> 存储所有的组队信息：存入哈希表，数据类型为“键:字符串”“值:一对字符串”
> 想要知道最终的分组情况，只需要查询数组中的队员情况与想同组 or 不想同组的成员名字是否一致即可
> 时间复杂度 $O(n)$，空间复杂度 $O(n\ len(name))_{max}$
```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

int main()
{
	int x;
	cin >> x;
	
	vector<pair<string, string>> X(x);
	
	for (int i = 0; i < x; i ++)
		cin >> X[i].first >> X[i].second;
	
	int y;
	cin >> y;
	
	vector<pair<string, string>> Y(y);
	
	for (int i = 0; i < y; i ++)
		cin >> Y[i].first >> Y[i].second;
		
	int sum;
	cin >> sum;
	
	unordered_map<string, pair<string, string>> a;
	
	for (int i = 0; i < sum; i ++)
	{
		string s, t, p;
		cin >> s >> t >> p;
		a[s] = {t, p};
		a[t] = {s, p};
		a[p] = {s, t};
	}
	
	int res = 0;
	
	// 想同组 
	for (int i = 0; i < x; i ++)
	{
		string s = X[i].first, t = X[i].second;
		if (a[s].first != t && a[s].second != t)
			res ++;
	}
	
	// 不想同组 
	for (int i = 0; i < y; i ++)
	{
		string s = Y[i].first, t = Y[i].second;
		if (a[s].first == t || a[s].second == t)
			res ++; 
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
>
> 更新答案逻辑：需要进行两个条件的约数，一个是是否匹配到了最后一个字母，一个是转弯次数不超过一次
>
> 转弯判断逻辑：
>
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

### 1. 对称山脉

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

## 博弈论

### 1. Salyg1n and the MEX Game

https://codeforces.com/contest/1867/problem/C

> tag：交互题+博弈+贪心
>
> 题面：对于给定n个数的数列，先手可以放入一个数列中不存在的数（0-1e9），后手可以从数列中拿掉一个数，但是这个数必须严格小于刚才先手放入的数。
>
> 终止条件：后手没法拿数或者操作次数达到了2n+1次
>
> 问：当你是先手时，如何放数可以使得最终数列的MEX值最大
>
> 思路：先手每次放入的数一定是当前数列的MEX值，此后不管后手拿掉什么数，先手都将刚刚被拿掉的数放进去即可。那么最多操作次数就刚好是2n+1次，因为加入当前数列就是一个从0开始的连续整数数列，那么先手放入的数就是最大数+1，即n，那么假如后手从n-1开始拿，后手最多拿n次，先手再放n次，那么就是2n+1次
>
> 时间复杂度：$O(n)$

```c++
#include <iostream>
#include <vector>

using namespace std;

int main()
{
	int T; cin >> T;
	
	while (T--)
	{
		int n; cin >> n;
		vector<int> a(n);
		
		for (int i = 0; i < n; i ++)
			cin >> a[i];
		
		int mex = n;
		for (int i = 0; i < n; i ++)
			if (a[i] != i)
			{
				mex = i;
				break;
			}
		
		cout << mex << endl;
		
		int remove;
		cin >> remove;
		
		while (remove != -1)
		{
			cout << remove << endl;
			cin >> remove;
		}
	}
	
	return 0;
} 
```

## 贪心

### 1. green_gold_dog, array and permutation

https://codeforces.com/contest/1867/problem/A

> 题意：给定n个数a[i]，其中可能有重复数，请构造一个n个数的排列b[i]，使得c[i]=a[i]-b[i]中，c[i]的不同数字的个数最多
>
> 思路：思路比较好想，就是最大的数匹配最小的数，次大的数匹配次小的数，以此类推。起初我想通过将原数列的拷贝数列降序排序后，创建一个哈希表来记录每一个数的排位，最终通过原数列的数作为键，通过哈希表直接输出排位，但是问题是原数列中的数可能会有重复的，那么在哈希的时候如果有重复的数，那么后来再次出现的数就会顶替掉原来的数的排位，故错误。
>
> 正确思路：为了==保证原数列中每个数的唯一性==，我们可以给原数列每一个数赋一个下标，排序后以下标进行唯一的一一对应的索引。那么就是：首先赋下标，接着以第一关键词进行排序，然后最大的数（其实是下标）匹配最小的结果以此类推
>
> 模拟一个样例：
> 5
> 8 7 4 5 5 9
>
> 最终的答案应该是
> 2 3 6 4 5 1
>
> 首先对每一个数赋予一个下标作为为唯一性索引
> 8 7 4 5 5 9
> 1 2 3 4 5 6（替身）
>
> 接着将上述数列按照第一关键词进行排序
> 9 8 7 5 5 4
> 6 1 2 4 5 3（替身）
>
> 对每一个数进行赋值匹配
> 9 8 7 5 5 4
> 6 1 2 4 5 3（替身）
> 1 2 3 4 5 6（想要输出的结果）
>
> 按照替身进行排序
> 8 7 4 5 5 9
> 1 2 3 4 5 6（替身）（排序后）
> 2 3 6 4 5 1（想要输出的结果）

```c++
#include <bits/stdc++.h>
using namespace std;

void solve()
{
	int n; cin >> n;
	
    // 第一关键词是原数列，第二关键词是赋予的唯一下标
	vector<pair<int, int>> a(n + 1);
	for (int i = 1; i <= n; i ++)
	{
		cin >> a[i].first;
		a[i].second = i;
	}
	
    // 按照第一关键词排序
	sort(a.begin() + 1, a.end(), [&](pair<int, int> x, pair<int, int> y) {
		return x.first > y.first;
	});
	
    // 以下标作为原数列的替身，每一个替身对应一个升序的最终排名
	vector<pair<int, int>> res(n + 1);
	for (int i = 1; i <= n; i ++)
	{
		res[i].first = a[i].second;
		res[i].second = i;
	}
	
    // 通过下标，还原原数列的排序
	sort(res.begin() + 1, res.end());
	
    // 输出第二关键词
	for (int i = 1; i <= n; i ++)
		cout << res[i].second << " \n"[i == n];
}

int main()
{
	int T; cin >> T;
	while (T --) solve();
	return 0;
}
```























