[Pixiv: 87020534]: 'https://cdn.jsdelivr.net/gh/LittleYang0531/Photo/87020534_p0.jpg'

## [USACO18DEC] Fine Dining G 做题记录

### 题目描述:

​漫长的一天结束了，作为牧民的小 P 需要把他家的牛群赶回牛棚，牧场一共有 $N$ 片草地，编号为 $1,2,...,N$。牛棚位于 $N$ 号草地， 其余 $N-1$ 片草地上都有一些牛群。牛群们可以通过 $M$ 条无向的小路在草地之间移动。第 $i$ 条小路连接草地 $a_i$ 和 $b_i$，通过这条小路需要的时间为 $t_i$，保证每一片草地上的牛群都可以通过若干条路回到牛棚。 

​由于饥饿，牛群会希望在他们回家的路上停留一段时间用于觅食，整片牧场上有 $K$ 个美味的干草堆，第 $i$ 个干草堆的美味程度为 $y_i$。每头牛可能会选择在会牛棚的路上的某一个干草堆旁停留，一头牛会在干草堆 $i$ 处停留，**当且仅当先到达干草堆 $i$ 所在的草场再回到牛棚的时间相比直接回到牛棚的时间**，增长不会超过这个干草堆的美味值。 

​需要注意一点，一头牛只会再一个干草堆处停留，如果此前它已经再一个干草堆处停留过了，那么它就不会再一次停留。干草堆不会因为牛群的停留而消失(你可以认为干草堆的大小是无限的)。

### 输入描述:

​输入的第一行包括三个用空格分隔的整数 $N,M$ 和 $K(2\leq N\leq 5\times 10^4,1\leq M \leq 10^5,K\leq 10^4)$。

​接下来的 $M$ 行，每行三个整数 $a_i,b_i$ 和 $t_i(a_i\not=b_i,0\leq t_i\leq 10^4)$。表示一条连接 $a_i$ 和 $b_i$ 的小路，通过它需要花费的时间为 $t_i$。

​接下来的 $K$ 行，每行以两个整数描述一个干草堆，第一个整数 $c_i$ 表示第 $i$ 捆干草堆所在的草地，第二个整数 $y_i(1\leq y_i\leq 10^9)$ 表示它的美味值。

### 输出描述:

​输出包括 $N-1$ 行，如果草地 $i$ 上的牛群可以在回牛棚的路上前往某一个干草堆并在此进食， 则第 $i$ 行输出一个整数1，否则为一个整数0。

### 测试样例:

#### 【样例1输入】

```
4 5 1
1 4 10
2 1 20
47 2 3
2 3 5
4 3 2
2 7
```

#### 【样例1输出】

```
1
1
1
```

#### 【样例1解释】

​对于草地1上的牛群，可以先到达牛棚，再从牛棚出发到达草地2进食，再回到牛棚，增加的路径长度为6，小于干草堆的美味值7。对于草地2上的牛群，可以不增加额外的路径吃到干草堆。对于草地3上的牛群，可以先到草地2，再到草地4，路径长度增加了6，小于美味值。

### 数据范围与提示:

| 子任务 | 分数 |             特殊性质             |
| :----: | :--: | :------------------------------: |
|   1    |  30  |           $N\leq 400$            |
|   2    |  30  | $K\leq 400,M\leq 1.5\times 10^4$ |
|   3    |  40  |            无特殊性质            |

### 题目分析:

​分析题目，此题需要对 1~N-1 的某一点 $i$ 找到一个带有干草堆的点 $j$，使得 $d_{j,n}+d_{i,j}-d_{i,n}\leq y_j$。其中 $d_{i,j}$ 表示 $i$ 到 $j$ 的最小距离。

​对于式子中 $d_{i,n}$ 和 $d_{j,n}$ 的值，只需要以 $n$ 为起点对于每一个点做一遍最短路的问题。

​接下来我们可以转换一下上面的式子: $d_{j,n}+d_{i,j}-d_{i,n}\leq y_j$ 即 $d_{j,n}+d_{i,j}-y_j\leq d_{i,n}$。

​那么现在的问题就是对于某个点 $i$，找到一个带有干草堆的点 $j$，使得 $d_{j,n}+d_{i,j}-y_j\leq d_{i,n}$。

​我们可以考虑再做一遍最短路，将所有带有干草堆的点到某个点 $i$ 的 $d_{j,n}+d_{i,j}-y_j$ 的值的最小值作为点 $i$ 的路径长度 $d2_i$，并按照正常的最短路做法去做就行了。只不过这一遍最短路的初始节点中有多个点，即所有带有干草堆的点，然后再用这些带有干草堆的点一步步去推到正常的点就行了，推导步骤与第一遍最短路完全一样。

​最后循环扫一遍所有的点看 $d2_i$ 是否小于等于 $d_{i,n}$ 就行了。

​时间复杂度为 $O(N\log N)$。

### AC代码:

```C++
#include<bits/stdc++.h>
#define ywhin cin
#define ywhout cout
#define N 60010
#define van long long
using namespace std;
vector<pair<van,van> > g[N];
van d[N],d2[N],add[N];bool used[N];
van n,m,k;bool ans[N];
//ifstream ywhin("dining.in");
//ofstream ywhout("dining.out");
void minimal_path() {
	memset(d,0x3f,sizeof d);
	priority_queue<pair<van,van> > q;
	q.push(make_pair(0,n));
	d[n]=0;
	while (!q.empty()) {
		van eid=q.top().second;q.pop();
		if (used[eid]) continue;
		used[eid]=1;
		for (int i=0;i<g[eid].size();i++) {
			van nid=g[eid][i].first;
			if (d[nid]>d[eid]+g[eid][i].second&&!used[nid]) {
				d[nid]=d[eid]+g[eid][i].second;
				q.push(make_pair(-d[nid],nid));
			}
		}
	}
}//第一遍最短路
void minimal_path2() {
	memset(used,0,sizeof used);
	memset(d2,0x3f,sizeof d2);
	priority_queue<pair<van,van> > q;
	for (int i=1;i<=k;i++) {
		van c,y;
		ywhin>>c>>y;
		q.push(make_pair(y-d[c],c));
		d2[c]=d[c]-y;//计算d2数组
	}
	while (!q.empty()) {
		van now=q.top().second;q.pop();
		if (used[now]) continue;
		used[now]=1;
		for (int i=0;i<g[now].size();i++) {
			van nxt=g[now][i].first,w=g[now][i].second;
			if (!used[nxt]&&d2[nxt]>d2[now]+w) {
				d2[nxt]=d2[now]+w;
				q.push(make_pair(-d2[nxt],nxt));
			}
		}
	}
}//第二遍最短路
int main() {
	ywhin>>n>>m>>k;
	for (int i=1;i<=m;i++) {
		van f,s,w;
		ywhin>>f>>s>>w;
		g[f].push_back(make_pair(s,w));
		g[s].push_back(make_pair(f,w));
	}//建图
	minimal_path();
	minimal_path2();
	for (int i=1;i<n;i++) {
		ywhout<<(d[i]>=d2[i])<<endl;
	}
}
```

