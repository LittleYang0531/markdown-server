# Ynoi 做题记录

## 提高+/省选-

### P6328 我是仙人掌

#### 题目简述

珂朵莉给你一个无向图，每次查询的时候给一堆二元组 $(x_i,y_i)$。

求图中有多少个点 $u$ 与至少一个这次询问给出的二元组 $(x_i,y_i)$ 满足 $\mathrm{dist}(u,x_i)\leq y_i$，$\mathrm{dist}$ 表示这两个点在图中的距离。

如果不连通 $\mathrm{dist} = +\infty$。

#### 输入描述

第一行三个整数表示 $n,m,q$。

$n$ 表示顶点个数，$m$ 表示边数。

之后 $m$ 行每行两个整数 $x,y$ 表示这两个点之间连有一条边~，边权都为 1。

之后 $q$ 次询问，每个询问先给你一个整数 $a$。

之后 $a$ 行每行两个整数，$x,y$，表示一个二元组。

#### 输出描述

$q$ 行，每行一个数表示这次询问的答案。

#### 数据范围

$1\leq n\leq 1000$，$1\leq m,q\leq 10^5$，$\sum a\leq 2.1\times 10^6$。

#### 题目分析

由于此题 $n$ 数据范围较小，考虑直接使用 Dijkstra 进行最短路搜索，时间复杂度 $O(n^2\log n)$。

使用 bitset 记录对于某一个点 $x$ 离他距离等于 $y$ 的点集，再用一遍前缀和计算小于等于 $y$ 的点集，时间复杂度为 $\frac{n^3}{w}$。

最后查询的时候直接或运算合并就行了，时间复杂度为 $\frac{\sum a\times n}{w}$。

#### 参考代码

```c++
#include<bits/stdc++.h>
using namespace std; 
typedef long long van;
template<typename T> inline 
void read(T& x) {
	T f=1,b=0;char ch=getchar();
	while (ch==' '||ch=='\n'||ch=='\r') ch=getchar();
	if (ch=='-') f=-1,ch=getchar();
	while (ch!=' '&&ch!='\n'&&ch!='\r') b*=10,b+=ch-'0',ch=getchar();
	x=f*b;return;
}
template<typename T> inline 
void print(T x) {
	if (x<0) putchar('-'),x=-x;
	if (x>9) print(x/10);putchar(x%10+'0');
} 
const van MaxN=1e3+10;
const van MaxQ=3e6+10;
van n,m,t;
vector<van> g[MaxN];
bitset<MaxN> f[MaxN][MaxN];
bool used[MaxN];van d[MaxN];
van x[MaxQ],y[MaxQ];
void Dijkstra(van x) {
	memset(used,0,sizeof used);
	memset(d,(1<<6),sizeof d);
	priority_queue<pair<van,van> > q;
	q.push(make_pair(0,x));d[x]=0;
	while (!q.empty()) {
		van id=q.top().second;q.pop();
		if (used[id]) continue;used[id]=1;
		for (int i=0;i<(van)g[id].size();i++) {
			if (d[g[id][i]]>d[id]+1) {
				d[g[id][i]]=d[id]+1;
				q.push(make_pair(-d[g[id][i]],g[id][i]));
			} 
		}
	}
	for (int i=1;i<=n;i++) if (d[i]<=n) f[x][d[i]][i]=1;
	for (int i=1;i<=n;i++) f[x][i]|=f[x][i-1];return;
}
signed main() {
	read(n),read(m),read(t);
    for (int i=1;i<=m;i++) {
		van u,v;read(u),read(v);
		g[u].push_back(v);
		g[v].push_back(u); 
	}
	for (int i=1;i<=n;i++) Dijkstra(i);
	for (int tt=1;tt<=t;tt++) {
		van q;read(q);
		for (int i=1;i<=q;i++) read(x[i]),read(y[i]);
		bitset<MaxN> res;res.reset();
		for (int i=1;i<=q;i++) res|=f[x[i]][y[i]];
		print(res.count());putchar('\n');
	}
}
```

### P6327 区间加区间sin和

#### 题目简述

给出一个长度为 $n$ 的整数序列 $a_1,a_2,\ldots,a_n$，进行 $m$ 次操作，操作分为两类。

操作 1：给出 $l,r,v$，将 $a_l,a_{l+1},\ldots,a_r$ 分别加上 $v$。

操作 2：给出 $l,r$，询问 $\sum\limits_{i=l}^{r}\sin(a_i)$。

#### 输入描述

第一行一个整数 $n$。

接下来一行 $n$ 个整数表示 $a_1,a_2,\ldots,a_n$。

接下来一行一个整数 $m$。

接下来 $m$ 行，每行表示一个操作，操作 1 表示为 `1 l r v`，操作 2 表示为 `2 l r`。

#### 输出描述

对每个操作 2，输出一行，表示答案，四舍五入保留一位小数。

保证答案的绝对值大于 0.1，且答案的准确值的小数点后第二位不是 4 或 5。

#### 数据范围

$1\leq n,m,a_i,v\leq 2\times 10^5$，$1\leq l\leq r\leq n$。

#### 题目分析

前置知识:

$\sin(a+x)=\sin a\times \cos x+\cos a\times \sin x$，

$\cos(a+x)=\cos a\times\cos x+\sin a\times\sin x$。

根据以上公式，我们可以开两棵线段树进行维护。一棵维护区间范围内 $\sum \sin(a_i)$，一棵维护区间范围内 $\sum \cos(a_i)$。

那么 $\sum\limits_{i=l}^r\sin (a_i+x)=\sum\limits_{i=l}^r [\sin(a_i)\times\cos(x)+\cos(a_i)\times\sin (x)]=\cos(x)\times\sum\limits_{i=l}^r\sin(a_i)+\sin(x)\times\sum\limits_{i=l}^r\cos(a_i)$。

还有$\sum\limits_{i-l}^r\cos(a_i+x)=\sum\limits_{i=l}^r[\cos(a_i)\times\cos(x)+\sin(a_i)\times\sin(x)]=\cos(x)\times\sum\limits_{i=l}^r\cos(a_i)+\sin(x)\times\sum\limits_{i=l}^r\sin(a_i)$。

于是我们就可以很轻易地使用线段树来进行区间修改和查询了，时间复杂度为 $O(n\log n)$。

#### 参考代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long van;
template<typename T> inline
void read(T& x) {
	T f=1,b=0;char ch=getchar();
	while (ch==' '||ch=='\n'||ch=='\r') ch=getchar();
	if (ch=='-') f=-1,ch=getchar();
	while (ch!=' '&&ch!='\n'&&ch!='\r') b*=10,b+=ch-'0',ch=getchar();
	x=f*b;return;
}
template<typename T> inline
void print(T x) {
	if (x<0) putchar('-'),x=-x;
	if (x>9) print(x/10);putchar(x+'0');
}
const van MaxN=2e5+10;
van n,a[MaxN],m;
struct SegmentTree {
	double sindata[MaxN<<3];
	double cosdata[MaxN<<3];
	van lazytag[MaxN<<3];
	void Initiative() {
		BuildTree();
		memset(lazytag,0,sizeof lazytag);
	}
	void Addition(van id,van add) {
		double sinsum=sindata[id],cossum=cosdata[id];
		sindata[id]=sinsum*cos(add)+cossum*sin(add);
		cosdata[id]=cossum*cos(add)-sinsum*sin(add);
	}
	void BuildTree(van p=1,van l=1,van r=n) {
		if (l==r) {
			sindata[p]=sin(a[l]);
			cosdata[p]=cos(a[l]);
			return;
		}
		van mid=(l+r)>>1;
		BuildTree(p*2,l,mid);
		BuildTree(p*2+1,mid+1,r);
		sindata[p]=sindata[p*2]+sindata[p*2+1];
		cosdata[p]=cosdata[p*2]+cosdata[p*2+1];
		return;
	}
	void spread(van p) {
		if (lazytag[p]) {
			Addition(p*2,lazytag[p]);
			Addition(p*2+1,lazytag[p]);
			lazytag[p*2]+=lazytag[p];
			lazytag[p*2+1]+=lazytag[p];
			lazytag[p]=0;
		}
	}
	void AddTree(van L,van R,van x,van p=1,van l=1,van r=n) {
		if (L<=l&&r<=R) {
			Addition(p,x);lazytag[p]+=x;
			return;
		}
		spread(p);
		van mid=(l+r)>>1;
		if (L<=mid) AddTree(L,R,x,p*2,l,mid);
		if (R>mid) AddTree(L,R,x,p*2+1,mid+1,r);
		sindata[p]=sindata[p*2]+sindata[p*2+1];
		cosdata[p]=cosdata[p*2]+cosdata[p*2+1];
	}
	double QueryTree(van L,van R,bool __sin,van p=1,van l=1,van r=n) {
		if (L<=l&&r<=R) return __sin?sindata[p]:cosdata[p];
		spread(p);van mid=(l+r)>>1;double sum=0;
		if (L<=mid) sum+=QueryTree(L,R,__sin,p*2,l,mid);
		if (R>mid) sum+=QueryTree(L,R,__sin,p*2+1,mid+1,r);
		return sum;
	}
}solve;
signed main() {
	read(n);for (int i=1;i<=n;i++) read(a[i]);
	solve.Initiative();
	read(m);for (int i=1;i<=m;i++) {
		van op,l,r,v;read(op);
		if (op==1) {
			read(l),read(r),read(v);
			solve.AddTree(l,r,v);
		}
		else {
			read(l),read(r);
			printf("%.1f\n",solve.QueryTree(l,r,1));
		}
	}
}
```

## 省选/NOI-

### P5354 [Ynoi2017] 由乃的 OJ

#### 题目简述

给你一个有 $n$ 个点的树，每个点的包括一个位运算 $opt$ 和一个权值 $x$，位运算有 `&`，`|`，`^` 三种，分别用 $1,2,3$ 表示。

每次询问包含三个整数 $x,y,z$，初始选定一个数 $v$。然后 $v$ 依次经过从 $x$ 到 $y$ 的所有节点，每经过一个点 $i$，$v$ 就变成 $v\ opt_i\ x_i$，所以他想问你，最后到 $y$ 时，希望得到的值尽可能大，求最大值。给定的初始值 $v$ 必须是在 $[0,z]$ 之间。

每次修改包含三个整数 $x,y,z$，意思是把 $x$ 点的操作修改为 $y$，数值改为 $z$。

#### 输入描述

第一行三个整数 $n,m,k$。$k$ 的意义是每个点上的数，以及询问中的数值 $z$ 都小于 $2^k$。

之后 $n$ 行，每行两个整数 $x,y$ 表示该点的位运算编号以及数值。

之后 $n-1$ 行，每行两个数 $x,y$ 表示 $x$ 和 $y$ 之间有边相连。

之后 $m$ 行，每行四个数，$Q,x,y,z$ 表示这次操作为 $Q$（1 为询问，2 为修改），$x,y,z$ 意义如题所述。

#### 输出描述

对于每个操作 1，输出到最后可以得到的最大值。

#### 数据范围

对于 30% 的数据，$n,m\leq 1$。

对于另外 20% 的数据，$k\leq 5$。

对于另外 20% 的数据，位运算只会出现一种。

对于 100% 的数据，$0\leq n,m \leq 10^5$，$0\leq k\leq 64$。

#### 题目分析

读题，树上问题与单点修改，区间查询的结合，让人自然而然地想到树链剖分与线段树的结合。

先将整棵树树链剖分，然后给每个重链建一棵线段树，维护某个重链区间内 DFS 访问时间戳为 $dfn_l$ 到 $dfn_r$ 的运算结果，其中 $dep_l<dep_r$

先考虑查询操作，对于一个从 $x$ 到 $y$ 的查询，先对两个点查找其最近公共祖先，在查找的过程中就可以顺便将路上的运算结果全部**合并**在一块。但是要注意左节点的运算结果每次合并时需要调换过来。

再考虑修改操作，对于某个区间 $dfn_l$ 到 $dfn_r$，设 $ls$ 为它的左子树，$rs$ 为它的右子树，那么这个区间的运算结果就是 $ls$ 的运算结果**合并**上 $rs$ 的运算结果就行了。

大致思路理解了，接下来我们需要解决的一个问题就是运算结果的合并。

参考 [P2114 [NOI2014] 起床困难综合症 - 洛谷 | 计算机科学教育新生态](https://www.luogu.com.cn/problem/P2114)

对于某一个线段树的区间，我们需要求出二进制某一位为 1 时经过运算后的结果和某一位为 0 时经过运算后的结果。由于二进制运算每一位都不会受其它位的影响，因此我们可以考虑直接把一个二进制下为 111...111 和二进制下为 000...000 的数拿进去算，最后获取某一位运算的结果时，只需要获取相对应的数拿进去算的结果的那一位上所对应的数就行了。例如要获取第 3 位初始值为 1 的数经过这些运算后的结果，只需要获取到 111...111 运算结果的第三位就行了，以此类推。

设一个数 111...111 经过左区间后第 $i$ 位的运算结果为 $f_1[i]$，一个数 000...000 经过左区间后第 $i$ 位的运算结果为 $f_0[i]$，一个数 111...111 经过右区间后第 $i$ 位的运算结果为 $g_1[i]$，一个数 000...000 经过右区间后第 $i$ 位的运算结果位 $g_0[i]$。

以一个数 000...000 先经过左区间再经过右区间为例，可以分为以下四种情况:

$f_0[i]=1\ \mathrm{and}\ g_1[i]=1\rightarrow F_0[i]=1$

$f_0[i]=0\ \mathrm{and}\ g_0[i]=1\rightarrow F_0[i]=1$

$f_0[i]=1\ \mathrm{and}\ g_1[i]=0\rightarrow F_0[i]=0$

$f_0[i]=0\ \mathrm{and}\ g_0[i]=0\rightarrow F_0[i]=0$

因此，我们可以推出 $F_0[i]=(f_0[i]\ \mathrm{and}\ g_1[i])\ \mathrm{or}\ (!f_0[i]\ \mathrm{and}\ g_0[i])$。

并以此推广到所有的 64 位 $F_0=(f_0\ \mathrm{and}\ g_1)\ \mathrm{or}\ (!f_0\ \mathrm{and}\ g_0)$。

以一个数 111...111 先经过左区间再经过右区间同理，并以此推广到从右区间往左区间的结果的合并方法。

于是这个题我们就可以使用线段树来进行维护了。

时间复杂度为 $O(n\log^2 n)$，但实现起来恶心至极。

#### 参考代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long van;
// 快读快写 
template<typename T> inline
void read(T& x) {
	T f=1,b=0;char ch=getchar();
	while (ch==' '||ch=='\n'||ch=='\r') ch=getchar();
	if (ch=='-') f=-1,ch=getchar();
	while (ch!=' '&&ch!='\n'&&ch!='\r') 
		b*=10,b+=ch-'0',ch=getchar();
	x=f*b;return;
}
template<typename T> inline 
void print(T x) {
	if (x<0) putchar('-'),x=-x;
	if (x>9) print(x/10);putchar(x%10+'0');
}
// 基础数据 
const van MaxN=1e5+10;
const van Default=114514;
van opt[MaxN],x[MaxN];
vector<van> graph[MaxN];
van n,m,k;bool used[MaxN];
// 树链剖分 
van siz[MaxN],dep[MaxN],fa[MaxN],wson[MaxN];
van belong[MaxN],dfn[MaxN],backup[MaxN],len[MaxN],cnt=0;
void DFS1(van now=1) {
	used[now]=1,siz[now]=1,wson[now]=0;
	for (int i=0;i<graph[now].size();i++) {
		if (!used[graph[now][i]]) {
			fa[graph[now][i]]=now;
			dep[graph[now][i]]=dep[now]+1;
			DFS1(graph[now][i]);
			siz[now]+=siz[graph[now][i]];
			if (siz[wson[now]]<siz[graph[now][i]]) wson[now]=graph[now][i];
		}
	}
}
void DFS2(van now=1,van bel=1) {
	belong[now]=bel;
	dfn[now]=++cnt;used[now]=1,backup[cnt]=now;
	if (wson[now]) DFS2(wson[now],bel);
	for (int i=0;i<graph[now].size();i++) {
		if (!used[graph[now][i]]) {
			DFS2(graph[now][i],graph[now][i]);
		}
	}
}
// 线段树 
struct SegmentTree {
	struct d {
		van ls0,ls1,rs0,rs1;
		d(van ls0=0,van ls1=-1,van rs=0,van rs1=-1):
			ls0(ls0),ls1(ls1),rs0(rs0),rs1(rs1){};
	}data[MaxN*5];
	van ls[MaxN*5],rs[MaxN*5],root[MaxN*5],cnt=0;
	van CalcTree(van a,van op,van b) {
		switch(op) {
			case 1: return a&b;break;
			case 2: return a|b;break;
			case 3: return a^b;break;
			default: return Default;break;
		}
		return Default;
	}
	d Update(d ls,d rs) {
		d res=d();
		res.ls0=(((~ls.ls0)&rs.ls0)|(ls.ls0&rs.ls1));
		res.ls1=(((~ls.ls1)&rs.ls0)|(ls.ls1&rs.ls1));
		res.rs0=(((~rs.rs0)&ls.rs0)|(rs.rs0&ls.rs1));
		res.rs1=(((~rs.rs1)&ls.rs0)|(rs.rs1&ls.rs1));
		return res;
	}
	van BuildTree(van l,van r) {
		van nowid=++cnt;
		if (l==r) {
			data[nowid].ls0=CalcTree(0,opt[backup[l]],x[backup[l]]);
			data[nowid].ls1=CalcTree(-1,opt[backup[l]],x[backup[l]]);
			data[nowid].rs0=CalcTree(0,opt[backup[l]],x[backup[l]]);
			data[nowid].rs1=CalcTree(-1,opt[backup[l]],x[backup[l]]);
			return nowid;
		}van mid=(l+r)>>1;
		ls[nowid]=BuildTree(l,mid);rs[nowid]=BuildTree(mid+1,r);
		data[nowid]=Update(data[ls[nowid]],data[rs[nowid]]);
		return nowid;
	}
	void ChangeTree(van p,van l,van r,van id,van op,van num) {
		if (l==r) {
			opt[backup[l]]=op,x[backup[l]]=num;
			data[p].ls0=CalcTree(0,opt[backup[l]],x[backup[l]]);
			data[p].ls1=CalcTree(-1,opt[backup[l]],x[backup[l]]);
			data[p].rs0=CalcTree(0,opt[backup[l]],x[backup[l]]);
			data[p].rs1=CalcTree(-1,opt[backup[l]],x[backup[l]]);
			return;
		}
		van mid=(l+r)>>1;
		if (id<=mid) ChangeTree(ls[p],l,mid,id,op,num);
		else ChangeTree(rs[p],mid+1,r,id,op,num);
		data[p]=Update(data[ls[p]],data[rs[p]]);return;
	}
	d QueryTree(van p,van l,van r,van L,van R) {
		if (L>R) return d();
		if (L<=l&&r<=R) return data[p];
		van mid=(l+r)>>1;d lres,rres;
		bool lused=false,rused=false;
		if (L<=mid) lres=QueryTree(ls[p],l,mid,L,R),lused=1;
		if (R>mid) rres=QueryTree(rs[p],mid+1,r,L,R),rused=1;
		return (lused&&rused?Update(lres,rres):(lused?lres:rres));
	}
	void InitTree() {
		for (int i=1;i<=n;i++) {
			if (belong[backup[i]]==backup[i])
				root[backup[i]]=BuildTree(i,i+len[backup[i]]-1);
		}
	}
}T;
// 树链剖分求 LCA 
SegmentTree::d LCA(van u,van v) {
	struct SegmentTree::d ls=SegmentTree::d(),rs=SegmentTree::d();
	while (belong[u]!=belong[v]) {
		if (dep[belong[u]]>=dep[belong[v]]) {
			SegmentTree::d res=T.QueryTree(T.root[belong[u]],dfn[belong[u]],
							   dfn[belong[u]]+len[belong[u]]-1,dfn[belong[u]],dfn[u]);
			swap(res.ls0,res.rs0);swap(res.ls1,res.rs1);
			ls=T.Update(ls,res);
			u=fa[belong[u]];
		} else {
			rs=T.Update(T.QueryTree(T.root[belong[v]],dfn[belong[v]],
						dfn[belong[v]]+len[belong[v]]-1,dfn[belong[v]],dfn[v]),rs);
			v=fa[belong[v]];
		}
	}
	if (dep[u]>dep[v]) {
		SegmentTree::d res=T.QueryTree(T.root[belong[u]],dfn[belong[u]],
						   dfn[belong[u]]+len[belong[u]]-1,dfn[v],dfn[u]);
		swap(res.ls0,res.rs0);swap(res.ls1,res.rs1);
		ls=T.Update(ls,res);
	}
	else rs=T.Update(T.QueryTree(T.root[belong[v]],dfn[belong[v]],
					 dfn[belong[v]]+len[belong[v]]-1,dfn[u],dfn[v]),rs);
	return T.Update(ls,rs);
}
bitset<64> build(van x) {
	bitset<64> x0(x>>32),xx0(x%((van)1<<32));
	return ((x0<<32)|xx0);
}
van Query(van x,van y,van z) {
	SegmentTree::d lineres=LCA(x,y);
	bitset<64> l0=build(lineres.ls0),l1=build(lineres.ls1);
	van now=0,res=0;
	for (int i=k-1;i>=0;i--) {
		if (l0[i]==1) res+=((van)1<<i);
		else if (l1[i]==1&&now+((van)1<<i)<=z) res+=((van)1<<i),now+=((van)1<<i);
	}
	return res;
}
void Change(van x,van y,van z) {
	T.ChangeTree(T.root[belong[x]],dfn[belong[x]],
				 dfn[belong[x]]+len[belong[x]]-1,dfn[x],y,z);
	return;
}
int main() {
	read(n),read(m),read(k);for (int i=1;i<=n;i++) read(opt[i]),read(x[i]);
	for (int i=1;i<n;i++) {
		van u,v;read(u),read(v);
		graph[u].push_back(v);
		graph[v].push_back(u);
	}dep[1]=1;DFS1();memset(used,0,sizeof used);DFS2();
	for (int i=1;i<=n;i++) len[belong[backup[i]]]++;
	T.InitTree();for (int qq=1;qq<=m;qq++) {
		van op,x,y,z;read(op),read(x),read(y),read(z);
		if (op==1) print(Query(x,y,z)),putchar('\n');
		else Change(x,y,z);
	}
	return 0;
}
```

### P5607 [Ynoi2013] 无力回天 NOI2017

#### 题目简述

给你一个长度为 $n$ 的整数序列 $a_1,a_2,...,a_n$，你需要实现以下两种操作，每个操作都可以用四个整数 $opt,l,r,v$ 来表示：

$opt=1$ 时，代表把一个区间 $[l,r]$ 内的所有数都 xor 上 $v$。

$opt=2$ 时， 查询一个区间 $[l,r]$ 内选任意个数（包括 0 个）数 xor 起来，这个值与 $v$ 的最大 xor 和是多少。

#### 输入描述

第一行有两个正整数 $n,m$。 第二行有 $n$ 个整数表示给你的序列。 之后 $m$ 行每行有四个整数 $opt, l, r, v$ 表示一个操作。

#### 输出描述

对于每个 $opt=2$ 的操作，输出一行一个数表示答案~

#### 数据范围

$1\leq n,m\leq 5\times 10^4$。

#### 题目分析

对于解决最大 xor 和的问题，我们可以自然而然地想到使用线性基来解决。

参考链接: [线性基 - OI Wiki](https://oi-wiki.org/math/basis/)

对于区间修改和区间查询，可以考虑开一棵线段树来维护。

但是这个区间异或的修改就很难受，于是我们可以考虑使用线段树来维护其差分数组 $b$。

那么怎么维护原数组区间的线性基呢？我们知道，$a_l,a_{l+1},...,a_r$ 其实就是 $a_l,a_l\oplus b_{l+1},...,a_l\oplus b_{l+1}\oplus ...\oplus b_{r}$。

因此我们就可以得出 $a_l,a_{l+1},...,a_r$ 的线性基其实就是 $a_l,b_{l+1},...,b_r$ 的线性基。

通过这个性质，我们就可以直接用线段树来维护差分数组的线性基。

时间复杂度为 $O(n\log^3 n)$。

#### 参考代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long van;
template<typename T> inline 
void read(T& x) {
	T f=1,b=0;char ch=getchar();
	while (ch==' '||ch=='\n'||ch=='\r') ch=getchar();
	if (ch=='-') f=-1,ch=getchar();
	while (ch!=' '&&ch!='\n'&&ch!='\r') 
		b*=10,b+=ch-'0',ch=getchar();
	x=b;return;
}
template<typename T> inline
void print(T x) {
	if (x<0) putchar('-'),x=-x;
	if (x>9) print(x/10);putchar(x%10+'0');
}
const van MaxN=5e4+10;
const van MaxK=33;
van n,m,a[MaxN];
struct SegmentTree {
	struct xyx {
		van data[64]={0};
		void init() {memset(data,0,sizeof data);}
		xyx operator + (van num) {
			xyx res=*this;
			for (int i=MaxK;i>=0;i--) 
				if (num&(1ull<<i)) {
					if (!res.data[i]) {res.data[i]=num;break;}
					num^=res.data[i];
				}
			return res;
		}
		xyx operator + (xyx a) {
			xyx tmp=a,res=*this;
			for (int i=MaxK;i>=0;i--) if (tmp.data[i])
				for (int j=MaxK;j>=0;j--) 
					if (tmp.data[i]&(1ull<<j)) {
						if (!res.data[j]) {res.data[j]=tmp.data[i];break;}
						tmp.data[i]^=res.data[j];
					}
			return res;
		}
	};
	xyx ans[MaxN*5];
	void BuildTree(van p=1,van l=1,van r=n) {
		if (l==r) {
			ans[p].init();
			ans[p]=ans[p]+a[l];
			return;
		}
		van mid=(l+r)>>1;
		BuildTree(p*2,l,mid);
		BuildTree(p*2+1,mid+1,r);
		ans[p]=ans[p*2]+ans[p*2+1];
	}
	void UpdateTree(van where,van num,van p=1,van l=1,van r=n) {
		if (l==r) {
			ans[p].init();
			ans[p]=ans[p]+(a[l]^num);
			return;
		}
		van mid=(l+r)>>1;
		if (where<=mid) UpdateTree(where,num,p*2,l,mid);
		else UpdateTree(where,num,p*2+1,mid+1,r);
		ans[p]=ans[p*2]+ans[p*2+1];
		return;
	}
	xyx QueryTree(van L,van R,van p=1,van l=1,van r=n) {
		if (L<=l&&r<=R) {
			return ans[p];
		}
		van mid=(l+r)>>1;xyx res;res.init();
		if (L<=mid) res=res+QueryTree(L,R,p*2,l,mid);
		if (R>mid) res=res+QueryTree(L,R,p*2+1,mid+1,r);
		return res;
	}
}T;
struct TreeArray {
	van data[MaxN]={0};
	van lowbit(van x) {return x&-x;}
	void InsertTree(van where,van x) {
		while (where<=n) {
			data[where]^=x;
			where+=lowbit(where);
		}
	}
	van QueryTree(van where) {
		van res=0;
		while (where) {
			res^=data[where];
			where-=lowbit(where);
		}
		return res;
	}
	void BuildTree() {
		for (int i=1;i<=n;i++) InsertTree(i,a[i]);
	}
}TA;
void Update(van l,van r,van v) {
	TA.InsertTree(l,v),T.UpdateTree(l,v),a[l]^=v;
	if (r<n) TA.InsertTree(r+1,v),T.UpdateTree(r+1,v),a[r+1]^=v;
}
van Query(van l,van r,van v) {
	SegmentTree::xyx res=T.QueryTree(l+1,r);res=res+TA.QueryTree(l);
	for (int i=MaxK;i>=0;i--) v=max(v,res.data[i]^v);
	return v;
}
int main() {
	read(n),read(m);
	for (int i=1;i<=n;i++) read(a[i]);
	for (int i=n;i>1;i--) a[i]^=a[i-1];
	T.BuildTree();TA.BuildTree();
	for (int mm=1;mm<=m;mm++) {
		van op,l,r,v;read(op),read(l),read(r),read(v);
		if (op==1) Update(l,r,v);
		else print(Query(l,r,v)),putchar('\n');
	}
}
```

### P6105 [Ynoi2010] y-fast trie

#### 题目简述

给定一个常数 $C$，你需要维护一个集合 $S$，支持 $n$ 次操作：

- 操作1：给出 $x$，插入一个元素 $x$，保证之前集合中没有 $x$ 这个元素
- 操作2：给出 $x$，删除一个元素 $x$，保证之前集合中存在 $x$ 这个元素

每次操作结束后，需要输出 $\max\limits_{{i,j in S\ i\not= j}}\bigl((i+j)\bmod C\bigr)$，即从 $S$ 集合中选出两个不同的元素，其的和 $\bmod~C$ 的最大值，如果 $S$ 集合中不足两个元素，则输出 `EE`。

本题强制在线，每次的 $x$ 需要 $\operatorname{xor}$ 上上次答案 ，如果之前没有询问或者输出了 `EE`，则上次答案为 0。

#### 输入描述

第一行两个整数 $n,C$。

接下来 $n$ 行，每行有两个整数 $1\ x1$ 或 $2\ x2$，表示第一类或第二类操作。

#### 输出描述

输出 $n$ 行，第 $i$ 行表示第 $i$ 次操作结束后的答案。

#### 数据范围

对于其中 1% 的数据，为样例 1。

对于另外 9% 的数据，集合中元素个数 $\le 1$。

对于另外 19% 的数据，$n\leq 500$。

对于另外 19% 的数据，$n\leq 10^4$。

对于另外 19% 的数据，$1\leq n,C \leq 10^5$。

对于 100% 的数据，$1\leq n \leq 5\times 10^5$，$1\leq C\leq 1073741823$，$0\leq x\leq 1073741823$。

#### 题目分析

先将输入进来的数提前 $\bmod C$，毕竟大于等于 $C$ 的部分不会对最终的答案起到任何的作用。

考虑如何存这些数，由于可能会出现重复的数，因此我们可以使用 C++ STL multiset 来存储，顺便还能排个序，方便我们的 lower_bound 的操作。

对于所有的数，随便选两个加起来肯定到不了 $2\times C$，因此我们可以分成 $[0,C)$ 和 $[C,2\times C)$ 两个部分来进行分类讨论。

对于第二种情况，是个人都能看出来直接选最大的两个数就行了吧......

接下来我们考虑如何求出第一种情况的最大值。

先考虑暴力做法，直接在 multiset 中暴力查找另一个与之相加最接近 $C$ 但又达不到 $C$ 的数就行了，可以直接使用 lower_bound 来进行查找。时间复杂度会达到 $O(n^2\log n)$。

接下来我们考虑去掉一只 $\log$ 的做法。对于一个数 $a$，如果我们只与和它相加最接近 $C$ 的数配对，并且每个数只能和另外一个数配对，那么我们可以惊奇地发现，最多只会出现 $\frac{n}{2}$ 对数，并且每一对数中一个数的最优解一定是另一个数，且没有相交 (Ps: 可以自己试试看)。

于是我们每一次的修改只会对某一对数产生作用，那么我们就可以直接在 multiset 中找出是哪一对数以及做出对应的修改即可。

时间复杂度为 $O(n\log n)$。

#### 参考代码

```c++
#include<bits/stdc++.h>
using namespace std;
template<typename T> inline 
void read(T& x) {
	T f=1,b=0;char ch=getchar();
	while (ch==' '||ch=='\n'||ch=='\r') ch=getchar();
	if (ch=='-') f=-1,ch=getchar();
	while (ch!=' '&&ch!='\n'&&ch!='\r') 
		b*=10,b+=ch-'0',ch=getchar();
	x=f*b;return;
}
template<typename T> inline 
void print(T x) {
	if (x<0) putchar('-'),x=-x;
	if (x>9) print(x/10);putchar(x%10+'0');
}
typedef long long van;
van n,c,lans=0;
struct Treap {
	multiset<van> num,sum;
	inline van QueryDiff(van x) {
		if (x==-1) return -1;
		multiset<van>::iterator it=num.lower_bound(c-x);
		if (it==num.begin()) return -1;--it;
		if ((*it)==x&&num.count(x)==1) {
			if (it==num.begin()) return -1;
			else return *(--it);
		}
		return *it;
	}
	inline van QueryFree(van x) {
		if (x==-1) return -1;
		multiset<van>::iterator it=num.lower_bound(c-x);
		if (it==num.begin()) return -1;--it;
		return *it;
	}
	void Insert(van x) {
		x%=c;if (num.size()==0) {num.insert(x);return;}
		van tmp1=QueryFree(x),tmp2=QueryDiff(tmp1),tmp3=QueryDiff(tmp2);
		num.insert(x);if (tmp2<x&&tmp1!=-1) {
			if (tmp2!=-1&&tmp1==tmp3) 
				sum.erase(sum.find(tmp1+tmp2));
			sum.insert(x+tmp1);
		}
	}
	void Erase(van x) {
		num.erase(num.find(x));
		if (num.size()==0) return;
		van tmp1=QueryFree(x),tmp2=QueryDiff(tmp1),tmp3=QueryDiff(tmp2);
		if (tmp2<x&&tmp1!=-1) {
			if (tmp2!=-1&&tmp1==tmp3) 
				sum.insert(tmp1+tmp2);
			sum.erase(sum.find(x+tmp1));
		}
	}
	void Output() {
		if (num.size()<2){printf("EE\n");lans=0;}
		else {
			if (sum.size()==0) {
				van res1=((*--num.end())+(*--(--num.end())))%c;
				print(max(res1,(van)0));putchar('\n');lans=max(res1,(van)0);
			}
			else {
				van res1=((*--num.end())+(*--(--num.end())))%c;
				van res2=(*--sum.end());print(max(res1,res2));
				putchar('\n');lans=max(res1,res2);
			}
		}
	}
}T;
int main() {
	read(n),read(c);
	for (int nn=1;nn<=n;nn++) {
		van op,x;read(op),read(x),x^=lans,x%=c;
		if (op==1) T.Insert(x);
		else T.Erase(x);T.Output();
	}
}
```

### P6108 [Ynoi2009] rprsvq

#### 题目简述

你有一个长度为 $n$ 的序列 $v_1,v_2,...,v_n$，初值全都为 0。

你要进行 m 次操作：

- 操作 1：给出 $l,r,a$，将 $v_l,v_{l+1},...,v_r$ 的值加上 $a$。
- 操作 2：给出 $l,r$，求在 $v_l,v_{l+1},...,v_r$ 中选出一个非空的子序列，求所有方案中选出的子序列的方差之和，答案对 998244353 取模。

一个序列 $a_1,a_2,...,a_n$ 的方差定义为令 $\bar{a}=\frac{1}{n}\sum_{i=1}^n a_i$，则方差为 $\frac{1}{n}\sum_{i=1}^n (a_i-\bar{a})^2$。

#### 输入描述

第一行两个整数 $n,m$。

接下来 $m$ 行，每行有四个整数 $1,l,r,a$ 或三个整数 $2,l,r$，表示第一类或第二类操作，保证 $0\leq a< 998244353$。

#### 输出描述

对每个第二类操作，输出一行表示答案。

#### 数据范围

对于 10% 的数据，$n\leq 10, m\leq 1000$。

对于 20% 的数据，$n\leq 16, m\leq 1000$。

对于 30% 的数据，$n\leq 100, m\leq 10^3$。

对于 50% 的数据，$n, m\leq 10^3$。

对于另外10%的数据，$n\leq 10^5$，保证首先是第一类操作，然后是第二类操作。

对于 80% 的数据，$n, m\leq 10^5$。

对于 100% 的数据，$1\leq n \leq 5\times 10^6,1\leq m\leq 10^5$。

#### 题目分析

先化简一下方差计算式:

$\because \bar{a}=\frac{1}{n}\sum a_i$

$\therefore \sum a_i=n\times \bar{a}$

$\therefore s^2=\frac{1}{n}\sum(a_i-\bar{a})^2=\frac{1}{n}(\sum a_i^2-2\times\bar{a}\times\sum a_i+\bar{a}^2)=\frac{1}{n}(\sum a_i^2-2\times n\times\bar{a}^2+\bar{a}^2)=\frac{1}{n}(\sum a_i^2-n\times\bar{a}^2)$

$=\frac{1}{n}(\sum a_i^2-n\times (\frac{1}{n}\sum a_i)^2)=\frac{1}{n}(\sum a_i^2-\frac{1}{n}(\sum a_i)^2)=\frac{1}{n}(\sum a_i^2-\frac{1}{n}(\sum a_i^2+2\sum a_ia_j))$

$=\frac{1}{n}(\frac{n-1}{n}\sum a_i^2-\frac{2}{n}\sum a_ia_j)=\frac{n-1}{n^2}\sum a_i^2-\frac{2}{n^2}\sum a_ia_j$

$\therefore \sum s^2=\sum\limits_{s\subseteq  \{a_l,a_{l+1},...,a_r\}} (\frac{|s|-1}{|s|^2}\sum a_i^2-\frac{2}{|s|^2}\sum a_ia_j)=\sum\limits_{t=1}^n\frac{t-1}{t^2}\times \mathrm{C}_{n-1}^{t-1}\times\sum a_i^2-\sum\limits_{t=2}^n\frac{2\mathrm{C}_{n-2}^{t-2}}{t^2}\sum a_ia_j$

$=\sum\limits_{t=1}^n\frac{\mathrm{C}_{t-1}^1\times \mathrm{C}_{n-1}^{t-1}}{t^2}\times\sum a_i^2-\sum\limits_{t=2}^n\frac{2\mathrm{C}_{n-2}^{t-2}}{t^2}\sum a_ia_j=\sum\limits_{t=1}^n\frac{\mathrm{C}_{n-1}^1\times\mathrm{C}_{n-2}^{t-2}}{t^2}\times\sum a_i^2-\sum\limits_{t=2}^{n}\frac{2\mathrm{C}_{n-2}^{t-2}}{t^2}\times\sum a_ia_j$

$=\sum\limits_{t=2}^n\frac{(n-1)\mathrm{C}_{n-2}^{t-2}}{t^2}\sum a_i^2-\sum\limits_{t=2}^{n}\frac{2\mathrm{C}_{n-2}^{t-2}}{t^2}\sum a_ia_j=\sum\limits_{t=2}^{n}\frac{\mathrm{C}_{n-2}^{t-2}}{t^2}((n-1)\sum a_i^2-2\sum a_ia_j)$

$=\sum\limits_{t=2}^n\frac{\mathrm{C}_{n-2}^{t-2}}{t^2}(n\sum a_i^2-(\sum a_i)^2)$

对于 $\sum\limits_{t=2}^n\frac{\mathrm{C}_{n-2}^{t-2}}{t^2}$ 这一堆，再转换一下可以变成:

$\sum\limits_{t=1}^{n}\frac{\mathrm{C}_{n-2}^{t-2}}{t^2}=\sum\limits_{t=1}^n\frac{\mathrm{C}_n^t\mathrm{C}_t^2}{t^2\mathrm{C}_n^2}=\sum\limits_{t=21}^n\frac{(t-1)\mathrm{C}_n^t}{n(n-1)t}=\frac{1}{n(n-1)}\sum\limits_{t=1}^n(1-\frac{1}{t})\mathrm{C}_n^t=\frac{2^n-1+\sum\limits_{t=1}^n\frac{\mathrm{C}_n^t}{t}}{n(n-1)}$

即我们只需要求出 $\sum\limits_{t=1}^n\frac{\mathrm{C}_n^t}{t}$ 即可，打个表，扔到 OEIS 上，可以得到这个东西的通项公式:

$a_{n+4}=2\times(n+3)\times(n+2)^2\times a_{n+1}-(n+3)\times(13+5\times n)\times a_{n+2}+(4\times n+13)\times a_{n+3}$

得到这个东西的值以后我们就可以反推出 $\sum\limits_{t=2}^n\frac{\mathrm{C}_{n-2}^{t-2}}{t^2}$。对于 $\sum a_i^2$ 和 $\sum a_i$ ，只需要开两棵线段树维护一下就行了。

注意这个题比较恶心，在计算递推式的时候 $i$ 记得要开 `long long` 类型，不然到后面项数一大起来就爆了。

还有要注意这个题的数据范围比较大，内存限制又比较小，可能开两个线段树数组加延迟标记就四百多 MB 了，所以什么乱七八糟的数组还是不要开比较好。

还有如果直接通过计算 $x^{p-2}$ 会超时，请选用合适的求逆元的方法。

#### 参考代码

```c++
#include<bits/stdc++.h>
using namespace std;
template<typename T> inline
void read(T& x) {
	T f=1,b=0;char ch=getchar();
	while (ch==' '||ch=='\n'||ch=='\r') ch=getchar();
	if (ch=='-') f=-1,ch=getchar();
	while (ch!=' '&&ch!='\n'&&ch!='\r') 
		b*=10,b+=ch-'0',ch=getchar();
	x=f*b;return;
}
template<typename T> inline 
void print(T x) {
	if (x<0) putchar('-'),x=-x;
	if (x>9) print(x/10);putchar(x%10+'0');
}
typedef long long van;
const van mod=998244353;
const van MaxN=5e6+10;
van c[MaxN];
van n,m,inv[MaxN];
van power(van x,van t) {
	van base=x,ans=1;
	while (t) {
		if (t%2==1) ans*=base,ans%=mod;
		base*=base,base%=mod,t>>=1;
	}
	return ans;
}
struct SegmentTree {
	van data1[MaxN*4],data2[MaxN*4],lazytag[MaxN*4];
	void BuildTree(van p=1,van l=1,van r=n) {
		if (l==r) {
			data1[p]=data2[p]=0;
			data1[p]%=mod,data2[p]%=mod;
			return;
		}
		van mid=(l+r)>>1;
		BuildTree(p*2,l,mid);
		BuildTree(p*2+1,mid+1,r);
		data1[p]=data1[p*2]+data1[p*2+1];
		data2[p]=data2[p*2]+data2[p*2+1];
		data1[p]%=mod,data2[p]%=mod;
		return;
	}
	void spread(van p,van ls,van rs) {
		if (lazytag[p]) {
			data1[p*2]+=(data2[p*2]*2%mod*lazytag[p]%mod+ls*lazytag[p]%mod*lazytag[p]%mod)%mod;
			data2[p*2]+=lazytag[p]*ls%mod,data1[p*2]%=mod,data2[p*2]%=mod;
			data1[p*2+1]+=data2[p*2+1]*2%mod*lazytag[p]%mod+rs*lazytag[p]%mod*lazytag[p]%mod;
			data2[p*2+1]+=lazytag[p]*rs%mod,data1[p*2+1]%=mod,data2[p*2+1]%=mod;
			lazytag[p*2]+=lazytag[p],lazytag[p*2]%=mod;
			lazytag[p*2+1]+=lazytag[p],lazytag[p*2+1]%=mod,lazytag[p]=0;
		}
	}
	void AddTree(van L,van R,van num,van p=1,van l=1,van r=n) {
		if (L<=l&&r<=R) {
			data1[p]+=(data2[p]*2%mod*num%mod+(r-l+1)*num%mod*num%mod)%mod,data1[p]%=mod;
			lazytag[p]+=num;data2[p]+=(r-l+1)*num%mod;
			lazytag[p]%=mod,data2[p]%=mod;
			return;
		}
		van mid=(l+r)>>1;spread(p,mid-l+1,r-mid);
		if (L<=mid) AddTree(L,R,num,p*2,l,mid);
		if (R>mid) AddTree(L,R,num,p*2+1,mid+1,r);
		data1[p]=data1[p*2]+data1[p*2+1],data1[p]%=mod;
		data2[p]=data2[p*2]+data2[p*2+1],data2[p]%=mod;
		return;
	}
	van QueryTree1(van L,van R,van p=1,van l=1,van r=n) {
		if (L<=l&&r<=R) return data1[p];
		van mid=(l+r)>>1,sum=0;spread(p,mid-l+1,r-mid);
		if (L<=mid) sum+=QueryTree1(L,R,p*2,l,mid),sum%=mod;
		if (R>mid) sum+=QueryTree1(L,R,p*2+1,mid+1,r),sum%=mod;
		return sum;
	}
	van QueryTree2(van L,van R,van p=1,van l=1,van r=n) {
		if (L<=l&&r<=R) return data2[p];
		van mid=(l+r)>>1,sum=0;spread(p,mid-l+1,r-mid);
		if (L<=mid) sum+=QueryTree2(L,R,p*2,l,mid),sum%=mod;
		if (R>mid) sum+=QueryTree2(L,R,p*2+1,mid+1,r),sum%=mod;
		return sum;
	}
}T;
int main() {
	read(n),read(m);T.BuildTree();
	c[1]=1,c[2]=5,c[3]=29;
	for (van i=0;i<=n;i++) c[i+4]=((2*(i+3)*(i+2)%mod*(i+2)%mod*c[i+1]%mod-(i+3)*(13+5*i)%mod*c[i+2]%mod+(4*i+13)*c[i+3]%mod)%mod+mod)%mod;
	inv[1]=1;for (int i=2;i<=n;i++) inv[i]=(mod-mod/i)*inv[mod%i]%mod;
	van tmp=1;for (int i=1;i<=n;i++) c[i]*=tmp,tmp*=inv[i+1],c[i]%=mod,tmp%=mod;
	for (int i=2;i<=n;i++) c[i]=(((power(2,i)-1-c[i])%mod+mod)%mod)*inv[i]%mod*inv[i-1]%mod;
	for (int mm=1;mm<=m;mm++) {
		van op,l,r,x;read(op),read(l),read(r);
		if (op==1) read(x),T.AddTree(l,r,x);
		else print((c[r-l+1]*(((r-l+1)*T.QueryTree1(l,r)%mod-power(T.QueryTree2(l,r),2))%mod+mod))%mod),putchar('\n');
	}
	return 0;
}
```

### P3934 [Ynoi2016\] 炸脖龙 I

#### 题目简述

给一个长为 $n$ 的序列，$m$ 次操作，每次操作：

1、区间 $[l,r]$ 加 $x$

2、对于区间 $[l,r]$ ，查询：$a[l]^{a[l+1]^{a[l+2]^{\dots ^{a[r]}}}} \mod p$

#### 输入描述

第一行两个整数 $n,m$ 表示序列长度和操作数

接下来一行，$n$ 个整数 表示这个序列

接下来 $m$ 行，可能是以下两种操作之一：

$1\ l\ r\ x$ 表示区间 $[l,r]$ 加上 $x$

$2\ l\ r\ p$ 表示对区间 $[l,r]$ 进行一次查询，模数为 $p$

#### 输出描述

对于每个询问，输出一个数表示答案。

#### 数据范围

对于100%的数据，$n,m\le 500000$，序列中每个数在 $[1,2\cdot 10^9]$ 内，$p\le 2\cdot 10^7$，每次加上的数在 $[0,2\cdot 10^9]$ 内。

#### 题目分析

前置知识：扩展欧拉定理：$a^b\mod p=a^{b\phi(p)+\phi(p)}\mod p,b\ge p$。( ~~不需要掌握证明步骤，因为那是数学竞赛学生该做的事~~ )

根据此定理，我们可以递归解出 $[l,r]$ 区间范围内 $a[l]^{a[l-1]^{...^{a[r]}}}\mod p$。

然后树状数组和差分将区间修改变为单点修改即可。

恶心卡常，最后一个点卡了我十几次的错误提交记录。

#### 参考代码

```c++
#include<bits/stdc++.h>
#define van long long
using namespace std;
template<typename T> inline
void read(T& x) {
	T f=1,b=0;char ch=getchar();
	while (ch<'0'||ch>'9') {
		if (ch=='-') f=-1;
		ch=getchar();
	}
	while (ch>='0'&&ch<='9') 
		b*=10,b+=ch-'0',ch=getchar();
	x=f*b;return;
}
template<typename T> inline
void print(T x) {
	if (x<0) putchar('-');
	if (x>9) print(x/10);putchar(x%10+'0');
}

const van MaxP=2e7+10;
const van MaxN=5e5+10;
van phi[MaxP],a[MaxN];
void GetPrime() {
	phi[1]=1;
	for (register van i=2;i<=2e7;i+=2) phi[i]=i,phi[i+1]=i+1;
	for (register van i=2;i<=2e7;i++) {
		if (phi[i]!=i) continue;
		for (register van j=i;j<=2e7;j+=i) 
			phi[j]=phi[j]/i*(i-1);
	}
}
van n,m;
struct TreeArray {
	van c[MaxN]={0};
	van lowbit(van x) {return x&-x;}
	void addTree(van x,van num) {
		while (x<=n) {
			c[x]+=num;
			x+=lowbit(x);
		}
	}
	van queryTree(van x) {
		van res=0;
		while (x) {
			res+=c[x];
			x-=lowbit(x);
		}return res;
	}
}T;
struct node {
	van res;bool flag;
	node(van res,bool flag=true):res(res),flag(flag){};
};
node power(van x,van t,van p) {
	bool flag=(x>=p);x%=p;
	van base=x,ans=1;
	while (t) {
		if (t%2) ans*=base,flag|=(ans>=p),ans%=p;
		base*=base,flag|=(base>=p),base%=p,t>>=1;
	}return node(ans,flag);
}
node solve(van l,van r,van p) {
	van x=T.queryTree(l);
	if (p==1) return node(0,true);
	if (x==1) return node(1,false);
	if (l==r) return node(x%p,x>=p);
	node res=solve(l+1,r,phi[p]);
	if (res.flag) res.res+=phi[p];
	return power(x,res.res,p);
}
int main() {
	read(n);read(m);GetPrime();
	for (register van i=1;i<=n;i++) read(a[i]);
	for (register van i=1;i<=n;i++) T.addTree(i,a[i]-a[i-1]);
	for (register van i=1;i<=m;i++) {
		van op,l,r,x;read(op),read(l),read(r),read(x);
		if (op==1) T.addTree(l,x),T.addTree(r+1,-x);
		else print(solve(l,r,x).res),putchar('\n');
	}
}
```

### P5071 [Ynoi2015] 此时此刻的光辉

#### 题目简述

一个长为 $n$ 的序列，有 $m$ 次查询，每次查询一段区间的乘积的约数个数 $\bmod 19260817$ 的值。

#### 输入描述

第一行两个整数 $n,m$。

第二行 $n$ 个整数表示这个序列 $a_i$。

之后 $m$ 行，每行两个整数 $l,r$ 表示查询的区间

#### 输出描述

$m$ 行，每行输出一个整数表示答案

#### 数据范围

$1\leq n,m\leq 10^5$，$1\leq a_i\leq 10^9$。

#### 题目分析

求因数个数，考虑先将这个数分解质因数。由于这个大数是由连续的几个小数构成的，因此我们可以求出每一个数分解质因数后是什么样子，再将它们加起来。

由于数字很多，我们可以考虑使用 Pollard_Rho 算法将其分解 (虽然我是抄的就是了)。

然后对于每个询问暴力地将所有数的因数个数加起来，计算一下 $\prod\limits_{i=1}^k (c_i+1)$ 的结果就是了。

但是这样做肯定是会超时的，考虑优化。第一个想到的优化就是使用莫队对询问进行离线分块处理即可，这样做明显速度提升了不少，但是还不够，交上去依然只能获得十几分的好成绩。

那么导致超时的一个重要原因到底是什么呢？答案是因数个数。由于 $a_i\leq 10^9$，就导致了我们的因数个数有很多个。考虑减少一点因数的个数。

我们想到 $a_i\leq 10^9$，考虑以 $\leq 10^3$ 的因数为界限，这部分的因数个数有 160 多个，但是 $> 10^3$ 的因数却只有 2 个。为什么？很简单，你如果一个数有三个 $>10^3$ 的数乘起来不就超过了 $10^9$ 了吗？

那么回到问题，你 $\leq 10^3$ 的数比较少，我们可以对于每个询问直接计算这部分加上 1 的乘积，那么对于 $> 10^3$ 的因数，可以考虑使用离散化+桶子来计算这部分对答案的贡献。注意一定要使用离散化，不然会出现有数本来就是质数，导致桶子爆掉，获得 34 分的好成绩。

也有一种思路就是使用 STL 哈希表来统计。我试过，会 T 掉，只能得到 30 分。

还有一点要注意的是模运算中不允许出现除法，在使用逆元的时候记得多来一点空间。

#### 参考代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long van;
template<typename T> inline
void read(T& x) {
	T f=1,b=0;char ch=getchar();
	while (!isdigit(ch)) {
		if (ch=='-') f=-1;
		ch=getchar();
	} while (isdigit(ch))
		b*=10,b+=ch-'0',ch=getchar();
	x=f*b;return;
}
template<typename T> inline
void print(T x) {
	if (x<0) putchar('-'),x=-x;
	if (x>9) print(x/10);putchar(x%10+'0');
}
const van MaxN=1e5+10;
const van MaxW=1e9+10;
const van MaxK=400;
const van N=20;
const van mod=19260817;
van sum[MaxN][MaxK];
bool used[MaxN];van res[MaxN];
vector<van> prime;van siz;
van n,m,a[MaxN];unordered_map<van,van> lstpri[MaxN];
void init() {
	memset(used,0,sizeof used);
	for (int i=2;i<=N;i++) {
		if (used[i]) continue;prime.push_back(i);
		for (int j=i*i;j<=sqrt(MaxW)+10;j+=i) used[j]=1;
	}
}
struct query {
	van l,r,id,bel;
}q[MaxN];
bool cmp(query a,query b) {
	if (a.bel!=b.bel) return a.bel<b.bel;
	else return a.bel&1?a.r<b.r:a.r>b.r;
}
van f(van x,van c,van n) {return (x*x+c)%n;}
van gcd(van a,van b) {return b==0?a:gcd(b,a%b);}
van power(van a,van b,van m=mod) {
	van base=a%m,ans=1;
	while (b) {
		if (b&1) ans*=base,ans%=m;
		base*=base,base%=m,b>>=1;
	} return ans;
}
bool Miller_Rabin(van p) {
    if (p<2) return 0;
    if (p==2) return 1;
    if (p==3) return 1;
    van d=p-1,r=0;
    while (!(d&1)) ++r,d>>=1;
    for (van k=0;k<10;++k) {
	    van a=rand()%(p-2)+2;
	    van x=power(a,d,p);
	    if (x==1||x==p-1) continue;
	    for (int i=0;i<r-1;++i) {
	        x=(__int128)x*x%p;
	        if (x==p-1) break;
	    }
	    if (x!=p-1) return 0;
    }
    return 1;
}
van Pollard_Rho(van x) {
    van s=0,t=0,c=rand()%(x-1)+1;
    van step=0,goal=1,val=1;
	for (goal=1;;goal<<=1,s=t,val=1) {
		for (step=1;step<=goal;++step) {
			t=f(t,c,x),val=val*abs(t-s)%x;
			if ((step%127)==0) {
				van d=gcd(val,x);
				if (d>1) return d;
			}
		}
		van d=gcd(val,x);
		if (d>1) return d;
    }
}
van cnm=1;van bra[1000100];
van inv[3*MaxN];
van verybig[MaxN*3],cnt;
void fac(van x,van id) {
    if (Miller_Rabin(x)) { 
    	verybig[++cnt]=x;
    	lstpri[id][x]++;
	    return;
    }
    van p=x;while (p>=x) p=Pollard_Rho(x);
    fac(x/p,id),fac(p,id);
}
void add(van id) {
	for (auto it:lstpri[id]) cnm*=inv[bra[it.first]+1],cnm%=mod,
	bra[it.first]+=it.second,bra[it.first]%=mod,cnm*=(bra[it.first]+1),cnm%=mod;
}
void del(van id) {
	for (auto it:lstpri[id]) cnm*=inv[bra[it.first]+1],cnm%=mod,
	bra[it.first]+=mod-it.second,bra[it.first]%=mod,cnm*=(bra[it.first]+1),cnm%=mod;
}
int main() {
	read(n),read(m);init();siz=sqrt(n);
	inv[1]=1;for (van i=2;i<=3*n;i++) inv[i]=(mod-mod/i)*inv[mod%i]%mod;
	for (int i=1;i<=n;i++) {
		read(a[i]);for (int j=0;j<prime.size();j++) 
		while (a[i]%prime[j]==0) sum[i][j]++,a[i]/=prime[j];
		if (a[i]==1) continue;fac(a[i],i);
	} for (int i=1;i<=m;i++) read(q[i].l),read(q[i].r),q[i].id=i,
	q[i].bel=q[i].l%siz==0?q[i].l/siz:q[i].l/siz+1;
	for (int i=1;i<=n;i++) for (int j=0;j<prime.size();j++) sum[i][j]+=sum[i-1][j];
	sort(verybig+1,verybig+cnt+1);cnt=unique(verybig+1,verybig+cnt+1)-verybig-1;
	for (int i=1;i<=n;i++) { unordered_map<van,van> tmp;for (auto it:lstpri[i]) 
		tmp[lower_bound(verybig+1,verybig+cnt+1,it.first)-verybig]=it.second;
		lstpri[i]=tmp;
	}
	sort(q+1,q+m+1,cmp);van l=1,r=1;add(1);
	for (int i=1;i<=m;i++) {
		while (r<q[i].r) add(++r);
		while (l>q[i].l) add(--l);
		while (l<q[i].l) del(l++);
		while (r>q[i].r) del(r--);
		van tmp=1;for (int j=0;j<prime.size();j++) 
			tmp*=(sum[q[i].r][j]-sum[q[i].l-1][j]+1)%mod,tmp%=mod;
		tmp*=cnm,tmp%=mod;res[q[i].id]=tmp;
	} for (int i=1;i<=m;i++) print(res[i]),putchar('\n');
	return 0;
}
```

### P5356 [Ynoi2017] 由乃打扑克

#### 题目简述

给你一个长为 $n$ 的序列 $a$，需要支持 $m$ 次操作，操作有两种：

1. 查询区间 $[l,r]$ 的第 $k$ 小值。
2. 区间 $[l,r]$ 加上 $k$。

#### 输入描述

一行两个整数 $n,m$。

接下来一行 $n$ 个整数， 第 $i$ 个整数表示 $a_i$。

接下来 $m$ 行，每行四个数 $opt,l,r,k$，其中 $opt$ 代表是哪种操作。

#### 输出描述

对于每个询问输出一个数表示答案，如果无解输出 -1。

#### 数据范围

$1\leq n,m\leq 10^5,-2\times 10^4\leq$ 每次加上的数和原序列的数 $\leq 2\times 10^4$。

#### 题目分析

由于涉及到查询区间第 $k$ 小和区间修改，考虑通过分块进行维护，分块内存储排序后的序列。

将整个数据分为若干个小块，对于区间修改，只需要对零散的暴力添加，整块的整块打标记即可。添加零散的块时，找出在这个区间内的数和不在这个区间内的数，在这个区间的加上 $k$，不在的就不处理。由于是按照顺序拖出来的这两个块，因此这两个块的元素都是单调的，可以考虑使用归并来合并，时间复杂度为 $O(\sqrt n)$。

对于区间查询，使用二分，枚举最终的答案，再去找小于等于这个数的个数。同样的，对于零散的块，暴力查询有哪些既小于等于这个数，又在这个区间的数的个数。最后再将所有的区块贡献的答案加起来，判断是否符合 $k$ 的限制即可，时间复杂度 $O(\frac{1}{2}\sqrt n\log^2 n)$。

这样做很容易拿到 24 分的好成绩。考虑优化算法。

[ 优化一 ] : 我们发现区间查询的时间复杂度直接升天，考虑优化这一部分的算法。最先考虑的就是零散的块，由于我们可以在二分最初的时候就将这两个块拉出来，每次枚举的时候直接使用二分就可以得出两个零散的块贡献的答案。

[ 优化二 ] : 观察数据范围，由于每次加上的数和原序列的数的绝对值都小于等于 $2\times 10^4$，我们可以考虑限制二分的枚举范围，不要直接一上来就是 $2\times 10^9$。

[ 优化三 ] : 由于查询每个块时都要去二分一下，就会消耗 $O(\frac{1}{2}\log n)$ 的时间复杂度。那么对于整个的块，我们可以先将二分枚举出来的这个数与当前区块的最大值 / 最小值比较，有可能可以直接就得到这个区块的贡献的结果。

通过以上优化，您应该就能得到 100 分的好成绩。

如果还是不行，可以考虑限制块内二分的范围，用两个指针限制一下就行。

#### 参考代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long van;
template<typename T> inline
void read(T& x) {
	T f=1,b=0;char ch=getchar();
	while (!isdigit(ch)) {
		if (ch=='-') f=-1;
		ch=getchar();
	}
	while (isdigit(ch)) 
		b*=10,b+=ch-'0',ch=getchar();
	x=f*b;return;
}
template<typename T> inline
void print(T x) {
	if (x<0) putchar('-'),x=-x;
	if (x>9) print(x/10);putchar(x%10+'0');
}
const van MaxN=1e5+10;
const van MaxS=710;
struct node {van val,pos;};
bool operator < (const node& a,const node& b) {return a.val<b.val;}
van n,m,size=700,c[MaxN];
van bel[MaxN],id[MaxN];
struct block {
	node num[MaxS];van siz,x,tag;
	void init(van l,van r) {
		for (int i=l;i<=r;i++) num[i-l+1]=(node){c[i],i};
		siz=r-l+1;sort(num+1,num+siz+1);
	}
	void add(van x) {tag+=x;}
	node a[MaxS],b[MaxS];
	void add(van l,van r,van x) {
		for (int i=1;i<=siz;i++) num[i].val+=tag;
		tag=0;van ll=0,rr=0;for (int i=1;i<=siz;i++) {
			if (num[i].pos<l||num[i].pos>r) a[++ll]=num[i];
			else b[++rr]=num[i];
		} for (int i=1;i<=rr;i++) b[i].val+=x;
		van ls=1,rs=1,now=0;while (ls<=ll&&rs<=rr) {
			if (a[ls].val<b[rs].val) num[++now]=a[ls++];
			else num[++now]=b[rs++];
		} while (ls<=ll) num[++now]=a[ls++];
		while (rs<=rr) num[++now]=b[rs++];
		return;
	}
	van query(van k) {
		if (k-tag<num[1].val) return 0;
		if (k-tag>num[siz].val) return siz;
		return upper_bound(num+1,num+siz+1,(node){k-tag,0})-num-1;
	}
	van query(van l,van r,van k) {
		van lim=upper_bound(num+1,num+siz+1,(node){k-tag,0})-num-1,ans=0;
		for (int i=1;i<=lim;i++) if (num[i].pos>=l&&num[i].pos<=r) ans++;
		return ans;
	}
}B[MaxS];
void Update(van l,van r,van x) {
	if (bel[l]==bel[r]) B[bel[l]].add(l,r,x);
	else {
		B[bel[l]].add(l,bel[l]*size,x),B[bel[r]].add(bel[r]*size-size+1,r,x);
		for (int i=bel[l]+1;i<bel[r];i++) B[i].add(x);
	}
}
van aa[MaxS],bb[MaxS],as,bs;
van Calc(van l,van r,van num) {
	if (bel[l]==bel[r]) return upper_bound(aa+1,aa+as+1,num-B[bel[l]].tag)-aa-1;
	van sum=upper_bound(aa+1,aa+as+1,num-B[bel[l]].tag)-aa-1;sum+=upper_bound(bb+1,bb+bs+1,num-B[bel[r]].tag)-bb-1;
	for (int i=bel[l]+1;i<bel[r];i++) sum+=B[i].query(num);
	return sum;
}
void Query(van id,van l,van r,van v) {
	if (r-l+1<v) {
		print(-1),putchar('\n');
		return;
	}
	van ll=-2e4*id,rr=2e4*id,ans=0;as=bs=0;
	for (int i=1;i<=size;i++) if (B[bel[l]].num[i].pos>=l&&B[bel[l]].num[i].pos<=r) aa[++as]=B[bel[l]].num[i].val;
	for (int i=1;i<=size;i++) if (B[bel[r]].num[i].pos>=l&&B[bel[r]].num[i].pos<=r) bb[++bs]=B[bel[r]].num[i].val;
	while (ll<=rr) {
		van mid=(ll+rr)>>1;
		if (Calc(l,r,mid)>=v) ans=mid,rr=mid-1;
		else ll=mid+1;
	}print(ans),putchar('\n');
}
int main() {
	read(n),read(m);
	for (int i=1;i<=n;i++) read(c[i]);
	for (int i=1;i<=n;i++) {
		if (i%size==0) bel[i]=i/size,id[i]=size;
		else bel[i]=i/size+1,id[i]=i%size;
	}
	for (int i=1;i<=n/size+1;i++) B[i].init(i*size-size+1,i*size);
	for (int mm=1;mm<=m;mm++) {
		van op,l,r,k;read(op),read(l),read(r),read(k);
		if (op==1) Query(mm,l,r,k);
		else Update(l,r,k);
	}
	return 0;
} 
```
