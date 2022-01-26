[Pixiv: 86036508]: 'https://cdn.jsdelivr.net/gh/LittleYang0531/Photo/86036508_p0.png'

## [USACO19DEC] Milk Visits G 做题记录

### 题目描述:

​作为冷酷资本大亨的 wby 购买了一块巨大的土地用于建造别墅建筑群。在这些土地上修建了 $N$ 座各式各样的建筑。这 $N$ 座建筑由 $N-1$ 条道路连接，构成了一棵树。每一个建筑有其独特的建筑风格，我们可以用一个 1 到 $N$ 之间的一个整数 $T_i$ 来表示其建筑风格。

​wby 经常会与他的生意伙伴在别墅区域内谈论生意。与第 $i$ 个生意伙伴会面时，他们会沿着建筑 $A_i$ 到建筑 $B_i$ 之间唯一的路径行走，交流。由于 wby 的生意伙伴们也都是革命时会被吊在路灯上资本家，他们也对建筑颇有心得，具体的说，第 $i$ 位生意伙伴喜欢 $C_i$ 建筑风格。

​如果某一个朋友在拜访 wby 的别墅建筑群的过程中看见了他喜欢的建筑风格的建筑，这位朋友就会非常高兴(即 $A_i$ 到 $B_i$ 的路径上存在建筑风格为 $C_i$ 的建筑)。作为一个无情资本大亨，wby 希望知道每一个资本家朋友会不会高兴。为此，它将这个任务委托给了你。

### 输入描述:

​输入的第一行是两个被空格隔开的正整数 $N,M(1\leq N,M\leq 10^5)$。分别表示别墅建筑群建筑的数目以及要来 wby 家拜访的生意伙伴的数目。

​输入的第二行包含 $N$ 个用空格隔开的整数 $T_1,T_2,...,T_N$。第 $i$ 个建筑的建筑风格为 $T_i$。

​接下来的 $N-1$ 行，每行两个不同的整数 $X,Y(1\leq X,Y\leq N)$，描述一条连接建筑 $x$ 和建筑 $y$ 的道路。

​接下来的 $M$ 行，每行包含三个整数 $A_i,B_i,C_i$。含义见题面。

### 输出描述:

​输出一个长为 $M$ 的二进制字符串，如果第 $i$ 位朋友会感到高兴，则字符串的第 $i$ 个字符为 1， 否则为 0。

### 测试样例:

#### 【样例1输入】

```
5 5
1 1 2 1 2
1 2
2 3
2 4
1 5
1 4 1
1 4 2
1 3 2
1 3 1
5 5 1
```

#### 【样例1输出】

```
10110
```

#### 【样例2输入】

```
6 4
1 2 3 3 3 3
1 2
2 3
3 4
2 5
5 6
4 6 1
4 6 2
4 6 3
4 6 4
```

#### 【样例2输出】

```
0110
```

### 数据范围与提示:

| 子任务 | 分数 |   特殊性质   |
| :----: | :--: | :----------: |
|   1    |  30  | $N\leq 1000$ |
|   2    |  30  | $C_i\leq 10$ |
|   3    |  40  |  无特殊性质  |

### 题目分析:

​设 $ax_{i,0}$ 为离 $x_i$ 最近的且建筑风格为 $c_i$ 的城市编号，$ax_{i,1}$ 为离 $y_i$ 最近的且建筑风格为 $c_i$ 的城市编号。由此我们易知若 $ax_{i,0}$ 和 $ax_{i,1}$ 中至少有一个点在 $lca(x_i,y_i)$ 以下的话，那么这个询问的答案就为1，否则就为0。

​现在的问题是如何求出 $ax$ 数组？

​一遍 DFS 即可，顺便就把 LCA 给初始化一下。记录一个数组 $res_i$ 表示对于 DFS 扫到的当前的点 $now$ 离他最近的且建筑风格为 $i$ 的城市编号。那么当输入询问的时候，我们就可以顺手记录我们需要对于当前节点 $now$ 的 $res$ 数组的哪些信息，并且在扫到这个节点时将我们所需要的信息给记录到 $ax$ 数组里去。

​时间复杂度为 $O(M\log N)$。

### AC代码:

```c++
#include<bits/stdc++.h>
#define van int
#define ywhin cin
#define ywhout cout
using namespace std;
bool ppppp;
const van N=100010;
vector<van> g[N];
vector<pair<van,van> > request[N];
van n,m;bool used[N];
van a[N],c[N],ax[N][2];
van x[N],y[N];
van tmp[N],ans[N];
van deep[N],up[N][20]; 
bool pppppp;
void DFS(van now=1) {
	van backup=tmp[a[now]];
	tmp[a[now]]=now;used[now]=1;
	for (int i=0;i<request[now].size();i++) {
		pair<van,van> id=request[now][i];
		ax[id.first][id.second]=tmp[c[id.first]];
	}//将我们所需要的信息记录到ax数组里去
	for (int i=0;i<g[now].size();i++) {
		if (!used[g[now][i]]) {
			deep[g[now][i]]=deep[now]+1;
			up[g[now][i]][0]=now;
			DFS(g[now][i]);
		}
	}
	tmp[a[now]]=backup;
}//DFS计算ax数组
van LCA(van x,van y) {
	if (deep[x]<deep[y]) swap(x,y);
	for (int i=19;i>=0;i--)if (deep[up[x][i]]>=deep[y]) x=up[x][i];
	if (x==y) return x;
	for (int i=19;i>=0;i--) {
		if (up[x][i]!=up[y][i]) {
			x=up[x][i],y=up[y][i];
		}
	}
	return up[x][0];
}//求LCA
int main() {
	ywhin>>n>>m;
	for (int i=1;i<=n;i++) {
		ywhin>>a[i];
	}
	for (int i=1;i<n;i++) {
		van f,s;
		ywhin>>f>>s;
		g[f].push_back(s);
		g[s].push_back(f);
	}//建边
	for (int i=1;i<=m;i++) {
		ywhin>>x[i]>>y[i]>>c[i];
		request[x[i]].push_back(make_pair(i,0));
		request[y[i]].push_back(make_pair(i,1));
	}//记录我们所需要的信息
	deep[1]=1;
	DFS();
	for (int j=1;j<=19;j++) {
		for (int i=1;i<=n;i++) {
			up[i][j]=up[up[i][j-1]][j-1];
		}
	}//初始化LCA
	for (int i=1;i<=m;i++) {
		if (deep[ax[i][1]]==0&&deep[ax[i][0]]==0) ywhout<<0;
		else ywhout<<(deep[ax[i][0]]>=deep[LCA(x[i],y[i])]||deep[ax[i][1]]>=deep[LCA(x[i],y[i])]);
	}//输出答案
}
```

