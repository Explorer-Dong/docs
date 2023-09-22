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

### 3. Short Sort

https://codeforces.com/contest/1873/problem/A

> 题意：对于一个由三个字符（就是abc三个字符的随机排列）组成的字符串，能否通过一次或零次交换两个字符使得字符串变为abc
>
> 思路：统计三个位置的字符是否分别为abc，如果不是，计数器加一，如果计数器小于等于2，就可以恢复，否则就不可以

```c++
#include <bits/stdc++.h>
using namespace std;

void solve()
{
	string s;
	cin >> s;
	
	int cnt = 0;
	for (int i = 0; i < 3; i ++)
		if (s[i] != 'a' + i)
			cnt ++;
	if (cnt <= 2) cout << "YES\n";
	else cout << "NO\n"; 
}

int main()
{
	int T; cin >> T;
	while (T --) solve();
	return 0;
}
```

### 4. Target Practice

https://codeforces.com/contest/1873/problem/C

> 题意：给了一个定尺寸的正方形，类似于靶子的得分机制，对于最终给出的打靶情况，计算最终的得分
>
> 思路：我们可以根据下标来观察规律，比如1分的靶位，横坐标都是0或9，或者纵坐标都是0或9，其余的得分同理，但是会出现一些重复，我们开一个相同尺寸的二维st表即可解决这个重复计数的问题

```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

void solve()
{
	vector<string> a(10);
	for (int i = 0; i < 10; i ++)
		cin >> a[i];
	
	bool st[10][10] {};
	
	int res = 0;
	
	for (int i = 0; i < 10; i ++)
		for (int j = 0; j < 10; j ++)
		{
			if (i == 0 || i == 9 || j == 0 || j == 9) if (a[i][j] == 'X') res += 1, st[i][j] = true;
			if (i == 1 || i == 8 || j == 1 || j == 8) if (a[i][j] == 'X' && !st[i][j]) res += 2, st[i][j] = true;
			if (i == 2 || i == 7 || j == 2 || j == 7) if (a[i][j] == 'X' && !st[i][j]) res += 3, st[i][j] = true;
			if (i == 3 || i == 6 || j == 3 || j == 6) if (a[i][j] == 'X' && !st[i][j]) res += 4, st[i][j] = true;
			if (i == 4 || i == 5 || j == 4 || j == 5) if (a[i][j] == 'X' && !st[i][j]) res += 5, st[i][j] = true;
		}
	cout << res << endl;
}

int main()
{
	int T; cin >> T;
	while (T --) solve();
	return 0;
}
```

## 二分

### 1. Building an Aquarium

https://codeforces.com/contest/1873/problem/E

> 题意：想象有一个二维平面，现在有一个数列，每一个数表示平面对应列的高度，现在要给这个平面在两边加上护栏，问护栏最高可以设置为多高，可以使得在完全填满的情况下，使用的水量不会超过给定的用水量。已知最大用水量为k
>
> 思路：对于一个护栏高度，水池高度低于护栏高度的地方都需要被水填满。为了便于分析，我们可以将水池高度进行排序。那么就会很显然的一个二分题目了，我们需要二分的就是护栏的高度（最小为1，最大需要考虑一下，就是只有一列的情况下，最大高度就是最高水池高度 $\max(a_i)+max(k)$），check的条件就是当前h的护栏高度时，消耗的水量与最大用水量之间的大小关系，如果超过了，那么高度就要下降，反之可以上升。由于是求最大高度，因此要使用的是求右边界的二分板子

```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

void solve()
{
	int n, w;
	cin >> n >> w;
	vector<ll> a(n);
	
	for (int i = 0; i < n; i ++)
		cin >> a[i];
	
	sort(a.begin(), a.end());
	
	ll l = 0, r = 2e9 + 1;
	while (l < r)
	{
		ll h = (l + r + 1) >> 1;
		
		ll t = 0;
		for (int i = 0; i < n; i ++)
			if (a[i] < h)
				t += h - a[i];
			else break;
		
		if (t <= w) l = h;
		else r = h - 1;
	}
	cout << r << endl;
}

int main()
{
	int T; cin >> T;
	while (T --) solve();
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

### 2. Good Kid

https://codeforces.com/contest/1873/problem/B

> 题意：对于一个数列，现在可以选择其中的一个数使其+1，问如何选择这个数，可以使得修改后的数列中的所有数之积的值最大
>
> 思路：其实就是选择n-1个数的乘积值加一倍，关键在于选哪一个n-1个数的乘积值，逆向思维就是对于n个数，去掉那个最小值，那么剩下来的n-1个数之积就会最大了，于是就是选择最小的数+1，最终数列之积就是答案了。

```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

void solve()
{
	int n; cin >> n;
	vector<int> a(n);
	
	for (int i = 0; i < n; i ++)
		cin >> a[i];
	
	sort(a.begin(), a.end());
	
	ll res = a[0] + 1;
	for (int i = 1; i < n; i ++)
		res *= a[i];
	cout << res << endl; 
}

int main()
{
	int T; cin >> T;
	while (T --) solve();
	return 0;
}
```

### 3. 1D Eraser

https://codeforces.com/contest/1873/problem/D

> 题意：给定一个只有B和W两种字符的字符串，现在有一种操作可以将一个指定大小的区间（大小为k）中所有的字符全部变为W，问对于这个字符串，至少需要几次上述操作才可以将字符串全部变为W字符
>
> 思路：我们直接遍历一边字符串即可，在遍历到B字符的时候，指针向后移动k位，如果是W字符，指针向后移动1位即可，最终统计一下遇到B字符的次数即可。

```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

void solve()
{
	int n, k;
	cin >> n >> k;
	string s;
	cin >> s;
	
	int res = 0;
	
	for (int i = 0; i < n;)
	{
		if (s[i] == 'B')
		{
			res ++;
			i += k;
		}
		else i++;
	}
	cout << res << endl;
}

int main()
{
	int T; cin >> T;
	while (T --) solve();
	return 0;
}
```

### 4. ABBC or BACB

https://codeforces.com/contest/1873/problem/G

> 题意：现在有一个只有A和B两种字符的字符串，有如下两种操作：第一种，可以将AB转化为BC，第二种，可以将BA转化为CB。问最多可以执行上述操作几次
>
> 思路：其实一开始想到了之前做过的翻转数字为-1，最后的逻辑是将数字向左右移动，于是这道题就有了突破口了。上述两种操作就可以看做将A和B两种字符交换位置，并且B保持不变，A变为另一种字符C。由于C是无法执行上述操作得到，因此就可以理解为B走过的路就不可以再走了，那么就是说对于一个字符串，最终都会变成B走过的路C。并且只要有B，那么一个方向上所有的A都会被利用转化为C（直到遇到下一个B停止），那么我们就可以将B看做一个独立的个体，他可以选择一个方向上的所有的A并且执行操作将该方向的A全部转化为C，那么在左和右的抉择中，就是要选择A数量最多的方向进行操作，但是如果B的足够多，那就不需要考虑哪个方向了，所有的A都可以获得一次操作机会。现在就转向考虑B需要多少呢，
>
> 答案是两种：如果B的数量小于A“块”的数量，其实就是有一块A无法被转化，那么为了让被转化的A数量尽可能的多，就选择A块数量最少的那一块不被转化。如果B的数量>=A块的数量，那么可以保证所有的A都可以被转化为C块，从而最终的答案就是A字符的数量

```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

void solve()
{
	string s; cin >> s;
	int cntb = 0, blocka = 0, cnta = 0; // B字符的数量，A连续区间的数量，A字符的个数 
	
	vector<int> a; // 每一个A连续区间中A字符的数量 
	
	for (int i = 0; i < s.size();)
	{
		if (s[i] == 'A')
		{
			int sum = 0;
			while (s[i] == 'A') cnta ++, sum ++, i ++;
			a.emplace_back(sum);
			blocka ++;
		}
		else
		{
			cntb ++;
			i ++;
		}
	}
	
	if (cntb >= blocka) cout << cnta << endl;
	else
	{
		sort(a.begin(), a.end());
		cout << cnta - a[0] << endl;
	}
}

int main()
{
	int T; cin >> T;
	while (T --) solve();
	return 0;
}
```























