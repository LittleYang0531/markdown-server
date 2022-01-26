## [USACO20OPEN] Favorite Colors G 做题记录

### 题目描述:

​在 OI 大陆，每一个人都喜欢一种不同类型的题目，数据结构，动态规划，组合计数等等。根据喜欢的题目类型，OIER 之间的关系也会受到映像。

​现在，在这些 OIER 之间存在 M 对欣赏关系 $(a,b)$，有可能存在 $a=b$。对于任意题型 $c$，如果两位 OIER $(x,y)$ 都欣赏一位喜欢题型 $c$ 的 OIER，那么 $x$ 和 $y$ 所喜欢的题型就是相同的。 

​我们会给定你 OIER 的人数和他们之间存在的 $M$ 对欣赏关系，你需要给出一种题型分配方法，使得存在的不同的题型数目最多，由于可能存在不只一组合法解，你需要输出字典序最小的一组解。（这意味着如果你需要 $k$ 种题型，你应该将他们编号为 $1,2,3,...,k$）

### 输入描述:

​输入的第一行包含两个正整数 $N,M(1\leq N,M\leq 2\times 10^5)$。分别表示 OIER 的数量以及他们之间存在的欣赏关系的数量。

​接下来的 $M$ 行，每行包含两个用空格隔开的整数 $a$ 和 $b(1\leq a,b\leq N)$，表示编号为 $b$ 的 OIER 欣赏编号为 $a$ 的 OIER。可能存在一队欣赏关系在输入中出现多次。

### 输出描述:

​对于 $1$~$N$ 中的每一个 $i$，用一行输出编号为 $i$ 的 OIER 所喜欢的题目类型。

### 测试样例:

#### 【测试1输入】

```
9 12
1 2
4 2
5 8
4 6
6 9
2 9
8 7
8 3
7 1
9 4
3 5
3 4
```

#### 【测试1输出】

```
1
2
3
1
1
2
3
2
3
```

### 数据范围与提示:

| 子任务 | 分数 |   特殊性质   |
| :----: | :--: | :----------: |
|   1    |  40  | $N\leq 1000$ |
|   2    |  60  |  无特殊性质  |

### 题目分析:

​由于两个题型相同的节点欣赏他们的节点的题型也会相等。考虑合并两个题型相同的节点，使用启发式合并，将两个节点合并在一起，并将合并之后的欣赏他们的节点数大于1的节点扔进 BFS 序列中待会继续合并，直到最后没有可以合并的节点时，剩下的节点的颜色一定不同。接下来只需要按照节点id从小到大分配题型编号就行。

​时间复杂度为 $O(N\log N)$

### AC代码:

```C++
#include<bits/stdc++.h>
#define van long long
using namespace std;
const van N=2e5+10;
bool ppppp;
van n,m,f[N];
vector<van> g[N],l[N];
van col[N],cnt=0;
queue<van> q;
bool pppppp;
void Update(van a,van b) {
	a=f[a],b=f[b];
	if (l[a].size()>l[b].size()) swap(a,b);//启发式合并
	for (unsigned int i=0;i<g[a].size();i++) {
		g[b].push_back(g[a][i]);
	}//合并g集合
	for (unsigned int i=0;i<l[a].size();i++) {
		f[l[a][i]]=b;
		l[b].push_back(l[a][i]);
	}//合并l集合
	if (g[b].size()>1) {
		q.push(b);
	}//若欣赏b的人数大于1，就将b加入BFS里待会继续合并
}
int main() {
	cin>>n>>m;
	for (int i=1;i<=m;i++) {
		van fff,s;
		cin>>fff>>s;
		g[fff].push_back(s);
	}
	for (int i=1;i<=n;i++) {
		f[i]=i;
		if (g[i].size()>1) q.push(i);
		l[i].push_back(i);
	}//初始化
	while (!q.empty()) {
		van now=q.front();q.pop();
		while (g[now].size()>1) {
			van a=g[now][1],b=g[now][0];g[now].erase(g[now].begin());
			if (f[a]!=f[b]) {
				Update(a,b);
			}
		}
	}//一遍BFS
	for (int i=1;i<=n;i++) {
		if (!col[f[i]]) col[f[i]]=++cnt;
		cout<<col[f[i]]<<endl;
	}//计算颜色
}
```

