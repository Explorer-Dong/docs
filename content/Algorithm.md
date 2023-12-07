# 一、解题记录

## implementation

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

### 3. Target Practice

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

## prefix and difference

### 1. 充能计划

https://www.lanqiao.cn/problems/8732/learning/?contest_id=147

> 题意：给定 $n$ 个数初始化为 $0$，现在给定 $q$ 个位置，每个位置给定两个参数 $p、k$，表示从第 $k$ 个数开始连续 $s[p]$ 个数 $+1$。有以下两种约束
>
> 1. 如果连续 $s[p]$ 个数越界了，则越界的部分就不 $+1$
> 2. 一个位置最多只能被一种 $p$ 对应的种类 $+1$
>
> 思路：
>
> - 现在假设只有一个种类的p，如果不考虑上述第二个条件的约束，那么就是纯差分。如果考虑了，那么我们从左到右考虑+1区间覆盖的问题，就需要判断当前位置是否被上一个+1区间覆盖过，解决办法就是**记录上一个区间覆盖的起始点or终止点**，这里选择起始点。
> - 现在我们考虑多个种类的p，那么就是分种类重复上述思路即可，因为不同种类之间是没有约束上的冲突的。那么如何分种类解决呢，我们可以对输入的q个位置的所有p、k参数进行排序，p为第一关键词，k为第二个关键词。
> - 时间复杂度：$O(q\log q+q+n)$

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 100010;

struct node {
	int p, k; // 第p种宝石丢在了第k个坑
	bool operator<(const node &t) const {
		if (p == t.p) {
			return k > t.k;
		} else {
			return p > t.p;
		}
	}
};

int n, m, q, s[N]; // n个坑，m种宝石，q个采集的宝石，s[i]表示第i种宝石的能量
vector<int> last(N, -1e6); // last[i]表示上一个第i种宝石的位置
int a[N], b[N]; // a[]为原数组，b[]为差分数组
priority_queue<node> que;

void solve() {
	cin >> n >> m >> q;
	for (int i = 1; i <= m; i++) {
		cin >> s[i];
	}

	while (q--) {
		int p, k;
		cin >> p >> k;
		que.push({p, k});
	}

	while (que.size()) {
		auto h = que.top();
		que.pop();
		int p = h.p, k = h.k; // 第p种宝石丢在了第k个坑

		int l, r;
		if (k - last[p] >= s[p]) {
			// 和上一种没有重叠
			l = k, r = min(n, k + s[p] - 1);
			b[l]++, b[r + 1]--;
			last[p] = k;
		} else {
			// 和上一种有重叠
			l = last[p] + s[p];
			r = min(n, k + s[p] - 1);
			if (l <= r) {
				b[l]++, b[r + 1]--;
			}
			last[p] = k;
		}
	}

	for (int i = 1; i <= n; i++) {
		a[i] = a[i - 1] + b[i];
		cout << a[i] << " \n"[i == n];
	}
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```



## binary search

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

### 2. 分组

https://www.lanqiao.cn/problems/5129/learning/

> 题意：给定一个序列，现在需要将这个数列分为k组，如何分组可以使得每一组的极差中，最大值最小
>
> 最开始想到的思路：
>
> 很容易联想到的一种方法其实就是高中组合数学中学到的“隔板法”，现在有n个数，需要分成k组，则方案数就是在n-1个空档中插入k-1个隔板，即 $C_{n-1}^{k-1}$ 种方案
>
> 时间复杂度 $O(n^2)$
>
> 优化思路：
>
> 上述思路是正向思维，即对于构思分组情况计算极差。我们不妨逆向思维，即枚举极差的情况，判断此时的分组情况。如果对于当前的极差lim，我们显然可以分成n组，即有一个最大分组值；我们也可以求一个最小分组值cnt，即如果再少分一组那么此时的极差就会超过当前约束的极差值lim。因此对于当前约束的极差值lim，我们可以求一个最小分组值cnt
>
> - 如果当前的最小分组值cnt > k，那么 $\left [ cnt,n \right ]$ 就无法包含k，也就是说当前约束的极差lim不符合条件，lim偏小
> - 如果当前的最小分组值cnt <= k，那么 $\left [ cnt,n \right ]$ 就一定包含k，且当前分组的最小极差一定是 <= 约束的极差值lim，lim偏大
>
> 于是二分极差的思路就跃然纸上了。我们二分极差，然后根据可以分组的最小数量cnt判断二分的结果进行左右约束调整即可。
>
> 时间复杂度 $O(n \log n)$

```c++
#include <bits/stdc++.h>
using namespace std;


bool check(int lim, vector<int>& a, int n, int k) {
    int cnt = 1; // 当前可以分的最小组数
    int pre = a[0];
    for (int i = 0; i < n; i++) {
        if (a[i] - pre > lim) {
            pre = a[i];
            cnt++;
        }
    }
    return cnt <= k;
}


void solve() {
    int n, k;
    cin >> n >> k;

    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    sort(a.begin(), a.begin() + n);

    int l = 0, r = a[n - 1] - a[0];
    while (l < r) {
        int mid = (l + r) >> 1;
        if (check(mid, a, n, k)) {
            // 分的最小组数 <= k，则当前极差大了
            r = mid;
        } else {
            // 分的最小组数 >  k，则当前极差小了
            l = mid + 1;
        }
    }

    cout << r << "\n";
}


int main() {
    ios::sync_with_stdio(false);
    cin.tie(0), cout.tie(0);
    int T = 1;
    // cin >> T;
    while (T--) {
        solve();
    }    
    return 0;
}
```

## hashing

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

## dfs and similar

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
>     1. 首先不能是起点开始的
>     2. 对于正十字：如果next的行&列都与pre的行和列不相等，就算转弯
>     3. 对于斜十字：如果next的行|列有和pre相等的，就算转弯

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

### 3. 数量

https://www.acwing.com/problem/content/5150/

法一：dfs

> 题意：给定一个数n，问[1, n]中有多少个数只含有4或7
>
> 思路：对于一个数，我们可以构造一个二叉搜数进行搜索，因为每一位只有两种可能，那么从最高位开始搜索。如果当前数超过了n就return，否则就算一个答案
>
> 时间复杂度：
> $$
> O(2^{1 + \lg{(\max(a[i])})})
> $$

```c++
#include <iostream>
using namespace std;

#define int long long

int n, res;

void dfs(int x) {
    if (x > n) return;
    
    res ++;
    
    dfs(x * 10 + 4);
    dfs(x * 10 + 7);
}

signed main() {
    cin >> n;
    dfs(4);
    dfs(7);
    cout << res << "\n";
    return 0;
}
```

法二：二进制枚举

> 题意：给定一个数n，问[1, n]中有多少个数只含有4或7
>
> 思路：按照数位进行计算。对于一个x位的数，1到x-1位的情况下所有的数都符合条件，对于一个t位的数，满情况就是 $2^t$ 种，所以[1, x - 1]位就一共有 $2^1 + 2^2 + \cdots + 2^{x - 1} = 2^{x} - 2$ 种情况 。对于第x位，采取二进制枚举与原数进行比较，如果小于原数，则答案+1，反之结束循环输出答案即可

```c++
#include <iostream>
using namespace std;

int WS(int x) {
	int res = 0;
	while (x) {
		res++;
		x /= 10;
	}
	return res;
}

int calc(int a[], int ws) {
	int res = 0;
	for (int i = ws - 1; i >= 0; i --) {
		res = res * 10 + a[i];
	}
	return res;
}

int main() {
	int n;
	cin >> n;
	
	int ws = WS(n);
	
	int ans = (1 << ws) - 2;
	
	int a[20] {};
	for (int i = 0; i < (1 << ws); i ++) {
		for (int j = 0; j < ws; j ++) {
			if ((1 << j) & i) {
				a[j] = 7;
			} else {
				a[j] = 4;
			}
		}
		if (calc(a, ws) <= n) {
			ans ++;
		} else {
			break;
		}
	}
	
	cout << ans;
	
	return 0;
}
```

### 4. 组合总和

https://leetcode.cn/problems/combination-sum/

> 题意：给定一个序列，其中的元素没有重复，问如何选取其中的元素，使得选出的数字总和为指定的数字target，选取的数字可以重复
>
> 思路：思路比较简答，很容易想到用dfs搜索出所有的组合情况，即对于每一个“结点”，我们直接遍历序列中的元素即可。但是由于题目的限制，即不允许合法的序列经过排序后相等。那么为了解决这个约束，我们可以将最终搜索到的序列排序后进行去重，但是这样的时间复杂度会很高，于是我们从搜索的过程切入。观看这一篇题解[防止出现重复序列的启蒙题解](https://leetcode.cn/problems/combination-sum/solutions/2363929/39-zu-he-zong-he-hui-su-qing-xi-tu-jie-b-9zx7/)，我们提取其中最关键的一个图解
>
> ![subset_sum_i_pruning.png](https://pic.leetcode.cn/1690625058-WYmZtD-subset_sum_i_pruning.png)
>
> 可见3，4和4，3的剩余选项（其中可能包含了答案序列）全部重复，因此我们直接减去这个枝即可。不难发现，我们根据上述优化思想，剪枝的操作可以为：让当前序列开始枚举的下标 `idx` 从上一层开始的下标 `i` 开始，于是剪枝就可以实现了。
>
> 时间复杂度：??? 
> $$
> O \left ( 2^{\frac{n}{\log n}}\right)
> $$

```c++
class Solution {
public:
    // 答案数组res，目标数组c，目标总和target，答案数组now，当前总和sum，起始下标idx
    void dfs(vector<vector<int>>& res, vector<int>& c, int target, vector<int>& now, int sum, int idx) {
        if (sum > target) {
            return;
        } else if (sum == target) {
            res.emplace_back(now);
            return;
        }
        for (int i = idx; i < c.size(); i++) {
            now.emplace_back(c[i]);
            dfs(res, c, target, now, sum + c[i], i);
            now.pop_back();
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> now;
        dfs(res, candidates, target, now, 0, 0);
        return res;
    }
};
```

### 5. 扩展字符串

https://www.acwing.com/problem/content/5284/

> 题意：给定一种字符串的构造方式，问构造n次以后的字符串中的第k个字符是什么
>
> 思路：由于构造的方法是基于上一种情况的，很容易可以想到一个递归搜索树来解决。只是这道题有好几个坑，故记录一下。
>
> - 首先说一下搜索的思路：对于当前的状态，我们想要知道第k个位置上的字符，很显然我们可以通过预处理每一种构造状态下的字符串长度得到下一个字符串的长度，于是我们可以在当前的字符串中，通过比对下标与**五段**字符串长度的大小，来确定是继续递归还是直接输出
> - 特判：可以发现，对于 $n=0$ 的情况，我们无法采用相同的结构进行计算，故进行特判，如果当前来到了最初始的字符串状态，我们直接输出相应位置上的字符即可
> - 最后说一下递归终点的设计：与搜索所有的答案情况不同，这道题的答案是唯一的，因此我们在搜索到答案后，可以通过一个 `bool` 变量作为一个标记，表示已经找到答案了，只需要不断回溯直到回溯结束为止，就不需要再遍历其他的分支了
> - **坑：**这道题的坑说实话有点难崩。
>     1. 首先是一个k的大小，是一定要开 `long long` 的，我一开始直接全局宏定义 `int` 为 `long long` 了
>     2. 还有一个坑可能是只要我才会犯的，就是字符串按照下标输出字符的时候，是需要 `-1` 的，闹心的是我有的加了，有的没加，还是debug的时候调出来的
>     3. 最后一个大坑，属于是引以为戒了。就是这句 `len[i] = min(len[i], (int)2e18)`，因为我们~~可以发现~~，抛开那三个固定长度的字符串来说，每一次新构造出来的字符串长度都是上一个字符串长度 $2$ 倍，那么构造 $n$ 次后的字符串长度就是 $s_0$ 长度的 $2^n$ 倍，那么对于 $n$ 的取值范围来说，直接存储长度肯定是不可取的。那么如何解决这个问题呢？方法是我们对 `len[i]` 进行一个约束即可，见代码。最后进行递归比较长度就没问题了。
> - 时间复杂度：$O(n)$ - 由于每一个构造的状态我们都是常数级别的比较，因此相当于一个状态的搜索时间复杂度为 $O(1)$，那么总合就是 $O(n)$

```c++
#include <bits/stdc++.h>
using namespace std;

#define int long long

int n, k;
string s = "DKER EPH VOS GOLNJ ER RKH HNG OI RKH UOPMGB CPH VOS FSQVB DLMM VOS QETH SQB";
string t1 = "DKER EPH VOS GOLNJ UKLMH QHNGLNJ A";
string t2 = "AB CPH VOS FSQVB DLMM VOS QHNG A";
string t3 = "AB";

// 记录每一层构造出来的字符串 Si 的长度 len，当前递归的层数 i (i>=1)，对于当前层数需要查询的字符的下标 pos
void dfs(vector<int>& len, int i, int pos, bool& ok) {
	// 已经搜到答案了就不断返回
	if (ok) {
		return;
	}

	// 如果还没有搜到答案，并且已经递归到了最开始的一层，就输出原始字符串相应位置的字符即可
	if (!i) {
		cout << s[pos - 1];
		return;
	}

	int l1 = t1.size(), l2 = l1 + len[i - 1], l3 = l2 + t2.size(), l4 = l3 + len[i - 1];
	if (pos <= l1) {
		cout << t1[pos - 1];
		ok = true;
		return;
	} else if (pos <= l2) {
		dfs(len, i - 1, pos - l1, ok);
	} else if (pos <= l3) {
		cout << t2[pos - l2 - 1];
		ok = true;
		return;
	} else if (pos <= l4) {
		dfs(len, i - 1, pos - l3, ok);
	} else {
		cout << t3[pos - l4 - 1];
		ok = true;
		return;
	}
}

void solve() {
	cin >> n >> k;

	vector<int> len(n + 10);
	len[0] = s.size();

	for (int i = 1; i <= n; i++) {
		len[i] = 2 * len[i - 1] + t1.size() + t2.size() + t3.size();
		len[i] = min(len[i], (int)2e18); // 点睛之笔...
	}

	// 特判下标越界的情况
	if (k > len[n]) {
		cout << ".";
		return;
	}

	// 否则开始从第n层开始递归搜索
	bool ok = false;
	dfs(len, n, k, ok);
}

signed main() {
	int T = 1;
	cin >> T;
	while (T--) {
		solve();
	}
	return 0;
}
```

### 6. 让我们异或吧

https://www.luogu.com.cn/problem/P2420

> 题意：给定一棵树，树上每一条边都有一个权值，现在有Q次询问，对于每次询问会给出两个结点编号u，v，需要输出结点u到结点v所经过的路径的所有边权的异或之和
>
> 思路：对于每次询问，我们当然可以遍历从根到两个结点的所有边权，然后全部异或计算结果，但是时间复杂度是 $O(n)$，显然不行，那么有什么优化策略吗？答案是有的。我们可以发现，对于两个结点之间的所有边权，其实就是根到两个结点的边权相异或得到的结果（异或的性质），我们只需要预处理出根结点到所有结点的边权已异或值，后续询问的时候直接 $O(1)$ 计算即可
>
> 时间复杂度：$O(n+q)$

```c++
const int N = 100010;

struct node {
	int id;
	int w;
};

int n, m, f[N];		// f[i] 表示从根结点到 i 号结点的所有边权的异或值
vector<node> G[N];
bool vis[N];

void dfs(int fa) {
	if (!vis[fa]) {
		vis[fa] = true;
		for (auto& ch: G[fa]) {
			f[ch.id] = f[fa] ^ ch.w;
			dfs(ch.id);
		}
	}
}

void solve() {
	cin >> n;

	for (int i = 0; i < n - 1; i++) {
		int a, b, w;
		cin >> a >> b >> w;
		G[a].push_back({b, w});
		G[b].push_back({a, w});
	}

	dfs(1);

	cin >> m;

	while (m--) {
		int u, v;
		cin >> u >> v;
		cout << (f[u] ^ f[v]) << "\n";
	}
}
```

### 7. Function

https://www.luogu.com.cn/problem/P1464

> 题意：
>
> 思路一：直接dfs
>
> - 直接按照题意进行dfs代码的编写，但是很显然时间复杂极高
> - 时间复杂度：$O(T \times 情况数)$
>
> 思路二：记忆化dfs
>
> - 记忆化逻辑：
>     - 如果当前的状态没有记忆过，就记忆一下
>     - 如果当前的状态已经记忆过了，就不需要继续递归搜索了，直接使用之前已经记忆过的答案即可
> - 上述起始状态需要和搜到答案的状态做一个区别。我们知道，对于一组合法的输入，答案一定是
> - 注意点：
>     - 输入终止条件不是 `a != -1 && b != -1 && c != -1`，而是要三者都不是 `-1` 才行
>     - 对于每一组输入，我们不需要 `memset` 记忆数组，因为每一组的记忆依赖是相同的
>     - 由于答案一定是 $>0$ 的，因此是否记忆过只需要看当前状态的答案是否 $>0$ 即可
> - 时间复杂度：$<O(T \times n^3)$

直接dfs代码：

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

ll dfs(int a, int b, int c) {
	if (a <= 0 || b <= 0 || c <= 0) return 1;
	else if (a > 20 || b > 20 || c > 20) return dfs(20, 20, 20);
	else if (a < b && b < c) return dfs(a, b, c - 1) + dfs(a, b - 1, c - 1) - dfs(a, b - 1, c);
	else return dfs(a - 1, b, c) + dfs(a - 1, b - 1, c) + dfs(a - 1, b, c - 1) - dfs(a - 1, b - 1, c - 1);
}

void solve() {
	int a, b, c;
	cin >> a >> b >> c;
	while (a != -1 && b != -1 && c != -1) {
		printf("w(%d, %d, %d) = %lld\n", a, b, c, dfs(a, b, c));
		cin >> a >> b >> c;
	}
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

记忆化dfs代码：

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 25;

ll f[N][N][N];

ll dfs(ll a, ll b, ll c) {
	// 上下界
	if (a <= 0 || b <= 0 || c <= 0) return 1;
	else if (a > 20 || b > 20 || c > 20) return dfs(20, 20, 20);

	if (f[a][b][c]) {
		// 已经记忆化过了，直接返回当前状态的解
		return f[a][b][c];
	}
	else {
		// 没有记忆化过，就递归计算并且记忆化
		if (a < b && b < c) return f[a][b][c] = dfs(a, b, c - 1) + dfs(a, b - 1, c - 1) - dfs(a, b - 1, c);
		else return f[a][b][c] = dfs(a - 1, b, c) + dfs(a - 1, b - 1, c) + dfs(a - 1, b, c - 1) - dfs(a - 1, b - 1, c - 1);
	}
}

void solve() {
	ll a, b, c;
	cin >> a >> b >> c;
	while (!(a == -1 && b == -1 && c == -1)) {
		printf("w(%lld, %lld, %lld) = %lld\n", a, b, c, dfs(a, b, c));
		cin >> a >> b >> c;
	}
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

### 8. 外星密码

https://www.luogu.com.cn/problem/P1928

> 线性递归
>
> 题意：给定一个压缩后的密码串，需要解压为原来的形式。压缩形式距离
>
> - `AC[3FUN]` $\to$ `ACFUNFUNFUN`
> - `AB[2[2GH]]OP` $\to$ `ABGHGHGHGHOP`
>
> 思路：
>
> - 我们采用递归的策略
>
> - 我们知道，对于每一个字符，一共有4种情况，分别是："字母"、"数字"、"["、"]"。如果是字母。我们分情况考虑
>
>     - "字母"：
>         1. 直接加入答案字符串即可
>     - "["：
>         1. 获取左括号后面的整体 - 采用**递归**策略获取后面的整体
>         2. 加入答案字符串
>     - "数字"：
>         1. 获取完整的数 - 循环小trick
>         2. 获取数字后面的整体 - 采用**递归**策略获取后面的整体
>         3. 加入答案字符串 - 循环尾加入即可
>         4. **返回当前的答案字符串**
>     - "]"：
>         1. **返回当前的答案字符串** - 与上述 "[" 对应
>
> - 代码设计分析：
>
>     - 我们将压缩后的字符串看成由下面两种单元组成：
>
>         1. **最外层中括号组成的单元**：如 `[2[2AB]]` 就算一个最外层中括号组成的单元
>         2. **连续的字母单元**：如 `OPQ` 就算一个连续的字母单元
>
>     - 解决各单元连接问题：
>     - 为了在递归处理完第一种单元后还能继续处理后续的第二种单元，我们直接按照压缩字符串的长度进行遍历，即 `while (i < s.size())` 操作
>     - 解决两种单元内部问题：
>         - 最外层中括号组成的单元：递归处理
>         - 连续的字母单元：直接加入当前答案字符串即可
>
> - 手模样例：
>
>     <img src="C:/Users/%E8%91%A3%E6%96%87%E6%9D%B0/AppData/Roaming/Typora/typora-user-images/image-20231125121853866.png" alt="image-20231125121853866" style="zoom: 50%;" />
>
>     - 显然按照定义，上述压缩字符串一共有五个单元
>     - 我们用红色表示进入递归，蓝色表示驱动递归结束并回溯。可以发现
>
> - 时间复杂度：$O(\text{res.length()})$

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

string s;
int i;

string dfs() {
	string res;

	while (i < s.size()) {
		if (s[i] >= 'A' && s[i] <= 'Z') {
			while (s[i] >= 'A' && s[i] <= 'Z') {
				res += s[i++];
			}
		}
		if (s[i] == '[') {
			i++;
			res += dfs();
		}
		if (isdigit(s[i])) {
			int cnt = 0;
			while (isdigit(s[i])) {
				cnt = cnt * 10 + s[i] - '0';
				i++;
			}
			string t = dfs();
			while (cnt--) {
				res += t;
			}
			return res;
		}
		if (s[i] == ']') {
			i++;
			return res;
		}
	}

	return res;
}

void solve() {
	cin >> s;
	cout << dfs() << "\n";
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

## dp

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

### 2. 最小化网络并发线程分配

https://vijos.org/d/nnu_contest/p/1492

> 题意：现在有一个线性网络需要分配并发线程，每一个网络有一个权重，现在有一个线程分配规则。对于当前网络，如果权重比相邻的网络大，则线程就必须比相邻的网络大。
>
> 思路：我们从答案角度来看，对于一个网络，我们想知道它的相邻的左边线程数和右边线程数，如果当前网络比左边和右边的权重都大，则就是左右线程数的最大值+1，当然这些的==前提是左右线程数已经是最优的状态==，因此我们要先求“左右线程”。分析可知，左线程只取决于左边的权重与线程数，右线程同样只取决于右边的权重和线程数，因此我们可以双向扫描一遍即可求得“左右线程”。最后根据“左右线程”即可求得每一个点的最优状态。

```c++
void solve() {
	int n; cin >> n;
	vector<int> w(n + 1), l(n + 1, 1), r(n + 1, 1);
	for (int i = 1; i <= n; i++) {
		cin >> w[i];
	}
	for (int i = 2, j = n - 1; i <= n && j >= 1; i++, j--) {
		if (w[i] > w[i - 1]) {
			l[i] = l[i - 1] + 1;
		}
		if (w[j] > w[j + 1]) {
			r[j] = r[j + 1] + 1;
		}
	}
	int res = 0;
	for (int i = 1; i <= n; i++) {
		res += max(l[i], r[i]);
	}
	cout << res << "\n";
}
```

### 3. 摘花生

https://www.acwing.com/problem/content/1017/

> 题意：给定一个二维矩阵，每一个位置有一个价值，问：从左上角（1,1）走到右下角（r,c）能获得的最大价值是多少
>
> 思路：我们不妨从结果位置出发，对于（r,c）这个位置而言，能走到这里的只有两个位置，即上面的位置（r-1,c）和左边（r,c-1）的位置，那么答案就是（r-1,c）和（r,c-1）中的最大价值 +（r,c）处的价值。那么对于（r-1,c）和（r,c-1）中的最大价值，同样==需要其相应位置的左方和上方的价值最优计算而来==，因此就很容易想到动态规划的思路。我们需要初始化`dp[1][j]`和`dp[i][1]`的答案，然后从`dp[2][2]`开始计算。
>
> 代码优化：不难发现，直接从`dp[1][1]`开始迭代也是可以的。因为`dp[0][j]`均为0，同样的`dp[1][0]`也均为0。

优化前代码：

```c++
void solve() {
	int r, c;
	cin >> r >> c;
	vector<vector<int>> w(r + 1, vector<int>(c + 1)), dp(r + 1, vector<int>(c + 1, 0));

	for (int i = 1; i <= r; i++) {
		for (int j = 1; j <= c; j++) {
			cin >> w[i][j];
		}
	}

	dp[1][1] = w[1][1];

	for (int j = 2; j <= c; j++) {
		dp[1][j] = dp[1][j - 1] + w[1][j];
	}

	for (int i = 2; i <= r; i++) {
		dp[i][1] = dp[i - 1][1] + w[i][1];
	}

	for (int i = 2; i <= r; i++) {
		for (int j = 2; j <= c; j++) {
			dp[i][j] = w[i][j] + max(dp[i - 1][j], dp[i][j - 1]);
		}
	}

	cout << dp[r][c] << "\n";
}
```

优化后代码：

```c++
void solve() {
	int r, c;
	cin >> r >> c;
	vector<vector<int>> w(r + 1, vector<int>(c + 1)), dp(r + 1, vector<int>(c + 1, 0));

	for (int i = 1; i <= r; i++) {
		for (int j = 1; j <= c; j++) {
			cin >> w[i][j];
		}
	}

	for (int i = 1; i <= r; i++) {
		for (int j = 1; j <= c; j++) {
			dp[i][j] = w[i][j] + max(dp[i - 1][j], dp[i][j - 1]);
		}
	}

	cout << dp[r][c] << "\n";
}
```

### 4. 最大社交深度和

https://vijos.org/d/nnu_contest/p/1534

> 算法：换根dp | BFS
>
> 题意：给定一棵树，现在需要选择其中的一个结点为根节点，使得深度和最大。深度的定义是以每个结点到树根所经历的结点数
>
> 思路：
>
> - **暴力**：
>
>     - 显然可以直接遍历每一个结点，计算每个结点的深度和，然后取最大值即可
>     - 时间复杂度：$O(n^2)$
>
> - **优化**：
>
>     先看图：
>
>     <img src="https://s2.loli.net/2023/11/05/wPA9eDqIjChMfkN.png" alt="image-20231105233631038" style="zoom:50%;" />
>
>     - 我们可以发现，对于当前的根结点 `fa`，我们选择其中的一个子结点 `ch`，将 `ch` 作为新的根结点（如右图）。那么对于当前的 `ch` 的深度和，我们可以借助 `fa` 的深度和进行求解。我们假设以 `ch` 为子树的结点总数为 `x`，那么这 `x` 个结点在换根之后，相对于 `ch` 的深度和，贡献了 `-x` 的深度；而对于 `fa` 的剩下来的 `n-x` 个结点，相对于 `ch` 的深度和，贡献了 `n-x` 的深度。于是 `ch` 的深度和就是 `fa的深度和` `-x+n-x`，即
>         $$
>         dep[ch] = dep[fa]-x+n-x = dep[fa]+n-2*x
>         $$
>         于是我们很快就能想到利用前后层的递推关系，$O(1)$ 的计算出所有子结点的深度和
>
>     - 代码实现：我们可以先计算出 `base` 的情况，即任选一个结点作为根结点，然后基于此进行迭代计算。在迭代计算的时候需要注意的点就是在一遍 `dfs` 计算某个结点的深度和 `dep[root]` 时，如果希望同时计算出每一个结点作为子树时，子树的结点数，显然需要分治计算一波。关于分治的计算我熟练度不够高，~~特此标注一下debug了3h的点~~：即在递归到最底层，进行回溯计算的时候，需要注意不能统计父结点的结点值（因为建的是双向图，所以一定会有从父结点回溯的情况），那么为了避开这个点，就需要在 $O(1)$ 的时间复杂度内获得当前结点的父结点的编号，从而进行特判，采用的方式就是增加递归参数 `fa`。
>
>         - 没有考虑从父结点回溯的情况的dfs代码
>
>             ```c++
>             void dfs(int now, int depth) {
>             	if (!st[now]) {
>             		st[now] = true;
>             		dep[root] += depth;
>             		for (auto& ch: G[now]) {
>             			dfs(ch, depth + 1);
>             			cnt[now] += cnt[ch];
>             		}
>             	}
>             }
>             ```
>
>         - 考虑了从父结点回溯的情况的dfs代码
>
>             ```c++
>             void dfs(int now, int fa, int depth) {
>             	if (!st[now]) {
>             		st[now] = true;
>             		dep[root] += depth;
>             		for (auto& ch: G[now]) {
>             			dfs(ch, now, depth + 1);
>             			if (ch != fa) {
>             				cnt[now] += cnt[ch];
>             			}
>             		}
>             	}
>             }
>             ```
>
>     - 时间复杂度：$O(2n)$

暴力代码：

```c++
const int N = 500010;

int n;
vector<int> G[N];
int st[N], dep[N];

void dfs(int id, int now, int depth) {
	if (!st[now]) {
		st[now] = 1;
		dep[id] += depth;
		for (auto& node: G[now]) {
			dfs(id, node, depth + 1);
		}
	}
}

void solve() {
	cin >> n;
	for (int i = 1; i <= n - 1; i++) {
		int a, b;
		cin >> a >> b;
		G[a].push_back(b);
		G[b].push_back(a);
	}

	int res = 0;

	for (int i = 1; i <= n; i++) {
		memset(st, 0, sizeof st);
		dfs(i, i, 1);
		res = max(res, dep[i]);
	}

	cout << res << "\n";
}
```

优化代码：

```c++
const int N = 500010;

int n, dep[N], root = 1;
vector<int> G[N], cnt(N, 1);;
bool st[N];

// 当前结点编号 now，当前结点的父结点 fa，当前结点深度 depth
void dfs(int now, int fa, int depth) {
	if (!st[now]) {
		st[now] = true;
		dep[root] += depth;
		for (auto& ch: G[now]) {
			dfs(ch, now, depth + 1);
			if (ch != fa) {
				cnt[now] += cnt[ch];
			}
		}
	}
}

void bfs() {
	memset(st, 0, sizeof st);
	queue<int> q;
	q.push(root);
	st[root] = true;

	while (q.size()) {
		int fa = q.front(); // 父结点编号 fa
		q.pop();
		for (auto& ch: G[fa]) {
			if (!st[ch]) {
				st[ch] = true;
				dep[ch] = dep[fa] + n - 2 * cnt[ch];
				q.push(ch);
			}
		}
	}
}

void solve() {
	cin >> n;
	for (int i = 1; i <= n - 1; i++) {
		int a, b;
		cin >> a >> b;
		G[a].push_back(b);
		G[b].push_back(a);
	}

	dfs(root, -1, 1);
	bfs();

	cout << *max_element(dep, dep + n + 1) << "\n";
}
```

### 5.  [NOIP2002 普及组] 过河卒

https://www.luogu.com.cn/problem/P1002

> 题意：给定一个矩阵，现在需要从左上角走到右下角，问一共有多少种走法？有一个特殊限制是，对于图中的9个点是无法通过的。
>
> 思路一：dfs
>
> - 我们可以采用深搜的方法。但是会超时，我们可以这样估算时间复杂度：对于每一个点，我们都需要计算当前点的右下角的矩阵中的每一个点，那么总运算次数就近似为阶乘级别。当然实际的时间复杂度不会这么大，但是这种做法 $n*m$ 一旦超过100就很容易tle
>
> - 时间复杂度：$O(nm!)$
>
> 思路二：dp
>
> - 我们可以考虑，对于当前的点，可以从哪些点走过来，很显然就是上面一个点和左边一个点，而对于走到当前这个点的路线就是走到上面的点和左边的点的路线之和，base状态就是 `dp[1][1] = 1`，即起点的路线数为1
>
> - 时间复杂度：$O(nm)$

dfs代码

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 30;

int n, m, a, b;
int res;
bool notsafe[N][N];

void init() {
	int px[9] = {0, -1, -2, -2, -1, 1, 2, 2, 1};
	int py[9] = {0, 2, 1, -1, -2, -2, -1, 1, 2};
	for (int i = 0; i < 9; i++) {
		int na = a + px[i], nb = b + py[i];
		if (na < 0 || nb < 0) continue;
		notsafe[na][nb] = true;
	}
}

void dfs(int x, int y) {
	if (x > n || y > m || notsafe[x][y]) {
		return;
	}

	if (x == n && y == m) {
		res++;
		return;
	}

	dfs(x, y + 1);
	dfs(x + 1, y);
}

void solve() {
	cin >> n >> m >> a >> b;
	init();
	dfs(0, 0);
	cout << res << "\n";
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

dp代码

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 30;

int n, m, a, b;
bool notsafe[N][N];
ll dp[N][N];

void solve() {
	cin >> n >> m >> a >> b;

	// 初始化
	++n, ++m, ++a, ++b;
	int px[9] = {0, -1, -2, -2, -1, 1, 2, 2, 1};
	int py[9] = {0, 2, 1, -1, -2, -2, -1, 1, 2};
	for (int i = 0; i < 9; i++) {
		int na = a + px[i], nb = b + py[i];
		if (na < 0 || nb < 0) continue;
		notsafe[na][nb] = true;
	}

	// dp求解
	dp[1][1] = 1;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= m; j++) {
			if (!notsafe[i - 1][j]) dp[i][j] += dp[i - 1][j];
			if (!notsafe[i][j - 1]) dp[i][j] += dp[i][j - 1];
		}
	}

	cout << dp[n][m] << "\n";
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

### 6. [NOIP2001 普及组] 数的计算

https://www.luogu.com.cn/problem/P1028

> 题意：给定一个数和一种构造方法，即对于当前的数，可以在其后面添加一个最大为当前一半大的数，以此类推构造成一个数列。问一共可以构造出多少个这种数列
>
> 思路一：dfs
>
> - 非常显然的一个搜索树，答案就是结点数
> - 时间复杂度：$O(方案数)$
>
> 思路二：dp
>
> - 非常显然的一个dp，我们定义一个dp记忆数组。其中 `dp[i]` 表示数字 `i` 的构造方案总数，那么状态转移方程就是
>     $$
>     dp[i]=\sum_{j=1}^{\left\lfloor i/2 \right\rfloor}dp[j]+1
>     $$
>
> - 时间复杂度：$O(n^2)$

dfs代码

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int n, res;

void dfs(int x) {
	res++;
	for (int i = x >> 1; i >= 1; i--) {
		dfs(i);
	}
}

void solve() {
	cin >> n;
	dfs(n);
	cout << res << "\n";
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

dp代码

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int n;

void solve() {
	cin >> n;

	vector<ll> dp(n + 1, 1);
	for (int i = 2; i <= n; i++) {
		for (int j = 1; j <= i >> 1; j++) {
			dp[i] += dp[j];
		}
	}

	cout << dp[n] << "\n";
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

### 7. [NOIP2003 普及组] 栈

https://www.luogu.com.cn/problem/P1044

> 题意：n个数依次进栈，随机出栈，问一共有多少种出栈序列？
>
> **思路一：dfs**
>
> - 我们可以这么构造搜索树：已知对于当前的栈，一共有两种状态
>     - 入栈 - 如果当前还有数没有入栈
>     - 出栈 - 如果当前栈内还有元素
> - 搜索参数：`i,j` 表示入栈数为 `i` 出栈数为 `j` 的状态
> - 搜索终止条件
>     - 入栈数 < 出栈数 - $i<j$
>     - 入栈数 > 总数 $n$ - $i = n$
> - 答案状态：入栈数为n，出栈数也为n
> - 时间复杂度：$O(方案数)$
>
> **思路二：dp**
>
> - 采用上述dfs时的状态表示方法，`i,j` 表示入栈数为 `i` 出栈数为 `j` 的状态。
>
> - 我们在搜索的时候，考虑的是接下来可以搜索的状态
>
>     1. 即出栈一个数的状态 - `i+1,j`
>     
>     2. 和入栈一个数的状态 - `i,j+1`
>     
>     如图：
>     
>     <img src="https://s2.loli.net/2023/11/21/wRQkKeEDhBZ6MC4.png" alt="image-20231121192133801" style="zoom:50%;" />
>     
>     而我们在dp的时候，需要考虑的是子结构的解来得出当前状态的答案，就需要考虑之前的状态。即当前状态是从之前的哪些状态转移过来的。和上述dfs思路是相反的。我们需要考虑的是
>     
>     1. 上一个状态入栈一个数到当前状态 - `i-1,j` $\to$ `i,j`
>     2. 上一个状态出栈一个数到当前状态 - `i,j-1` $\to$ `i,j`
>     
>     - 特例：$i=j$ 时，只能是上述第二种状态转移而来，因为要始终保证入栈数大于等于出栈数，即 $i \ge j$
>     
>     如图：
>     
>     <img src="https://s2.loli.net/2023/11/21/NIKbWduh9P3rGD2.png" alt="image-20231121192152555" style="zoom: 33%;" />
>     
> - 我们知道，入栈数一定是大于等于出栈数的，即 $i\ge j$。于是我们在枚举 $j$ 的时候，枚举的范围是 $[1,i]$
>
> - $base$ 状态的构建取决于 $j=0$ 时的所有状态，我们知道没有任何数出栈也是一种状态，于是
>     $$
>     dp[i][0]=0,(i=1,2,3,...,n)
>     $$
>
> - 时间复杂度：$O(n^2)$

dfs代码

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int n, res;

// 入栈i个数，出栈j个数
void dfs(int i, int j) {
	if (i < j || i > n) return;

	if (i == n && j == n) res++;

	dfs(i + 1, j);
	dfs(i, j + 1);
}

void solve() {
	cin >> n;
	dfs(0, 0);
	cout << res << "\n";
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

dp代码

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 20;

int n;
ll dp[N][N]; // dp[i][j] 表示入栈数为i，出栈数为j的方案总数

void solve() {
	cin >> n;

	// base状态：没有数出栈也是一种状态
	for (int i = 1; i <= n; i++) dp[i][0] = 1;

	// dp转移
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= i; j++)
			if (i == j) dp[i][j] = dp[i][j - 1];
			else dp[i][j] = dp[i - 1][j] + dp[i][j - 1];

	cout << dp[n][n] << "\n";
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

### 8. 数楼梯

https://www.luogu.com.cn/problem/P1255

> 题意：给定楼梯的台阶数，每次可以走1步或者2步，问走到第n层台阶可以走的方案数
>
> 思路一：dfs
>
> - 我们可以从前往后思考，即正向思维。对于剩余的台阶，我们可以走1步或者走2步，最终走到第n层就算一种
> - 时间复杂度：指数级别
>
> 思路二：递推（dp）
>
> - 对于递推的思路，我们从后往前考虑，即逆向思维。对于当前的层数，是从之前的哪几个台阶走过来的（即对于当前的状态，是之前哪几个状态转移过来的）。
>
> - 我们定义 `f[i]` 为走到第 `i` 层台阶的方案数
>
> - 则很显然第 `i` 层台阶是从前1个或者前2个台阶走过来的，于是状态转移方程就是
>     $$
>     f[i] = f[i - 1] + f[i - 2]
>     $$
>
> - 时间复杂度：$O(n)$
>
> 注意：采用高精度加法

dfs代码：

```c++
int n, res;

void dfs(int x) {
	if (x < 0) return;

	if (x == 0) res++;

	dfs(x - 1);
	dfs(x - 2);
}
```

递推（dp）代码：

```c++
int n;
Int dp[N];

void solve() {
	cin >> n;

	dp[1] = 1, dp[2] = 2;
	for (int i = 3; i <= n; i++) {
		dp[i] = dp[i - 1] + dp[i - 2];
	}

	cout << dp[n] << "\n";
}
```

### 9. 蜜蜂路线

https://www.luogu.com.cn/problem/P2437

> 题意：可以按照下面的路线从小数到相邻大数，问给定起点和终点，一共有多少种走法
>
> <img src="C:/Users/%E8%91%A3%E6%96%87%E6%9D%B0/AppData/Roaming/Typora/typora-user-images/image-20231126231329349.png" alt="image-20231126231329349" style="zoom:33%;" />
>
> 思路：
>
> - 可以发现，对于每一个数 $i$，可以从 $i-1$ 和 $i-2$ 两个位置走过来。定义 $dp[i]$ 表示从 $1$ 走到 $i$ 的路线数（状态数），于是状态转移方程就是
>     $$
>     dp[i] = dp[i-1]+dp[i-2]
>     $$
>
> - 方程一写其实就是斐波那契数，需要使用高精度加法
>
> 时间复杂度：$O(n)$

```c++
int m, n;
Int dp[N];

void solve() {
	cin >> m >> n;
	n = n - m + 1;

	dp[1] = 1;
	for (int i = 2; i <= n; i++) {
		dp[i] = dp[i - 1] + dp[i - 2];
	}

	cout << dp[n] << "\n";
}
```

### 10. Block Sequence :fire:

https://codeforces.com/contest/1881/problem/E

> 题意：给定一个序列，问最少可以删除其中的几个数，使得**最终**的序列满足：从中选择一些数 `num[]`，使得 `num[i]` 后面有 `num[i]` 个数，且这些数包括 `num[]` 涵盖且只涵盖了一遍最终序列中所有的数
>
> 思路：我们考虑动态规划。
>
> - 我们考虑状态表示：其中 `dp[i]` 表示使得序列 `[i,n]` 满足上述条件的最少删除个数
> - 我们考虑状态转移：我们知道对于当前的数 `num[i]`，有两种状态 - 删除 | 不删除。

```c++

```

## games

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

## greedy

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

### 5. Smilo and Monsters

https://codeforces.com/contest/1891/problem/C

> 题意：给定一个序列，其中每一个元素代表一个怪物群中的怪物的数量。现在有两种操作：
>
> 1. 选择一个怪物群，杀死其中的一只怪物。同时累加值 $x+=1$
> 2. 选择一个怪物群，通过之前积累的累加值，一次性杀死当前怪物群中的 $x$ 只怪物，同时累加值 $x$ 归零
>
> 现在询问杀死所有怪物的最小操作次数
>
> 思路：一开始我是分奇偶进行判断的，但是那只是局部最优解，全局最优解的计算需要我们“纵观全局”。
>
> 我们可以先做一个假设：假设当前只有一个怪物群，为了最小化操作次数，最优解肯定是先杀死一半的怪物（假设数量为n，则获得累计值x=n），然后无论剩余n个还是n+1个，我们都使用累计值x一次性杀死n个怪物。
>
> 现在我们回到原题，有很多个怪物群，易证，最终的最优解也一点是最大化利用操作2，和特殊情况类似，能够利用的一次性杀死的次数就是怪物总数的一半。区别在于：此时不止一个怪物群。那么我们就要考虑累加值如何使用。很容易想到先将怪物群按照数量大小进行排序，关键就在于将累加值从小到大进行使用还是从大到小进行使用。可以发现，对于一个确定的累加值，由于操作2的次数也会算在答案中。那么如果从最小的怪物群开始使用，就会导致操作2的次数变多。因此我们需要从最大值开始进行操作2。
>
> 那么最终的答案就是：
>
> - 首先根据怪物数量的总和计算出最终的累加值s（s=sum/2）
> - 接着我们将怪物群按照数量进行升序排序
> - 然后我们从怪物数量最大的开始进行操作2，即一次性杀死当前怪物群，没进行一次，答案数量+1
> - 最后将答案累加上无法一次性杀死的怪物群中怪物的总数
>
> 时间复杂度：$O(n \log n)$

```c++
void solve() {
	n = read(); s = 0; res = 0;
	for (int i = 1; i <= n; i++) {
		a[i] = read();
		s += a[i];
	}
 
	s >>= 1;
 
	sort(a + 1, a + n + 1);
 
	for (int i = n; i >= 1; i--) {
		if (s >= a[i]) {
			s -= a[i], a[i] = 0, res++;
		} else if (s) {
			a[i] -= s;
			s = 0;
			res++;
		}
	}
 
	for (int i = 1; i <= n; i++) {
		res += a[i];
	}
 
	printf("%lld\n", res);
}
```

### 6. 通关

https://www.lanqiao.cn/problems/5889/learning/?contest_id=145

> 声明：谨以此题记录我的第一道正式图论题（存图技巧抛弃了y总的纯数组，转而使用动态数组`vector`进行建图）e.g.
>
> ```c++
> struct Node {
>  int id;		// 当前结点的编号
>  int a;	// 属性1
>  int b;	// 属性2
> };
> vector<Node> G[N];
> ```
>
> 题意：给定一棵树，每个结点代表一个关卡，每个关卡拥有两个属性：通关奖励值`val`和可通关的最低价值`cost`。现在从根节点开始尝试通关，每一个结点下面的关卡必须要当前结点通过了之后才能继续闯关。问应该如何选择通关规划使得通过的关卡数最多？
>
> 思路：一开始想到的是直接莽bfs，对每一层结点按照cost升序排序进行闯关，然后wa麻了。最后一看正解原来是还不够贪。正确的闯关思路应该是始终选择当前可以闯过的cost最小的关卡，而非限定在了一层上。因此我们最终的闯关顺序是：对于所有解锁的关卡，选择cost最小的关卡进行通关，如果连cost最小的关卡都无法通过就直接结束闯关。那么我们应该如何进行代码的编写呢？分析可得，解锁的关卡都是当前可通过的关卡的所有子结点，而快速获得当前cost最小值的操作可以通过一个堆来维护。
>
> 时间复杂度：$O(n \log n)$

```c++
const int N = 100010;
typedef long long ll;

struct Node {
    int id;
    int val;
    int cost;
    // 有趣的重载
    bool operator< (const Node& t) const {
        return this->cost > t.cost;
    }
};

int n, p;
ll res;
vector<Node> G[N];
priority_queue<Node> q;

void bfs() {
    q.push(G[0][0]);

    while (q.size()) {
        Node now = q.top();
        q.pop();
        if (p >= now.cost) {
            res++;
            p += now.val;
            for (auto& child: G[now.id]) {
                q.push(child);
            }
        } else {
            break;
        }
    }
}

void solve() {
    cin >> n >> p;
    for (int i = 1; i <= n; i++) {
        int fa, v, c;
        cin >> fa >> v >> c;
        G[fa].push_back({i, v, c});
    }
    bfs();
    cout << res << "\n";
}
```

### 7. 串门

https://www.lanqiao.cn/problems/5890/learning/?contest_id=145

> 题意：给定一棵无向树，边权为正。现在询问在访问到每一个结点的情况下最小化行走的路径长度
>
> 思路：首先我们不管路径长度的长短，我们直接开始串门。可以发现，我们一点会有访问的起点和终点，那么在起点到终点的这条路上，可能会有很多个分叉，对于每一个分叉，我们都需要进入再返回来确保分叉中的结点也被访问到，那么最终的路径长度就是总路径长度的2倍 - 起点到终点的距离，知道了这个性质后。我们可以发现，路径的总长度是一个定值，我们只有最大化起点到终点距离才能确保行走路径长度的最小值。那么问题就转化为了求解一棵树中，最长路径长度的问题。即求解树的直径的问题。
>
> 树的直径：首先我们反向考虑，假设知道了直径的一个端点，那么树的直径长度就是这棵树以当前结点为根的深度。那么我们就需要先确定一个直径的端点。不难发现，对于树中的任意一个结点，距离它最远的一个（可能是两个）结点一定是树的直径的端点。那么问题就迎刃而解了。
>
> 为了降低代码量，我们可以设置一个dist数组来记录当前结点到根的距离。
>
> - 在确定直径端点的时候，选择任意一个结点为根节点，然后维护dist数组，最终dist[i~n]中的最大值对应的下标maxi就是直径的一个端点。
> - 接着在计算直径长度的时候，以maxi为树根，再来一次上述的维护dist数组的过程，最终dist[1~n]中的最大值就是树的直径的长度
>
> 时间复杂度：$O(n)$

```c++
const int N = 100010;
typedef long long ll;

struct Node {
	int id;
	ll w;
};

int n;
vector<Node> G[N];
bool st[N];
ll sum, d, dist[N];

/**
 * @note 计算当前所有子结点到当前点的距离
 * @param now 当前点的编号
 * @param x 上一个点到当前点的距离
 */
void dfs(int now, ll x) {
	if (!st[now]) {
		st[now] = true;
		dist[now] = x;
		for (int i = 0; i < G[now].size(); i++) {
			int ch = G[now][i].id;
			dfs(ch, x + G[now][i].w);
		}
	}
}

void solve() {
	cin >> n;
	for (int i = 0; i < n - 1; i++) {
		int a, b, w;
		cin >> a >> b >> w;
		sum += w;
		G[a].push_back({b, w});
		G[b].push_back({a, w});
	}

	// 以1号点为根，计算所有点到1号点的距离
	dfs(1, 0);

	memset(st, 0, sizeof st);

	// 到1号点最远的那个点的编号 maxi，即直径的一个端点
	int maxi = 0;
	for (int i = 1; i <= n; i++) {
		if (dist[i] > dist[maxi]) {
			maxi = i;
		}
	}

	// 以直径的一个端点 maxi 为根，计算所有点到 maxi 点的距离
	dfs(maxi, 0);

	// 找到直径长度
	for (int i = 1; i <= n; i++) {
		d = max(d, dist[i]);
	}

	cout << sum * 2 - d << "\n";
}
```

### 8. Codeforces rating

https://vijos.org/d/nnu_contest/p/1532

> 题意：现在已知一个初始值R，现在给定了n个P值，选择其中的合适的值，使得最终的 $R'=3/4 R + 1/4 P$ 最大
>
> 思路：首先一点就是我们一定要选择P比当前R大的值，接下来就是选择合适的P使得最终迭代出来的R'最大。首先我们知道，对于筛选出来的比当前R大的P集合，任意选择其中的一个P，都会让R增大，但是不管增加多少都是不会超过选择的P。那么显然，如果筛选出来的P集合是一个递增序列，那么就可以让R不断的增加。但是这一定是最大的吗？我们不妨反证一下，现在我们有两个P，分别为x，y，其中 $x<y$。
>
> 那么按照上述思路，首先就是 
> $$
> R’=\frac{3}{4}R+\frac{1}{4}x(R'<x)
> $$
> 接着就是 
> $$
> R''_1=\frac{3}{4}R'+\frac{1}{4}y(R''_1<y)=\frac{9}{16}R+\frac{3}{16}x+\frac{1}{4}y
> $$
> **反之**，首先选择一个较大的P=y，再选择一个较小的P=x，即首先就是
> $$
> R’=\frac{3}{4}R+\frac{1}{4}y(R'<y)
> $$
> 此时我们还不一点可以继续选择x，因为此时的R'可能已经超过了x的值，那么我们按照最优的情况计算，可以选择x，那么就是
> $$
> R''_2=\frac{3}{4}R'+\frac{1}{4}x(R''_2<x)=\frac{9}{16}R+\frac{3}{16}y+\frac{1}{4}x
> $$
> 可以发现
> $$
> R''_2-R''_1=\frac{1}{16}(x-y)<0
> $$
> 即会使得最终的答案变小。因此最优的策略就是按照增序进行叠加计算
>
> ==注意点：==
>
> 四舍五入的语句
>
> ```c++
> cout << (int)(res + 0.5) << "\n";
> ```

```c++
void solve() {
	cin >> n >> k;
	for (int i = 0; i < n; i++) {
		cin >> a[i];
	}

	sort(a, a + n);

	int i = 0;
	for (; i < n; i++) {
		if (a[i] > k) {
			break;
		}
	}

	double res = k;

	for (; i < n; i++) {
		res = (res * 3.0 + a[i] * 1.0) / 4.0;
	}

	cout << (int)(res + 0.5) << "\n";
}
```

## number theory

### 1. Divide and Equalize

> 题意：给定 $n$ 个数，问能否找到一个数 $num$，使得 $num^n = \prod_{i=1}^{n}a_i$
>
> 原始思路：起初我的思路是二分，我们需要寻找一个数使得n个数相乘为原数组所有元素之积，那么我们预计算出所有数之积，并且在数组最大值和最小值之间进行二分，每次二分出来的数计算n次方进行笔比较即可。但是有一个很大的问题是，数据量是 $10^4$，而数字最大为 $10^6$，最大之积为 $10^{10}$ 吗？不是！最大之和才是，最大之积是 $10^{6\times10^4}$
>
> 最终思路：我们可以将选数看做多个水池匀水的过程。现在每一个水池的水高都不定，很显然我们一定可以一个值使得所有的水池的高度一致，即 $\frac{\sum_{i=1}^{n}a_i}{n}$。但是我们最终的数字是一个整数，显然不可以直接求和然后除以n，那么应该如何分配呢？我们知道水池之所以可以直接除以n，是因为水的最小分配单位是无穷小，可以随意分割；而对于整数而言，最小分配单位是什么呢？答案是==质因子==！为了通过分配最小单位使得最终的“水池高度一致”，我们需要让每一个“水池”获得的数值相同的质因子数量相同。于是我们只需要统计一下数组中所有数的质因子数量即可。如果对于每一种质因子的数量都可以均匀分配每一个数（即数量是n的整数倍），那么就一定可以找到这个数使得 $num^n = \prod_{i=1}^{n}a_i$

```c++
void solve() {
	int n;
	cin >> n;

	// 统计所有数字的所有质因数
	unordered_map<int, int> m;
	for (int i = 0; i < n; i++) {
		int x;
		cin >> x;

		for (int k = 2; k <= x / k; k++) {
			if (x % k == 0) {
				while (x % k == 0) {
					m[k]++;
					x /= k;
				}
			}
		}

		if (x > 1) {
			m[x]++;
		}
	}

	// 查看每一种质因数是否是n的整数倍
	for (auto& x: m) {
		if (x.second % n) {
			cout << "No\n";
			return;
		}
	}

	cout << "Yes\n";
}
```

### 2. Deja Vu

https://codeforces.com/contest/1891/problem/B

> 题意：给点序列a和b，对于b中的每一个元素 $b_i$，如果a中的元素 $a_j$ 能够整除 $2^{b_i}$，则将 $a_j$ 加上 $2^{b_i - 1}$。给出最后的a序列
>
> 思路：我们很容易就想到暴力的做法，即两层循环，第一层枚举b中的元素，第二层枚举a中的元素，如果a中的元素能够整除2
>
> 的bi次方，就将a中相应的元素加上一个值即可。但是时间复杂度肯定过不了。于是我们考虑优化。
>
> 时间复杂度：$O(nm)$
>
> 优化：现在我们假设a中有一个数 $a_j$ 是 $2^{b_i}$ 的整数倍（其中 $b_i$ 是b序列中第一个枚举到的能够让 $a_i$ 整除的数），那么就有 $a_j = k2^{b_i}(k=1,2,...,)$，那么 $a_j$ 就要加上 $2^{b_i-1}$，于是 $a_j$ 就变为了 $k2^{b_i}+2^{b_i-1}=(2k+1)2^{b_i-1}$。此后 $a_j$ 就一定是 $2^t(t\in \left[ 1,b_i-1 \right])$的倍数。
>
> - 因此我们需要做的就是首先找到b序列中第一个数x，能够在a中找到数是 $2^x$ 的整数倍
>
>     > 这一步可以这样进行：对于a中的每一个数，我们进行30次循环统计当前数是否是 $2^i$ 的倍数，如果是就用哈希表记录当前的 $i$。最后我们在遍历寻找x时，只需要查看当前的x是否被哈希过即可
>
> - 接着我们统计b序列中从x开始的严格降序序列c（由题意知，次序列的数量一定 $\le$ 30，因为b序列中数值的值域为 $[1~30]$）
>
> - 最后我们再按照原来的思路，双重循环a序列和c序列即可
>
> 时间复杂度：$O(30n)$

```c++
void solve() {
	int n, m;
	cin >> n >> m;

	vector<int> a(n + 1), b(m + 1);
	unordered_map<int, int> ha;

    // 边读边哈希
	for (int i = 1; i <= n; i++) {
		cin >> a[i];
		for (int j = 30; j >= 1; j--) {
			if (a[i] % (1 << j) == 0) {
				ha[j]++;
			}
		}
	}

	for (int i = 1; i <= m; i++) {
		cin >> b[i];
	}

	// 寻找b中第一个能够让a[j]整除的数b[flag]
	int flag = -1;
	for (int i = 1; i <= m; i++) {
		if (ha[b[i]]) {
			flag = i;
			break;
		}
	}

    // 特判
	if (flag == -1) {
		for (int j = 1; j <= n; j++) {
			cout << a[j] << " \n"[j == n];
		}
		return;
	}

    // 寻找b中从flag开始的严格单调递减的序列c
	vector<int> c;
	c.push_back(b[flag]);
	for (; flag <= m; flag++) {
		if (b[flag] < c.back()) {
			c.push_back(b[flag]);
		}
	}

    // 暴力循环一遍即可
	for (int j = 1; j <= n; j++) {
		for (int k = 0; k < c.size(); k++) {
			if (a[j] % (1 << c[k]) == 0) {
				a[j] += 1 << (c[k] - 1);
			}
		}
	}

	for (int j = 1; j <= n; j++) {
		cout << a[j] << " \n"[j == n];
	}
}
```

## geometry

### 1. Minimum Manhattan Distance

https://codeforces.com/gym/104639/problem/J

> 题意：给定两个圆的直径的两个点坐标，其中约束条件是两个圆一定是处在相离的两个角上。问如何在C2圆上或圆内找到一点p，使得点p到C1圆的所有点的曼哈顿距离的期望值最小
>
> 思路：
>
> 1. 看似需要积分，其实我们可以发现，对于点p到C1中某个点q1的曼哈顿距离，我们一定可以找到q1关于C1对称的点q2，那么点p到q1和q2的曼哈顿距离之和就是点p到C1的曼哈顿距离的两倍（证明就是中线定理）那么期望的最小值就是点p到C1的曼哈顿距离的最小值。目标转化后，我们开始思考如何计算此目标的最小值，思路如下图
> 2. <img src="C:/Users/%E8%91%A3%E6%96%87%E6%9D%B0/AppData/Roaming/Typora/typora-user-images/image-20231031233842979.png" alt="image-20231031233842979" style="zoom: 25%;" />
>
> ==注意点：==
>
> 1. double的读取速度很慢，可以用 `int` or `long long` 读入，后续强制类型转换（显示 or 和浮点数计算）
> 2. 注意输出答案的精度控制 `cout << fixed << setprecision(10) << res << "\n";`

```c++
void solve() {
	double x1, y1, x2, y2;
	long long a, b, c, d;

	cin >> a >> b >> c >> d;
	x1 = (a + c) / 2.0;
	y1 = (b + d) / 2.0;

	cin >> a >> b >> c >> d;
	x2 = (a + c) / 2.0;
	y2 = (b + d) / 2.0;

	double r2 = sqrt((a - c) * (a - c) + (b - d) * (b - d)) / 2;

	cout << fixed << setprecision(10) <<  abs(x1 - x2) + abs(y1 - y2) - sqrt(2) * r2 << "\n";
}
```

## data structures

### 1. 单调栈

https://www.acwing.com/problem/content/832/

> 题意：对于一个序列中的每一个元素，寻找每一个元素前面第一个比他小的元素
>
> 思路：
>
> - 首先很容易想到一个暴力的做法，就是对于每一个数，再从 $i \to 0$ 进行枚举寻找第一个比当前数小的那个数
> - 时间复杂度：$O(n^2)$
>
> 优化：首先每一个数一定是要遍历到的，那么关键在于如何优化掉寻找前面的最近的比他小的数的计算过程。我们逆向思考一下，不要考虑当前数字 `a[i]` 前面最近的一个比他小的数字我们考虑当前数字如何才能成为后面数字的最近的最小值。
>
> 我们将这个序列想象成一个散点图
>
> - 如果当前数字能够成为后面的最近的最小值，那么当前数字就一定严格小于后面的数字。保留当前的散点
>
> - 那么与上面相反的是，如果 `a[i]>=后面的数字` 那么当前数字就一定不可能成为后面数字的最近的比他小的数字，就需要不断删除当前数以及前面的数，直到第一次寻找到比当前数小的那个数。
>
> 经过上述流程之后，我们发现，此时的“散点图”上剩下来的点，呈现一个严格单调递增的形状。于是优化的思路就来了：
>
> 我们只需要在扫描到 `a[i]` 的时候，维护 `a[0]` 到 `a[i-1]` 中的单调递增的序列即可，算法思路就是：
>
> 如果当前数 >   容器中的最后一个数，那么当前数的最近的那个数就是容器中的最后一个数
>
> 如果当前数 <= 容器中的最后一个数，那么就需要不断删除容器中的最后一个数，直到最后一个数 < 当前数，那么当前容器中的最后一个数就是当前数最近的那个比他小的数
>
> 最后将当前数加入容器尾部即可维护一个单调递增序列了。
>
> 至于选用什么样的容器，支持高效率查询尾部元素、高效率尾插入、高效率尾删除，即可。那么数组、栈、队列等很多线性结构的容器都是可以的。我们这里选用数组。
>
> 时间复杂度：$O(2n)$

```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr), cout.tie(nullptr);
    
    int n; cin >> n;
    vector<int> a(n + 1);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    
    vector<int> v;
    
    for (int i = 0; i < n; i++) {
        if (v.empty()) {
            cout << -1 << " ";
            v.push_back(a[i]);
        } 
        else {
            while (!v.empty() && a[i] <= v.back()) {
                v.pop_back();
            }
            if (v.empty()) {
                cout << -1 << " ";
            } else {
                cout << v.back() << " ";
            }
            v.push_back(a[i]);
        }
    }
    
    return 0;
}
```

### 2. 【模板】最近公共祖先（LCA）

https://www.luogu.com.cn/problem/P3379

> 题意：寻找树中指定两个结点的最近公共祖先
>
> 思路：对于每次查询，我们可以从指定的两个结点开始往上跳，第一个公共结点就是目标的LCA，每一次询问的时间复杂度均为 $O(n)$，为了加速查询，我们可以采用倍增法，预处理出往上跳的结果，即 `fa[i][j]` 数组，表示 $i$ 号点向上跳 $2^j$ 步后到达的结点。接下来在往上跳跃的过程中，利用二进制拼凑的思路，即可在 $O(\log n)$ 的时间内查询到LCA。
>
> 预处理：可以发现，对于 `fa[i][j]`，我们可以通过递推的方式获得，即 `fa[i][j] = fa[fa[i][j-1]][j-1]`，当前结点向上跳跃 $2^j$ 步可以拆分为先向上 $2^{j-1}$ 步，在此基础之上再向上 $2^{j-1}$ 步。于是我们可以采用宽搜 $or$ 深搜的顺序维护 $fa$ 数组。
>
> 跳跃：我们首先需要将两个结点按照倍增的思路向上跳到同一个深度，接下来两个结点同时按照倍增的思路向上跳跃，为了确保求出最近的，我们需要确保在跳跃的步调一致的情况下，两者的祖先始终不相同，那么倍增结束后，两者的父结点就是最近公共祖先，即 `fa[x][k]` 或 `fa[y][k]`
>
> 时间复杂度：$O(n \log n + m \log n)$ 
>
> - $n \log n$ 为预处理每一个结点向上跳跃抵达的情况
> - $m \log n$ 为 $m$ 次询问的情况

```c++
const int N = 5e5 + 10;

int n, Q, root;
vector<int> G[N];
int fa[N][20], dep[N];
queue<int> q;

void init() {
	dep[root] = 1;
	q.push(root);

	while (q.size()) {
		int now = q.front();
		q.pop();
		for (int ch: G[now]) {
			if (!dep[ch]) {
				dep[ch] = dep[now] + 1;
				fa[ch][0] = now;
				for (int k = 1; k <= 19; k++) {
					fa[ch][k] = fa[ fa[ch][k-1] ][k-1];
				}
				q.push(ch);
			}
		}
	}
}

int lca(int a, int b) {
	if (dep[a] < dep[b]) swap(a, b);

	for (int k = 19; k >= 0; k--)
		if (dep[fa[a][k]] >= dep[b])
			a = fa[a][k];

	if (a == b) return a;

	for (int k = 19; k >= 0; k--)
		if (fa[a][k] != fa[b][k])
			a = fa[a][k], b = fa[b][k];

	return fa[a][0];
}

void solve() {
	cin >> n >> Q >> root;
	for (int i = 0; i < n - 1; ++i) {
		int a, b;
		cin >> a >> b;
		G[a].push_back(b);
		G[b].push_back(a);
	}

	init();

	while (Q--) {
		int a, b;
		cin >> a >> b;
		cout << lca(a, b) << "\n";
	}
}	
```

### 3. [USACO19DEC] Milk Visits S

https://www.luogu.com.cn/problem/P5836

> tag：并查集
>
> 题意：给定一棵树，结点被标记成两种，一种是H，一种是G，在每一次查询中，需要知道指定的两个结点之间是否含有某一种标记
>
> 思路：对于树上标记，我们可以将相同颜色的分支连成一个连通块
>
> - 如果查询的两个结点在同一个连通块，则查询两个结点所在的颜色与所需的颜色是否匹配即可
> - 如果查询的两个结点不在同一个连通块，两个结点之间的路径一定是覆盖了两种颜色的标记，则答案一定是1
>
> 时间复杂度：$O(n+m)$

```c++
const int N = 100010;

int n, m, p[N];
char col[N];

int find(int x) {
	if (p[x] != x) {
		p[x] = find(p[x]);
	}
	return p[x];
}

void solve() {
	cin >> n >> m;
	cin >> (col + 1);

	for (int i = 1; i <= n; i++) {
		p[i] = i;
	}

	for (int i = 1; i <= n - 1; i++) {
		int a, b;
		cin >> a >> b;
		if (col[a] == col[b]) {
			p[find(a)] = find(b);
		}
	}

	string res;

	while (m--) {
		int u, v;
		cin >> u >> v;

		char cow;
		cin >> cow;

		if (find(u) == find(v)) {
			res += to_string(col[u] == cow);
		} else {
			res += '1';
		}
	}

	cout << res << "\n";
}
```

## graphs

### 1. 有向图的拓扑序列

https://www.acwing.com/problem/content/850/

> 题意：输出一个图的拓扑序，不存在则输出-1
>
> 思路：
>
> - 首先我们要知道拓扑图的概念，感官上就是一张图可以从一个方向拓展到全图，用数学语言就是：若一个由图中所有点构成的序列 A 满足：对于图中的每条边 (x,y)，x 在 A 中都出现在 y 之前，则称 A 是该图的一个拓扑序列
> - 接着我们就想要寻找这样的序列 A 了，可以发现对于每一个可扩展的点，入度一定为0，那么我们就从这些点开始宽搜，将搜到的点的入度-1，即删除这条边，直到最后。如果全图的点的入度都变为了0，则此图可拓扑
>
> 时间复杂度：$O(n+m)$

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 100010;

int n, m;
vector<int> G[N];

void solve() {
	// 建图 
	cin >> n >> m;
	vector<int> d(n + 1, 0);
	for (int i = 1; i <= m; i++) {
		int a, b;
		cin >> a >> b;
		d[b]++;
		G[a].push_back(b);
	}
	
	// 预处理宽搜起始点集
	queue<int> q;
	for (int i = 1; i <= n; i++)
		if (!d[i])
			q.push(i);
	
	// 宽搜处理
	vector<int> res;
	while (q.size()) {
		auto h = q.front();
		q.pop();
		res.push_back(h);
		
		for (auto& ch: G[h]) {
			d[ch]--;
			if (!d[ch]) q.push(ch);
		}
	}
	
	// 输出合法拓扑序
	if (res.size() == n) {
		for (auto& x: res) {
			cout << x << " ";
		}
	} else {
		cout << -1 << "\n";
	}
}

int main() {
	solve();
	return 0;
}
```

### 2. Mad City:fire:

https://codeforces.com/contest/1873/problem/H

> tag：基环树、拓扑排序
>
> 题意：给定一个基环树，现在图上有两个点，分别叫做A，B。现在B想要逃脱A的抓捕，问对于给定的局面，B能否永远逃离A的抓捕
>
> 思路：思路很简单，我们只需要分B所在位置的两种情况讨论即可
>
> 1. B不在环上：此时我们记距离B最近的环上的那个点叫 $tag$，我们需要比较的是A到tag点的距离 $d_A$ 和B到tag的距离 $d_B$，如果 $d_B < d_A$，则一定可以逃脱，否则一定不可以逃脱
> 2. B在环上：此时我们只需要判断当前的A点是否与B的位置重合即可，如果重合那就无法逃脱，反之B一定可以逃脱。
>
> 代码实现：
>
> 1. 对于第一种情况，我们需要找到tag点以及计算A和B到tag点的距离，
>
> 时间复杂度：

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 200010;

int n, a, b;
vector<int> G[N];
int rd[N], tag, d[N];
bool del[N], vis[N];

void init() {
	for (int i = 1; i <= n; i++) {
		G[i].clear();		// 存无向图 
		rd[i] = 0;			// 统计每一个结点的入度 
		del[i] = false;		// 拓扑删点删边时使用 
		d[i] = 0;			// 图上所有点到 tag 点的距离 
		vis[i] = false;		// bfs计算距离时使用 
	}
}

void topu(int now) {
	if (rd[now] == 1) {
		rd[now]--;
		del[now] = true;
		for (auto& ch: G[now]) {
			if (del[ch]) continue;
			rd[ch]--;
			if (now == tag) {
				tag = ch;
			}
			topu(ch);
		}
	}
}

void bfs() {
	queue<int> q;
	q.push(tag);
	d[tag] = 0;
	
	while (q.size()) {
		auto now = q.front();
		vis[now] = true;
		q.pop();
		
		for (auto& ch: G[now]) {
			if (!vis[ch]) {
				d[ch] = d[now] + 1;
				q.push(ch);
				vis[ch] = true;
			}
		}
	}
}

void solve() {
	// 初始化
	cin >> n >> a >> b; 
	init();
	
	// 建图 
	for (int i = 1; i <= n; i++) {
		int u, v;
		cin >> u >> v;
		G[u].push_back(v), rd[v]++;
		G[v].push_back(u), rd[u]++;
	}
	
	// 拓扑删边 & 缩b点
	tag = b;
	for (int i = 1; i <= n; i++) {
		topu(i);
	}

	// 判断结果 & 计算距离 
	if (rd[b] == 2 && a != b) {
		// b点在环上
		cout << "Yes\n";
	} else {
		// b不在环上
		bfs();
		cout << (d[a] > d[b] ? "Yes\n" : "No\n");
	}
}

int main() {
	ios::sync_with_stdio(false);
	cin.tie(0);
	int T = 1;
	cin >> T;
	while (T--) solve();
	return 0;
}
```

### 3. 染色法判定二分图

https://www.acwing.com/problem/content/862/

> 题意：给定一个无向图，可能有重边和自环。问是否可以构成二分图。
>
> 二分图的定义：一个图可以被分成两个点集，每个点集内部没有边相连（可以不是连通图）
>
> 思路：利用**染色法**，遍历每一个连通分量，选择连通分量中的任意一点进行染色扩展
>
> - 如果扩展到的点没有染过色，则染成与当前点相对的颜色
> - 如果扩展到的点已经被染过色了且染的颜色和当前点的颜色相同，则无法构成二分图（奇数环）
>
> 时间复杂度：$O(n+e)$

```c++
const int N = 100010;

int n, m;
vector<int> G[N], col(N);

bool bfs(int u) {
	queue<int> q;
	q.push(u);
	col[u] = 1;

	while (q.size()) {
		int now = q.front();
		q.pop();
		for (auto& ch: G[now]) {
			if (!col[ch]) {
				col[ch] = -col[now];
				q.push(ch);
			}
			else if (col[ch] == col[now]) {
				return false;
			}
		}
	}

	return true;
}

void solve() {
	cin >> n >> m;
	while (m--) {
		int u, v;
		cin >> u >> v;
		G[u].push_back(v);
		G[v].push_back(u);
	}

	// 遍历每一个连通分量
	for (int i = 1; i <= n; i++) {
		if (!col[i]) {
			bool ok = bfs(i);
			if (!ok) {
				cout << "No\n";
				return;
			}
		}
	}

	cout << "Yes\n";
}
```

### 4. Kruskal算法求最小生成树

https://www.acwing.com/problem/content/861/

> 题意：给定一个无向图，可能含有重边和自环。试判断能否求解其中的最小生成树，如果可以给出最小生成树的权值
>
> 思路：根据数据量，可以发现顶点数很大，不适用 $Prim$ 算法，只能用 $Kruskal$ 算法，下面简单介绍一下该算法的流程
>
> - 自环首先排除 - 显然这条边连接的“两个”顶点是不可能选进 $MST$ 的
> - 首先将每一个结点看成一个连通分量
> - 接着按照权值将所有的边升序排序后，依次选择
>     - 如果选出的这条边的两个顶点不在一个连通分量中，则选择这条边并将两个顶点所在的连通分量合并
>     - 如果选出的这条边的两个顶点在同一个连通分量中，则不能选择这条边（否则会使得构造的树形成环）
> - 最后统计选择的边的数量 $num$ 进行判断即可
>     - $num=n-1$，则可以生成最小生成树
>     - $num<n-1$，则无法生成最小生成树
> - 时间复杂度：$O(e\log e)$ - 因为最大的时间开销在对所有的边的权值进行排序上

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 100010;

struct edge {
	int a, b;
	int w;
};

int n, m;
vector<edge> edges;
vector<int> p(N);

int Find(int now) {
	if (p[now] != now) {
		p[now] = Find(p[now]);
	}
	return p[now];
}

void solve() {
	cin >> n >> m;
	for (int i = 1; i <= m; i++) {
		int a, b, w;
		cin >> a >> b >> w;
		if (a == b) {
			continue;
		}
		edges.push_back({a, b, w});
	}

	// 按照边权升序排序
	sort(edges.begin(), edges.end(), [&](edge& x, edge& y) {
		return x.w < y.w;
	});

	// 选边
	for (int i = 1; i <= n; i++) {
		p[i] = i;
	}

	int res = 0, num = 0;

	for (auto& e: edges) {
		int pa = Find(e.a), pb = Find(e.b);
		if (pa != pb) {
			num++;
			p[pa] = pb;
			res += e.w;
		}

		if (num == n - 1) {
			break;
		}
	}

	// 特判：选出来的边数无法构成一棵树
	if (num < n - 1) {
		cout << "impossible\n";
		return;
	}

	cout << res << "\n";
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

### 5. Prim算法求最小生成树

https://www.acwing.com/problem/content/860/

> 题意：给定一个稠密无向图，有重边和自环。求出最小生成树
>
> 思路：根据题目的数据量，可以使用邻接矩阵存储的方法配合 $Prim$ 算法求解最小生成树，下面给出该算法的流程
>
> - 首先明确一下变量的定义：
>     - `g[i][j]` 为无向图的邻接矩阵存储结构
>     - `MST[i]` 表示 $i$ 号点是否加入了 $MST$ 集合
>     - `d[i]` 表示 `i` 号点到 $MST$ 集合的最短边长度
> - 自环不存储，重边只保留最短的一条
> - 任选一个点到集合 $MST$ 中，并且更新 $d$ 数组
> - 选择剩余的 $n-1$ 个点，每次选择有以下流程
>     - 找到最短边，记录最短边长度 $e$ 和相应的在 $U-MST$ 集合中对应的顶点序号 $v$
>     - 将 $v$ 号点加入 $MST$ 集合，同时根据此时选出的最短边的长度来判断是否存在最小生成树
>     - 根据 $v$ 号点，更新 $d$ 数组，即更新在集合 $U-MST$ 中的点到 $MST$ 集合中的点的交叉边的最短长度
> - 时间复杂度：$O(n^2)$

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int N = 510;

int n, m;
vector<vector<int>> g(N, vector<int>(N, INT_MAX));
vector<int> d(N, INT_MAX); // d[i]表示i号点到MST集合中的最短边长度
bool MST[N];
int res;

void prim() {
	// 选任意一个点到MST中并更新d数组
	MST[1] = true;
	for (int i = 1; i <= n; i++)
		if (!MST[i])
			d[i] = min(d[i], g[i][1]);

	// 选剩下的n-1个点到MST中
	for (int i = 2; i <= n; i++) {
		// 1. 找到最短边
		int e = INT_MAX, v = -1; // e: 最短边长度，v: 最短边不在MST集合中的顶点
		for (int j = 1; j <= n; j++)
			if (!MST[j] && d[j] < e)
				e = d[j], v = j;

		// 2. 加入MST集合
		MST[v] = true;
		if (e == INT_MAX) {
			// 特判无法构造MST的情况
			cout << "impossible\n";
			return;
		} else {
			res += e;
		}

		// 3. 更新交叉边 - 迭代（覆盖更新）
		for (int j = 1; j <= n; j++)
			if (!MST[j])
				d[j] = min(d[j], g[j][v]);
	}

	cout << res << "\n";
}

void solve() {
	cin >> n >> m;
	while (m--) {
		int a, b, w;
		cin >> a >> b >> w;

		if (a == b) {
			continue;
		}

		if (g[a][b] == INT_MAX) {
			g[a][b] = w;
			g[b][a] = w;
		} else {
			g[a][b] = min(g[a][b], w);
			g[b][a] = min(g[b][a], w);
		}
	}

	prim();
}

signed main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr), cout.tie(nullptr);
	int T = 1;
//	cin >> T;
	while (T--) solve();
	return 0;
}
```

## 思维题

### 1. 最长严格递增子序列

https://www.acwing.com/problem/content/5273/

> 题意：给定长度为n的序列，问将这个序列拼接n次后，最长严格递增子序列的长度为多少？
>
> 思路：其实最终的思路很简单，讲题目转化为在每个序列中选一个数，一共可以选出多少个不同的数。但是在产生这样的想法之前，先讲一下我的思考过程。我将序列脑补出一幅散点折线图，然后将这些点投影到y轴上，最终投影点的个数就是答案的数量，但是投影会有重合，因此答案最多就是n个数，最少1个数，从而想到就是在n个序列中选数，选的数依次增大即可，相信的就是一个求序列不重复数的个数的过程。去重即可。
>
> 注意点：由于C++的STL的unique函数的前提是一个有序的序列，因此在unique之前需要将序列进行排序。
>
> 时间复杂度：$O(n)$

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int T;
    cin >> T;

    while (T--)
    {
        int n;
        cin >> n;

        vector<int> a;
        for (int i = 0; i < n; i++)
        {
            int x;
            cin >> x;
            a.emplace_back(x);
        }

        // 去重老套路，string也可以用
        sort(a.begin(), a.end());
        a.erase(unique(a.begin(), a.end()), a.end());

        cout << a.size() << endl;
    }

    return 0;
}
```

### 2. 三元组

https://www.acwing.com/problem/content/5280/

> 题意：在一个序列中如何选择一个三元组，使得三个数之积最小，给出情况数
>
> 思路：首先很容易得知，这三个数一定是序列排序后的前三个数，那么就是对这三个数可能的情况进行讨论
>
> - 情况1：**前三个数都相等 **`（2 2 2 2 2 4 5）`，则就是在所有的 `a[0]` 中选3个，情况数就是 $C_{cnt\_a[0]}^{3}$
> - 情况2：**前三个数中有两个数相等**，由于数组经过了排序，因此情况2分为两种情况
>     - 前两个数相等 `（1 1 3 3 3 5）`，则就是在所有的 `a[2]` 中选1个，情况数就是 $C_{cnt\_a[2]}^{1}$
>     - 后两个数相等 `（1 2 2 2 4 6）`，则就是在所有的 `a[1]` 中选2个，情况数就是 $C_{cnt\_a[1]}^{2}$
> - 情况3：**前三个数中全都不相等** `（1 2 4 4 4 5 7）`，这就是在所有的 `a[2]` 中选1个，情况数就是 $C_{cnt\_a[2]}^{1}$
>
> 可以发现上述情况2的第一种和情况3的答案相等，故分为三种情况即可。

```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

// 计算组合数 C_{a}^{b}
ll calc(ll a, ll b) {
    ll fz = 1, fm = 1;
    for (int i = 0, num = a; i < b; i++, num--) {
        fz *= num;
    }
    for (int i = 1; i <= b; i++) {
        fm *= i;
    }
    return fz / fm;
}

int main() {
    int n; cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    
    sort(a.begin(), a.begin() + n);
    
    ll res = 0;
    
    if (a[0] == a[1] && a[1] == a[2]) {
        // 情况1：前三个数都相等（2 2 2 2 2 4 5），则就是在所有的a[0]中选3个
        ll cnt = count(a.begin(), a.begin() + n, a[0]);
        res = calc(cnt, 3);
    } else if (a[0] == a[1] || (a[0] != a[1] && a[1] != a[2])) {
        // 情况2：前两个数相等（1 1 3 3 3 4 5） or 三个数都不相等（1 2 4 4 4 5 7），则就是在所有的a[2]中选2个
        res = count(a.begin(), a.begin() + n, a[2]);
    } else {
        // 情况3：后两个数相等（1 2 2 2 4 6），则就是在所有的a[1]中选2个
        ll cnt = count(a.begin(), a.begin() + n, a[1]);
        res = calc(cnt, 2);
    }
    
    cout << res << "\n";
    
    return 0;
}
```

### 3. 删除元素

https://www.acwing.com/problem/content/5281/

> 题意：给定一个排列为1~n，定义一个数“有价值”为当前数的前面没有数比当前的数大。现在需要删除一个数，使得序列中增加尽可能多的“有价值”的数，如果这个数有多个，则删除最小的那个数
>
> ==最开始想到的思路==：枚举每一个数，如果删除，则序列中会增加多少个“有价值”的数，算法设计如下：
>
> 1. 首先判断每一个数是否是有价值的数
>     - 创建一个变量来记录当前数的前面序列的最大值
>     - 比较判断当前数和前方最大值的关系，如果小于，则无价值，反之有价值
> 2. 接着枚举每一个数，如果删除该数，则序列会损失多少个有价值的数
>     - 首先判断自己是不是有价值的数，如果是，则当前损失值 -1
>     - 接着判断删除当前数对后续的影响 :star: ：我们在枚举后续数的时候，起始的newd（前驱除掉a[i]的最大值）应该就是当前的d，只不过需要使用新变量newd而非d是因为这一步与整体无关，不可以改变整体的d。否则会出现错误，比如对于`4 3 5 1 2`，如果我们在枚举后续数的时候直接对d进行迭代，那么在第一轮d就会被更新为5，就再也无法更新了
> 3. 最终根据维护的损失值数组cnt即可求解
> 4. 时间复杂度 $O(n^2)$
>
> ---
>
> ==优化的思路==：
>
> 1. 同样是枚举每一个数，如果当前数字是有价值的数，那么cnt[a[i]]就 -1
>
>     - 取决于当前数字前面是否有比它大的数，如果没有，那么就是有价值的
>
> 2. 接下来我们不需要遍历j=i+1到j=n-1，而是讨论当前数a[i]不是有价值数时的所有情况。现在我们想要维护cnt数组。不难发现，对于某个位置上的数a[k]，能否成为有价值的数只取决于前排序列是否有比他大的数以及大的数的个数。那么理论上我们在枚举a[i]的时候维护cnt[1~i-1]就可以确保cnt数组的正确性了。
>
>     - 假设a[k]的前排有一个比它大的数d：那么把d去掉之后，a[k]就会从无价值变为有价值
>     - 假设a[k]的前排有至少两个比它大的数d1,d2,...dj...：那么不管去掉哪一个 $d_j$，我们都没法让a[k]变得有价值
>
>     如此一来，cnt数组就可以正确维护了
>
> 3. 最终根据维护的损失值数组cnt即可求解
>
> 4. 时间复杂度 $O(n)$

暴力：

```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
	int n;
	cin >> n;
	
	vector<int> a(n);
	for (int i = 0; i < n; i++) {
		cin >> a[i];
	}
	
	// 判断每个数是否是有价值的数
	int d = 0; // 前方序列中的最大值
	vector<bool> is_val(n + 1);
	for (int i = 0; i < n; i++) {
		if (a[i] > d) {
			is_val[a[i]] = true;
			d = a[i];
		}
	}
	
	// 枚举每一个数，计算删除该数之后会增加的有价值的数的个数 
	d = 0; // 同上
	vector<int> cnt(n + 1); // cnt[x]表示删除数x之后可以增加的有价值的数的个数
	for (int i = 0; i < n; i++) {
		if (is_val[a[i]]) {
			cnt[a[i]]--;
		}
		
		int newd = d; // 去掉a[i]后的前方序列的最大值 newd
		for (int j = i + 1; j < n; j++) {
			if (is_val[a[j]]) {
				if (a[j] < newd) {
					cnt[a[i]]--;
				}
			} else {
				if (a[j] > newd) {
					cnt[a[i]]++;
				}
			}
			if (a[j] > newd) {
			    newd = a[j];
			}
		}
		
		// 更新前方序列的最大值 d
		if (a[i] > d) {
			d = a[i];
		}
	}

	// 找出可以增加的有价值的数的最大个数 ma
	int ma = -n - 1;
	for (int i = 0; i < n; i++) {
		if (cnt[a[i]] > ma) {
			ma = max(ma, cnt[a[i]]);
		}
	}

	// 答案是满足个数的最小的数字
	int res = 0;
	for (int i = 1; i <= n; i++) {
		if (cnt[i] == ma) {
			res = i;
			break;
		}
	}
	
	cout << res << "\n";
	
	return 0;
}
```

优化：

```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n; 
    cin >> n;
    
    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
     
    vector<int> cnt(n + 1);     // cnt[x]表示删除数x之后可以增加的有价值的数的个数
    int d1 = 0, d2 = 0;         // d1: 最大值, d2: 次大值
    for (int i = 0; i < n; i++) {
        if (a[i] > d1) {
            // 前方序列 没有数比当前的数大
            cnt[a[i]]--;
        } else if (a[i] > d2) {
            // 前方序列 只有一个数比当前大
            cnt[d1]++;
        }
        
        // 更新最大值d1和次大值d2
        if (a[i] > d1) d2 = d1, d1 = a[i];
        else if (a[i] > d2) d2 = a[i];
    }
    
    // 找到可以增加的最大值
    int ma = -1;
    for (int i = 1; i <= n; i++) {
        if (cnt[i] > ma) {
            ma = cnt[i];
        }
    }
    
    // 取元素的最小值
    int res = n + 1;
    for (int i = 1; i <= n; i++) {
        if (cnt[i] == ma) {
            res = i;
            break;
        }
    }
    
    cout << res << "\n";
    
    return 0;
}
```

### 4.Sorting with Twos

https://codeforces.com/contest/1891/problem/A

> 题意：给定一个序列，现在需要通过以下方法对序列进行升序排序
>
> - 可以选择前 $2^m$ 个数执行 $-1$ 的操作（保证不会越界的情况下）
>
> 现在需要确定给定的序列经过k次上述后能否变为升序序列
>
> 思路：我们可以将每 $2^k$ 个数看成一个数，可以不断的-1。那么可以发现执行一定的次数后，一定可以实现升序序列。但是现在是 $2^k$ 个数，那么只需要这相邻区段的数是升序即可，即 $2^{k-1} \to 2^k$ 之间的数是升序即可。按区间进行判断即可
>
> 时间复杂度：$O(n)$

```c++
void judge(vector<int>& a, int l, int r, bool& ok, int n) {
	for (int i = l + 1; i <= r && i <= n; i++) {
		if (a[i] < a[i - 1]) {
			ok = false;
			return;
		}
	}
}
 
void solve() {
	int n;
	cin >> n;
 
	vector<int> a(n + 1);
	for (int i = 1; i <= n; i++) {
		cin >> a[i];
	}
 
	bool ok = true;
	judge(a, 3, 4, ok, n);
	judge(a, 5, 8, ok, n);
	judge(a, 9, 16, ok, n);
	judge(a, 17, 20, ok, n);
 
	if (ok) {
		cout << "YES\n";
	} else {
		cout << "NO\n";
	}
}
```

# 二、模板

## 高精度

```c++
class Int : public std::vector<int> {
public:
	Int(int n = 0) {
		push_back(n);
		check();
	}

	Int& check() {
		for (int i = 1; i < size(); ++i) {
			(*this)[i] += (*this)[i - 1] / 10;
			(*this)[i - 1] %= 10;
		}
		while (back() >= 10) {
			push_back(back() / 10);
			(*this)[size() - 2] %= 10;
		}
		return *this;
	}

	friend std::istream& operator>>(std::istream& in, Int& n) {
		std::string s;
		in >> s;
		n.clear();
		for (int i = s.size() - 1; i >= 0; --i) n.push_back(s[i] - '0');
		return in;
	}

	friend std::ostream& operator<<(std::ostream& out, const Int& n) {
		if (n.empty()) out << 0;
		for (int i = n.size() - 1; i >= 0; --i) out << n[i];
		return out;
	}

	friend Int& operator+=(Int& a, const Int& b) {
		if (a.size() < b.size()) a.resize(b.size());
		for (int i = 0; i != b.size(); ++i) a[i] += b[i];
		return a.check();
	}

	friend Int operator+(Int a, const Int& b) {
		return a += b;
	}
};
```





































































































​	
