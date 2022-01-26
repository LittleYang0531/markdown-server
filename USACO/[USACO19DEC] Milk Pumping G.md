## [USACO19DEC] Milk Pumping G 做题记录

### 题目描述:

​JK 手下，一个庞大的物流网络正在 C 市飞速建立起来，C 市内部有 $N$ 个物流集散点。现在 JK 需要开辟一条物流通路，连接 C 市转运中心（1号集散点）以及公司（$N$号集散点）。 

​集散点之间存在 $M$ 条道路，开辟第 $i$ 条道路作为运输道路需要花费 $c_i$ 的代价，同时物流流量为 $f_i$。为了节约成本，JK 希望开辟若干条道路形成一条路径，使得路径的两端分别为 1 号集散点以及 $N$ 号集散点。开辟这条路径的花费就是路径上所有道路的花费的和，这条路径的流量则为路径上所有道路中流量最小的道路的物流流量。 

​JK 想要最大化路径流量与路径花费的比值，保证存在从 1 号集散点到 $N$ 号集散点之间的路径。

### 输入描述:

​输入的第一行包含两个正整数 $N,M(1\leq N,M\leq 1000)$，分别表示集散点的数目以及道路的数目。 

​接下来的 $M$ 行，每行以四个正整数 $a,b,c,f(1\leq a,b\leq N,1\leq c,f\leq 1000)$ 来描述一条道路。一条连接 $a$ 号集散点和 $b$ 号集散点的，花费为 $c$，物流流量为 $f$ 的道路。

### 输出描述:

​输出 $10^6$ 乘以最优解的值并向下取整（也就是说，将答案乘 $10^6$ 之后取整数部分）。

### 测试样例:

#### 【样例1输入】

```
3 2
2 1 2 4
2 3 5 3
```

#### 【样例1输出】

```
428571
```

#### 【样例1解释】

​在这个例子中，仅仅存在一条路径从1到 $N$。它的流量为 $\min(3,4)=3$，花费为 $2+5=7$。

### 数据范围与提示:

| 子任务 | 分数 |        特殊性质        |
| :----: | :--: | :--------------------: |
|   1    |  20  |   $1\leq N,M\leq 50$   |
|   2    |  30  | $N=M$ 并且任意两点联通 |
|   3    |  50  |       无特殊性质       |

### 题目分析:

​最暴力的做法: DFS。

​直接从 1 号集散点开始扫描，直到扫到了 $N$ 号集散点后计算 $\frac{f}{c}$ 并与答案进行比较就行了。

​当然直接这样搞肯定是要挂的，我们可以在中间剪一下枝。考虑扫到了某个点且 $\frac{f}{c}\leq ans$ 的时候，沿着这条路继续扫下去得到的结果一定不比 $ans$ 优。为什么? 越往后面扫，$f$ 的值一定会小于等于当前点时的 $f$ 值，$c$ 的值一定会大于等于当前点时的 $c$ 值。因此可以根据这条性质进行剪枝。

​时间复杂度为 $O(???)$。(太玄学了算不出来，但比跑 $M$ 次最短路要快)

### AC代码:

```c++
#include<bits/stdc++.h>
#define van long long
#define ywhin cin
#define ywhout cout
using namespace std;
bool ppppp;
const van N=1010;
struct node {
	van nid;
	double f,c;
};
vector<node> g[N];
bool used[N];
double ans;
van n,m;
bool pppppp;
void DFS(van now,double f,double c) {
	if (c>0&&f/c<=ans) return;//剪枝
	if (now==n) {
		ans=max(ans,f/c);
		return;
	}
	used[now]=1;
	for (int i=0;i<g[now].size();i++) {
		node nxt=g[now][i];
		if (!used[nxt.nid]) {
			DFS(nxt.nid,min(f,nxt.f),c+nxt.c);
		}
	}
	used[now]=0;
}//普普通通的DFS
int main() {
	ywhin>>n>>m;
	for (int i=1;i<=m;i++) {
		van now,nid;double f,c;
		ywhin>>now>>nid>>c>>f;
		g[now].push_back((node){nid,f,c});
		g[nid].push_back((node){now,f,c});
	}
	DFS(1,1e9,0);
	ywhout<<(van)(ans*1e6)<<endl;
}
```

